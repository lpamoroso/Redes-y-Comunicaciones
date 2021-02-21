# Programación con sockets

1. ¿Qué es un socket?

Interfaz local al host, creada por la aplicación y controlada por el S.O.(una "puerta") a través de la cual los procesos pueden enviar y recibir mensajes desde y hacia otros procesos. Encapsula todo el manejo de la comunicación entre un punto y otro y da la sensación a la aplicación de estar dialogando directamente con la aplicación del otro lado. Los sockets están asociados a una dirección y un puerto en el host y conocen la dirección y puerto donde está el socket destino.

## Programación de sockets TCP

1. ¿Cómo empiezo a crear una API para los socket?

En principio, es necesario definirle ciertas primitivas:

|Primitiva|Descripción|
|:---:|:---:|
|SOCKET|Crea un nuevo punto de comunicación|
|BIND|Engancha el socket con una dirección y puerto locales|
|LISTEN|Anuncia que se aceptarán conexiones e indica el largo de la cola|
|ACCEPT|Bloquea al llamador hasta que llega un intento de conexión|
|CONNECT|Activamente intenta establecer una conexión|
|SEND|Envía datos sobre la conexión|
|RECEIVE|Recibe datos de la conexión|
|CLOSE|Cierra la conexión|

2. ¿Qué hace el servidor y qué el cliente?

### Cliente

* Crea un socket TCP local al cliente.
* Especifica la dirección IP y el puerto del servidor.
* Cuando el cliente crea el socket, establece la conexión TCP con el servidor.
* Cuando es contactado por el cliente, el servidor TCP crea un nuevo socket a través del cual el proceso servidor se comunicará con el cliente
  + Esto permitirá que un servidor dialogue con muchos clientes.
  + También, cada cliente será atendido en un número de puerto distinto.

### Servidor

* El proceso servidor debe estar ejecutándose.
* El servidor debe haber creado un socket que recibe el contacto del cliente.

## Programación de sockets UDP

1. ¿Qué debo tener en cuenta?

* No hay "conexión" entre el cliente y el servidor(no hay *handshaking*).
* El emisor indicará la IP y el puerto en cada paquete que envíe.
* El receptor sabrá a qué IP y puerto contestar pues lo obtendrá del paquete UDP.
* Los datos pueden llegar a destino en desorden, o no llegar.

## Armando un servidor simple

1. ¿Qué consideraciones debo tener?

* Maneja conexiones HTTP de a una(un solo thread).
* Acepta la conexión.
* Procesa el cabezal.
* Obtiene el archivo pedido del disco.
* Crea el mensaje de respuesta.
  + Cabezal y archivo.
* Envía respuesta al cliente.
* Si el protocolo HTTP se respeta, este servidor podrá ser invocado desde un browser(IE, Firefox).
