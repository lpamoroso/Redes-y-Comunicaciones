# HTTP

1. ¿Qué es HTTP?

Es un protocolo de comunicación que permite las transferencias de información en la World Wide Web.. Últimamente está siendo reemplazado por **HTTPS** dado que es un protocolo más seguro(la *s* es de *secure*).

2. ¿Cómo funciona HTTP?

Es un esquema *cliente/servidor request/response stateless*, es decir, el servidor no guarda ningún dato de quién le preguntó. Además, corre sobre TCP(requiere un protocolo de transporte confiable) usando el puerto por default 80. El cliente puede escoger cualquier puerto(siempre y cuando no sea privilegiado) mayor a 1024. Existen 16 bits para formar ese puerto. También trabaja sobre texto ASCII, permitiendo enviar informacion binaria con encabezados MIME. En este modelo, representamos el cliente como un navegador y el servidor como el *software* que atiende las peticiones de los clientes.

3. Versiones de HTTP

* **HTTP 0.9**: la primera versión(no estándar) de HTTP. Las motivaciones de este protocolo eran la funcionalidad para transferir archivos, la habilidad de pedir una búsqueda indexada de un archivo de hipertexto y la posibilidad de referenciar el cliente a otro servidor. Para obtener un documento primero se establecía la conexion TCP. Luego se realizaba el HTTP request vıa comando GET. Posteriormente se enviaba el HTTP response(la página requerida). Luego se cerraba la conexión TCP por parte del servidor. Si el documento no existía o había un error, la conexión era cerrada sin informarse nada al respecto(no existía nada de códigos de error).  
Ésta versión se caracterizaba por solo una forma de requerimiento y solo una forma de respuesta. Además, el *Request/Response* **SOLO** se daba de forma *stateless*.

* **HTTP 1.0**: ésta versión de HTTP es estándar(está definida en el [RFC-1945]). Define formato, el cuál está basado en HTTP 0.9 con las siguientes características:
  + Se debe especificar la versión en el requerimiento del cliente.
  + Para los *request*, define diferentes métodos HTTP.
  + Define códigos de respuesta.
  + Admite repertorio de caracteres, además del ASCII, como ISO-8859-1, UTF-8, etc.
  + Admite MIME (No sólo sirve para descargar HTML e imágenes).
  + Por default NO utiliza conexiones persistentes.



      Request may consist of multiple newline separated header fields.

      Response object is prefixed with a response status line.

      Response object has its own set of newline separated header fields.

      Response object is not limited to hypertext.

      The connection between server and client is closed after every request.

Funciona de la siguiente forma:
1. El pedido podría consistir de múltiples saltos de línea separados por headers.
2. El objeto respuesta está prefijado con una línea de estado de respuesta.

Los comandos que utiliza HTTP:
|Comando|Uso|
|:---:|:---:|
|**GET**|obtiene el documento requerido. Puede enviar informacion por la URL, pero no demasiada. Su formato es ``` ?var1=val1&var2=val2...```. Su tamaño se limita de acuerdo a las implementación.|
|**HEAD**|idéntico a GET, pero sólo requiere la meta información del documento, por ejemplo, su tamaño.Es usado por clientes con caché.|
|**POST**|hace un requerimiento de un documento, perotambién envía información en el body. Generalmente es usado en el fill-in de un formulario HTML. Puede enviar mucha más información que un GET.|
|**PUT**|es usado para reemplazar un documento en el servidor. En general, este comando se encuentra deshabilitado. Es utilizado, por ejemplo, por protocolos montados sobre HTTP, como WebDAV.|
|**DELETE**|es usado para borrar un documento en el servidor. En general, este comando se encuentra deshabilitado. También, puede ser utilizado por WebDAV.|
|**LINK**|establece relaciones entre documentos.|
|**UNLINK**|des-establece relaciones entre documentos.|

Una de las particularidades por las que se distingue HTTP 1.0 es la implementación de host virtuales, es decir, multiplexar varios servicios sobre un mismo host. Esto significa que todos los sitios web alojados comparten los recursos disponibles. El virtual hosting es una alternativa económica frente a los servidores dedicados y los servidores virtuales.  
Otra característica son los **WWW-autenticate headers**, es decir, encabezados que permiten que el cliente y el servidor intercambien información de autenticado. El servidor, ante un requerimiento de un documento que requiere autenticación, enviará un mensaje 401 indicando la necesidad de autenticación y un dominio/realm. El navegador solicitará al usuario los datos de user/password(si es que no los tiene cacheados) y los enviará en texto claro al servidor. El servidor dará o no acceso en base a esos valores. Para los siguientes requerimientos, el navegador usará los valores que tiene almacenados para el realm solicitado.  
Otra peculiaridad es que no se contemplan las conexiones persistentes por default(esto significa que se abre una conexión TCP para cada recurso), lo cual hace que el vínculo sea altamente ineficiente. Sin embargo estas pueden solicitarse de forma explícita.

* **HTTP 1.1**: surgió en 1997 con la [RFC-2068], actualizando además la [RFC-2616]. Con la nueva versión aparecen nuevos mensajes(OPTIONS, TRACE, CONNECT) a los ya existentes. Las conexiones persistentes se realizan por default lo que permite que el vínculo sea más eficiente. También se introduce otra característica, el pipelining, que mejora el tiempo de respuesta ya que no se vuelve necesario esperar la respuesta para pedir otro objeto HTTP. Sin embargo, esta funcionalidad solo puede aplicarse con conexiones persistentes. Además, se vuelve necesario mantener el orden de los objetos que se devuelven sobre la misma conexión. Se pueden utilizar varios threads para cada conexión.

Los nuevos comandos que introduce HTTP 1.1:
|Comando|Uso|
|:---:|:---:|
|**TRACE**|se utiliza para debbuging. El servidor copia el requerimiento tal cual y el cliente puede comparar lo que envía con lo que recibe|
|**CONNECT**|se utiliza para generar conexiones a otros servicios montados sobre HTTP|

3. ¿Qué es una cookie?

Es un mecanismo que permite a las aplicaciones web del servidor "manejar estados". El cliente hace un request y el servidor retorna un recurso (un objeto HTTP, como una página HTML) indicando al cliente que almacene determinados valores por un tiempo. La cookie es introducida al cliente mediante el mensaje en el header ```Set-Cookie: (nombre,valor)``` en donde *nombre* representa el nombre de la cookie y *valor*, su identificador. El cliente en cada requerimiento luego de haber almacenado la cookie se la enviar ́a al servidor con el header ```Cookie:```, pudiendo el servidor utilizarla o no o borrarla. Esta información puede ser usada por client-side scripts.
