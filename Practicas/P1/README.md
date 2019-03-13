# Configuración de las máquinas M1 y M2

## Instalación

En estas prácticas utilizaré el software de virtualización VirtualBox, ya que
estoy acostumbrada de otras asignaturas en las que la he usado y estoy familiarizada
con su funcionamiento.

Comenzaré con una instalación por defecto de Ubuntu Server 16.04 LTS, con 512MB
de memoria RAM y 8GB de almacenamiento cada máquina. A estas máquinas las llamaré
M1 y M2.

Una vez instaladas, y con ayuda del instalador de la imagen de este S.O., se instalarán
el servicio SSH y la pila LAMP. Las instalaciones finalizadas y con las máquinas en
funcionamiento se muestran en las imágenes siguientes.

![M1 con un fresh install](1.png)
![M2 con un fresh install](2.png)

Por el funcionamiento de VirtualBox (no es la primera vez que me pasa), sé que
en el momento de la instalación, ambas máquinas tendrán la misma dirección IP, por
lo que estableceré manualmente y de forma estática la IP de la máquina M1 de
10.0.2.15 a 10.0.2.16 (de forma similar a como hice en las prácticas de
la asignatura Ingeniería de Servidores).

## Adaptador e interfaz de red

Para conectar ambas máquinas y que se vean entre ellas, y también con el anfitrión,
es necesario crear una nueva interfaz de red, y para ello, es necesario crear en
VirtualBox un nuevo adaptador de red, entrando en Archivo->Administrador de red
de anfitrión. Se abrirá una ventana como la de la primera imagen, donde podremos
añadir el nuevo adaptador virtual; tras esto, en la configuración de cada máquina
se crea una interfaz solo-anfitrión para que la máquina se conecte con el nuevo
adaptador de red (en la segunda imagen se muestra con la M1; se lleva a cabo el
mismo procedimiento en la M2).

![Adaptador de red](5.png)
![Interfaz host-only](6.png)

Una vez añadido el adaptador de red, pasamos a cambiar la IP de una de las máquinas
para que ambas no tengan la misma y a añadir la nueva interfaz de red en cada máquina.
Para ello, accedemos al archivo /etc/network/interfaces, donde, como se puede observar
se modifica la interfaz de red enp0s3 en la M1: "inet dhcp" pasa a ser "inet static"
de forma que no se active el protocolo dhcp cada vez que iniciamos la máquina y
se actualice automáticamente la IP nueva que he establecido. De esta manera, la
IP que establezco pasa a ser 10.0.2.16/24 (la primera imagen).

Además, en ambos archivos se puede ver que hay una nueva interfaz de red, la
enp0s8. Esta interfaz es la que se ha utilizado para conectar las máquinas entre
sí y con el host. Como se ha observado en la imagen anterior del adaptador de red
configurado, da soporte al conjunto de direcciones 192.168.56.0/24, así que,
siguiendo la configuración que hice en las máquinas de la asignatura Ingeniería
de Servidores, he utilizado:

+ 192.168.56.105, para M1.
+ 192.168.56.106, para M2.

![Archivo /etc/network/interfaces de M1](3.png)
![Archivo /etc/network/interfaces de M2](4.png)

Por tanto, la configuración final de red mostrada por el comando ifconfig de cada
máquina es:

![ifconfig de M1](7.png)
![ifconfig de M2](8.png)

## Conexión entre M1 y M2

Una vez establecidas, antes de poder conectarlas entre sí, levantamos la conexión
en cada máquina o reiniciamos. Yo he levantado la red con el comando ifup enp0s8
tanto en M1 como en M2. Una vez hecho esto, se comprueba si ambas máquinas se ven
mediante un ping (de M1 a M2 y de M2 a M1 respectivamente).

![ping M1 a M2](9.png)
![ping M2 a M1](10.png)

## Pila LAMP: funcionamiento

A partir de aquí, mostraré todo lo que haga solo en la M1, de forma análoga a como
lo haría en la M2. Pero para simplificar el documento y facilitar un poco las cosas,
solo utilizaré capturas de M1.

Durante la instalación hemos instalado una pila LAMP, de la cual vamos a comprobar
si el servidor de apache (A) y el intérprete php (P) están en funcionamiento. El
gestor MySQL (M) no será mostrado aquí porque no tomé captura durante la instalación,
no obstante, se pedía una contraseña para el administrador del gestor.

Para comprobar si apache está instalado, y en qué estado está (inactivo, corriendo...)
podemos usar el comando apache2 -v, o bien, systemctl status apache, ambos métodos
mostrados en la imagen siguiente. Está claro que apache está, en efecto, instalado
y en funcionamiento.

![Estado de apache2](11.png)

Para comprobar que php se ha instalado correctamente solo tenemos que entrar
en el modo interactivo del intérprete (opción -a).

![Estado de php](12.png)

## Servicio SSH: funcionamiento

Ahora comprobaré el estado de SSH, intentando conectarme a la máquina M2 a través
de M1. Para ello, accedemos mediante el comando:

+ ssh 192.168.56.106 -l nazaretrogue

Este comando nos permite acceder mediante ssh a la dirección IP dada con el usuario
dado con la opción -l.

Lo primero que ocurre es que ssh pide crear una clave simétrica para poder conectarse.
Una vez creada esta clave, la conexión es segura y se puede comenzar a establecer
la conexión.

![Clave SSH](13.png)

Tras esto, pide la contraseña del usuario con la que he entrado, es decir, el
login dado con la opción anteriormente mencionada.

![Contraseña del user](14.png)

Una vez introducida, entramos a la máquina de la cual hemos dado la IP en el
comando. Podemos comprobar que estamos en dicha máquina porque el prompt, en el
que antes ponía ub1 (en la M1), ahora ha cambiado a ub2 (en la M2). Además, compruebo
también la IP mediante ifconfig, para demostrar que en efecto, estamos en la máquina
con IP 10.0.2.15, es decir, M2.

![Prompt ub2](15.png)
![ifconfig con SSH](16.png)

## Servidor en funcionamiento desde el host

Por último, creo un archivo, "hola.html" en /var/www/html de la M1 y accedo a él
a través de la IP (desde el anfitrión, de forma que M1 actúa como servidor web).
El archivo y el resultado del acceso están reflejados en las imágenes que siguen:

![Archivo /var/www/html/hola.html](17.png)
![Acceso al servidor M1](18.png)
