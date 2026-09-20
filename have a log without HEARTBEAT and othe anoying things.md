docker logs -f --tail=50 evck-ocpp 2>&1 | grep --line-buffered -vE '\[HEARTBEAT\]|\[STATION INFO\]|\[REG-CHECK\]|No config changes detected|\[TX ACK\]'
#docker
#log 
#grep