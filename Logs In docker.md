
sudo mkdir -p /opt/evck/logs

sudo docker logs -t \
  --since "2026-10-01T05:00:00" \
  --until "2026-10-01T06:10:00" \
  evck-ocpp \
  > /opt/evck/logs/evck-ocpp-2026-10-01-0534-to-0610.log 2>&1
