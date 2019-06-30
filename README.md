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
addons_path = /opt/odoo/server/addons,/opt/odoo/server/extra-addons/addons_3ros,/opt/odoo/server/extra-addons/addons_general,/opt/odoo/server/extra-addons/addons_chile
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
git clone git@bitbucket.org:marlonodoo/addons_chile.git
```

## 17 - Tools
```linux
sudo service odoo stop
su - odoo -s /bin/bash
python2 /opt/odoo/server/odoo-bin -c /etc/odoo/odoo.conf -d db12-chile-sii -u all --stop-after-init
sudo service odoo restart

tail -f /var/log/odoo/odoo-server.log
```










