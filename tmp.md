

فایل messaging.py
جدید رو بهم بده

(.venv) sara@MacBook-Pro evck160c- decentralized % ls opt/evck/gui_app                          
__init__.py                     guitest.py                      templates
__pycache__                     logs                            test_gui_decentralized.py
gui_decentralized_mock.py       offline_packages                verify_templates.py
gui_decentralized.py            requirements.txt
gui.py                          static



(.venv) sara@MacBook-Pro evck160c- decentralized % python opt/evck/gui_app/test_gui_decentralized.py
Traceback (most recent call last):
  File "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/test_gui_decentralized.py", line 8, in <module>
    from gui import app, socketio, state_cache, on_mqtt_message, envelope
  File "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/gui.py", line 6, in <module>
    from common.messaging import MessageBroker
ModuleNotFoundError: No module named 'common'
(.venv) sara@MacBook-Pro evck160c- decentralized % export PYTHONPATH="$(pwd)/opt/evck"
(.venv) sara@MacBook-Pro evck160c- decentralized % python opt/evck/gui_app/test_gui_decentralized.py
Traceback (most recent call last):
  File "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/test_gui_decentralized.py", line 8, in <module>
    from gui import app, socketio, state_cache, on_mqtt_message, envelope
ImportError: cannot import name 'on_mqtt_message' from 'gui' (/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/gui.py)
(.venv) sara@MacBook-Pro evck160c- decentralized % python opt/evck/gui_app/test_gui_decentralized.py
Traceback (most recent call last):
  File "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/test_gui_decentralized.py", line 8, in <module>
    from gui_decentralized import app, socketio, state_cache, on_mqtt_message, envelope
ImportError: cannot import name 'envelope' from 'gui_decentralized' (/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/gui_decentralized.py)
(.venv) sara@MacBook-Pro evck160c- decentralized % python opt/evck/gui_app/test_gui_decentralized.py
Traceback (most recent call last):
  File "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/test_gui_decentralized.py", line 8, in <module>
    from gui_decentralized import app, socketio, state_cache, on_mqtt_message, envelope
ImportError: cannot import name 'envelope' from 'gui_decentralized' (/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/gui_app/gui_decentralized.py)
(.venv) sara@MacBook-Pro evck160c- decentralized % 
