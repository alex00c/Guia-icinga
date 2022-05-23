# Aqui veremos como configurar el agente para su monitorización #

Hemos de tener instalados icinga2 , icingaweb2 y nodejs para llevar a cabo la guia , en los pasos del host se puede visualizar como hacerlo

## Tenemos que configurar nuestro agente para poder conectarlo ##

´´´bash
icinga2 node wizard
Welcome to the Icinga 2 Setup Wizard!

We will guide you through all required configuration details.

Please specify if this is an agent/satellite setup ('n' installs a master setup) [Y/n]:
Starting the Agent/Satellite setup routine...

Please specify the common name (CN) [icinga2-agent1.localdomain]: icinga2-agent1.localdomain
Please specify the parent endpoint(s) (master or satellite) where this node should connect to:
Master/Satellite Common Name (CN from your master/satellite node): icinga2-master1.localdomain
Do you want to establish a connection to the parent node from this node? [Y/n]:
Please specify the master/satellite connection information:
Master/Satellite endpoint host (IP address or FQDN): 10.0.5.5
Master/Satellite endpoint port [5665]: 5665
Add more master/satellite endpoints? [y/N]:
Parent certificate information:

 Subject:     CN = icinga2-master1.localdomain
 Issuer:      CN = Icinga CA
 Valid From:  Sep  7 13:41:24 2017 GMT
 Valid Until: Sep  3 13:41:24 2032 GMT
 Fingerprint: AC 99 8B 2B 3D B0 01 00 E5 21 FA 05 2E EC D5 A9 EF 9E AA E3

Is this information correct? [y/N]: y
Please specify the request ticket generated on your Icinga 2 master (optional).
 (Hint: # icinga2 pki ticket --cn 'icinga2-agent1.localdomain'):
e3f6fccfb9c0f35ac89710b6251f30abb3cc1120
´´´
Si teneis el siguiente fallo :
´´´bash
critical/Application: Error: boost::filesystem::copy_file: Permission denied: "/etc/icinga2/features-available/api.conf", "/etc/icinga2/features-available/api.conf.orig"
´´´
Tendreis que conceder los siguientes permisos:

´´´bash
root@idp:/home/alumno# chown -R :nagios /etc/icinga2/conf.d/
root@idp:/home/alumno# chmod 775 /etc/icinga2/conf.d/
root@idp:/home/alumno# chmod 664 /etc/icinga2/conf.d/*
root@idp:/home/alumno# chown -R :nagios /etc/icinga2/features-available
root@idp:/home/alumno# chmod 775 /etc/icinga2/features-available
root@idp:/home/alumno# chmod 664 /etc/icinga2/features-available/*
root@idp:/home/alumno# systemctl restart  icinga2
root@idp:/home/alumno# systemctl status icinga2
´´´
** Ahora tendremos que ir a la configuracion de los nodos **

Enlace a la [Guia Para Configurar los Nodos](/nodos.md)