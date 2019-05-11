# Replición de bases de datos MySQL

## Creación de la base de datos e inserción de datos

El primer paso es crear la base de datos que voy a utilizar, en este caso llamada
*contactos*; la base de datos deberá ser creada a mano en ambos servidores ya
que en el siguiente paso, usando *mysqldump*, la base datos no será creada como tal,
debe estar creada de antemano. En la siguiente imagen se muestra como se ha creado
en la máquina principal (M1) y la de backup (que será el esclavo, M2).

![Creación de la base de datos](img/2.png)

Tras esto, en la máquina principal se crea la tabla y se insertan datos, tal y
como muestra la siguiente captura:

![Creación de la tabla en M1](img/3.png)

Una vez insertados los datos, hay que bloquear las tablas para copiarlas, de forma
que no haya nuevas escrituras mientras se produzca el traspaso de datos y se produzcan
discrepancias entre ambas tablas.

![Bloqueo de tablas](img/4.png)

## Replicar una BD MySQL con *mysqldump*

Una vez hecho esto, se genera el archivo que voy a copiar en la otra máquina mediante
*mysqldump*:

```sh
mysqldump contactos -u root -p > /tmp/contactos.sql
```

Una vez creado, ya se pueden desbloquear las tablas que previamente se habían bloqueado
para evitar nuevas escrituras:


![Desbloqueo de tablas](img/6.png)

Ahora, en la máquina de backup, se copia el archivo mediante *scp* tal y como 
muestra la imagen que sigue:

![Copia del archivo](img/7.png)

Tras la copia, se ejecutan las sentencias sql del archivo en la base de datos de
la máquina de backup. Para eso utilizo el comando que se muestra en el imagen, que
abre mysql y vuelca el contenido del archivo en la base de datos especificada, en
este caso, contactos:

![Volcado del archivo](img/8.png)

Una vez hecho esto, se comprueba si ambas tablas son iguales, y en efecto, lo son.

## Replicar una BD MySQL a mano

Aunque no es la mejor opción a la hora de replicar la información en una granja web
real, también se puede llevar a cabo la replicación a mano, es decir, escribiendo
las mismas sentencias en ambas máquinas de forma paralela.

![Replicación manual](img/11.png)

Esta opción presenta muchas desventajas, como por ejemplo fallos, olvidos, mayor
inversión de esfuerzo y dinero (ya que debe haber un operario que se dedique 
exclusivamente a esto) e inviabilidad cuando las consultas crecen a una velocidad 
muy grande.

## Configuración maestro-esclavo

Para llevar a cabo esta configuración, lo primero que se debe hacer es modificar
los archivos de configuración del maestro y del esclavo, de manera que se pueda
establecer correctamente la sincronización entre ambas máquinas.

Para ello, hay que comentar la dirección en la que están escuchando los servicios
de mysql en ambas máquinas, que por defecto es localhost (recuadro rojo):

![Bind address](img/13.png)

Una vez comentado este parámetro, hay que indicar los archivos donde se llevará
el registro de errores y el de log; además hay que establecer un id para cada
uno (las líneas modificadas están recuadradas en rojo):

![Archivo /etc/mysql/mysql.d.conf/mysqld.cnf](img/14.png)

Tras modificar los archivos, reinicio los servicios y compruebo si hay errores.
Si al reiniciar todo está correcto, puedo comenzar a configurar el maestro y el
esclavo.

![Reinicio de /etc/init.d/mysql](img/15.png)

Una vez reiniciado, dentro del maestro, introducimos los siguientes comandos:

+ Creación de un usuario *esclavo*:
~~~~sql
CREATE USER esclavo IDENTIFIED BY 'esclavo';
~~~~

+ Dar permiso al esclavo de replicación:
~~~~sql
GRANT REPLICATION SLAVE ON \*.\* TO 'esclavo'\@'%' IDENTIFIED BY 'esclavo';
~~~~

+ Eliminar privilegios y eliminar la posibilidad de escritura en tablas mientras
se crea el maestro:

~~~~sql
FLUSH PRIVILEGES;
FLUSH TABLES;
FLUSH TABLES WITH READ LOCK;
~~~~

Una vez hecho esto, se comprueba el estado del maestro:

~~~~slq
SHOW MASTER STATUS;
~~~~








