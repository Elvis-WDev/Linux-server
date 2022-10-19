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


## :star: Instalación de NODE:
