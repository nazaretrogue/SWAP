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
\- | - | Total | 0.99214

## Ejercicio 2
Buscar frameworks y librerías para diferentes lenguajes que permitan hacer aplicaciones
altamente disponibles con relativa facilidad.

+ **PM2**: es un administrador de tareas que permite mantener, creado sobre un
    balanceador de carga que permite mantener activas las aplicaciones con una
    altísima disponibilidad y que facilita el trabajo de los administradores de
    sistemas.
+ **Vaadin**: es una plataforma para aplicaciones web que incluye un conjunto de
    componentes, entre los que está un framework de Java para web, que ayuda al
    mantenimiento de la disponibilidad de las aplicaciones hospedadas en la
    plataforma. Es sencillo de usar y está pensado especialmente para principiantes.
+ **Sprint MVC**: es una buena alternativa a *Vaadin*; altamente escalable, permite
    una disponibilidad de los servicios alta y tiene una gran cantidad de documentación.
    Pero, a diferencia de *Vaadin*, mantener el código con este framework es bastante
    más complejo y puede ser difícil para alguien recién iniciado.

## Ejercicio 3
¿Cómo analizar el nivel de carga de cada uno de los subsistemas en el servidor? Buscar
herramientas y aprender a usarlas.

Para poder medir el nivel de carga de cada subsistema en un servidor, se utilizan
los llamados *profilers*, monitores que se centran en analizar el nivel de carga
de los distintos componentes. También existen los monitores que analizan el tráfico
del sistema al completo (como el monitor *sar*, que se basa en dos órdenes
complementarias, *sadc*, que recopila datos, y *sar*, que traduce dichos datos a
un formato legible y entendible.) Entre los *profilers*, destacan:

+ **gprof**: un monitor fácil de utilizar, que da el uso de CPU de un programa.
    Para utilizarlo, es necesario instrumentar el programa en la compilación
    para poder recoger datos, y tras conseguirlos, analizarlos.
+ **Perf**: esta herramienta ofrece la posibilidad de analizar muchos más componentes
    según las necesidades del servidor. Está basado en eventos hardware y software
    y posee una gran cantidad de comandos según lo que se vaya a analizar.
+ **V-Tune**: al igual que *Perf*, está basado en eventos y soporta tanto eventos
    hardware como software. También se puede utilizar como depurador y es capaz
    de medir el rendimiento de cada componente que se analice.

## Ejercicio 4
Buscar ejemplos de balanceadores software y hardware (productos comerciales). Buscar
productos comerciales para servidores de aplicaciones. Buscar productos comerciales
para servidores de almacenamiento.

### Hardware

+ **F5 Networks Big/IP**: uno de los productos más conocidos para balanceo de
    carga con hardware. Este dispositivo está contenido en un rack, listo para
    ser montado en un armario, y posee software específico de forma que realiza
    la carga de forma eficiente.
+ **Cisco Load Balancer**: la metodología de Cisco consiste en crear routers que
    directamente actúan como balanceadores, de forma que no tiene que disponer de
    hardware distinto al que acostumbran. De esta forma, crean balanceadores de
    carga hardware con una tecnología que dominan y dan lugar a dispositivos
    pequeños y manejables pero potentes y eficientes.
+ **Kemp Technologies**: han creado un balanceador de carga al alcance de todos
    (barato en comparación con los existentes) pero que tiene una potencia similar
    o mínimamente inferior, aunque los consumidores de estos aún creen que necesitan
    algo más de experiencia en el mercado para crear balanceadores capaces de
    soportar una mayor funcionalidad.

### Software

+ **Seesaw**: usado por Google, ha demostrado ser un software de balanceo eficiente
    para redes en la capa de transporte. En el caso de tener redes que se quieran
    balancear en capa 7, quizá ésta no sea la mejor opción.
+ **LoadMaster**: balanceador de carga por software creado por *Kemp*; es gratuito
    y con gran funcionalidad. Soporta balanceo en capa 4 y capa 7, incluye un
    cortafuegos para aplicaciones web, persistencia de cookies, tunneling, etc.
+ **HAProxy**: uno de los más populares, da lugar a alta disponibilidad, proxy,
    y el propio balanceo de carga. Lo utilizan grandes sitios como GitHub o Reddit.

# Tema 3

## Ejercicio 1
Buscar con qué órdenes de terminal o herramientas podemos configurar bajo Windows y bajo
Linux el enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra.

## Ejercicio 2
Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows
y bajo Linux el filtrado y bloqueo de paquetes.

# Tema 4

## Ejercicio 1
Buscar informaión sobre cuánto costaría en la actualidad un mainframe que tuviera
las mismas prestaciones que una granja web con balanceo de carga y 10 servidores finales.
Comparar precioy potencia entre esa hipotética máquina y la granja web de una
prestaciones similares.

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
