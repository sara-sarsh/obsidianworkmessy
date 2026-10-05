How to erase edge's database:

sudo systemctl stop tesla_charge.server 
 
sudo cp /opt/tesla-charge/tesla_charge.db /opt/tesla-charge/tesla_charge.db.backup 
 
sudo rm -f /opt/tesla-charge/tesla_charge.db 
sudo rm -f /opt/tesla-charge/tesla_charge.db-wal 
sudo rm -f /opt/tesla-charge/tesla_charge.db-shm 
 
sudo systemctl start tesla_charge.server