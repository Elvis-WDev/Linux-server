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

Cambiar TMZ en UBUNTU local:
> Verificamos el TMZ actual:
```
timedatectl O sudo dpkg-reconfigure tzdata
```
> Buscamos el TMZ a cambiar con:
```
timedatectl list-timezones
```
> Cambiar el TMZ:
```
sudo timedatectl set-timezone <TMZ>
```
Cambiar TMZ en MySql local:

> Ingresamos al shell de MySql como ROOT:
```
mysql -u root -p
```
> Verificamos TMZ actual:
```
SELECT NOW();
```
> Nos dirigimos hacia la siguiente ruta y creamos el archivo data.conf:
```
nano /etc/mysql/conf.d/date.conf
```
> Dentro del archivo date.conf agregamos la siguiente linea:
```
default_time_zone='America/Guayaquil'
```
> Reiniciamos MySql service:
```
service mysql restart
```
Crear acceso directo de un archivo en la ruta actual:
```
sudo ln -s /etc/apache2/sites-available/<acrchivo>
```
Activar un archivo de sites-enabled:
```
sudo a2ensite <ARCHIVO DE SITES_AVAILABLE>
```
Desactivar un archivo de sites-enabled:
```
sudo a2dissite <ARCHIVO DE SITES_AVAILABLE>
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
> Ingresamos al shell de MySql como ROOT:
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

## :star: Agregar capa de seguridad a PhpMyAdmin:

Nos dirigimos hacia:
```
nano /etc/phpmyadmin/apache.conf
```
En el Alias cambiamos el nombre por defecto por cualquier otro:
```
Alias /<NAME> /usr/share/phpmyadmin
```

![Referencia:](https://res.cloudinary.com/app-test-elvis/image/upload/v1666407122/Github-readmes/phpmyadmin_rqi4re.png)

Permitir reescritura del .htaccess dentro del TAG directory:
```
AllowOverride All
```
Nos dirigimos hacia:
```
cd /usr/share/phpmyadmin/
```
Creamos un archivo .htaccess:
```
nano .htaccess
```
Agreamos las siguientes líneas dentro del archivo .htaccess:
```
AuthType Basic
AuthName "Restricted Files"
AuthUserFile /etc/phpmyadmin/.htpasswd
Require valid-user
```
Creamos las credenciales para esta capa de autenticación al ingresar a PhpMyAdmin:
```
sudo htpasswd -c /etc/phpmyadmin/.htpasswd <USER>
```

## :star: Permitir archivo .htaccess:
Activamos el modo rescritura:
```
sudo a2enmod rewrite
```
Ingresamos a la ruta de sites-availables y editamos el archivo a activar:
```
sudo nano /etc/apache2/sites-availables/<archivo>
```
Dentro del archivo agregamos las siguientes lineas al final:
```
<Directory /var/www/<dir>>
 	AllowOverride All 
</Directory>
```
> Reiniciados apache2

## :star: Instalación de SSL con certbot:
> Para apache2:
```
sudo apt install certbot python3-certbot-apache
```
> Para Nginx:
```
sudo apt install certbot python3-certbot-nginx
```
Iniciamos certbot:
> Aapche2:
```
sudo certbot --apache
```
> Nginx:
```
sudo certbot --nginx
```

Verificamos que certbot esté funcionando correctamente:
```
sudo systemctl status certbot.timer
```
Ejecutamos una simulación de renovación de SSL de certbot si no da errores todo OK:
```
sudo certbot renew --dry-run
```
## :star: Estructura de un archivo para http en sites-availables:
```
<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/<DIRECTORY>
        ServerName <DOMINIO>
        ServerAlias <SUBDOMINIO.DOMINIO>
	
	ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```
Para habilitar reescritura .htaccess:
```
<Directory /var/www/<DIRECTORY>>
 AllowOverride All
</Directory>
```
## :star: Enlazamiento de dominio:
Para poder enlazar un dominio primero nos dirigimos hacia la siguiente ruta:
> Para apache2
```
cd /etc/apache2/sites-available/
```
> Para Nginx
```
cd /etc/nginx/sites-available/
```
Aquí encontraremos los archivos a editar para el dominio correspondiente:
> Dentro del archivo debemos colocar las siguientes líneas:
```
ServerAdmin webmaster@localhost
DocumentRoot var/wwww/<DIRECTORY>
ServerName <DOMINIO.TLD>
ServerAlias <SUBDOMINIO.DOMINIO.TLD> OR <DOMINIO.TLD>
```

Nos diriimos a namecheap y enlazamos nuestro dominio hacia el servidor:

> A-record: Sirve para enlazar nuestro dominio con los valores host=@ y value=IP-server
> C-name: Sirve para habilitar un subdominio con los valores host=subdominio y value:dominio

![Referencia](https://res.cloudinary.com/app-test-elvis/image/upload/v1666403208/Github-readmes/namecheapExample_r2ybhw.png)

## :star: Instalación de NODE:
Nos aseguramos si tenemos instalados los paquetes necesarios:
```
curl --version
node --version
```
Si no tentemos instalado curl:
```
sudo apt install curl
```
Tomamos el paquete de instalación de la página oficial:
```
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```
Instalamos NODE:
```
sudo apt-get install -y nodejs
```
Comprobamos instalaciones correctas:
```
Node --version
npm --version
```














