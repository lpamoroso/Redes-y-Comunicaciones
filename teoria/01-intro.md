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

Fue así como surgió OSI(Open System Interconnection), un modelo estándar y abierto creado por la ISO con el fin de desarrollar componentes de red también estándares.

5. ¿Qué es OSI?

Es un modelo estándar y abierto basado en DECNET, SNA y TCP/IP. Es el modelo que se usa de referencia para crear otros modelos y consta de siete capas:
* **Aplicación(7)**: servicios de red a los usuarios y a procesos, aplicaciones. Sin esta capa formar redes no tendría mucho sentido. Es el punto fuerte de la red.
* **Presentación/Representación(6)**: va a definir cuál es el formato que se le da a los datos que se transmitirán a los *hosts*. Si los datos no tienen un formato establecido, la comunicación no se va a poder llevar a cabo.
* **Sesión(5)**: mantiene el track(estado) de sesiones de la aplicación, es decir, quién se comunica con quién.
* **Transporte(4)**: establece y mantiene canal “seguro” end-to-end (application-to-application). Maneja la comunicación de equipo a equipo entre el emisor y el receptor. Es un nivel de proceso.
* **Red(3)**: permite, a traves del direccionamiento lógico, interconectar computadoras. Comunica varias redes.
* **Enlace de Datos(2)**: comunica entes directamente conectados entre ellos. Comunica una misma red. Acceso al Medio.
* **Física(1)**: transporta la información como señal por el medio físico(fibra óptica, aire, cable, etc). Características físicas. La información es binaria, digital.

A su vez podemos clasificar las capas en:

* **Host layers(7,6,5,4)**: proveen envío de datos de forma conﬁable.
* **Media layers(1,2,3)**: controlan el envío físico de los mensajes sobre la red.

6. ¿Qué es el modelo TCP/IP?

Es un modelo abierto que se convirtió en estándar. Viene implementado en todos los dispositivos. Su API es abierta por lo que permite generar nuevos protocolos.  
Es un modelo de cinco capas:
* Capa de Aplicación (Process/Application).
* Capa de Transporte o Host-to-Host.
* Capa de Internet o Internetworking: maneja el direccionamiento ip y el comportamiento.
* Capa de Enlace(Link Layer).
* Capa de Física.
Por simplicidad, algunos autores hablan de 4 capas, agrupando a la Capa de Enlace y Capa física en una sola capa que llaman *capa de acceso a la Red*

7. OSI vs. TCP/IP

***Similitudes:***
* Se dividen en capas.
* Tienen capas de aplicación, aunque incluyen servicios distintos.
* Tienen capas de transporte similares.
* Tienen capa de red similar pero con distinto nombre.
* Se supone que la tecnología es de conmutación de paquetes(no de conmutación de circuitos).
* Es importante conocer ambos modelos.

***Diferencias:***
* TCP/IP combina las funciones de la capa de presentación y de sesión en la capa de aplicación.
* TCP/IP combina la capas de enlace de datos y la capa física del modelo OSI en una sola capa.
* TCP/IP más simple porque tiene menos capas.
* Los protocolos TCP/IP son los estándares en torno a los cuales se desarrolló Internet, de modo que la credibilidad del modelo TCP/IP se debe en gran parte a sus protocolos.
* El modelo OSI es un modelo más de referencia, teórico, aunque hay implementaciones.

8. ¿Qué es el Protocol Data Unit?

Es una unidad que define cada capa. La idea es que los datos de las capas superiores vayan encapsulándose para que puedan ser transmitidos por la capa física.

9. ¿Cómo se da la comunicación entre capas?

La idea es que cada capa usa el servicio de la de abajo y que cada capa se comunica con la capa del otro extremo. Lo que ocurre es que hay dispositivos en el medio de la comunicación que no van a comprender toda la información, es decir, un router va a entender hasta la capa tres y no va a entender si estamos hablando de http o tcp o dns. No va a brindar servicios de capas superiores.

10. ¿Cómo se clasifican las redes?

Existen diferentes clasiﬁcaciones de acuerdo a diferentes aspectos. Se pueden mencionar: 
* Por cobertura.
* Por acceso.
* Por topología física.
* Por tipo de conexión/medio. 
* Etc.

Según cobertura:
* LAN: (Local Area Network). Red de cobertura local. Ethernet, Wi-Fi.
* MAN: (Metropolitan Area Network). red de cobertura metropolitana, dentro de una ciudad. MetroEthernet, MPLS, Wi-Max.
* WAN: (Wide Area Network). red de cobertura de área amplia. Geográﬁcamente distribuida. PPP, Frame-Relay, MPLS, HDLC, SONET/SDH.
* SAN: (Storage Area Network). red de almacenamiento. iSCSI, Fibre Channel, ESCON.
* PAN: red de cobertura personal. Red con alcance de escasos metros para conectar dispositivos cercanos a un individuo. Bluetooth, IrDA, USB.

Según acceso:
* Internet: es una red pública global que utiliza la tecnología TCP/IP.
* Intranet: es una red privada que utiliza la tecnología de Internet.
* Extranet: es una red privada virtualizada sobre enlaces WAN: Internet. Es una intranet con acceso de usuarios remotos. VPN (Virtual Private Network). IPSec, PPTP, SSL, OpenVPN. Una intranet mapeada sobre una red pública como Internet.

Según topología física:
* Redes de Conmutación de Circuitos.
* Redes de Conmutación de Tramas/Paquetes. 
  + Servicios Orientados a Conexión. Circuitos Virtuales. Acá se establece el camino que va a seguir el paquete.
  + Servicios NO Orientados a Conexión. Datagramas. Acá NO se establece el camino que va a seguir el paquete.
  
10. ¿Qué es internet?

Es una red de redes de computadoras, descentralizada, pública, que ejecutan el conjunto abierto de protocolos(suite) TCP/IP. Integra diferentes protocolos de un nivel más bajo.

11. ¿Cómo se estructura internet?

Es una estructura jerárquica, en forma de niveles:
* Capa de acceso: contempla cómo se entra a internet.
* Capa de núcleo: consta de dispositivos de red de alta velocidad fundamentales para la interconectividad entre los aparatos de la capa de acceso. 

12. ¿Qué son los RFC?

Los RFC(*request for comments*) son una serie de publicaciones del grupo de trabajo de ingeniería de internet que describen diversos aspectos del funcionamiento de Internet y otras redes de computadoras, como protocolos, procedimientos, etc. y comentarios e ideas sobre estos. Cada RFC constituye un monográfico o memorando que ingenieros o expertos en la materia han hecho llegar al IETF, el consorcio de colaboración técnica más importante en Internet, para que éste sea valorado por el resto de la comunidad.

