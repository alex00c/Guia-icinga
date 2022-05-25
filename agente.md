## Primero en el equipo principal ##

Accedemos al archivo  */etc/icinga2/conf.d/hosts.conf* añadiendo la siguiente linea

```bash
object Host "Nombre-cliente" {
    import "generic-host"
    address = "IP-Cliente"
}
```
![Ejemplo](/img/host.jpg)

Para monitorizar algo , en este ejemplo usaremos 'swap' para saber el espacio libre en el swap del equpo linux cliente  Accedemos al archivo  */etc/icinga2/conf.d/services.conf* añadiendo la siguiente linea

```bash
object Service "swap" {
  import "generic-service"
  host_name = "Icinga-cliente"
  check_command = "swap"

}
```

![Ejemplo](img/servises.jpg)

## En el navegador ya nos mostrará el otro equipo ##

![Ejemplo](/img/monitor.jpg)



Enlace a la [Guia Para monitorizar CPU , RAM y Disco duro](/agente.md)