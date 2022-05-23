# Documentación
Icinga es un sistema de monitorización opensource, que controla cualquier recurso de la red, notifica al usuario los errores, genera datos de rendimiento para la presentación de informes e informa del estado de los recursos. Es escalable y extensible, Icinga puede controlar entornos complejos y grandes a través de lugares dispersos.
## Introducción 

Vamos a instalar y usar icinga en una maquina virtual para mostrar como hacerlo

## Descarga de Icinga

Para descargar icinga tenemos que añadir el siguiente repositorio

```bash
apt-get update
apt-get -y install apt-transport-https wget gnupg

wget -O - https://packages.icinga.com/icinga.key | apt-key add -

. /etc/os-release; if [ ! -z ${UBUNTU_CODENAME+x} ]; then DIST="${UBUNTU_CODENAME}"; else DIST="$(lsb_release -c| awk '{print $2}')"; fi; \
 echo "deb https://packages.icinga.com/ubuntu icinga-${DIST} main" > \
 /etc/apt/sources.list.d/${DIST}-icinga.list
 echo "deb-src https://packages.icinga.com/ubuntu icinga-${DIST} main" >> \
 /etc/apt/sources.list.d/${DIST}-icinga.list

apt-get update
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

## Para monitorizar otros equipos hemos de configurar un master  ##

```bash
apt-get install nodejs

icinga2 node wizard

Welcome to the Icinga 2 Setup Wizard!

We will guide you through all required configuration details.

Please specify if this is a satellite/agent setup ('n' installs a master setup) [Y/n]: n

Starting the Master setup routine...

Please specify the common name (CN) [icinga2-master1.localdomain]: icinga2-master1.localdomain
Reconfiguring Icinga...
Checking for existing certificates for common name 'icinga2-master1.localdomain'...
Certificates not yet generated. Running 'api setup' now.
Generating master configuration for Icinga 2.
Enabling feature api. Make sure to restart Icinga 2 for these changes to take effect.

Master zone name [master]:

Default global zones: global-templates director-global
Do you want to specify additional global zones? [y/N]: N

Please specify the API bind host/port (optional):
Bind Host []:
Bind Port []:

Do you want to disable the inclusion of the conf.d directory [Y/n]:
Disabling the inclusion of the conf.d directory...
Checking if the api-users.conf file exists...

Done.

Now restart your Icinga 2 daemon to finish the installation!

sudo systemctl restart icinga2
```
## Usaremos CSR Auto-Singing ##

Para ello hemos de poder generar un ticket asique nos dirigimos al siguiente archivo y escribimos :

```bash
[root@icinga2-master1.localdomain /]# nano /etc/icinga2/conf.d/api-users.conf

object ApiUser "client-pki-ticket" {
  password = "bea11beb7b810ea9ce6ea" //Esto lo cambiamos
  permissions = [ "actions/generate-ticket" ]
}

# systemctl restart icinga2

Generamos el ticket 

# curl -k -s -u client-pki-ticket:bea11beb7b810ea9ce6ea -H 'Accept: application/json' \
 -X POST 'https://localhost:5665/v1/actions/generate-ticket' -d '{ "cn": "icinga2-agent1.localdomain" }'
 También podemos usar 
# icinga2 pki ticket --cn icinga2-agent1.localdomain

```
Este lo guardamos para nuestro agente

Enlace a la [Guia Para el agente/cliente](/agente.md)