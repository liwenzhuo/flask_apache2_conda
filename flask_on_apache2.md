1  安装apache2 libapache2-mod-wsgi  并

```bash
sudo a2enmod wsgi
```

2 安装apache2-dev

3 conda create -n   flaskit python=3.8

4 pip install flask

   pip install mod-wsgi (这个需要前面的apache2-dev)

5 激活modwsgi

​	`mod_wsgi-express module-config`

6  在flask程序目录/var/www/hello

下面新建一个  hello.wsgi文件，内容如下

```python
import sys
sys.path.insert(0, "/var/www/hello")
from hello import app as application
```
hello.py程序内容如下

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def index():
  return "hello, world!"

if __name__ == '__main__':
  app.run(port=5001, debug=True)

```

7 在 /etc/apache2/mods-enabled/wsgi.load里面换成如下路径，由第5步输出

LoadModule wsgi_module /home/alvin/miniconda3/envs/flaskit/lib/python3.6/site-packages/mod_wsgi/server/mod_wsgi-py36.cpython-36m-x86_64-linux-gnu.so

8 在/etc/apache2/sites-available/hello.conf里面

```apache2
WSGIRestrictEmbedded On
<VirtualHost *>
        ServerName example.com
        WSGIDaemonProcess hello python-home=/home/alvin/miniconda3/envs/flaskit
        WSGIScriptAlias / /var/www/hello/hello.wsgi
        <Directory /var/www/hello>
                WSGIProcessGroup hello
                WSGIApplicationGroup %{GLOBAL}
                Order deny,allow
                Allow from all
        </Directory>
</VirtualHost>
```

9 启动这个site

```bash
sudo a2dissite 000-default.conf`
sudo a2ensite app.conf`
sudo systemctl reload apache2
```



  

