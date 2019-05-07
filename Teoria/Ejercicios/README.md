# Tema 1

## Ejercicio 1
Buscar información sobre las tareas o servicios web para los que se usan más los programas
que comentamos al principio.

+ **Apache**: este servicio es usado principalmente para servir páginas web (ya
    sean estáticas o dinámicas).
+ **nginx**: inicialmente fue creado para servir páginas web estáticas para sitios
    web con mucho tráfico, ya que ocupa mucha menos memoria que *apache*. No obstante
    también se utiliza como un potente balanceador de carga y como un proxy para
    protocolos de correo eléctrónico (como *IMAP*/*POP3*).
+ **thttpd**: es un servidor web ligero, rápido y seguro que se utiliza para servir
    grandes volúmenes de información estática, al igual que *nginx*. Se usa con la
    intención de aumentar la velocidad de transferencia de achivos y reducir el
    uso de recursos.
+ **Cherokee**: servidor web rápido y ligero, utilizado para servir peticiones web;
    al igual que *nginx*, también se utiliza como balanceador de carga. Puede ser
    utilizado en sistemas embebidos o en grandes CPDs gracias a que es multiplataforma
    y soporta una gran cantidad de complementos para aumentar y mejorar su funcionalidad.
+ **Node.js**: es un entorno en tiempo de ejecución creado para poder crear programas
    muy escalables (como servidores web) aunque no se limita solo a eso. Aporta
    flexibilidad y es muy rápido.

# Tema 2

## Ejercicio 1
Calcular la disponibilidad del sistema si tenemos dos réplicas de cada elemento (en total
3 elementos en cada subsistema).

Componente | A<sub>n</sub> | A<sub>n-1</sub> | Resultado
-----------|---------------|-----------------|----------
Web | 0.9775 | 0.85 | 0.99662
Aplicación | 0.99 | 0.9 | 0.999
Base de datos | 0.999 | 0.9999 | 1
DNS | 0.9996 | 0.98 | 0.99999
Cortafuegos | 0.9775 | 0.85 | 0.99662
Switch | 0.9999 | 0.99 | 1
Data Center | 0.9999 | 0.9999 | 1
ISP | 0.9975 | 0.95 | 0.99987
 | | Total | 0.99214

## Ejercicio 2
Buscar frameworks y librerías para diferentes lenguajes que permitan hacer aplicaciones
altamente disponibles con relativa facilidad.

## Ejercicio 3
¿Cómo analizar el nivel de carga de cada uno de los subsistemas en el servidor? Buscar
herramientas y aprender a usarlas.

## Ejercicio 4
Buscar ejemplos de balanceadores software y hardware (productos comerciales). Buscar
productos comerciales para servidores de aplicaciones. Buscar productos comerciales
para servidores de almacenamiento.

# Tema 3

## Ejercicio 1
Buscar con qué órdenes de terminal o herramientas podemos configurar bajo Windows y bajo
Linux el enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra.

## Ejercicio 2
Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows
y bajo Linux el filtrado y bloqueo de paquetes.

# Tema 4

## Ejercicio 1
Buscar informaión sobre cuánto costaría en la actualidad un mainframe que tuviera las mismas prestaciones que una granja web con balanceo de carga y 10 servidores finales. Comparar precio
y potencia entre esa hipotética máquina y la granja web de una prestaciones similares.

## Ejercicio 2
Buscar información sobre precio y características de balanceadores comerciales (hardware)
específicos. Compara las prestaciones que ofrecen unos y otros.

## Ejercicio 4
Buscar información sobre los métodos de balanceo que implementan los dispositivos recogidos en
el ejercicio 2 (o el software que hemos propuesto para la práctica 3).

## Ejercicio 6
Buscar información sobre los bloques de IP para los distintos países o continentes. Implementar
en JavaScript o PHP la detección de la zona desde donde se conecta un usuario.

## Ejercicio 7
Buscar información sobre métodos y herramientas para implementar GSLB.

# Tema 5

## Ejercicio 1
Buscar información sobre cómo calcular el número de conexiones por segundo.

## Ejercicio 2
Instalar Wireshark y observar cómo fluye el tráfico de red en uno de los servidores web
mientras se le hacen peticiones HTTP... o en la red de casa.

## Ejercicio 3
Buscar información sobre características, funcionalidad, disponibilidad para diversos SO, etc
de herramientas para monitorizar las prestaciones de un servidor.

# Tema 6

## Ejercicio 1
Aplicar con iptables una política de denegar todo el tráfico en una de las máquinas de
prácticas. Comprobar el funcionamiento. Aplicar con iptables una política de permitir todo
el tráfico en una de las máquinas de práticias. Comprobar el funcionamiento.

## Ejercicio 2
Comprobar qué puertos tienen abiertos nuestras máquinas, su estado y qué programa o demonio
lo ocupa.

## Ejercicio 3
Buscar información acerca de los tipos de ataques más comunes en servidores web. Detallar en
qué consisten y cómo se pueden evitar.
