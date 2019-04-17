# DNS

1. Investigue y describa cómo funciona el DNS ¿Cuál es su objetivo?

    DNS es un sistema de nomenclatura jerárquico y descentralizado que sirve para traducir los dominios en IPs.  
    El resolver es quien determina los servidores en lo que se puede realizar la consulta por el nombre del dominio en cuestión mediante una secuencia de consultas empezando por aquella etiqueta que está más a la derecha.  
    Asumiendo que el resolver no tienen cacheado registros para acelerar el proceso, el proceso de resolución empieza con una consulta a uno de los servidores raíces. Usualmente, los servidores raíces no responden directamente, sino que responden con referencias a otro servidores más autoritativos, por ejemplo, si se busca *test.com*, se refiere a los servidores de *com*. El resolver ahora consultará a los servidores que le fueron referidos, y así, iterativamente, repetirá el proceso hasta que llegue a una respuesta autoritativa.  
    A priori, este mecanismo produciría un tráfico muy alto en la carga de los servidores raíces, si cada resolución de internet requiriera empezar por los servidores raiz. En la práctica, el cacheado de servidores DNS es esencial para liberar de esa carga a los servidores raíces y, consecuentemente, los servidores raíces se ven envueltos en una fracción de pedidos relativamente pequeña.

2. ¿Qué es un root server? ¿Qué es un *generic top-level domains* (GTLDs)?

    Un root server o servidor raíz es un servidor que delega todos los TLD a servidores más autoritativos. No debería permitir consultas recursivas.  
    Los GTLDs son una de las categorías de TLDs que contienen dominios con propósitos particulares, de acuerdo a diferentes actividades. Se organizan en Unsponsored TLD, si las políticas están definidas por el ICANN, o en Sponsored TLD, si estas estuvieran definidas por otras organiaciones.

3. ¿Qué es una respuesta del tipo autoritativa?

    Una respuesta autoritativa es aquella dada por un servidor que tiene la autoridad sobre el dominio que se está buscando.

4. ¿Qué diferencia una consulta DNS recursiva de una iterativa?

    Una consulta DNS recursiva obliga a un servidor DNS para responder a una solicitud con un error o una respuesta de éxito. Los clientes DNS(resoluciones) normalmente realizan consultas recursivas. Con una consulta recursiva, el servidor DNS debe ponerse en contacto con otros servidores DNS que necesita para resolver la solicitud. Cuando reciba correcta de DNS(los otros servidores), a continuación, envía una respuesta al cliente DNS. La consulta recursiva es el tipo de consulta típica usado por una resolución de consultas a un servidor DNS y por un servidor DNS cuando consulta su reenviador, que es otro servidor DNS configurado para procesar solicitudes reenviadas a él.
    Cuando un servidor DNS procesa una consulta recursiva y la consulta no se puede resolver desde datos locales(cache de consultas anteriores o archivos de zona local), la consulta recursiva debe trasladarse a un servidor DNS raíz. Cada aplicación basada en estándares de DNS incluye un archivo de caché o sugerencias del servidor raíz que contiene entradas para los servidores DNS raíces de los dominios de inernet.
    Una consulta iterativa es una en la que se espera que el servidor DNS responda con la mejor información local que tiene, basado en lo que sabe el servidor DNS de los archivos de zona local o de la caché. Esta respuesta es tambíen conocida como una remisión, si el servidor DNS no está autorizado para el nombre. Si un servidor DNS no tiene ninguna información local que pueda responder la consulta, simplemente envía una respuesta negativa. Un servidor DNS realiza este tipo de consulta al intentar encontrar nombres fuera de su dominio local. Podría tener que consutar un número de servidores DNS externos en un intento de resolver el nombre.

5. ¿Qué es el resolver?

    Es una parte del sistema operativo que se encarga de realizar las consultas DNS, interpretarlas y devolverlas al programa que ha efectuado la consulta. Los servidores DNS también pueden incorporar un resolver que gestione las consultas que un servidor DNS debe hacer. Un resolver siempre suele hacer consultas recursivas exclusivamente.

6. Describa para qué se utilizan los siguientes tipos de registros de DNS:

    <table>
    <tr>
        <td>A</td>
        <td>NS</td>
    </tr>
    <tr>
        <td>MX</td>
        <td>CNAME</td>
    </tr>
    <tr>
        <td>PTR</td>
        <td>SOA</td>
    </tr>
    <tr>
        <td>AAAA</td>
        <td>TXT</td>
    </tr>
    <tr>
        <td>SRV</td>
    </tr>
    </table>

### Registro A

Un registro A o de direcciones(también denominado "registro de host") vincula un dominio con la dirección IP física de un ordenador en que se alojan los servicios de ese dominio.

### Registro MX

Los registros MX(del inglés "Mail Exchange", intercambio de correo) dirigen el correo electrónico de un dominio a los servidores que alojan las cuentas de usuario del dominio. Se pueden definir múltiples registros MX para un dominio, cada uno con una prioridad diferente. Si el correo no puede enviarse mediante el registro de prioridad más elevada, se utilizará el registro siguiente y así sucesivamente.

### Registro PTR

El registro PTR es el registro de recurso de un dominio que define las direcciones IP de todos los sistemas en una notación invertida(devuelve el nombre del dominio a partir de una determinada IP).

### Registro AAAA

Devuelve una dirección IPv6 de 128 bits, la más utilizada para asignar nombres de host a una dirección IP del host.

### Registro SRV

Es el registro en el que se especifica información sobre los servicios disponibles del dominio. A efectos prácticos, se requiere en ciertos protocolos donde se consulta la información sobre dónde ir para encontrar el servicio

### Registro NS

Los registros de Servidor de nombres(NS, del inglés "Name Server") determinan los servidores que comunicarán la información del DNS de un dominio. Por lo general, dispones de registros de servidor de nombres principales y secundarios para tu dominio.

### Registro CNAME

Un nombre canónico o registro CNAME enlaza un nombre de alias con otro nombre de dominio canónico o auténtico. Por ejemplo, www.example.com puede enlazar con example.com.

### Registro SOA

Start of Authority es un tipo de registro que especifica información del DNS. Todos los nombres de dominio tienen un registro SOA que muestra las características básicas del dominio y de la zona en la que se encuentra. Solo puede haber un SOA configurado por zona, y solo está presente si el servidor es autoritario del domio. Suele ser el primer registro de cada dominio en un servidor de nombres de dominio.

### Registro TXT

Un registro TXT es un registro DNS que proporciona información de texto a fuentes externas a tu dominio y que se puede utilizar con distintos fines. El valor de los registros TXT puede ser texto legible por máquinas o por personas.

8. Usando el comando dig, averigüe la dirección IP de www.google.com. Observe los números que aparecen antes de la palabra IN. Vuelva a ejecutar la misma consulta y observe nuevamente esos números ¿Qué ocurrió? ¿Por qué? ¿Qué significado cree que tienen dichos números?

    El número cambia con cada consulta realizada dado que representa el tiempo de vida(TTL) de la consulta, es decir, eventualmente, el número va a llegar a 0 y, cuando llegue, la consulta se eliminará de la cache, por lo que si se la vuelve a hacer, deberá realizarse la consulta completa y no podrá obtenerse de la cache. Si se pega siempre al mismo servidor, podrá apreciarse que el TTL baja de a uno, de lo contrario, tomará valores distintos dependiendo el TTL de cada servidor.

9. Observe nuevamente las respuestas del paso anterior ¿El orden de los servidores en la respuesta es siempre el mismo? ¿Por qué piensa que sucede esto?

    El orden no es el mismo por varias razones, la primera, que el TTL es distinto en cada servidor. Otra razón es por la falta de sincronización, es decir, los servidores no van todos a la par, funciona cada uno por su lado cada uno con sus valores.

10. Utilizando el comando dig responda (debe tener conexión a Internet para realizar este ejercicio):

    1. Cantidad de servidores que aceptan correos para el dominio gmail.com.
    