# Capa de transporte

### 1. ¿Qué es la capa de tranporte?

La capa de transporte es el cuarto nivel del modelo OSI encargada de la transferencia libre de errores de los datos entre el emisor y el receptor, aunque no estén directamente conectados, así como de mantener el flujo de la red. Es la base de toda la jerarquía de protocolo. La tarea de esta capa es proporcionar un transporte de datos confiable y económico de la máquina de origen a la máquina destino, independientemente de las de redes físicas en uno.

### 2. ¿Qué servicios proporciona esta capa a las capas superiores?

La meta final de la capa de transporte es proporcionar un servicio eficiente, confiable y económico a sus usuarios, que normalmente son procesos de la capa de aplicación. Para lograr este objetivo, la capa de transporte utiliza los servicios proporcionados por la capa de red. El hardware o software de la capa de transporte que se encarga del transporte se llama entidad de transporte, la cual puede estar en el núcleo del sistema operativo, en un proceso independiente, en un paquete de biblioteca o en la tarjeta de red.

Hay dos tipos de servicio en la capa de transporte: orientado y no orientado a la conexión. El servicio orientado a la conexión consta de tres partes: establecimiento, transferencia de datos y liberación. En el servicio no orientado a la conexión se tratan los paquetes de forma individual.

### 3. ¿Cuáles son los elementos de los protocolos de transporte?

##### Direccionamiento

Cuando un proceso desea establecer una conexión con un computador de aplicación remoto, debe especificar a cuál se conectará (¿A quién le llegará el mensaje?). El método que normalmente se emplea es definir direcciones de transporte en las que los procesos pueden estar a la escucha de solicitudes de conexiones. En Internet, estos puntos terminales se denominan puertos, pero usaremos el término genérico de TSAP (*Transport Service Access Point*). Los puntos terminales análogos de la capa de red se llaman NSAP (*Network Service Access Point*). Las direcciones IP son ejemplos de NSAPs.

##### Establecimiento de una conexión

El establecimiento de una conexión parece fácil, pero en realidad es sorprendentemente difícil. A primera vista, parecería que es suficiente con mandar un TPDU(*Transport Protocol Data Unit*) con la petición de conexión y esperar a que el otro acepte la conexión. El problema viene cuando la red puede perder, almacenar, o duplicar paquetes. El principal problema es la existencia de duplicados retrasados. Esto puede solucionarse de varias maneras(ninguna es muy satisfactoria). Una es utilizar direcciones de transporte desechables. En este enfoque cada vez que necesitemos una dirección la creamos. Al liberarse la conexión descartamos la dirección y no se vuelve a utilizar. También podemos asignar una secuencia dentro de los datos transmitidos, pero estos plantean el problema de que si se pierde la conexión perdemos el orden del identificador y ya no funciona. La solución seria más fácil si los paquetes viejos se eliminaran de la subred cada cierto tiempo de vida. Para ello podemos utilizar las siguientes técnicas: un diseño de subred restringido. Colocar un contador de saltos en cada paquete. Marcar el tiempo de cada paquete. Pero en la práctica no vale solo con hacer esto sino que tenemos que garantizar que todas las confirmaciones de los paquetes también se eliminan.

##### Liberación de una conexión

La liberación de una conexión es más fácil que su establecimiento. No obstante, hay más escollos de los que uno podría imaginar. Hay dos estilos de terminación de una conexión: liberación asimétrica y liberación simétrica. La liberación asimétrica es la manera en que funciona el mecanismo telefónico: cuando una parte cuelga, se interrumpe la conexión. La liberación simétrica trata la conexión como dos conexiones unidireccionales distintas, y requiere que cada una se libere por separado. La liberación asimétrica es abrupta y puede resultar en la pérdida de datos, por lo que es obvio que se requiere un protocolo de liberación más refinado para evitar tal pérdida. Una posibilidad es usar la liberación simétrica, en la que cada dirección se libera independientemente de la otra. Aquí, un host puede continuar recibiendo datos aun tras haber enviado un TPDU de desconexión.

La liberación simétrica es ideal cuando un proceso tiene una cantidad fija de datos por enviar y sabe con certidumbre cuándo los ha enviado. En otras situaciones, la determinación de si se ha efectuado o no todo el trabajo y si se debe terminarse o no la conexión no es tan obvia. Podríamos pensar en un protocolo en el que el host 1 diga: "Ya terminé, ¿Terminaste también?". Si el host 2 responde "Ya terminé también. Adiós", la conexión puede liberarse con seguridad.

Pero no es tan fiable por el problema de que siempre tendremos que esperar la confirmación de los mensajes recibidos y si esta confirmación no llega, la conexión no es liberada y después puede que sea necesaria la confirmación de que llegó la confirmación anterior(valga la redundancia) y entraríamos en un bucle del que no podríamos salir.

Podríamos hacer que si al host 1 no le llegara la confirmación después de N intentos es que quiere la desconexión, y, por lo tanto, se libere. Esto produciría una conexión semiabierta en la que el host 1 estaría desconectado, pero el host 2 no dado que, como nunca le hubiera llegado la confirmación, no se hubiera desconectado nunca. Para solucionar esto, creamos una regla por la cual si al host 2 no le llega ningún TPDU durante cierta cantidad de segundos, se libera automáticamente.

##### Control de Flujo y almacenamiento en buffer

Respecto de la manera en que se manejan las conexiones mientras están en uso, uno de los aspectos clave es el control de flujo. Se necesita un esquema para evitar que un emisor rápido desborde a un receptor lento. La diferencia principal es que un enrutador por lo regular tiene relativamente pocas líneas, y un host puede tener numerosas conexiones. Esta diferencia hace poco práctico emplear la implementación que se hace en la capa de enlace.

En esta capa lo que se hace es que si el servicio de red no es confiable, el emisor debe almacenar en un buffer todas los TPDUs enviados, igual que en la capa enlace de datos. Sin embargo, con un servicio de red confiable son posibles otros arreglos. En particular, si el emisor sabe que el receptor siempre tiene espacio de buffer, no necesita tener copias de los TPDUs que envía. Sin embargo, si el receptor no garantiza que se aceptará cada TPDU que llegue, el emisor tendrá que usar buffers de todas maneras. En el último caso, el emisor no puede confiar en la confirmación de recepción de la capa de red porque esto sólo significa que ha llegado el TPDU, no que ha sido aceptado.

Los Buffers pueden ser de tres tipos, y usaremos cada uno de ellos cuando más nos convenga.

El equilibrio óptimo entre el almacenamiento del buffer en el origen y en el destino depende del tipo de tráfico transportado por la conexión.

##### Multiplexión

La multiplexión de varias conversaciones en conexiones, circuitos virtuales o enlaces físicos desempeña un papel importante en diferentes capas de la arquitectura de red. En la capa de transporte puede surgir la necesidad de multiplexión por varias razones. Por ejemplo, si en un host sólo se dispone de una dirección de red, todas las conexiones de transporte de esa máquina tendrán que utilizarla. Cuando llega un TPDU, se necesita algún mecanismo para saber a cuál proceso asignarla. Esta situación se conoce como multiplexión hacia arriba.

La multiplexión también puede ser útil en la capa de transporte para la utilización de circuitos virtuales, que dan más ancho de banda cuando se reasigna a cada circuito una tasa máxima de datos. La solución es abrir múltiples conexiones de red y distribuir el tráfico entre ellas. Esto se denomina multiplexión hacia abajo.

##### Recuperación de caídas

Si los hosts y los enrutadores están sujetos a caídas, la recuperación es fundamental. Si la entidad de transporte está por entero dentro de los hosts, la recuperación de caídas de red y de enrutadores es sencilla. Si la capa de red proporciona servicio de datagramas, las entidades de transporte esperan pérdida de algunos TPDUs todo el tiempo, y saben cómo manejarla. Si la capa de red proporciona servicio orientado a la conexión, entonces la pérdida de un circuito virtual se maneja estableciendo otro nuevo y sondeando la entidad de transporte remota para saber cuales TPDUs ha recibido y cuales no.

Un problema más complicado es la manera de recuperarse de caídas del host. Al reactivarse, sus tablas están en el estado inicial y no sabe con precisión donde estaba.

En un intento por recuperar su estado previo, el servidor podría enviar un TPDU de difusión a todos los demás host, anunciando que se acaba de caer y solicitando a todos sus clientes que le informen el estado de todas la conexiones abiertas.

### 4. ¿Cuáles son los protocolos de transporte de internet?

Internet tiene dos protocolos principales en la capa de transporte, uno orientado a la conexión y otro no orientado a la conexión. El protocolo no orientado a la conexión es el **UDP** y el orientado es el **TCP**.

##### UDP

El conjunto de protocolos de Internet soporta un protocolo de transporte no orientado a la conexión denominado UDP (*user datagram protocol*). Este protocolo proporciona una forma para que las aplicaciones envíen datagramas IP encapsulados sin tener una conexión.

##### TCP

TCP (*transmission control protocol*) se diseñó específicamente para proporcionar un flujo de bytes confiable de extremo a extremo a través de una interred no confiable. Una interred difiere de una sola red debido a que diversas partes podrían tener diferentes topologías, anchos de banda, retardos, tamaños de paquete... TCP tiene un diseño que se adapta de manera dinámica a las propiedades de la interred y que se sobrepone a muchos tipos de situaciones.
