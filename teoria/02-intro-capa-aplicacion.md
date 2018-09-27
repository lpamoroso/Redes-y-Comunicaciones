# Capa de Aplicación

1. ¿Cuáles son sus objetivos?

* Aspectos conceptuales y de implementación de protocolos de aplicación
  + Modelos de capa de transporte
  + Paradigma cliente servidor
  + Paradigma P2P(peer to peer)

* Comprender protocolos de aplicación populares
  + HTTP
  + FTP
  + SMTP / POP3 / IMAP
  + DNS

* Programar aplicaciones de red
  + API de sockets

2. ¿Qué representa una aplicación de red?

Son programas que ejecutan en sistemas diferentes por lo que utilizan la red para comunicarse. Algunos ejemplos son el *e-mail*, juegos en red, la web en general, etc.
No necesitan escribir programas para dispositivos internos de la red(network-core devices) ya que los dispositivos internos no ejecutan aplicaciones de usuario y las aplicaciones en sistemas finales permiten su rápido desarrollo.

3.  ¿Cuáles son las arquitecturas de aplicaciones?

  + Cliente-servidor: en una arquitectura cliente-servidor, el servidor es un equipo de alta disponibilidad(siempre encendido) con una IP fija. Existen también granjas de servidores, lo que permite que la arquitectura sea escalable. El cliente, por su parte, es quien se comunica con el servidor a demanda, es decir, intermitentemente. Su IP es dinámica y solo se comunica con el servidor, nunca con otros clientes.

  + P2P(peer to peer): en una arquitectura P2P, el servidor es de disponibilidad variable(no siempre está encendido).Los pares se comunican directamente, de forma intermitente, con sistemas finales diversos y su IP puede ser dinámica. Es una arquitectura altamente escalable pero compleja de administrar.

  + Hibridas cliente-servidor/P2P: muchas aplicaciones combinan el potencial de las técnicas mencionadas antes creando aplicaciones que no son del todo cliente-servidor o P2P. Un ejemplo es la mensajería instantánea en la que las conversaciones entre usuarios se realiza de forma P2P, pero el usuario se registra con un servidor central y se conecta también con un servidor centralizado para encontrar contactos.

4. ¿Cómo se da la comunicación de procesos?

Existe dos tipos de procesos: el proceso cliente y el proceso servidor. Ámbos son programas ejecutándose en un equipo(host). El proceso cliente es quien inicia la comunicación, mientras que el proceso servidor es quien espera la comunicación de un proceso cliente. En un mismo equipo, los procesos usan comunicación inter-procesos(definida por el sistema operativo). En diferentes equipos, los procesos usan intercambio de mensajes.
Para comunicarse, los procesos usan **sockets**. Un socket es algo así como una puerta de comunicación. Los procesos envían/reciben mensajes a través del socket. El proceso que envía deja mensajes en la puerta y confía en en una infraestructura del otro lado de la puerta que se encarga de manejar y dejar el mensaje en el socket del proceso receptor. El programador puede elegir el método de transporte y fijar parámetros para tal método.
Si un proceso desea recibir mensajes debe tener un identificador. El *host* tiene una uńica dirección IP de 32 bits, pero dado que suelen haber múltiples procesos corriendo sobre una misma máquina, ésta dirección no es suficiente: para suplir ese inconveniente es que hay números de puertos asociados a cada proceso.

5. ¿Qué es el protocolo de capa de aplicación?

Es aquel que define el tipo de mensajes intercambiados(por ejemplo, el request, response, etc), la sintáxis de los mensajes(esto es, qué campos o parámetros y cómo son enviados), la semántica de los mensajes(es decir, qué significa la información en los campos) y reglas para cómo y cuándo un proceso debe enviar y otro responder a los mensajes
Los hay de dominio público y privado(Ej: Skype). Aquellos de dominio público están definidos en los RFC(*request for comments*) y permiten interoperabilidad entre procesos de diferentes máquinas. Ej: HTTP y SMTP.

6. Servicio de transporte

Definen ciertas características como:
* Pérdida de datos
  - Se pueden tolerar pérdidas(ej: audio)
  - No se pueden tolerar pérdidas(ej: transferencia de archivos)

* Tiempo: algunas aplicaciones requieren que no haya retardos(delay) en las transferencias(ej: VoIP)

* Tasa de Transferencia Efectiva(*throughput*): algunas aplicaciones requieren una gran tasa de transferencia efectiva de datos(ej: video).

* Seguridad
  - Encriptación de los datos
  - Integridad de los datos

De acuerdo a éstas características es que hay dos tipos de servicios de transporte por excelencia:

* Servicios TCP: es orientado a conexión, es decir, hay un establecimiento previo entre los procesos cliente y servidor.
  + Transporte confiable: los datos llegan en forma correcta.
  + Control de flujo: el proceso no envía más de lo que puede aceptar el receptor.
  + Control de congestión: maneja el envío cuando la red esta sobrecargada.
  + No provee
    - Control de retardo
    - Asegura o garantiza una mínima tasa de transferencia
    - Seguridad

* Servicios UDP
  + Transferencia de datos no confiable
  + No provee:
    - Establecimiento previo de conexión
    - Confiabilidad
    - Control de flujo
    - Control de congestión
    - Control de retardo
    - Garantía de tasa de transferencia
    - Seguridad
    
* ¿Por qué proveer servicios UDP cuando podría usar TCP en todos los casos? Si bien TCP tiene muchas ventajas, debe asegurar que no se pierdan datos en el trayecto y acomodarse a lo que el usuario pueda manejar. Esto lo hace lento y complejo. El uso principal de UDP, por lo tanto, es para protocolos en los que no sea rentable usar TCP por la lentitud y complejidad que este pudiera generar. Un ejemplo es el streaming de videos o audio: es tolerable que se pierdan algunos paquetes dado que no afectará mucho al producto final; sin embargo, no es deseable que llegue una parte del correo electrónico y otra no porque se pierda en el trayecto.
