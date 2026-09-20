sudo apt update
sudo apt install -y mosquitto mosquitto-clients
sudo systemctl enable mosquitto
sudo systemctl start mosquitto
sudo systemctl status mosquitto

ران کردن سرور ام کیو تی تی روی مک:

docker run -it -p 1883:1883 docker.arvancloud.ir/eclipse-mosquitto
docker run -it -p 1884:1884 docker.arvancloud.ir/eclipse-mosquitto
"/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/.venv/bin/python" -m pip install -i https://mirror-pypi.runflare.com/simple paho-mqtt


pwd = /Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized
export PYTHONPATH="$(pwd)/opt/evck"
python opt/evck/gui_app/gui_decentralized.py

#todo باید سرویس های ران خودکار این مورد رو اوکی کنم 
#todo باید پکیج سرور و کلاینت این رو نصب کنم

pip install --index-url https://mirror2.chabokan.net/pypi/simple/ aiohttp




pip download  \
-i https://mirror-pypi.runflare.com/simple pytest\
-d "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/offline_packages"

pip install --no-index aiohttp \
--find-links="/Users/sara/Downloads/IDM file/aiohttp-3.13.5"\


pip install --no-index pyinstxtractor \
--find-links="/Users/sara/Documents/Personal/internetconn/scan sni/package"\

pip install --no-index pytest \
--find-links="/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/offline_packages"\

"/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/.venv/bin/pip" install \
--no-index \
--find-links="/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/offline_packages" \
pytest


which python
which pip
pip list | grep pytest



python opt/evck/gui_app/test_gui_decentralized.py


  export PYTHONPATH="$(pwd)/opt/evck"
  pytest opt/evck/gui_app/test_gui_decentralized.py -v
