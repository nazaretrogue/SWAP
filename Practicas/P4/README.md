# Asegurar la granja web

## Instalar un certificado SSL autofirmado

Primero generaré el certificado en la máquina M1, y después este certificado será
copiado con _rsync_ a M2 y al balanceador de carga.

Para generar el certificado, se necesita utilizar la herramienta de apache2 que
habilita módulos, _a2enmod_, con la que habilitaremos SSL:

![a2enmod](img/1.png)

Una vez habilitado, reinicio el servicio y creo un directorio en /etc/apache2
para situar dentro los archivos que se generarán durante la creación del certificado:

```sh
service apache2 restart
mkdir /etc/apache2/ssl
```

Ahora genero el certificado x509, con una clave de 2048 bytes, válida durante 1 año.
Durante la generación, pide una serie de datos como el país, la provincia y localidad, la organización y un correo electrónico:

![Generación del certificado](img/2.png)

Tras generarlo, hay que modificar el archivo de configuración del sitio y añadir dos líneas:

```ssh
SSLCertificateFile /etc/apache2/ssl/apache.crt
SSLCertificateKeyFile /etc/apache2/ssl/apache.key
```

![/etc/apache2/sites-available/default-ssl.conf](img/3.png)

Tras esto, activo el sitio y reinicio apache:

![Activación y reinicio](img/4.png)

Una vez hecho esto, comprobamos si efectivamente está funcionando correctamente:

![Host gráfico M1](img/5.png)
![curl M1](img/6.png)

Una vez hecho esto en la M1, para M2 se clona la M1 y se cambia la IP para dejar la
máquina con la misma configuración que tenía anteriormente (al clonar, el nombre y
la IP cambian y quedan iguales a M1, por lo que hay que reconfigurar).

Para cambiar el nombre de la máquina, se modifican los dos archivos siguientes y
se pone el nombre que se desee en ellos:

```sh
vi /etc/hosts
vi /etc/hostname
```

Tras hacer esto, compruebo si M2 funciona, con la misma prueba que he utilizado para
M1: accediendo a la página con el navegador del host y con curl:

![Host gráfico M2](img/7.png)
![curl M2](img/8.png)

Por último, configuraré el balanceador de carga (M3) de forma que la granja entera
acepte peticiones por HTTPS y el balanceador reparta correctamente el tráfico.

Para esto, utilizo _rsync_ y copio los archivos en un nuevo directorio dentro del
home, ya que si lo pusiera en /tmp como el guión, se eliminaría al apagar la máquina.
El funcionamiento de _rsync_ es el que sigue:

![rsync](img/9.png)

Tras esto, modificamos el archivo de configuración de **_nginx_** para que el balanceador
escuche en el puerto 443, el de HTTPS.

![/etc/nginx/conf.d/default.conf](img/10.png)

Tras esto, y mediante el comando curl, comprobamos que el balanceador está repartiendo
correctamente el tráfico HTTPS entre las máquinas. Hay que destacar que el balanceador
está configurado mediante ponderación, con la M1 supuesta el doble de potente que M2,
motivo por el cual el reparto no es equitativo.

![curl M3](img/11.png)
