# Aqui monitorizaremos HTTP y SSH #

Para ello vamos al archivo */etc/icinga2/conf.d/services.conf* 

Y añadimos las siguientes lineas al documento

**SSH**

```bash
apply Service "ssh" {
  import "generic-service"

  check_command = "ssh"

  assign where (host.address || host.address6) && host.vars.os == "Linux"
}  
```

**HTTP**

```bash
apply Service "http" {
  import "generic-service"

  check_command = "http"

  assign where (host.address || host.address6) && host.vars.os == "Linux"
}  
```
Se tendriá que ver de la siguiente manera

![Ejemplo](/img/RED.jpg)

# Si buscamos hacerlo en el cliente #

Para ello vamos al archivo */etc/icinga2/conf.d/hosts.conf* 

Y añadimos las siguientes lineas al documento

**SSH**

```bash
object Service "ssh" {
  import "generic-service"
  host_name = "Icinga-cliente"
  check_command = "ssh"

} 
```

**HTTP**

```bash
object Service "http" {
  import "generic-service"
  host_name = "Icinga-cliente"
  check_command = "http"

}
 
```

Se tendriá que ver de la siguiente manera

![Ejemplo](/img/redcliente.jpg)


Enlace a la [Guia Para la monitorización de otros equipos](/agente.md)

Enlace a la [Guia Para Monitorizar sitio web](/cpu.md)