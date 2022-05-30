# Monitoreo de sitio web 

Es parecido a pasos anteriores , con algunos cambios , hemos de ir al archico */etc/icinga2/conf.d/hosts.conf* y configurar lo siguiente:



```bash
object Host "Webinstituto" {
    address = "81.88.48.71"
    import "generic-host"
    vars.http_vhosts["iesdomingoperezminik.es"] = {
        http_vhost = "iesdomingoperezminik.es"
        http_ssl = true
      }
}
```

Podemos añadirle tambien algun servicio como este 

```bash
object Service "Certificate " {
  import "generic-service"
  host_name = "Webinstituto"
  check_command = "http"
 
}
```
![Ejemplo](/img/web2.jpg)

## Y nos deberia de aparecer en el panel


![Ejemplo](/img/web3.jpg)




Enlace a la [Guia de instalación](docs.md)


Enlace a la [Guia Para la monitorización de otros equipos](/agente.md)


Enlace a la [Guia Para añadir servicios de red a monitorizar](/servicios.md)