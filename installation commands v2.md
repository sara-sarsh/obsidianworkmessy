Here is the fully updated, finalized script. I have integrated the `OPi.GPIO-ex` fix directly into the Mac compilation steps so it is entirely seamless.

### Part 1: Run on MacBook (Generate, Download, and Sync)

```bash
# 1. Install the Required Tools
pip install pipreqs pip-tools

# 2. Common Module
pipreqs --force opt/evck/common
mv opt/evck/common/requirements.txt opt/evck/common/requirements.in
sed -i '' '/^OPi==/d' opt/evck/common/requirements.in
pip-compile opt/evck/common/requirements.in

# 3. LPC Interface Module
pipreqs --force opt/evck/lpc_interface
mv opt/evck/lpc_interface/requirements.txt opt/evck/lpc_interface/requirements.in
sed -i '' '/^common==/d' opt/evck/lpc_interface/requirements.in
sed -i '' '/^OPi==/d' opt/evck/lpc_interface/requirements.in
pip-compile opt/evck/lpc_interface/requirements.in

# 4. OPi Hardware Module (Includes GPIO-ex fix)
pipreqs --force opt/evck/opi_hardware
mv opt/evck/opi_hardware/requirements.txt opt/evck/opi_hardware/requirements.in
sed -i '' '/^common==/d' opt/evck/opi_hardware/requirements.in
sed -i '' '/^OPi==/d' opt/evck/opi_hardware/requirements.in
echo "OPi.GPIO-ex" >> opt/evck/opi_hardware/requirements.in
pip-compile opt/evck/opi_hardware/requirements.in

# 5. OCPP Client Module
pipreqs --force opt/evck/ocpp_client
mv opt/evck/ocpp_client/requirements.txt opt/evck/ocpp_client/requirements.in
sed -i '' '/^common==/d' opt/evck/ocpp_client/requirements.in
sed -i '' '/^ocpp_ws_client_real==/d' opt/evck/ocpp_client/requirements.in
pip-compile opt/evck/ocpp_client/requirements.in

# 6. GUI App Module
pipreqs --force opt/evck/gui_app
mv opt/evck/gui_app/requirements.txt opt/evck/gui_app/requirements.in
sed -i '' '/^common==/d' opt/evck/gui_app/requirements.in
sed -i '' '/^gui==/d' opt/evck/gui_app/requirements.in
pip-compile opt/evck/gui_app/requirements.in

# 7. Download Main Packages
pip download -r opt/evck/common/requirements.txt --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages
pip download -r opt/evck/lpc_interface/requirements.txt --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages
pip download -r opt/evck/opi_hardware/requirements.txt --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages
pip download -r opt/evck/ocpp_client/requirements.txt --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages
pip download -r opt/evck/gui_app/requirements.txt --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages

# 8. Download Edge Cases
pip download async-timeout tomli exceptiongroup --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages
pip download netifaces==0.11.0 OPi.GPIO-ex -d opt/evck/offline_packages
pip download typing-extensions --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages

# 9. Sync to Orange Pi
rsync -av --delete --exclude='.venv' --exclude='logs' --exclude='__pycache__' --exclude='.DS_Store' "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/" orangepi@192.168.0.102:/opt/evck/

```

---

### Part 2: Run on Orange Pi (Install)

```bash
# 1. Fix Permissions and Activate Environment
sudo chown -R orangepi:orangepi /opt/evck
cd /opt/evck
source .venv/bin/activate

# 2. Install all PyPI dependencies from the single master folder
pip install --no-index --find-links=/opt/evck/offline_packages -r /opt/evck/common/requirements.txt
pip install --no-index --find-links=/opt/evck/offline_packages -r /opt/evck/lpc_interface/requirements.txt
pip install --no-index --find-links=/opt/evck/offline_packages -r /opt/evck/opi_hardware/requirements.txt
pip install --no-index --find-links=/opt/evck/offline_packages -r /opt/evck/ocpp_client/requirements.txt
pip install --no-index --find-links=/opt/evck/offline_packages -r /opt/evck/gui_app/requirements.txt

# 3. Install your local modules
cd /opt/evck
pip install -e .

```

Now that the hardware environment is locked, are you going to test the systemd services first, or will you run the Python modules manually to monitor the initial logs?





# 1. Lock the stable version in both modules that use GPIO
sed -i '' '/^OPi.GPIO-ex/d' opt/evck/opi_hardware/requirements.in
echo "OPi.GPIO-ex==0.5.5" >> opt/evck/opi_hardware/requirements.in

sed -i '' '/^OPi.GPIO-ex/d' opt/evck/lpc_interface/requirements.in
echo "OPi.GPIO-ex==0.5.5" >> opt/evck/lpc_interface/requirements.in

# 2. Re-compile the lock files
pip-compile opt/evck/opi_hardware/requirements.in
pip-compile opt/evck/lpc_interface/requirements.in

# 3. Download the specific 0.5.5 offline file
pip download OPi.GPIO-ex==0.5.5 -d opt/evck/offline_packages

# 4. Sync the changes to the board
rsync -av --delete --exclude='.venv' --exclude='logs' --exclude='__pycache__' --exclude='.DS_Store' "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/" orangepi@192.168.0.102:/opt/evck/


[[installation commands- attachments]]
[[installation commands- attachments- kiosk service]]