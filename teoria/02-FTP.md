# FTP

1. ¿Qué es FTP?

FTP(File Transfer Protocol) es uno de los protocolos más viejos de la Internet aún utilizados. Fue propuesto en el año 1971 y está descripto en el RFC-114. Existió antes de TCP/IP y se ejecutaba sobre NCP(*Network Control Program*, el predecesor de TCP). Tuvo gran auge con la Internet comercial: en 1992 ya era el protocolo que más volumen transportaba(en la actualidad, ha sido superado por HTTP).
Este protocolo se caracteriza por la codificación en ASCII estandár de los mensajes(7 bits codificados en 8). Usa un modelo cliente/servidor, command/response. Corre sobre TCP ya que requiere un protocolo de transporte confiable. Los clientes FTP, no requieren interfaz gráfica. Es soportado por los browsers/clientes web mediante la URI ```ftp://...```.

2. ¿Cómo funciona FTP?

FTP utiliza dos conexiones TCP: la conexión de Control(Out-Of-Band Control), que corre sobre el puerto 21, y la conexión para la transferencia de datos. Cada conexión posee requerimientos y servicios diferentes: por un lado, la conexión de control es la que se encarga del min relay; mientras que la conexión de datos maneja el min delay.
Para conectarse, el cliente escoge cualquier puerto no privilegiado(cualquiera mayor a 1023) y genera una conexión de control contra el puerto 21 del servidor. El servidor recibe los comandos por dicha conexión y responde/recibe por la conexión de datos aquellos que le sea requerido. La conexión de datos se crea y se cierra bajo demanda(esto es, no es persistente, para cada petición se abre una conexión de datos diferente). El estado de cada operación se transmite por el canal de control.

3. Comandos de FTP

|Comando|Uso|
|:---:|:---:|
|**RETR**|Obtiene un archivo desde el servidor. A nivel de interfaz de usuario, el comando que lo inicia es el get.|
|**STOR**|Envía un archivo al servidor. A nivel de interfaz de usuario, el comando que lo inicia es el put.|
|**LIST**|Envía una petición para listar los archivos del directorio actual en el servidor. A nivel de interfaz de usuario, el comando que lo inicia es el ```ls``` o ```dir``` dependiendo el sistema operativo.|
|**DELE**|Borra un archivo del servidor.|
|**SIZE**|Obtiene el tamaño de un archivo en el servidor.|
|**STAT**|Muestra el estado actual de la conexión.|
|**CD**|Cambia de directorio.|
|**PWD**|Muestra cuál es el directorio actual.|
|**RMD**|Borra un directorio.|
|**MKD**|Crea un directorio.|

4. Modalidades de FTP

Tiene dos modalidades:
* **FTP Activo**(modalidad vieja): la conexión de control corre sobre el puerto 21 y la de datos, sobre el 20. La principal diferencia con la otra modalidad es cómo es manejada la conexión de datos: el servidor, de forma activa, ***se conecta al cliente para generar la conexión de datos***.
* **FTP Pasivo**: la conexión de control sigue estando en el puerto 21, pero la conexión de datos puede correr sobre cualquier puerto que no sea privilegiado(mayor a 1023). En este caso, el servidor, de forma pasiva, ***indica al cliente sobre qué puerto debe conectarse***.

5. Comandos que requieren conexión de datos VS. Comandos que no requieren conexión de datos

|Comandos que requieren conexión de datos|Comandos que NO requieren conexión de datos|
|:---:|:---:|
| RETR, STOR | PWD, STAT, SIZE, DELE, MKD, RMD, etc. |

6. Formato de datos en FTP

Debido a los diferentes tipos de plataformas, las archivos pueden ser convertidos a diferentes representaciones. Es responsabilidad del cliente indicarle al servidor el formato, dado que el default tradicionalmente es ASCII o, en los clientes más modernos, image.

7. Formato de Archivos en FTP

Las diferentes plataformas pueden almacenar los archivos en diversas estructuras. FTP define estructuras para transportar los datos. El formato para transferencia se especifica con el comando STRU.

8. Modo de tranferencia FTP

El comando ```MODE``` se usa para especificar una codificación adicional aplicada sobre los datos transmitidos, de forma independiente del Formato del Archivo.

9. Alternativas a FTP

Depende lo que se quiera hacer:
* Si lo que nos preocupa es una transferencia segura entonces FTPS u FTP usando SSL es lo que estamos buscando.
* Si, por otro lado, lo que queremos es solo compartir recursos, entonces podemos usar NFS, SMB, etc.
* Si lo que queremos es en realidad versiones integradas con la suite de Open-SSH entonces podemos usar SCP(Secure remote Copy) o SFTP(Secure FTP).

10. FTP VS. HTTP

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
