# Balanceo de carga

## Configuración de **__nginx__**

Para poder configurar el balanceador de carga con nginx, he utilizado una máquina
diferente a las dos máquinas que actuan como servidores. La he llamado
M3 (lb nginx), es decir load balancer usando nginx.

Para esta máquina no he instalado la pila LAMP, ya que no debe tener el servicio
de apache activo y ocupando el puerto 80. Además, la IP de esta máquina es:

+ 192.168.56.107/24

![Configuración de M3 (lb nginx)](img/1.png)

Una vez que tengo la configuración de la máquina para que se vea con las demás,
configuro el servicio de balanceo que va a ofrecer esta máquina. Para ello, lo
primero es instalar nginx:

```sh
sudo apt-get install nginx
```

Tras esto, compruebo que está funcionando:

```sh
systemctl start nginx
systemctl status nginx
```

![Inicio del servicio nginx](img/2.png)

Tras ver que nginx está funcionando correctamente, modifico el archivo de configuración
para que balancee los servidores M1 y M2. Para ello se accede al archivo mediante:

```sh
vi /etc/nginx/conf.d/default.config
```

El archivo queda tal y como se muestra en la imagen, con las IPs de los servidores
en la parte upstream apaches:

![Archivo /etc/nginx/conf.d/default.conf](img/3.png)

Además, hay que cambiar otro archivo, el /etc/nginx/nginx.conf, para asegurarnos
que nginx no actúa como otro servidor más, sino como balanceador. Para ello,
se comenta la línea previa a la sección mail:

```sh
#include /etc/nginx/sites-enabled/*;
```

![Archivo /etc/nginx/nginx.conf](img/4.png)

Tras estos cambios, se puede probar si de verdad está funcionando como balanceador;
para ello, utilizo el comando curl con la IP del balanceador. La salida es la siguiente:

![curl](img/5.png)

Que demuestra que, en efecto, está actuando de balanceador.

## Configuración de **__haproxy__**

De manera análoga a como lo he hecho con nginx, lo haré con haproxy. Comienzo con
una instalación, en una cuarta máquina (con el nombre de M3 (lb haproxy)), sin
apache, con la misma IP que la máquina con nginx:

+ 192.168.56.107/24

![Configuración de M3 (lb haproxy)](img/6.png)

Tras esto, instalo el servicio de haproxy, igual que con nginx.

```sh
sudo apt-get install haproxy
```

Cuando se ha acabado de instalar, hay que cambiar (o crear) el archivo de configuración
para que balancee la carga. Para ello, se accede al archivo /etc/haproxy/haproxy.cfg
que queda de la siguiente forma:

![Archivo /etc/haproxy/haproxy.cfg](img/7.png)

Tras esto, lanzamos el balanceador con el comando:

```sh
sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg
```

No obstante, al ejecutar este comando con el fichero tal y como lo he escrito, hay
tres errores, como se muestran en la imagen:

![Errores del archivo](img/8.png)

Los errores se basan en un error de directivas obsoletas; de modo que cambiando
los errores que se muestran y lanzando de nuevo el servicio, todo funciona correctamente
y sin errores:

![Archivo /etc/haproxy/haproxy.cfg modificado](img/9.png)
![Inicio del balanceador](img/10.png)

Al igual que con nginx, comprobamos ahora que balancea la carga entre los servidores
con el comando curl:

![curl](img/11.png)
