Got it. You want to verify the systemd service itself is correctly loading the environment variables.

## Check if service is loading variables correctly

```bash
# 1. Check service status
sudo systemctl status evck-gui.service

# 2. See the actual environment variables the service has loaded
sudo systemctl show evck-gui.service --property=Environment

# 3. Better: see ALL environment variables in the running process
# First get the PID
PID=$(systemctl show evck-gui.service --property=MainPID --value)

# Then dump its environment
sudo cat /proc/$PID/environ | tr '\0' '\n'

# Or more readable:
sudo strings /proc/$PID/environ | sort
```

## Test the EnvironmentFile directives before starting service

```bash
# Manually source the files in the same order as systemd
set -a  # auto-export all variables

# Load in systemd's order
source /etc/evck/common/base.env
source /etc/evck/local.env
[ -f /etc/evck/local/settings.env ] && source /etc/evck/local/settings.env
[ -f /etc/evck/local/calibration.env ] && source /etc/evck/local/calibration.env

# Check if SPI is mounted
if mountpoint -q /mnt/spi; then
    [ -f /mnt/spi/evck/local/settings.env ] && source /mnt/spi/evck/local/settings.env
    [ -f /mnt/spi/evck/local/calibration.env ] && source /mnt/spi/evck/local/calibration.env
fi

set +a

# Now check specific variables
echo "EVCK_REDIS_URL=$EVCK_REDIS_URL"
echo "EVCK_MAX_POWER_KW=$EVCK_MAX_POWER_KW"
echo "DEV_OCPP_ONLY=$DEV_OCPP_ONLY"
# etc...
```

## Quick validation script

Create `/opt/evck/test_service_env.sh`:

```bash
#!/bin/bash
set -a
source /etc/evck/common/base.env
source /etc/evck/local.env 2>/dev/null || true
[ -f /etc/evck/local/settings.env ] && source /etc/evck/local/settings.env
[ -f /etc/evck/local/calibration.env ] && source /etc/evck/local/calibration.env

if mountpoint -q /mnt/spi 2>/dev/null; then
    echo "✓ SPI mounted"
    [ -f /mnt/spi/evck/local/settings.env ] && source /mnt/spi/evck/local/settings.env
    [ -f /mnt/spi/evck/local/calibration.env ] && source /mnt/spi/evck/local/calibration.env
else
    echo "✗ SPI not mounted"
fi
set +a

echo "=== Environment Variables ==="
env | grep EVCK_ | sort
env | grep DEV_ | sort
```

Run it:
```bash
chmod +x /opt/evck/test_service_env.sh
./test_service_env.sh
```

This shows you exactly what the service *should* see. Compare with what the running service actually has using the `/proc/$PID/environ` method above.