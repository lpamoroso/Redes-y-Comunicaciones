# HTTP

## Definición

El protocolo de transferencia de hipertexto (en inglés HTTP, _HyperText Transfer Protocol_) es el protocolo de la capa de aplicación de la web por excelencia. Utiliza una arquitectura cliente-servidor para su implementación, es decir, dos programas en terminales diferentes que se comunican a través de HTTP. Una de ellas es un host activo (el servidor) que siempre atiende peticiones de otros hosts (clientes) que pueden ser permanentes o intermitentes. Antes de explicar HTTP en detalle, hay que tener en cuenta varios conceptos básicos.

Una página web consta de objetos. Estos objetos no son más que archivos que pueden direccionarse mediante un único URL. La mayoría de estas páginas web se componen de un archivo base HTML y de varios objetos referenciados. Este archivo HTML es el que hace referencia a los otros archivos contenidos en la página mediante la URL de los objetos. Cada URL tiene dos partes: **el nombre de host** del servidor que alberga el objeto y **el nombre de la ruta** al objeto. Por ejemplo, en la url <http://www.unaescuela.com/dptos>, _www.unaescuela.com_ es el nombre del host e _dptos_ es el nombre de una ruta.

## Funcionamiento

HTTP define cómo los clientes web solicitan páginas web a los servidores web y cómo estos servidores transfieren esas páginas web a los clientes. HTTP utiliza TCP como protocolo de transporte subyacente. El cliente HTTP primero inicia una conexión TCP con el servidor. Una vez que la conexión se ha establecido, los procesos de navegador y de servidor acceden a TCP a través de sus interfaces de socket. El cliente envía mensajes de solicitud HTTP a su interfaz de socket y recibe mensajes de respuesta HTTP procedentes de su interfaz de socket y envía mensajes de respuesta a través de la interfaz de su socket. Una vez que el cliente envía un mensaje a través de su interfaz de socket, el mensaje deja de depender del cliente y pasa a depender de TCP. TCP proporciona un servicio de transferencia de datos fiable a HTTP, por lo que cada mensaje de solicitud HTTP que envíe el cliente llegará intacto al servidor. Idem con las respuestas del servidor hacia el cliente. Esta es una de las ventajas de la arquitectura en capas: HTTP no tiene que preocuparse por las pérdidas de datos o por los detalles acerca de cómo TCP recupera los datos perdidos o los reordena dentro de la red. Este es el trabajo de TCP y de los protocolos de las capas inferiores de la pila de protocolos.

Es importante observar que el servidor envía los archivos solicitados a los clientes sin almacenar ninguna información acerca del estado del cliente. Esto significa que si un mismo cliente pide dos veces un mismo objeto con diferencia de segundos, el servidor no responderá que acaba de servirlo; en su lugar, reenviará el objeto ya que ha olvidado completamente que antes lo había servido.

## Conexiones persistentes VS. no persistentes

Dependiendo la aplicación o cómo se use, es que se puede optar entre manejar solicitudes una tras otra, periódicamente a través de intervalos regulares o de forma intermitente. Dado que esta interacción tiene lugar sobre TCP, lo que hay que preguntarse es si conviene que cada par solicitud/respuesta se envíen a través de una conexión separada o utilizando la misma conexión. El primer método es el de conexiones persistenes; el segundo, el de no persistentes. HTTP, por defecto, utiliza las conexiones persistentes pero es posible configurar, tanto para los clientes como para el servidor, si se desea optar por conexiones no persistentes.

Si lo que se busca es que las conexiones no sean persistentes, los pasos para obtener el archivo HTML base y 10 archivos JPEG de un servidor <http://www.unaescuela.com/dptos> sería algo así:

1. El proceso cliente HTTP inicia una conexión TCP con el servidor <http://www.unaescuela.com> en el puerto 80, que es el número de puerto por defecto para HTTP. Asociados con la conexión TCP, habrá un socket en el cliente y un socket en el servidor.
2. El cliente HTTP envía un mensaje de solicitud HTTP al servidor a través de su socket. El mensaje de solicitud incluye el nombre de la ruta /dptos.
3. El proceso servidor HTTP recibe el mensaje de solicitud a través de su socket, recupera el objeto /dptos de su medio de almacenamiento , encapsula el objeto en un mensaje de respuesta HTTP y lo envía al cliente a través de su socket.
4. El proceso servidor HTTP indica a TCP que cierre la conexión TCP (Sin embargo, TCP no cierra la conexión hasta que está seguro de que el cliente ha recibido el mensaje de respuesta en perfecto estado).
5. El cliente HTTP recibe el mensaje de respuesta. La conexión TCP termina. El mensaje indica que el objeto encapsulado es un archivo HTML. El cliente extrae el archivo del mensaje de respuesta, examina el archivo HTML, y localiza las referencias a los 10 objetos JPEG.
6. Se repiten los cuatro primeros pasos para cada uno de los archivos JPEG referenciados.

Es importante notar que cada conexión TCP transporta exactamente un mensaje de solicitud y uno de respuesta. Esto indicaría que para solicitar la página web se estarían efectuando 11 conexiones TCP. A lo anterior hay que sumarle, además, los retardos de propagación de los paquetes, los retardos de cola en los routers y switches intermedios y los retardos de procesamientos de los paquetes. Todo esto se mide con el tiempo de ida y vuelta (en inglés RTT, _Round Trip Time_). Así, TCP, al iniciarse, define un acuerdo en tres fases. En la primera y segunda parte, el cliente envía un pequeño segmento TCP al servidor; el servidor reconoce y responde con otro pequeño segmento TCP; y por último el cliente devuelve un mensaje de reconocimiento al servidor. Las primeras dos partes de este proceso de acuerdo en tres fases tardan un período de tiempo RTT. Completadas las primeras dos fases, la tercera envía la petición HTTP junto con la tercera parte de la negociación (el mensaje de reconocimiento). Una vez que el mensaje llega al servidor, este envía el archivo HTML a la conexión TCP. Este mensaje de solicitud/respuesta consume otro período de tiempo RTT. Es así que el tiempo de respuesta total es aproximadamente igual a dos RTT más el tiempo de transmisión del archivo HTML en el servidor.

De esta manera, es que podemos intuir que las conexiones no persistentes presentan algunos inconvenientes. El más notable radica en que por cada objeto solicitado ha de establecerse una nueva conexión, con lo que ello significa: para cada conexión han de asignarse los buffers TCP y las variables que tendrán que mantenerse tanto en el cliente como en el servidor. Esto podría sobrecargar de forma significativa al servidor web, mucho más teniendo en cuenta que podría estar sirviendo múltiples solicitudes simultáneamente. Por otro lado, cada objeto sufre un retardo de entrega de dos RTT: uno para establecer la conexión TCP y otro para solicitar y recibir un objeto.

Con las conexiones persistentes, el servidor deja la conexión TCP abierta después de enviar una respuesta. Las subsiguientes solicitudes y respuestas que tienen lugar entre el mismo cliente y servidor pueden enviarse a través de la misma conexión, realizándose una tras otra, sin esperar a obtener las respuestas a las solicitudes pendientes. Luego, quien se encarga de cerrar la conexión cuando esta no se ha utilizado durante un tiempo, es el servidor HTTP.

## Tipos de mensajes

### Mensaje de solicitud HTTP

Un mensaje de solicitud típico de HTTP se asemeja a lo siguiente:

```HTTP
GET /unadireccion/pagina.html HTTP/1.1
Host: www.unaescuela.edu
Connection: close
User-agent: Mozilla/4.0
Accept-language: fr
```

La primera línea de un mensaje de solicitud HTTP se denomina línea de solicitud y las siguientes líneas son las líneas de cabecera. La línea de solicitud consta de tres campos: el campo de método, el campo URL y el campo de la versión HTTP. El campo del método puede tomar varios valores, entre los que se incluyen GET, POST, HEAD, PUT y DELETE. La mayoría de mensajes de solicitud HTTP son de tipo GET. Este método se utiliza cuando el navegador solicita un objeto, identificando dicho objeto en el campo URL. El campo correspondiente a la versión, indica la versión utilizada por la solicitud.
