# DNS

1. Investigue y describa cómo funciona el DNS ¿Cuál es su objetivo?

    DNS es un sistema de nomenclatura jerárquico y descentralizado que sirve para traducir los dominios en IPs.  
    El resolver es quien determina los servidores en lo que se puede realizar la consulta por el nombre del dominio en cuestión mediante una secuencia de consultas empezando por aquella etiqueta que está más a la derecha.  
    Asumiendo que el resolver no tienen cacheado registros para acelerar el proceso, el proceso de resolución empieza con una consulta a uno de los servidores raíces. Usualmente, los servidores raíces no responden directamente, sino que responden con referencias a otro servidores más autoritativos, por ejemplo, si se busca *test.com*, se refiere a los servidores de *com*. El resolver ahora consultará a los servidores que le fueron referidos, y así, iterativamente, repetirá el proceso hasta que llegue a una respuesta autoritativa.  
    A priori, este mecanismo produciría un tráfico muy alto en la carga de los servidores raíces, si cada resolución de internet requiriera empezar por los servidores raiz. En la práctica, el cacheado de servidores DNS es esencial para liberar de esa carga a los servidores raíces y, consecuentemente, los servidores raíces se ven envueltos en una fracción de pedidos relativamente pequeña.

2. ¿Qué es un root server? ¿Qué es un *generic top-level domains* (GTLDs)?

    Un root server o servidor raíz es un servidor que delega todos los TLD a servidores más autoritativos. No debería permitir consultas recursivas.  
    Los GTLDs son una de las categorías de TLDs que contienen dominios con propósitos particulares, de acuerdo a diferentes actividades. Se organizan en Unsponsored TLD, si las políticas están definidas por el ICANN, o en Sponsored TLD, si estas estuvieran definidas por otras organiaciones.