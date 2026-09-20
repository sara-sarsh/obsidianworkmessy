توضیح نحوه کار این کد
[[UI projection in orchestrator explanation]]
```python
def _calculate_ui_phase(self, gun: str, gun_live_state: GunState) -> Phase:
        """
        Purpose:
            Derive UI phase as a pure projection from hardware truth plus local overlays.
        Inputs:
            gun id and current per-gun runtime state.
        Side effects:
            None.
        Channels:
            None.
        State mutations:
            May clear expired local error hold for the gun.
        Concurrency:
            Called from multiple handlers before publishing UI status.
        """
        charge_s = gun_live_state.charge_status

        if self._emc_active:
            return Phase.FAULTED

        hold_code = self._error_hold_code.get(gun)
        hold_until = self._error_display_until.get(gun, 0.0)
        if hold_code and time.time() < hold_until:
            return hold_code
        if hold_code and str(charge_s) != hold_code and time.time() >= hold_until:
            self._error_hold_code[gun] = None

        cs_str = charge_s.name if hasattr(charge_s, 'name') else str(charge_s)
        if cs_str.startswith("ERROR") or cs_str.startswith("ChargeStatus.ERROR"):
            if cs_str.startswith("ChargeStatus."):
                return cs_str.split(".")[1]
            return cs_str

        # Nominal status-to-phase mapping.
        match charge_s:
            case ChargeStatus.NOT_CONNECTED:
                return Phase.IDLE

            case ChargeStatus.CONNECTED:
                # Keep explicit auth workflow phases while cable stays connected.
                if gun_live_state.phase == Phase.AUTHORIZING:
                    return Phase.AUTHORIZING
                if gun_live_state.phase == Phase.AUTH_REJECTED:
                    return Phase.AUTH_REJECTED
                if gun_live_state.phase == Phase.AUTHORIZED:
                    return Phase.AUTHORIZED
                
                # WAIT_CARD is a temporary overlay and auto-expires to QR flow.
                if gun_live_state.phase == Phase.WAIT_CARD:
                    start_wait = self._wait_card_ts.get(gun, 0)
                    if (time.time() - start_wait) > self._wait_card_timeout_s:
                        log.info("[TIMEOUT] Gun %s WAIT_CARD timed out. Reverting to QR.", gun)
                        return Phase.PLUGGED
                    return Phase.WAIT_CARD

                return Phase.PLUGGED

            case ChargeStatus.CHECKING:
                return Phase.WAITING

            case ChargeStatus.CHARGING:
                if gun_live_state.phase == Phase.SCAN_WHILE_CHARGING:
                    return Phase.SCAN_WHILE_CHARGING
                return Phase.CHARGING

            case ChargeStatus.FINISH:
                f_lock = self._finished_lock.get(gun)
                if f_lock is not None:
                    if (time.time() - f_lock) < self._finished_lock_timeout_s:
                        return Phase.FINISHED
                    else:
                        self._set_finished_lock(gun, None, "finished_lock_timeout")
                return Phase.FINISHED
            
            case ChargeStatus.UNKNOWN:
                return Phase.FAULTED

            case _:
                log.warning("[PHASE] Unknown charge_status: %s", charge_s)
                return Phase.IDLE


    def _pub_nozzle(self, state: str | None = None, gun: str = "A", message: str = None):
        """
        Purpose:
            Publish derived per-gun UI snapshot used by kiosk.
        Inputs:
            Optional compatibility state override, gun id, and optional message.
        Side effects:
            Emits phase-change FLOW log and publishes nozzle payload.
        Channels:
            Publishes `nozzle_status` on `gui_status_channel`.
        State mutations:
            Updates transient UI-related fields on GunState (phase, cost, duration, finals).
        Concurrency:
            Called from event handlers and monitor loops.
        """
        gun = gun.upper()
        gun_live_state: GunState = self.state_by_gun[gun]

        final_phase = self._calculate_ui_phase(gun, gun_live_state)

        # Compatibility override map for existing event paths.
        if state:
            m = {
                "connected": Phase.PLUGGED,
                "auth": Phase.AUTHORIZED,
                "waiting": Phase.WAITING,
                "idle": Phase.IDLE,
                "charging": Phase.CHARGING,
                "scan_while_charging": Phase.SCAN_WHILE_CHARGING,
                "stopped": Phase.FINISHED,
                "error": Phase.FAULTED,
                "auth_fail": Phase.AUTH_REJECTED,
                "revert": Phase.WAIT_CARD,
            }
            final_phase = m.get(state, final_phase)

        phase_name = final_phase.name if isinstance(final_phase, Phase) else str(final_phase)
        
        if not isinstance(final_phase, Phase) and not phase_name.startswith("ERROR"):
            log.warning("[GUI STATE] Unknown UI Phase: %s", final_phase)

        gun_live_state.phase = final_phase

        # QR is only visible in the PLUGGED flow and debounced to avoid flicker.
        qr_candidate = (final_phase == Phase.PLUGGED)

        now = time.time()
        cur = self._qr_visible[gun]
        pend = self._qr_pending[gun]
        


        if qr_candidate == cur:
            self._qr_pending[gun] = None
        else:
            if pend is None:
                self._qr_pending[gun] = (qr_candidate, now)
            else:
                pend_val, pend_ts = pend
                if pend_val != qr_candidate:
                    self._qr_pending[gun] = (qr_candidate, now)
                elif (now - pend_ts) >= self._qr_debounce_s:
                    self._qr_visible[gun] = qr_candidate
                    self._qr_last_change_ts[gun] = now
                    self._qr_pending[gun] = None

        show_qr_code = self._qr_visible[gun]

        # Hard hide QR in advanced flow phases.
        if final_phase in (
            Phase.WAITING,
            Phase.CHARGING,
            Phase.SCAN_WHILE_CHARGING,
            Phase.FINISHED,
            Phase.FAULTED,
            Phase.AUTHORIZED,
        ):
            show_qr_code = False

        # Build runtime and final-session metrics exposed to GUI.
        duration_s = gun_live_state.duration_s
        if gun_live_state.t_start:
            if final_phase in (Phase.CHARGING, Phase.SCAN_WHILE_CHARGING):
                duration_s = int(now - gun_live_state.t_start)
                gun_live_state.duration_s = duration_s
            elif final_phase == Phase.FINISHED and gun_live_state.duration_s:
                duration_s = int(gun_live_state.duration_s)

        price = self.price_per_kwh_irr.get(gun, PRICE_IRR)
        total_cost = int(gun_live_state.energy_kwh * price)
        gun_live_state.total_cost = total_cost

        if final_phase == Phase.FINISHED:
             if gun_live_state.final_duration_s is None: gun_live_state.final_duration_s = duration_s
             if gun_live_state.final_energy_kwh is None: gun_live_state.final_energy_kwh = gun_live_state.energy_kwh
             if gun_live_state.final_cost is None:       gun_live_state.final_cost = total_cost

        metrics = {
            "voltage_v": gun_live_state.voltage_v,
            "current_a": gun_live_state.current_a,
            "power_kw": gun_live_state.power_kw,
            "energy_kwh": gun_live_state.energy_kwh,
            "duration_s": gun_live_state.duration_s,
            "total_cost": gun_live_state.total_cost,
            "final_duration_s": gun_live_state.final_duration_s,
            "final_energy_kwh": gun_live_state.final_energy_kwh,
            "final_cost": gun_live_state.final_cost,
            "extended_metrics": {
                "battery_voltage_v": gun_live_state.battery_voltage_v,
                "demand_current_a":  gun_live_state.demand_current_a,
                "soc":               gun_live_state.soc,
                "charge_mode":       gun_live_state.charge_mode,
                "charge_limiter":    gun_live_state.charge_limiter,
                "res_p":             gun_live_state.res_p,
                "res_n":             gun_live_state.res_n,
                "temp_p":            gun_live_state.temp_p,
                "temp_n":            gun_live_state.temp_n,
                "cells_mean_v":      gun_live_state.cells_mean_v,
                "cells_mean_temp":   gun_live_state.cells_mean_temp,
                "dual_power":        gun_live_state.dual_power,
            },
        }

        pl = {
            "id": gun,
            "message": message if message else "",
            "state": phase_name,
            "charge_status": gun_live_state.charge_status.value if hasattr(gun_live_state.charge_status, 'value') else str(gun_live_state.charge_status),
            "available_power_kw": gun_live_state.available_power_kw,
            "gun_connected": phase_name not in (Phase.IDLE.value, Phase.FAULTED.value, ChargeStatus.ERROR_NOT_CONNECTED.value),
            "price_per_kwh": price,
            "metrics": metrics,
        }
        
        
        # Reduce log noise by reporting only phase transitions.
        if gun_live_state.phase != gun_live_state._prev_logged_phase:
            log.info(
                "[FLOW] component=orchestrator session=%s event=phase_change gun=%s state_from=%s state_to=%s charge_status=%s qr_visible=%s pending_hw_start=%s id_tag=%s",
                gun_live_state.session_id or "NONE",
                gun,
                gun_live_state._prev_logged_phase or "NEW",
                phase_name,
                gun_live_state.charge_status.name if hasattr(gun_live_state.charge_status, 'name') else gun_live_state.charge_status,
                "true" if show_qr_code else "false",
                self._pending_hw_start.get(gun),
                gun_live_state.id_tag or "NONE",
            )
            gun_live_state._prev_logged_phase = phase_name

        self.bus.publish(
            "gui_status_channel", envelope("status", "nozzle_status", "orchestrator", pl)
        )
   
```