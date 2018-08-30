# Intro

1. ¿Qué es una red? ¿Cuál es el principal objetivo para construir una red?

Es un conjunto de dispositivos interconectados por cable o de forma inalámbrica con el objetivo de compartir recursos, ya sea otros dispositivos, información, servicios, etc. El conjunto de computadoras, software de red, medios y dispositivos de interconexión forma un sistema de comunicación. Ejemplos: red de la facultad, Internet, etc.

2. ¿Qué es Internet? Describa los principales componentes que permiten su funcionamiento.

Es una red de redes de computadoras, descentralizada, pública, que ejecutan el conjunto abierto de protocolos(suite) TCP/IP. Integra diferentes protocolos de un nivel más bajo.

3. ¿Qué son las RFCs?

Los RFC(Request for Comments) son una serie de publicaciones del grupo de trabajo de ingeniería de internet que describen diversos aspectos del funcionamiento de Internet y otras redes de computadoras, como protocolos, procedimientos, etc. y comentarios e ideas sobre estos. Cada RFC constituye un monográfico o memorandum que ingenieros o expertos en la materia han hecho llegar al IETF(Internet Engineering Task Force) para que éste sea valorado por el resto de la comunidad.

4. ¿Qué es un protocolo?

Es un conjunto de conductas y normas a conocer, respetar y cumplir no sólo en el medio oﬁcial ya establecido, sino también en el medio social, laboral, etc. Deﬁne el formato, el orden de los mensajes intercambiados y las acciones que se llevan a cabo en la transmisión y/o recepción de un mensaje u otro evento.  
Un protocolo de Red, de forma análoga, es un conjunto de reglas que especiﬁcan el intercambio de datos u órdenes(mensajes de control) durante la comunicación entre las entidades que forman parte de una red. Permiten la comunicación y, por lo general, ya están implementados y estandarizados en las componentes. Representa un lenguaje que ambas partes deben conocer para poder entenderse.

5. ¿Por qué dos máquinas con distintos sistemas operativos pueden formar parte de una misma red?

Porque ámbas entienden el mismo protocolo estándar. En efecto, si bien esto puede parecer un detalle o algo natural, es una característica reciente de las redes: no hace muchos años existían muchos y traían consigo muchas incompatibilidades entre ellos dado que eran privativos, complicados, evolucionaban poco y carecían de estándares.

6. ¿Cuáles son las 2 categorías en las que pueden clasiﬁcarse a los sistemas ﬁnales o End Systems? Dé un ejemplo del rol de cada uno en alguna aplicación distribuida que corra sobre Internet.

Se puede clasificar en cliente y servidor. Depende el protocolo unos podrán ser clientes, otros servidores o ámbos podrán ser cualquiera de las dos categorías. Un ejemplo: desde una notebook X accedo a Google. En este ejemplo, la notebook representa el cliente y Google representa el servidor.

7. ¿Cuál es la diferencia entre una red conmutada de paquetes de una red conmutada de circuitos?

En la red conmutada por circuitos, para establecer comunicación se debe efectuar una llamada y, cuando se establece la conexión, los usuarios disponen de un enlace directo a través de los distintos segmentos de la red.  
En la red conmutada por paquetes, la información/datos a transmitir es previamente ensamblada en paquetes. Cada paquete es entonces transmitido individualmente y éste puede seguir diferentes rutas hacia su destino. Una vez que los paquetes hayan llegado a su destino, éstos son otra vez reensamblados.
La principal diferencia está en que la conmutación por circuitos es un tipo de comunicación que establece o crea un canal dedicado(o circuito) durante la duración de una sesión. Este sistema es ideal para comunicaciones que requieren que los datos/información sean transmitidos en tiempo real. La conmutación de paquetes, por su parte, es más eficiente y robusto para datos que pueden ser enviados con retardo en la transmisión(no en tiempo real), tales como el correo electrónico, páginas web, archivos, etc.



8. Analice qué tipo de red es una red de telefonía y qué tipo de red es Internet.
9. Describa brevemente las distintas alternativas que conoce para acceder a Internet en su hogar.
10. ¿Qué ventajas tiene una implementación basada en capas o niveles?
11. ¿Cómo se llama la PDU de cada una de las siguientes capas: Aplicación, Transporte, Red y Enlace?
12. ¿Qué es la encapsulación? Si una capa realiza la encapsulación de datos, ¿qué capa del nodo receptor realizará el proceso inverso?
13. Describa cuáles son las funciones de cada una de las capas del stack TCP/IP o protocolo de Internet.
14. Compare el modelo OSI con la implementación TCP/IP.
