# Balanceo de carga

## Configuración de **_nginx_**

Para poder configurar el balanceador de carga con *nginx*, he utilizado una máquina
diferente a las dos máquinas que actuan como servidores. La he llamado
M3 (lb nginx), es decir load balancer usando *nginx*.

Para esta máquina no he instalado la pila LAMP, ya que no debe tener el servicio
de apache activo y ocupando el puerto 80. Además, la IP de esta máquina es:

+ 192.168.56.107/24

![Configuración de M3 (lb nginx)](img/1.png)

Una vez que tengo la configuración de la máquina para que se vea con las demás,
configuro el servicio de balanceo que va a ofrecer esta máquina. Para ello, lo
primero es instalar *nginx*:

```sh
sudo apt-get install nginx
```

Tras esto, compruebo que está funcionando:

```sh
systemctl start nginx
systemctl status nginx
```

![Inicio del servicio nginx](img/2.png)

Tras ver que *nginx* está funcionando correctamente, modifico el archivo de configuración
para que balancee los servidores M1 y M2. Para ello se accede al archivo mediante:

```sh
vi /etc/nginx/conf.d/default.config
```

El archivo queda tal y como se muestra en la imagen, con las IPs de los servidores
en la parte upstream apaches:

![Archivo /etc/nginx/conf.d/default.conf](img/3.png)

Además, hay que cambiar otro archivo, el /etc/nginx/nginx.conf, para asegurarnos
que *nginx* no actúa como otro servidor más, sino como balanceador. Para ello,
se comenta la línea previa a la sección mail:

```sh
#include /etc/nginx/sites-enabled/*;
```

![Archivo /etc/nginx/nginx.conf](img/4.png)

Tras estos cambios, se puede probar si de verdad está funcionando como balanceador;
para ello, utilizo el comando curl con la IP del balanceador. La salida es la siguiente:

![curl](img/5.png)

Que demuestra que, en efecto, está actuando de balanceador.

## Configuración de **_haproxy_**

De manera análoga a como lo he hecho con *nginx*, lo haré con *haproxy*. Comienzo con
una instalación, en una cuarta máquina (con el nombre de M3 (lb haproxy)), sin
apache, con la misma IP que la máquina con *nginx*:

+ 192.168.56.107/24

![Configuración de M3 (lb haproxy)](img/6.png)

Tras esto, instalo el servicio de *haproxy*, igual que con *nginx*.

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

Al igual que con *nginx*, comprobamos ahora que balancea la carga entre los servidores
con el comando curl:

![curl](img/11.png)

Y como es evidente, funciona.

## Someter a alta carga al servidor balanceado con **_ab_**

En este apartado, utilizaré **_ab_** (**_apache benchmark_**) para sobrecargar
el servidor balanceado.

### Nginx como servidor balanceado con round-robin

Para testar como reacciona *nginx* ante una gran carga, llevaré a cabo dos
experimentos. Para comprobar el estado de cada máquina, usaré *htop*, un monitor
del sistema por línea de comandos pero que ofrece una interfaz con colores, donde
muestra la carga de la CPU, la memoria y el area de intercambio principalmente. En
este primer caso, lo haré con el algoritmo de balanceo de __round-robin__.

Mediante el comando

```sh
ab -n 100000 -c 10 192.168.56.107
```

envío 100000 peticiones con una concurrencia de 10 a la máquina dada por la IP.
La carga de cada máquina es la que sigue (M3, M1 y M2 respectivamente):

![M3 htop](img/12.png)
![M1 htop](img/13.png)
![M2 htop](img/14.png)

Como se puede comprobar, la CPU del balanceador está casi al 100%, mientras que
en las máquinas M1 y M2, la carga está repartida equitativamente, por lo que el
uso de la CPU es similar entre ambas máquinas.

### Nginx como servidor balanceado con ponderación

En este segundo caso, probaré el comportamiento de *nginx* cuando se le da más
peso a la M1 que a M2 (pesos 2 y 1 respectivamente, es decir, M1 recibirá el
doble de carga porque la creemos el doble de potente).

![Configuración con pesos](img/18.png)

Las imágenes que muestran el funcionamiento son las que sigue (M3, M1 y M2):

![M3 htop](img/19.png)
![M1 htop](img/20.png)
![M2 htop](img/21.png)

Con este algoritmo, el balanceador sigue estando al 100% de uso de CPU, pero
el uso de CPU en las máquinas varía: M1 tiene el doble de carga que M2 (que tiene
un 24% de carga de CPU solamente).

### Haproxy como servidor balanceado con round-robin

Al igual que acabo de hacer con *nginx*, lo haré con *haproxy*; utilizaré las
mismas herramientas, tanto **_ab_** como *htop*. El algoritmo usado es __round-robin__.

De nuevo, con el mismo comando que previamente he mencionado, lanzaré 100000
peticiones al balanceador, con una concurrencia de 10 (es decir, de 10 en 10).
El resultado de cada máquina (M3, M1 y M2, en este orden) es:

![M3 htop](img/15.png)
![M1 htop](img/16.png)
![M2 htop](img/17.png)

Al igual que antes, el balanceador tiene la CPU al 100%, mientras que las máquinas
servidoras tienen las peticines repartidas, de forma que ambas tiene la CPU
más o menos a la mitad de su capacidad.

### Haproxy como servidor balanceado con ponderación

Ahora testaré *haproxy* con **_ab_**, mostrando los resultados mediante *htop* y
ponderando los pesos de las máquinas, dando el doble de carga a M1 que a M2. En
la imagen se muestra dicha configuración.

![Configuración con pesos](img/22.png)

En las imágenes se muestra el uso de CPU de M3, M1 y M2, correspondientemente:

![M3 htop](img/23.png)
![M1 htop](img/24.png)
![M2 htop](img/25.png)

Como se puede observar, el reparto de carga es el doble en la M1 que en M2, pero
esto no afecta al balanceador, que sigue teniendo el uso de su CPU al 100%.

## Someter a alta carga al servidor balanceado con **_jMeter_**

En este caso, y como trabajo opcional para la práctica, utilizaré el benchmark
**_jMeter_** . He intentado utilizar *seesaw* y *gobetween*, pero he tenido muchos
problemas, de instalación en el primer caso y de ejecución en el segundo. Por
tanto, al final me he decidido a usar **_jMeter_**, ya que conozco su funcionamiento
gracias a la asignatura Ingeniería de Servidores. En el repositorio está incluido
el test de jMeter que he utilizado para generar la carga.

### Nginx como servidor balanceado

Utilizando un test que he creado para **_jMeter_**, y con el algoritmo de __round-robin__,
los resultados dados son los que siguen, para las máquinas M3, M1 y M2, usando *nginx*:

![M3 htop](img/26.png)
![M1 htop](img/27.png)
![M2 htop](img/28.png)

Como no ha cambiado el funcionamiento del balanceador ni de los servidores, los
resultados son similares con los demás tests usando *nginx*.

### Haproxy como servidor balanceado

Para este último caso, utilizaré *haproxy* con el algoritmo de __round-robin__ y
el test de **_jMeter_**. Los resultados de M3, M1 y M3 son estos:

![M3 htop](img/29.png)
![M1 htop](img/30.png)
![M2 htop](img/31.png)

Al igual que en todos los casos anteriores, los resultados son los esperados,
con el balanceador al 100% de su capacidad y las máquinas utilizadas como servidoras
a la mitad de uso de CPU aproximadamente.
