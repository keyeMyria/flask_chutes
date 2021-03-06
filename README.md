flask_chutes
============

Redis/Socket.IO Pipeline Mixin for Flask

###Server
``` python
from flask_chutes import enable_chutes
from flask import Flask

app = Flask(__name__)
app.config['REDIS_CONN'] = {'host':'redis-host', 'db':0}
enable_chutes(app) 
```
###Client

    <!DOCTYPE html>
    <html>
      <head>
        <script type="text/javascript" src="http://code.jquery.com/jquery-1.4.2.min.js"></script>
        <script type="text/javascript">
           var ws = new WebSocket("ws://localhost:8000/chutes");
           ws.onopen = function() {
               //ws.send("{\"channel\": \"test\"}");
               ws.send(JSON.stringify({"channel":"MY_CHANNEL"}));
           };
           ws.onmessage = function(e) {
                $('#messages').append('<br>Received :' + e.data);
           }
           ws.onclose = function(evt) {
               alert("socket closed");
           };
        </script>
      </head>
      <body>
        <div id="messages">
        </div>
      </body>
    </html>

``` python
from flask_chutes import Chute
chute = Chute('MY_CHANNEL', **{'host':'redis-host', 'db':0})

chute.send({'my':'data'}) # 1->1 messaging
chute.publish({'my':'data') # 1->many messaging
```
### Running the Server
``` shell
gunicorn -k flask_sockets.worker example:app
```

Adapted from https://github.com/kennethreitz/flask-sockets.

    
