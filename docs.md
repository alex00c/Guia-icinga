# Documentación


![icinga](/img/icinga.png)


**Icinga es un sistema de monitorización opensource, que controla cualquier recurso de la red, notifica al usuario los errores, genera datos de rendimiento para la presentación de informes e informa del estado de los recursos. Es escalable y extensible, Icinga puede controlar entornos complejos y grandes a través de lugares dispersos.**


# Introducción 

**Vamos a instalar y usar icinga en una maquina virtual para mostrar como hacerlo**

## Descarga de Icinga

Para descargar icinga tenemos que añadir el siguiente repositorio

```bash
sudo apt-get update
```
```bash
sudo apt-get -y install apt-transport-https wget gnupg
```
```bash
wget -O - https://packages.icinga.com/icinga.key | apt-key add -

. /etc/os-release; if [ ! -z ${UBUNTU_CODENAME+x} ]; then DIST="${UBUNTU_CODENAME}"; else DIST="$(lsb_release -c| awk '{print $2}')"; fi; \
 echo "deb https://packages.icinga.com/ubuntu icinga-${DIST} main" > \
 /etc/apt/sources.list.d/${DIST}-icinga.list
 echo "deb-src https://packages.icinga.com/ubuntu icinga-${DIST} main" >> \
 /etc/apt/sources.list.d/${DIST}-icinga.list
```
```bash
sudo apt-get update
```
```bash
apt-get install icinga2
```
Podemos realizar un checkeo de la instalación mediante el comando de validación
```bash
icinga2 daemon -C
```
Deberia arrojarnos algo asi


![Ejemplo](/img/confirmacion.jpg)

## Instalamos los plugins
Sin ellos icinga no podria monitorizar servicios externos
```bash
apt-get install monitoring-plugins
```
## Configuración y descarga de la base de datos
Icinga Web 2 se conecta a la base de datos IDO para visualizar los datos correctamente.
Se recomienda instalar y configurar la función IDO antes de continuar con la instalación de Icinga Web 2.

**Instalar MySQL Server**

```bash
apt-get install mariadb-server mariadb-client

mysql_secure_installation

```
**Instalar la función IDO**
```bash
apt-get install icinga2-ido-mysql
```
Aqui nos pedirá dos pasos de configuracion que son los siguientes


![Ejemplo](/img/paso1ido.jpg)

![Ejemplo](/img/paso2ido.jpg)

Para acabar nos piden la contraseña que queramos poner

**Configurar la base de datos MySQL**
```bash
# mysql -u root -p

CREATE DATABASE icinga;
GRANT ALL ON icinga.* TO 'icinga'@'localhost' IDENTIFIED BY 'icinga';
FLUSH PRIVILEGES;
quit

```
**Habilitamos la funcion IDO**
```bash
icinga2 feature enable ido-mysql

Module 'ido-mysql' was enabled.
Make sure to restart Icinga 2 for these changes to take effect.

```
Nuestro archivo de configuración debe ser algo asi

![Ejemplo](/img/nano.jpg)

**Reiniciamos el servicio**

```bash
systemctl restart icinga2

```

## Instalamos los siguiente paquetes que son necesarios para abrir icingaweb2 en nuestro navegador ##

**Apache2** 

```bash
sudo apt install apache2 -y
```
```bash
sudo systemctl start apache2
```
```bash
sudo systemctl enable apache2
```
```bash
sudo apt install php php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip php-cli php-mysql php8.0-common php8.0-opcache php-gmp php-imagick -y
```
Tenemos que modificar el siguiente archivo

```bash
sudo nano /etc/php/8.0/apache2/php.ini
```
Cambiando el siguiente parámetro :

```bash
cgi.fix_pathinfo=0
```


**PHP**

```bash
sudo apt install software-properties-common

sudo add-apt-repository ppa:ondrej/php

```

**Instalar Icingaweb2**

```bash
sudo apt install icingaweb2 icingacli libapache2-mod-php -y
```
```bash
sudo mysql -u root -p

CREATE DATABASE icinga2web;
GRANT ALL ON icinga2web.* TO 'icinga2web'@'localhost' IDENTIFIED BY 'contraseña';
FLUSH PRIVILEGES;
EXIT
```
```bash
sudo icingacli setup token create
```
El token lo copiaremos en algún documento para usarlo posteriormente

## Configuración del Icinga Web 2 en el navegador ##

Abrimos nuestro navegador y en el buscador introducimos lo siguiente

```bash
http://tu_direccion_IP/icingaweb2/setup
```


**Y seguimos los siguientes pasos pulsando siguiente en las paginas que no se muestren :**


![Ejemplo](/img/icingaweb.jpg)



![Ejemplo](/img/icingawebdatos.jpg)



![Ejemplo](/img/datosusuariosicinga.jpg)




![Ejemplo](/img/banked%20name.jpg)



![Ejemplo](/img/admin.jpg)


![Ejemplo](/img/configuraricinga.jpg)


![Ejemplo](/img/commandicingaweb.jpg)



![Ejemplo](/img/securityicinga.jpg)


![Ejemplo](/img/finalicingaweb.jpg)



![Ejemplo](/img/conexionwindows.jpg)

![Ejemplo](/img/servicios.jpg)

**Y ya habriamos terminado y podremos monitorizar todo nuestro equipo**


Enlace a la [Guia Para la monitorización de otros equipos](/agente.md)


Enlace a la [Guia Para añadir servicios de red a monitorizar](/servicios.md)

Enlace a la [Guia Para Monitorizar sitio web](/cpu.md)