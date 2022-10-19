# Linux-server
## :star: How to create ubuntu server
Actualizamos repos:
```
sudo apt-get update
sudo apt-get upgrade
```

Creamos usuario nuevo:
```
sudo adduser <USER>
```
Damos provilegios al usuario:
```
sudo usermod -aG sudo <USER>
```
Ingresamos con el usuario creado:
```
sudo su - <USER>
```
Comprobamos que mi usuario tiene los privilegios listando repos root:
```
sudo ls -la /root
```
## :star: Comandos complementarios:
Cambiar permisos de directorios:
```
chmod XXX <FILE>
```
Cambiar usuario editor:
```
chown www-data:www-data <FILE>
```
Instalación de mLocate para encontrar archivos rápido:
```
sudo apt install mlocate
```
Buscamos archivo con mLocate:
```
locate <FILE>
```
## :star: Preparamos servicios HTTP:
Instalamos APACHE o NGINX:
```
sudo apt install apache2
<O>
sudo apt install nginx
```
Comprobamos el servicio:
```
sudo systemctl status apache2
<O>
sudo systemctl status nginx
```
Iniciamos servicio:
```
sudo systemctl start apache2
<O>
sudo systemctl start nginx
```
Detenemos servicio:
```
sudo systemctl stop apache2
<O>
sudo systemctl stop nginx
```
## :star: Preparamos Firewall:
Comprobamos status:
```
sudo ufw status
```
Habilitamos Firewall en caso de estar inactivo:
```
sudo ufw enable
```
Verificamos Firewall list:
```
sudo ufw app list
```
Permitimos tráfico HTTP (3 Opciones):
```
sudo ufw allow 'Apache'
sudo ufw allow HTTP
sudo ufw allow 80
```

Permitimos tráfico HTTPS (3 Opciones):
```
sudo ufw allow 'Apache Full'
sudo ufw allow HTTPS
sudo ufw allow 443
```
Permitimos tráfico SSH (2 Opciones):
```
sudo ufw allow ssh
sudo ufw allow 22
```

## :star: Preparamos configuraciones IP-TABLES del KERNELL:
Lista de comando IP-TABLES permitidos:
```
sudo iptables -S
```
Permitimos que acepte el tráfico HTTP a nuestro Kernell IP-TABLES:
```
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT
```
Permitimos que acepte el tráfico HTTPS a nuestro Kernell IP-TABLES:
```
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 443 -j ACCEPT
```

## :star: Instalación de PHP:
Instalamos repos necesarios:
```
sudo apt install ca-certificates apt-transport-https software-properties-common
```
Agregamos el repo ondrej:
```
sudo add-apt-repository ppa:ondrej/php
```
Instalamos PHP(8.1) con sus librerias:
```
sudo apt install php8.1 libapache2-mod-php8.1
```
(OPCIONAL) Instalamos librerias necesarias para phpMyAdmin y Mysql:
```
sudo apt install php8.1-mbstring
sudo apt install php8.1-mysql
```

## :star: Instalación de MYSQL:
Instalamos MySql:
```
sudo apt install mysql-server
```
Comprobamos estado del servicio:
```
systemctl status mysql.service
```
Iniciamos el servicio:
```
systemctl start mysql.service
```
Detenemos el servicio:
```
systemctl stop mysql.service
```
Configuramos MYSQL:
```
sudo mysql_secure_installation
```
> Nos pedirá ingresar una contraseña para MySql.

Ingresamos al shell de MySql en modo ROOT:
```
sudo mysql
```
Comprobamos estado del los usuarios MySql:
```
SELECT user,authentication_string,plugin,host FROM mysql.user;
```
Cambiamos el complemento de auth_socket por NATIVE PASSWORD:
```
ALTER USER '<USER>'@'<HOST>' IDENTIFIED WITH mysql_native_password BY <PASSWORD>;
```
Le decimos a MySql que aplique los cambios:
```
FLUSH PRIVILEGES;
```
> Listamos la tabla nuevamente para comprobar el cambio.

Creación de usuario MySql para conexiones a base de datos:
> Ingresamos en modo Root
```
mysql -u root -p
```
Creamos el usuario con:
```
CREATE USER '<NEWUSER>'@'<HOST>' IDENTIFIED BY '<PASSWORD>';
```
Otorgamos todos lo privilegios de ser el caso:
```
GRANT ALL PRIVILEGES ON * . * TO '<NEWUSER>'@'<HOST>';
```
Salimos del shell de MySql con:
```
quit
```
## :star: Instalación de PhpMyAdmin:
Instalamos con:
```
sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl
```
> Seleccionamos opcion "Apache2" y luego "Yes"
> Si salta un error le damos en "abort"

Para solucionar el error de instalación de PhpMyAdmin debemos entrar al shell de MySql y eliminar el componente de autenticación por un momento:
>Ingresamos al shell de MySql como ROOT:
```
mysql -u root -p
```
> Desinstalamos el componente de autenticación:
```
UNINSTALL COMPONENT "file://component_validate_password";
```
> Salimos del shell de MySql.
> Volvemos a instalar PhpMyAdmin.

> Despues de haber instalamos debemos volver a instalar el componente de autenticación con:
```
INSTALL COMPONENT "file://component_validate_password";
```
Nos dirigimos hacia:
```
sudo nano /etc/apache2/apache2.conf
```
Agregamos la siguiente linea al final del archivo apache2.conf
```
Include /etc/phpmyadmin/apache.conf
```
Reiniciamos los servicios para que se apliquen los cambios:
```
sudo systemctl restart apache2
```







## :star: Instalación de NODE:
