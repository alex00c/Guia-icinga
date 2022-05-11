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
GRANT SELECT, INSERT, UPDATE, DELETE, DROP, CREATE VIEW, INDEX, EXECUTE ON icinga.* TO 'icinga'@'localhost' IDENTIFIED BY 'icinga';
quit

```
**Habilitamos la funcion IDO**
```bash
icinga2 feature enable ido-mysql

Module 'ido-mysql' was enabled.
Make sure to restart Icinga 2 for these changes to take effect.

```
Reiniciamos el servicio
```bash
systemctl restart icinga2

```