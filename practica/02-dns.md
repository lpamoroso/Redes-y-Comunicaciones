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