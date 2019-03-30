# Balanceo de carga

## Configuración de **__nginx__**

Para poder configurar el balanceador de carga con nginx, he utilizado una máquina
diferente a las dos máquinas que actuan como servidores. La he llamado
M3 (lb nginx), es decir load balancer usando nginx.

Para esta máquina no he instalado la pila LAMP, ya que no debe tener el servicio
de apache activo y ocupando el puerto 80. Además, la IP de esta máquina es:

+ 192.168.56.107/24

![Configuración de M3 (lb nginx)](1.png)

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

![Inicio del servicio nginx](2.png)

Tras ver que nginx está funcionando correctamente, modifico el archivo de configuración
para que balancee los servidores M1 y M2. Para ello se accede al archivo mediante:

```sh
vi /etc/nginx/conf.d/default.config
```

El archivo queda tal y como se muestra en la imagen, con las IPs de los servidores
en la parte upstream apaches:

![Archivo /etc/nginx/conf.d/default.conf](3.png)

Además, hay que cambiar otro archivo, el /etc/nginx/nginx.conf, para asegurarnos
que nginx no actúa como otro servidor más, sino como balanceador. Para ello,
se comenta la línea previa a la sección mail:

```sh
#include /etc/nginx/sites-enabled/*;
```

![Archivo /etc/nginx/nginx.conf](4.png)

Tras estos cambios, se puede probar si de verdad está funcionando como balanceador;
para ello, utilizo el comando curl con la IP del balanceador. La salida es la siguiente:

![curl](5.png)

Que demuestra que, en efecto, está actuando de balanceador.
