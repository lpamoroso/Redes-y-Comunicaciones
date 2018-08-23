# Introducción

1. ¿Qué es una Red?

Es un conjunto de dispositivos interconectados por cable o de forma inalámbrica con el objetivo de compartir recursos, ya sea otros dispositivos, información, servicios, etc. El conjunto de computadoras, software de red, medios y dispositivos de interconexión forma un sistema de comunicación. Ejemplos: red de la facultad, Internet, etc.

2. ¿Cuáles son las partes de un sistema de comunicación?

* **Fuente(Software)**: un programa, como podría ser un navegador web, que se va a querer conectar a algo.
* **Emisor/Transmisor(Hardware)**: es quien dirije la comunicación, un dispositivo(podría ser la placa de red de la computadora, por ejemplo) que va a permitir que la información salga de la computadora.
* **Medio de transmisión y dispositivos intermedios(Hardware)**: pueden ser cables o medios inalámbricos(medios de transmisión), routers o switchers(dispositivos intermedios). Son dispositivos que forman parte de la red que van a permitir la comunicación con el otro sector y viceversa.
* **Procesos intermedios que tratan la información(Software y Hardware)**.
* **Receptor(Hardware)**: un dispositivo(como podría ser una computadora o una impresora) en la que habrá un programa(destino) que entenderá qué es lo que el emisor le está pidiendo e intentará resolverlo. Si bien el receptor es una pieza de hardware, es necesario que el destino sea software ya que éste debe entender lo que el emisor requiere.
* Otros:
  + **Protocolos(Software)**: el lenguaje en común que hablan el emisor y el receptor.
  + Información: lo que el emisor pide al receptor y aquello que el receptor envía al emisor. Hay diversas formas de enviarla: por cable de cobre, por fibra óptica, etc.
  + mensaje transmitido(Software). 
* Señal de Información, materialización del mensaje sobre el medio(Hardware?).

3. ¿Qué es un protocolo?

Es un **conjunto de conductas y normas a conocer, respetar y cumplir** no sólo en el medio oﬁcial ya establecido, sino también en el medio social, laboral, etc. Deﬁne el formato, el orden de los mensajes intercambiados y las acciones que se llevan a cabo en la transmisión y/o recepción de un mensaje u otro evento.  
Un **protocolo de Red**, de forma análoga, es un conjunto de reglas que especiﬁcan el intercambio de datos u órdenes(mensajes de control) durante la comunicación entre las entidades que forman parte de una red. Permiten la comunicación y, por lo general, ya están implementados y estandarizados en las componentes. Representa un lenguaje que ambas partes deben conocer para poder entenderse.

4. ¿Cuál es el problema de los protocolos?

El problema de los protocolos es que existían muchos y traían consigo muchas incompatibilidades entre ellos dado que eran privativos, complicados, evolucionaban poco y carecían de estándares.
Dado que en internet no existe un solo protocolo(son muchos protocolos que interactúan entre ellos), fue(y es) necesario un modelo de organización entre los componentes de red. Estos modelos dividen la complejidad en capas(interfaces reusables) que permite:
* Reducir la complejidad en componentes más pequeños.
* Ocultar la complejidad de las capas de abajo a las de arriba(abstracción).
* Que las capas de arriba utilicen servicios de las de abajo(algo asi como una API).
* Que los cambios en una capa no deberían afectar a las demás si la interfaz se mantiene.
* Facilitar el desarrollo y evolución de los componentes de red asegurando interoperabilidad.
* Facilitar aprendizaje, diseño y administración de las redes.

Fue así como surgió OSI(Open System Interconnection), un modelo estándar y abierto creado por la ISO con el fin de desarrollar componentes de red también estándares. Está basado en DECNET, SNA y TCP/IP. Es el modelo que se usa de referencia para crear otros modelos y consta de dos tipos de etapas:
* **Host layers**(7,6,5,4): proveen envío de datos de forma conﬁable.
* **Media layers**(1,2,3): controlan el envío físico de los mensajes sobre la red.
