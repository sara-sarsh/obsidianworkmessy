Here is the complete, end-to-end command sequence to run on your Mac.

This sequence automates the entire process: it scans your code with `pipreqs`, converts the output to a `.in` file, automatically deletes the false local PyPI matches (using macOS native `sed` commands so you don't have to open a text editor), and finally uses `pip-compile` to generate the locked `requirements.txt`.

Copy and paste these directly into your Mac terminal from the root `evck160c- decentralized` directory.

### 1. Install the Required Tools

```bash
pip install pipreqs pip-tools

```

### 2. Common Module

```bash
pipreqs --force opt/evck/common
mv opt/evck/common/requirements.txt opt/evck/common/requirements.in
sed -i '' '/^OPi==/d' opt/evck/common/requirements.in
pip-compile opt/evck/common/requirements.in

```

### 3. LPC Interface Module

```bash
pipreqs --force opt/evck/lpc_interface
mv opt/evck/lpc_interface/requirements.txt opt/evck/lpc_interface/requirements.in
sed -i '' '/^common==/d' opt/evck/lpc_interface/requirements.in
sed -i '' '/^OPi==/d' opt/evck/lpc_interface/requirements.in
pip-compile opt/evck/lpc_interface/requirements.in

```

### 4. OPi Hardware Module

```bash
pipreqs --force opt/evck/opi_hardware
mv opt/evck/opi_hardware/requirements.txt opt/evck/opi_hardware/requirements.in
sed -i '' '/^common==/d' opt/evck/opi_hardware/requirements.in
sed -i '' '/^OPi==/d' opt/evck/opi_hardware/requirements.in
pip-compile opt/evck/opi_hardware/requirements.in

```

### 5. OCPP Client Module

```bash
pipreqs --force opt/evck/ocpp_client
mv opt/evck/ocpp_client/requirements.txt opt/evck/ocpp_client/requirements.in
sed -i '' '/^common==/d' opt/evck/ocpp_client/requirements.in
sed -i '' '/^ocpp_ws_client_real==/d' opt/evck/ocpp_client/requirements.in
pip-compile opt/evck/ocpp_client/requirements.in

```

### 6. GUI App Module

```bash
pipreqs --force opt/evck/gui_app
mv opt/evck/gui_app/requirements.txt opt/evck/gui_app/requirements.in
sed -i '' '/^common==/d' opt/evck/gui_app/requirements.in
sed -i '' '/^gui==/d' opt/evck/gui_app/requirements.in
pip-compile opt/evck/gui_app/requirements.in

```

Once you run these, your decentralized modules will each have a perfectly locked `requirements.txt` file containing every single explicit and hidden sub-dependency. You can then run your offline `pip download` sequence, and pip will safely grab every file you need for the Orange Pi without missing anything.


pip download -r opt/evck/common/requirements.txt --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages
pip download -r opt/evck/lpc_interface/requirements.txt --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages
pip download -r opt/evck/opi_hardware/requirements.txt --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages
pip download -r opt/evck/ocpp_client/requirements.txt --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages
pip download -r opt/evck/gui_app/requirements.txt --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages
pip download async-timeout tomli exceptiongroup --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages
pip download netifaces==0.11.0 -d opt/evck/offline_packages
pip download typing-extensions --platform manylinux2014_aarch64 --python-version 3.10 --only-binary=:all: -d opt/evck/offline_packages


sudo chown -R orangepi:orangepi /opt/evck


# # 1. Install all dependencies from the single master folder
pip install --no-index --find-links=/opt/evck/offline_packages -r /opt/evck/common/requirements.txt
pip install --no-index --find-links=/opt/evck/offline_packages -r /opt/evck/lpc_interface/requirements.txt
pip install --no-index --find-links=/opt/evck/offline_packages -r /opt/evck/opi_hardware/requirements.txt
pip install --no-index --find-links=/opt/evck/offline_packages -r /opt/evck/ocpp_client/requirements.txt
pip install --no-index --find-links=/opt/evck/offline_packages -r /opt/evck/gui_app/requirements.txt

# 2. Install your local modules
pip install -e ./common
pip install -e ./lpc_interface
pip install -e ./opi_hardware
pip install -e ./ocpp_client
pip install -e ./gui_app