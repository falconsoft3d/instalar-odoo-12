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




