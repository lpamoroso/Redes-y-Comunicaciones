# HTTP

1. ¿Qué es HTTP?

Es un protocolo de aplicación de la Web que permite las transferencias de información en la World Wide Web. Últimamente está siendo reemplazado por **HTTPS** dado que es un protocolo más seguro(la *s* es de *secure*).

2. ¿Cómo funciona HTTP?

Es un esquema *cliente/servidor request/response stateless*, es decir, el servidor no mantiene información sobre los pedidos hechos, simplemente responde a cada pedido independientemente. Si el protocolo fuera con estado, aumentaría la complejidad dado que debería mantenerse información sobre los pedidos; además, si se interrumpiera el procesamiento en cliente o servidor, el estado podría quedar inconsistente y esto debería ser solucionado.
El protocolo corre sobre TCP porque requiere un protocolo de transporte confiable. El cliente inicia una conexión TCP(crea el socket) al servidor, en el puerto 80. El servidor acepta una conexión TCP del cliente. Mensajes HTTP son intercambiados entre cliente y servidor. Se cierra la conexión TCP.
El cliente puede escoger cualquier puerto(siempre y cuando no sea privilegiado) mayor a 1024. Existen 16 bits para formar ese puerto. También trabaja sobre texto ASCII, permitiendo enviar informacion binaria con encabezados MIME. En este modelo, representamos el cliente como un navegador y el servidor como el *software* que atiende las peticiones de los clientes.
Su funcionamiento puede ser persistente o no. Si no es persistente:

|Cliente|Servidor|
|:---:|:---:|
|Cliente inicia conexión TCP.||
||Servidor HTTP espera conexión en puerto 80.|
|Cliente envía pedido HTTP por el socket TCP. El mensaje indica que quiere obtener la página Web.||
||Servidor HTTP recibe el pedido y genera un mensaje de respuesta que contiene el objeto pedido.|
|Cliente HTTP recibe la respuesta con el mensaje conteniendo la página HTML que referencia 10 imágenes.||
|Se repiten todos los pasos para las 10 imágenes||
||El servidor HTTP cierra la conexión TCP|

Todo lo anterior provoca la sobrecarga del sistema y red por conexiones TCP extras por lo que los navegadores suelen abrir conexiones paralelas para obtener objetos referenciados.
Si fuera persistente el servidor deja la conexión abierta luego de enviar la respuesta, lo cual ahorra las conexiones TCP extras. Los mensajes subsecuentes entre el mismo cliente/servidor son enviados por la misma conexión abierta. El cliente envía pedidos cuando encuentra objetos referenciados.

3. Versiones de HTTP

* **HTTP 0.9**: La primera version de HTTP fue la 0.9 y nunca fue estandarizada. Define un protocolo del tipo Request/Response muy sencillo. Solo contiene un método o comando: el GET.
Solo existía una forma de hacer el requerimiento y también una única forma de responder. El cliente se conectaba al puerto 80, puerto default de HTTP, aunque bien podría utilizar otro.
Cuando el servidor retornaba el contenido de la página solicitada, cerraba la conexión TCP. Si el documento requerido no existía, el servidor no enviaba nada y solo cerraba la conexión. En tal caso, al cliente no se le mostraba nada. No existía la posibilidad de enviar mucha información del cliente al servidor solo vía el comando GET. No existían códigos que distinguiesen errores de respuestas correctas. No utiliza persistencia.

* **HTTP 1.0**: ésta versión de HTTP es estándar(está definida en el [RFC-1945]). Define un formato de mensaje para el Request y otro para el Response. Posee las siguientes características:
  + Se debe especificar la versión en el requerimiento del cliente.
  + Para los *request*, define diferentes métodos HTTP.
  + Define códigos de respuesta.
  + Admite repertorio de caracteres, además del ASCII, como ISO-8859-1, UTF-8, etc.
  + Admite MIME(Multipurpose Internet Mail Extensions), no solo sirve para descargar HTML e imágenes.
  + Por default, NO utiliza conexiones persistentes.

  Los comandos que utiliza HTTP:

  |Comando|Uso|
  |:---:|:---:|
  |**GET**|obtiene el documento requerido. Puede enviar informacion por la URL, pero no demasiada. Su formato es `?var1=val1&var2=val2...`. Su tamaño se limita de acuerdo a las implementación.|
  |**HEAD**|idéntico a GET, pero sólo pide la meta información del documento, por ejemplo, su tamaño. Es usado por clientes con caché.|
  |**POST**|hace un requerimiento de un documento, perotambién envía información en el body. Generalmente es usado en el fill-in de un formulario HTML. Puede enviar mucha más información que un GET.|
  |**PUT**|es usado para reemplazar un documento en el servidor. En general, este comando se encuentra deshabilitado. Es utilizado, por ejemplo, por protocolos montados sobre HTTP, como WebDAV.|
  |**DELETE**|es usado para borrar un documento en el servidor. En general, este comando se encuentra deshabilitado. También, puede ser utilizado por WebDAV.|
  |**LINK**|establece relaciones entre documentos.|
  |**UNLINK**|des-establece relaciones entre documentos.|

  Una de las particularidades por las que se distingue HTTP 1.0 es la implementación de host virtuales, es decir, multiplexar varios servicios sobre un mismo host en lugar de tener un servidor por cada puerto. Primero, se deben crear los contenidos para cada uno de los hosts virtuales. Una vez creado el “nuevo servidor”, se debe modificar Apache2 para que sea capaz de trabajar con hosts virtuales.
  Otra característica son los **WWW-autenticate headers**, es decir, encabezados que permiten que el cliente y el servidor intercambien información de autenticado. El servidor, ante un requerimiento de un documento que requiere autenticación, enviará un mensaje 401 indicando la necesidad de autenticación y un dominio/realm. El navegador solicitará al usuario los datos de user/password(si es que no los tiene cacheados) y los enviará en texto claro al servidor. El servidor dará o no acceso en base a esos valores. Para los siguientes requerimientos, el navegador usará los valores que tiene almacenados para el realm solicitado.
  Otra peculiaridad es que no se contemplan las conexiones persistentes por default(esto significa que se abre una conexión TCP para cada recurso), lo cual hace que el vínculo sea altamente ineficiente. Sin embargo estas pueden solicitarse de forma explícita mediante los headers.

* **HTTP 1.1**: surgió en 1997 con la [RFC-2068] y luego fue actualizado por la [RFC-2616]. Con la nueva versión aparecen nuevos mensajes(OPTIONS, TRACE, CONNECT) a los ya existentes. Las conexiones persistentes se realizan por default lo que permite que el vínculo sea más eficiente. También se introduce otra característica, el pipelining, que mejora el tiempo de respuesta ya que no se vuelve necesario esperar la respuesta para pedir otro objeto HTTP. Sin embargo, esta funcionalidad solo puede aplicarse con conexiones persistentes. Además, se vuelve necesario mantener el orden de los objetos que se devuelven sobre la misma conexión. Se pueden utilizar varios threads para cada conexión.

  Los nuevos comandos que introduce HTTP 1.1:

  |Comando|Uso|
  |:---:|:---:|
  |**TRACE**|se utiliza para debbuging. El servidor debe copiar el requerimiento en la respuesta al cliente. De esta forma, el cliente puede observar que es lo que está recibiendo el servidor. Es muy util cuando se utiliza a través de proxies.|
  |**OPTIONS**|Este comando sirve para consultar las opciones de operaciones/métodos que ofrece un server sobre un documento.|
  |**CONNECT**|es util cuando se utiliza HTTP como medio de transporte de una conexion TCP(no necesariamente HTTP) a través de un proxy. Por ejemplo podría utilizarse HTTP para conectarse a un servidor de mail SMTP.|

3. Otras características de HTTP: redirects

Los redirect son mensajes que se utilizan para enviar al user-agent(browser) a otra ubicación debido a que el recurso ha sido movido.
Cuando una URL/URI es reemplazada permanentemente por otra URL/URI se debería utilizar un mensaje *301 Moved Permanently*. Lo que esto indica es que cualquier acceso futuro deberá realizarse sobre la nueva ubicación. Este mensaje es comúnmente usado cuando se genera un hostname, donde reside un servidor web, con y sin el prefijo *www* en el nombre. Es importante marcar que ambos representen lo mismo y se utilice una redirección permanente. Si no lo fuesen, podrían surgir problemas, por ejemplo con las Cookies. Este mensaje es ́util también para los motores de búsqueda ya que pueden presentar la URL final.

4. Más cosas locas de HTTP: CGI scripts

Una CGI es una aplicación que interactúa con un servidor web para obtener información de los requerimiento generados por el cliente y así poder responder. Existe un estándar del W3C para las CGIs. De esta manera, se le agrega cierto dinamismo a los sitios que trabajan en la *www*. Las CGIs leen de STDIN (POST) o de variables de entorno `QUERY_STRING` los datos del usuario y escriben en la STDOUT response.
Estos métodos están definidos en el estándar del W3C: CGI 1.1. Todo el procesamiento de CGI es realizado del lado del servidor: server-side, el cliente solo genera los datos y los envía para que luego sean procesados. Los programas CGI mayormente se los relaciona con código binario, compilado previamente, aunque los ejemplos mostrados son sobre lenguajes de script. La evolución de los CGI ha dado nuevos lenguajes de scripting que ejecutan del lado del servidor, interpretando y/o compilando al vuelo los programas a ejecutar, interactuando como CGI o módulo binario con el servidor web. Un caso es PHP(PHP Hypertext Pre-processor), dise ̃nado originalmente a partir de Perl para generar contenido web de forma dinámica.

5. Client-Side Script: JavaScript

De la misma forma que existen lenguajes de script que ejecutan del lado del servidor, los hay que lo hacen del lado del cliente, dentro del navegador/browser. El primer más popularizado fue JavaScript. Hoy ya es un estándar. El W3C ha definido un modelo de objetos conocido como DOM(Document Object Model), sobre el cual trabaja el lenguaje Javascript.

4. ¿Qué es una cookie?

Es un mecanismo que permite a las aplicaciones web desde el servidor(como CGIs o scripts) almacenar y leer información en el lado del cliente y de esta forma poder manejar de forma sencilla estados persistentes. Esta información también puede ser consultada o generada por aplicaciones del lado del cliente como, por ejemplo, desde JavaScript. Habitualmente son utilizadas para:
* Autenticación por aplicación.
* Carritos de compras(shopping carts).
* Preferencias, recomendaciones.
* Estado de sesión de usuario(user session state, e.g. web-email).

El servidor cuando retorna un recurso(un objeto HTTP, como una página HTML) puede solicitar al cliente que almacene de forma temporal información de estado. Este estado es almacenado en el cliente y asociado con una fecha de expiración. Las cookies se almacenan por site y puede haber varias por cada uno.
Mediante este agregado se provee a un protocolo sin estados la posibilidad de simularlos permitiendo generar aplicaciones web más complejas, por ejemplo aplicaciones de venta on-line, manejo de autenticación y preferencias de usuarios.
Una Cookie es introducida al cliente mediante el mensaje en el header `Set-Cookie: (nombre,valor)` de una respuesta HTTP, en donde *nombre* representa el nombre de la cookie y *valor*, su identificador. El cliente, en cada requerimiento, luego de haber almacenado la Cookie se la enviará al servidor con el header `Cookie:`, pudiendo el servidor utilizarla o no o borrarla. Esta información puede ser usada por client-side scripts.

5. ¿Qué es el HTTPS?

Aparte del HTTP convencional, existe otra rama que trabaja sobre una capa de “transporte” que ofrece seguridad, como por ejemplo, cifrado y chequeo de identidad. Esta capa en el modelo OSI estaría a nivel presentación y de aplicación: estamos hablando de SSL(Secure Sockets Layer). La combinación de HTTP junto con SSL se lo conoce como HTTPS o 'HTTP Seguro' y corre, habitualmente, sobre el port TCP 443. La capa SSL/TLS no sólo sirve para HTTP, también se puede combinar con otros protocolos que no tienen seguridad integrada como SMTP, FTP, etc.

6. ¿Qué es la WEB-cache?

Consiste en cachear recursos HTTP. La idea es satisfacer el pedido del cliente sin involucrar el servidor original, lo que mejora el tiempo de respuesta(reduce retardo en descarga), ahorrar BW(*balance wage*, recursos de la red), permite atender a todos los clientes, etc. La configuración se realiza en el navegador(opción *acceso mediante cache*).
El funcionamiento es el de cualquier cache: se solicita el objeto, si esta en la cache y esta "fresco"(sin manosear) se retorna desde allí(HIT); si el objeto no esta o es viejo se solicita al destino y se cachea(MISS). Se puede realizar control de acceso.
Del lado del cliente, los web browser tienen sus propias cache locales. Como *servers*, las cache funcionan como proxy. Son servidores a los clientes y clientes a los servidores web. Los instalan ISP(*Internet Service Provider*, Ej: F*bertel) o redes grandes que desean optimizar el uso de los recursos.
Hay dos protocolos de comunicación entre web-cache servers: ICP(Internet Cache Protocol) y HTCP(Hyper Text Caching Protocol). Comunican el router y los web-cache servers. También existe el WCCP(Web Cache Control Protocol). En general todos corren sobre UDP.
