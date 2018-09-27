# FTP

1. ¿Qué es FTP?

FTP(File Transfer Protocol) es uno de los protocolos más viejos de la Internet aún utilizados. Fue propuesto en el año 1971 y funcionó antes de que exista TCP/IP, ejecutaba sobre NCP(*Network Control Program*, el predecesor de TCP). FTP es un protocolo para copiar archivos completos, a diferencia de otros protocolos que brindan acceso a archivos, pero dando un servicio más transparente, como NFS (Network File System).
De la misma forma que la mayorı́a de los protocolos de Internet, como HTTP, SMTP y TELNET, sus mensajes se codifican en ASCII estándar(de 7 bits codificados en 8). Los clientes FTP no requieren interfaz gráfica. Es soportado por los browsers/clientes web mediante la URI `ftp://...`.
El funcionamiento del protocolo FTP utiliza una arquitectura cliente/servidor, pero se distingue de los demás servicios por el hecho de usar una conexión para el protocolo de control (Out-Of-Band Control) y otra conexión para la transferencia de datos. Cada conexión tendrá requerimientos de servicios diferentes. Por ejemplo, la de control, minimizar el delay, y la de datos, maximizar el throughput. El protocolo se ejecuta sobre TCP porque requiere un protocolo de transporte que le brinde detección de errores(confiabilidad).
El protocolo FTP provee funcionalidad de la capa de sesión al permitir hacer re-starts de las transferencias de archivos que fueron canceladas por errores “graves”, como caı́da del servidor.
Además, FTP se encarga de varios aspectos de la capa de Presentación, como es el formato de los datos. Puede manejar los formatos EBCDIC, ASCII o IMAGE (BINARY, RAW). Estos formatos deben ser escogidos por el usuario antes de la transferencia. En un principio, el formato default era ASCII, pero hoy es más común encontrar IMAGE.
En el caso de ASCII, se convierte el archivo origen a formato ASCII y, luego, el receptor lo debe pasar de ASCII al formato local. Cada fin de lı́nea se traduce a CR,LF. En el caso de Unix, el fin de lı́nea es solo LF. Si se selecciona un formato no adecuado de transferencia, pueden producirse problemas. Por ejemplo, si se transfiere un archivo binario, como lo es un ejecutable, en formato ASCII, se “romperá”.
Las plataformas pueden almacenar los archivos en diferentes estructuras. FTP define estructuras para transportar datos. Su formato es el *default file*. Se especifica el formato para transferencia con el comando `STRU`. Los formatos son:
* File F secuencia de bytes sin estructura.
* Record R una serie de registro.
* Page P una serie de bloques(pages).
Con respecto a la capa de Presentación, FTP define diferentes modos de transferencia:
* Un hilo de bytes: Stream. EOF como Registro o cierre de stream de acuerdo al formato del archivo: STRU.
* En Bloques: 1 header + serie de bloques, permite interrupción.
* Comprimido: utiliza RLE(Run Length Encoding).
El primero, Stream, es el más utilizado.

2. ¿Cómo funciona FTP?

Como se mencionó más arriba, FTP mantiene una conexión o sesión para enviar la información de control y otra para la transferencia de datos. El funcionamiento de FTP es el siguiente:

1. Primero, el cliente se conecta al servidor estableciendo la sesión de control(escoge cualquier puerto no privilegiado mayor a 1023). El servidor, para la sesión de control, escucha en el puerto 21.
2. El cliente tiene la posibilidad de autenticarse o de entrar como usuario anónimo. Esto lo hace utilizando la conexión y comandos de control.
3. Luego, el cliente enviará los comandos respectivos de acuerdo a las operaciones que quiera realizar. Entre éstos nos encontramos con comandos que requerirán la conexión de datos y otros que no:
Algunos comandos que requieren la sesión de datos:
* Enviar un archivo al servidor(`STOR`).
* Traer un archivo desde el servidor(`RETR`).
* Listar los archivos en el servidor(`LIST`).
Algunos comandos que no requieren la sesión de datos:
*Ver en el directorio donde está parado (`CWD`, `PWD`).
*Ver información de un archivo en particular (`STAT`, `SIZE`).
* Cambiar el modo (`MODE`).
* Obtener ayuda (`HELP`).
* Borrar un archivo (`DELE`).
* Crear un directorio (`MKD`).
* Borrar un directorio (`RMD`).
* Moverse entre directorios (`CD`).
4. Antes de enviar un comando que requiera una conexión de datos, necesitará establecerla. Para esto, como se verá más adelante, existen dos modos.
5. La sesión de datos se abre y cierra por cada transferencia individual.
6. Por último, el cliente decidirá terminar la sesión de control. Los servidores pueden tener configurados timers que luego de estar la sesión de control y de datos relacionadas un tiempo sin usar pueden dar de baja la conexión.

La conexión de datos se crea y se cierra bajo demanda(esto es, no es persistente, para cada petición se abre una conexión de datos diferente). El estado de cada operación se transmite por el canal de control.

3. Modos del FTP

FTP tiene la capacidad de trabajar de dos modos diferentes con respecto a la sesión de datos.

* **Activo**: En el modo Activo, el cliente le debe indicar al servidor con el comando PORT en que dirección IP y puerto escuchará la conexión de datos. Es activo con respecto al servidor. Este puerto será un puerto no privilegiado o efı́mero (por encima del 1024). El servidor, desde el puerto número 20, se conectará al puerto indicado por el cliente para establecer la conexión de datos. Esto se realizará por cada operación que requiera una conexión de datos. Éste era el modo default. El formato del comando port es el siguiente:

`PORT h1,h2,h3,h4,p1,p2`
`PORT 127,0,0,1,4,3 == 127.0.0.1:1027 (4*256)+3`

El problema que presenta este modo es que se pueden encontrar complicaciones al realizar NAT (Network Address Translation) o al configurar un firewall que protege a los clientes. Actualmente, no se recomienda su uso, pero existen algunos servidores viejos que solo soportan este modo.

* **Pasivo**: En el modo Pasivo, en lugar de que el servidor se conecte al cliente, es el cliente el que se conecta al servidor. Es pasivo con respecto al servidor. El cliente debe indicarle al servidor que quiere trabajar con este modo usando el comando `PASV`. El servidor, como respuesta, indicará el puerto, con el mismo formato que el comando `PORT`, donde esperará la conexión. Es el modo recomendado actualmente.

4. FTP Anónimo

El protocolo FTP permite conectarse a un servidor sin necesidad de poseer una cuenta. Una de las formas más populares de usar FTP es la modalidad anónima *Anonymous FTP*, la cual admite *loguearse* sin necesidad de una cuenta. Esta modalidad es, generalmente, en modo de solo lectura, es decir, sólo se pueden obtener archivos pero no eliminarlos. Usualmente, se suele encontrar un directorio pub/incoming en el cual se pueden dejar archivos hasta cierto tamaño. Cuando se accede a un servidor con esta modalidad se usa, como nombre de usuario, el texto anonymous y como contraseña, la dirección de e-mail. Para habilitar una sesión de este tipo es necesario reconfigurar el servidor.
Es importante, al utilizar FTP anónimo, restringir el acceso a un directorio en particular, no permitiendo que el usuario anónimo se pueda cambiar a otro directorio o listar contenidos del sistema. Es por esto que se dedica un directorio que decide el usuario en particular para este propósito.

5. Alternativas a FTP

Depende lo que se quiera hacer:
* Si lo que nos preocupa es una transferencia segura entonces FTPS u FTP usando SSL es lo que estamos buscando.
* Si, por otro lado, lo que queremos es solo compartir recursos, entonces podemos usar NFS, SMB, etc.
* Si lo que queremos es en realidad versiones integradas con la suite de Open-SSH entonces podemos usar SCP(Secure remote Copy) o SFTP(Secure FTP).

6.FTP sobre SSL: FTPS
Usando el protocolo FTP, la información confidencial como las contraseñas o, incluso, el contenido de los archivos viaja en la red sin cifrar. SSL puede montarse sobre FTP de forma similar como sucede con HTTP. Para FTP sobre SSL existen puertos asignados especialmente.

7. Usos de FTP
En la actualidad, el protocolo FTP ha caı́do en desuso a causa del avance de HTTP. En 1992, era el protocolo que más datos transportaba en la Internet. La desventaja de HTTP, con respecto a FTP, es que fue pensado para trabajar más como protocolo de descarga(download) que de carga(upload); en cambio, FTP tiene los dos roles. Si bien HTTP, a partir de otros comandos y programación extra, permite ser usado como protocolo de carga, FTP parece ser más eficiente para grandes archivos. La gran ventaja de HTTP es la facilidad que ofrece ante la existencia de un firewall o servicios de NAT en redes que utilizan este servicio, para FTP esta labor es más difı́cil. Otra ventaja de HTTP parece ser que sus conexiones consumen menos recursos.
Con respecto a su uso como de protocolo de descarga, FTP sigue estando disponible en la mayorı́a de los sitios donde los archivos para descargar suelen ser grandes, de gran cantidad o donde hay muchas descargas. La ventaja que tenı́a FTP ante HTTP era que permitı́a resumir una descarga. El protocolo FTP en 2017 se lo encuentra en sites de descargas de distibuciones de sistemas operativos, como los mirrors de OpenBSD. Pero de alguna forma podemos ver que va perdiendo fuerza, por
ejemplo en este mismo año, 2017, se quito el soporte de los mirrors de los Kernel de GNU/Linux, dejando solo RSYNC, GIT y HTTPS.

8. FTP VS. HTTP

|FTP|HTTP|
|:---:|:---:|
|Diseñado para upload de archivos|Puede uplodear archivos con PUT o POST, pero no es lo único que permite|
|Soporte de Download|Soporte de Download|
|Formato ASCII/binary, el cliente decide|meta-data with files, Content-Type, mucho más flexible|
|No maneja Headers|Maneja Headers, por lo que puede controlar más información|
|FTP Command/Response, es más lento para archivos pequeños|Pipelining|
|Más complejo con Firewalls y NAT|Más amigable con Firewalls y NATs|
|Dos conexiones(control y datos) y un modo de transferencia que puede ser activo o pasivo|Una conexión, más sencillo|
|Soporta Ranges/resume|Utiliza Range/resume HTTP que provee opciones más avanzadas|
|Soporte de Autenticación|Soporte de Autenticación|
|Soporte de Cifrado, pero puede traer problemas con el firewall|Soporte de Cifrado|
|Soporte de Compresión RLE|Soporte de Compresión Deflate: LZ77+Huffman|
