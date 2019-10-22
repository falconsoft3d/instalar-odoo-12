# Instalación de Odoo 12

![Alt text](https://github.com/falconsoft3d/instalar-odoo-10/blob/master/img/logo-ynext.png?raw=true "Ynext")

## 1- Actualizamos el sistema

```linux
apt-get update && apt-get upgrade -y
```


---------------------
## 2- Creamos el usuario Odoo

```linux
adduser --system --home=/opt/odoo --group odoo
```


---------------------
## 3- Instalamos postgresql

```linux
sudo apt install postgresql postgresql-contrib
```


---------------------
## 4- Reiniciamos postgres, iniciamos sesión en postgres y creamos el usuario postgres

```linux
service postgresql restart
su - postgres
createuser --createdb --username postgres --no-createrole --no-superuser --pwprompt odoo
exit
```

## 5- Instalamos Librerias

```linux
sudo apt-get update && sudo apt-get install postgresql postgresql-server-dev-10 build-essential python3-pil python3-lxml python-ldap3 python3-dev python3-pip python3-setuptools npm nodejs git gdebi libldap2-dev libxml2-dev libxslt1-dev libjpeg-dev -y

sudo apt-get install libsasl2-dev
```

## Respaldos
```
mkdir /opt/odoo/backups
chown odoo:root /opt/odoo/backups
```

## 5- Descargamos Odoo 12

```linux
sudo git clone --depth 1 --branch 12.0 https://github.com/odoo/odoo /opt/odoo/server
sudo pip3 install -r /opt/odoo/server/requirements.txt
```

## 6- Usamos npm, que es el gestor de paquetes Node.js para instalar less

```linux
sudo npm install -g less less-plugin-clean-css -y && sudo ln -s /usr/bin/nodejs /usr/bin/node
```

## 7- Instalar wkhtmltopdf para generar PDF en odoo

```linux
sudo apt install xfonts-base xfonts-75dpi -y
cd /tmp
wget http://security.ubuntu.com/ubuntu/pool/main/libp/libpng/libpng12-0_1.2.54-1ubuntu1.1_amd64.deb && sudo dpkg -i libpng12-0_1.2.54-1ubuntu1.1_amd64.deb
wget https://downloads.wkhtmltopdf.org/0.12/0.12.5/wkhtmltox_0.12.5-1.xenial_amd64.deb && sudo dpkg -i wkhtmltox_0.12.5-1.xenial_amd64.deb
sudo ln -s /usr/local/bin/wkhtmltopdf /usr/bin/
sudo ln -s /usr/local/bin/wkhtmltoimage /usr/bin/
```

## 8- Hacemos que Odoo inicie Automatico

```linux
update-rc.d odoo defaults
sudo service odoo start
# Para que postgres se inicie automáticamente
update-rc.d postgresql restart
```

## 9- Configuramos Postgree

```linux
nano /etc/postgresql/10/main/pg_hba.conf
```
```linux
local   all             all        peer

*Sustituimos por:

local   all             all       trust
```

## 10 - Reiniciando
```linux
service postgresql restart
/etc/init.d/odoo restart
```

## 11 - Instalamos ngix para cambiar el puerto
```linux
sudo apt-get install nginx -y
cd /etc/nginx/sites-available
git clone https://github.com/falconsoft3d/ngix-para-odoo-erp/
cd ngix-para-odoo-erp/
sudo cp /etc/nginx/sites-available/ngix-para-odoo-erp/default.conf /etc/nginx/sites-available/default.conf
cd ..
mv default default-temp
mv default.conf default

cd /etc/nginx/sites-available
nano default
server_name j.wemakeyourdayeasy.com 11.64.123.12;
nginx -s reload
```

## 12 - Log
```linux
mkdir /var/log/odoo/
chown odoo:root /var/log/odoo

mkdir /etc/odoo
cp /opt/odoo/server/debian/odoo.conf /etc/odoo/odoo.conf
chown odoo: /etc/odoo/odoo.conf
chmod 640 /etc/odoo/odoo.conf
```

## 13 - Creamos Carpeta Extra-addons
```linux
mkdir /opt/odoo/server/extra-addons
chown odoo: /opt/odoo/ -R
```

## 14 - Configuramos el archivo conf
```linux
nano /etc/odoo/odoo.conf
```
```linux
db_user = odoo
db_password = CLAVE DEL USUARIO  ODOO EN POSTGRES
addons_path = /opt/odoo/server/addons,/opt/odoo/server/extra-addons/addons_3ros,/opt/odoo/server/extra-addons/addons_general,/opt/odoo/server/extra-addons/odoo_chile
logfile = /var/log/odoo/odoo-server.log
logrotate = True
```

## 15 - Odoo Incio
```linux
sudo cp /opt/odoo/server/debian/init /etc/init.d/odoo && sudo chmod +x /etc/init.d/odoo
sudo ln -s /opt/odoo/server/odoo-bin /usr/bin/odoo
update-rc.d odoo defaults
sudo service odoo start
```

## 16 - Clonar repositorio
```linux
cd /opt/odoo/server/extra-addons/
git clone git@bitbucket.org:marlonodoo/addons_3ros.git
git clone git@bitbucket.org:marlonodoo/addons_general.git
git clone git@bitbucket.org:marlonodoo/odoo_chile.git
```

## 17 - Tools
```linux
sudo service odoo stop
su - odoo -s /bin/bash
python2 /opt/odoo/server/odoo-bin -c /etc/odoo/odoo.conf -d db12-chile-sii -u all --stop-after-init
sudo service odoo restart

tail -f /var/log/odoo/odoo-server.log
```


# Pycham lxml ERROR
```linux
sudo apt-get install python3 python-dev python3-dev \
     build-essential libssl-dev libffi-dev \
     libxml2-dev libxslt1-dev zlib1g-dev \
     python-pip
```

# Librerias
```linux
pip3 install Babel==2.3.4
pip3 install chardet==3.0.4
pip3 install decorator==4.0.10
pip3 install docutils==0.12
pip3 install ebaysdk==2.1.5
pip3 install feedparser==5.2.1
pip3 install gevent==1.1.2 ; sys_platform != 'win32' and python_version < '3.7'
pip3 install gevent==1.3.4 ; sys_platform != 'win32' and python_version >= '3.7'
pip3 install greenlet==0.4.10 ; python_version < '3.7'
pip3 install greenlet==0.4.13 ; python_version >= '3.7'
pip3 install html2text==2016.9.19
pip3 install Jinja2==2.10.1
pip3 install libsass==0.12.3
pip3 install lxml==3.7.1 ; sys_platform != 'win32' and python_version < '3.7'
pip3 install lxml==4.2.3 ; sys_platform != 'win32' and python_version >= '3.7'
pip3 install lxml ; sys_platform == 'win32'
pip3 install Mako==1.0.4
pip3 install MarkupSafe==0.23
pip3 install mock==2.0.0
pip3 install num2words==0.5.6
pip3 install ofxparse==0.16
pip3 install passlib==1.6.5
pip3 install Pillow==4.0.0
pip3 install psutil==4.3.1; sys_platform != 'win32'
pip3 install psycopg2==2.7.3.1; sys_platform != 'win32'
pip3 install pydot==1.2.3
pip3 install pyldap==2.4.28; sys_platform != 'win32'
pip3 install pyparsing==2.1.10
pip3 install PyPDF2==1.26.0
pip3 install pyserial==3.1.1
pip3 install python-dateutil==2.5.3
pip3 install pytz==2016.7
pip3 install pyusb==1.0.0
pip3 install qrcode==5.3
pip3 install reportlab==3.3.0
pip3 install requests==2.20.0
pip3 install suds-jurko==0.6
pip3 install vatnumber==1.2
pip3 install vobject==0.9.3
pip3 install Werkzeug==0.11.15
pip3 install XlsxWriter==0.9.3
pip3 install xlwt==1.3.*
pip3 install xlrd==1.0.0
pip3 install pypiwin32 ; sys_platform == 'win32'
```

```linux
pip3 install xmltodict
pip3 install dicttoxml
pip3 install cchardet
pip3 install cryptography
sudo pip3 install pyOpenSSL
sudo apt-get install python-m2crypto
sudo apt-get install libssl-dev swig python3-dev gcc
sudo pip3 install M2Crypto
sudo pip3 install SOAPpy
sudo pip3 install signxml
sudo pip3 install pdf417gen
```








