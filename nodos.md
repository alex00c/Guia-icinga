# Configuración de Nodos #

El metodo mas usado es configurar objetos de monitorización en el master y distribuirlos a los satelites o agentes
Es importante aseguarse de que los dos nodos tienen los endpoints y la zona en elñ archivo **/etc/icinga2/zones.conf**

Se debería de ver algo asi 

![Ejemplo](/img/masterzone.jpg)

Despues deberemos validar la configuracion y reiniciar icinga2 en los dos equipos

´´´bash
[root@icinga2-agent1.localdomain /]# icinga2 daemon -C
[root@icinga2-agent1.localdomain /]# systemctl restart icinga2

[root@icinga2-master1.localdomain /]# icinga2 daemon -C
[root@icinga2-master1.localdomain /]# systemctl restart icinga2
´´´
Cuando esten conectados podemos pasar a monitorear remotamente el agente