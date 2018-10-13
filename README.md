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


---------------------
## 5- Si instalamos el módulo de búsqueda avanzada tenemos que correr el siguiente comando

```linux
apt-get update
sudo apt-get install postgresql-contrib
sudo /etc/init.d/postgresql restart
```


---------------------
## 6- Descargamos Odoo, Instalamos unzip

```linux
apt-get install unzip
```


---------------------
## 7- Ingresamos en la carpeta /opt/odoo y descargamos la fuente para la versión comunity

```linux
cd /opt/odoo/
wget https://github.com/odoo/odoo/archive/10.0.zip
unzip 10.0.zip
```


---------------------
## 8- Ingresamos en la carpeta /opt/odoo y descargamos la fuente para la versión comunity

```linux
mv odoo-10.0 server
chown -R odoo: server
```


---------------------
## 9- Instalación de librerias, actualizamos pip e instalamos dependencias python de Odoo

```linux
apt install python-pip libcups2-dev libxml2-dev libxslt-dev node-less libsasl2-dev libldap2-dev python-lxml -y
```


---------------------
## 10- Actualizamos PIP

```linux
pip install --upgrade pip
hash -d pip

# Con el usuario ubuntu
export LC_ALL=C
source .bashrc
```


---------------------
## 11- Crear carpeta para el módulo de dropbox

```linux
mkdir /opt/odoo/backups
chown odoo:root /opt/odoo/backups
```


---------------------
## 12- Librerias Ncesarias

```linux
pip install -r /opt/odoo/server/requirements.txt
```


---------------------
## 13- Creando un directorio para almacenar el archivo de logs

```linux
mkdir /var/log/odoo/
chown odoo:root /var/log/odoo
```


---------------------
## 14- Configurando Odoo Server

```linux
mkdir /etc/odoo
cp /opt/odoo/server/debian/odoo.conf /etc/odoo/odoo.conf
chown odoo: /etc/odoo/odoo.conf
chmod 640 /etc/odoo/odoo.conf
```


---------------------
## 15- Creamos la carpeta de los ExtraAddons

```linux
mkdir /opt/odoo/server/extra-addons
chown odoo: /opt/odoo/ -R
```


---------------------
## 16- Editamos el archivo odoo.conf

```linux
nano /etc/odoo/odoo.conf
```


---------------------
## 17- Modificamos y/o agregamos lo siguiente y guardamos el archivo, si no tienes módulo en estra-addons no coloque la ruta sino te dará problemas.

```linux
db_user = odoo
db_password = CLAVE DEL USUARIO  ODOO EN POSTGRES
addons_path = /opt/odoo/server/addons,/opt/odoo/server/extra-addons/odoo_chile_community,/opt/odoo/server/extra-addons/odoo-modulos-3ros,/opt/odoo/server/extra-addons/odoo_general,/opt/odoo/server/extra-addons/odoo_chile_rrhh,/opt/odoo/server/extra-addons/odoo-modulos-web,/opt/odoo/server/extra-addons/odoo_general_web
logfile = /var/log/odoo/odoo-server.log
logrotate = True
log_level = warn
```


---------------------
## 18- Script de inicio automático de Odoo-Server en Ubuntu 16

```linux
cp /opt/odoo/server/debian/init /etc/init.d/odoo
chmod 755 /etc/init.d/odoo
chown root: /etc/init.d/odoo
```


---------------------
## 19- Editamos el archivo:

```linux
nano /etc/init.d/odoo
```


---------------------
## 20- Modificamos los siguientes valores, y guardamos el archivo:

```linux
DAEMON=/opt/odoo/server/odoo-bin
```


---------------------
## 21- Haciendo que Odoo se inicie automáticamente cuando reiniciemos nuestro servidor:

```linux
update-rc.d odoo defaults
```


---------------------
## 22- Haciendo que Postgresql se inicie automáticamente cuando reiniciemos nuestro servidor :

```linux
update-rc.d postgresql enable
```


---------------------
## 23- Manipulamos el servicio

```linux
/etc/init.d/odoo start|stop|restart
```


---------------------
## 24- Editar archivo de configuración de postgres pg_hba.conf

```linux
nano /etc/postgresql/9.5/main/pg_hba.conf
```
Editamos la siguiente linea

```linux
local   all             all        peer

*Sustituimos por:

local   all             all       trust
```


---------------------
## 25- Reiniciamos servicio de postgresql y odoo
```linux
service postgresql restart
/etc/init.d/odoo restart
```


---------------------
## 26- Instalar Libreria wkhtmltopdf

```linux
sudo apt-get -f install
sudo apt-get install libxrender1 fontconfig xvfb libjpeg-turbo8
cd /opt
wget https://downloads.wkhtmltopdf.org/0.12/0.12.5/wkhtmltox_0.12.5-1.xenial_amd64.deb
dpkg -i wkhtmltox_0.12.5-1.xenial_amd64.deb
apt install -f
cp /usr/local/bin/wkhtmltoimage /usr/bin/wkhtmltoimage
cp /usr/local/bin/wkhtmltopdf /usr/bin/wkhtmltopdf
```


---------------------
## 27- Reiniciamos odoo erp

```linux
/etc/init.d/odoo restart
```


---------------------
## 28- Vemos el Log

```linux
tail -f /var/log/odoo/odoo-server.log
```


---------------------
## 29- Actualizamos la Zona horaria desde la consola de ubuntu

```linux
tzselect
```


---------------------
## 30- Actualizamos zona horaria servidor

```linux
sudo dpkg-reconfigure tzdata
```


---------------------
## 31- Actualizamos la hora

```linux
date --set "2007-05-27 17:27"
hwclock --set --date="2007-05-27 17:27"
hwclock
date
```


---------------------

## 32- Por seguridad le ponemos un pass a Postgre

```linux
sudo -u postgres psql postgres
\password postgres
Enter new password:
```


---------------------
## 33- Clonamos los módulo de la localización de Chile

```linux
cd /opt/odoo/server/extra-addons
git clone git@bitbucket.org:marlonodoo/odoo-modulos-3ros.git
git clone git@bitbucket.org:marlonodoo/odoo_chile_community.git
git clone git@bitbucket.org:marlonodoo/odoo_general.git
git clone git@bitbucket.org:marlonodoo/odoo_chile_rrhh.git
git clone git@bitbucket.org:marlonodoo/odoo-modulos-web.git
git clone git@bitbucket.org:marlonodoo/odoo_general_web.git
```


---------------------
## 34- Instalamos ngix para cambiar el puerto

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


---------------------
## 35- Instalando librerías de Python complementarias

```linux
cd /opt/odoo/server/extra-addons
apt install libxml2 python-openpyxl python-libxml2 ghostscript libssl-dev -y
apt install python-m2crypto
pip install mammoth xmltodict Crypto elaphe cchardet suds urllib3 SOAPpy xlwt xlsxwriter pybase64 dicttoxml rsa dropbox==7.3.1 
pip install requests pysftp cryptography openpyxl pycrypto pyopenssl signxml==1.0.1
pip install python-telegram-bot
pip install telegram
sudo pip install mega.py
pip install Pillow==5.2.0
pip install qrcode==6.0
pip install pyotp==2.2.6
```


---------------------
## 36- Instalando el certificado digital ( https://certbot.eff.org/lets-encrypt/ubuntuxenial-nginx  )

```linux
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx

sudo certbot --nginx
sudo certbot --nginx certonly

Dentro de Odoo configuras los parámetros.
Configuración > Parámetros > Parámetros del sistema
```


---------------------
Hasta aquí la instalación…. los siguientes comandos son para configuración de programación y no son necesarios.

## K1 – revision de version 8

```linux
/etc/init.d/o +Tab
/etc/init.d/odoov8 restart
tail -f /var/log/odoo/odoo-server.log
```


---------------------
## K2 – Actualizar pass de Postgres

```linux
sudo -su postgres
psql
alter role odoo with password 'odoo';
```


---------------------
## K3 – Filtrar por base de datos en el fichero conf

```linux
dbfilter = db10_*
```


---------------------

## K4 – Configuracion de PyCharm

```linux
/home/marlon/odoo/odoo_10/odoo-bin
--config=/home/marlon/odoo/odoo_10/odoo.conf
/home/marlon/odoo/odoo_10
```
![Alt text](https://github.com/falconsoft3d/instalar-odoo-10/blob/master/img/Screenshot-from-2017-05-24-00-16-08.png?raw=true "Configuracion de PyCharm")

---------------------
## k5 – Actualizar pass de una carpeta

```linux
sudo chown marlon: -R odoo_10/
```


---------------------
## k6 – /home/marlon/odoo/odoo_10/odoo.conf

```linux
[options]
; This is the password that allows database operations:
; admin_passwd = admin
db_host = localhost
db_port = 5432
db_user = odoo
db_password = odoo
addons_path = /home/marlon/odoo/odoo_10/addons
```


---------------------
## k7 – Descargando Odoo

```linux
git clone https://github.com/odoo/odoo.git --branch 10.0 --single-branch odoo_10
```


---------------------
## k8 – Configuracion de Pycharm

```linux
/home/marlon/Documentos/odoo-apt/odoo-10.0/odoo-bin
--config=/home/marlon/Documentos/odoo-apt/odoo-10.0/debian/odoo.conf
```


---------------------
## k9 – Acceso SSH con un fichero ppk

```linux
sudo apt-get install putty
puttygen private.ppk -o private-key -O private-openssh
ssh -i private-key username@remote-server-ip
```


---------------------
## k10 – Permiso de carpeta

```linux
chown -R marlon odoo-10.0
```


---------------------

## k11 – Busqueda en el log, cuando necesitemos buscar en el Log

```linux
grep "243028" /var/log/odoo/odoo-server.lo*
grep "Documento no enviado" /var/log/odoo/odoo-server.log
grep "'STATUS', u'0" /var/log/odoo/odoo-server.log
grep "242893" /var/log/odoo/odoo-server.log
```


---------------------
