# Práctica 10 Capa de Enlace - Parte I

1. ¿Qué función cumple la capa de enlace? Indique qué servicios presta esta capa.

    Es la responsable de la transferencia fiable de información a través de un circuito de transmisión de datos. Recibe peticiones de la capa de red y utiliza los servicios de la capa física.  
    El objetivo de la capa de enlace es conseguir que la información fluya, libre de errores, entre dos máquinas que estén conectadas directamente(servicio orientado a la conexión). Para lograr tal objetivo es que monta bloques de información(llamados tramas), dotarles de una dirección de capa de enlace(dirección MAC), gestionar la detección o corrección de errores, y ocuparse del "control de flujo" entre equipos(para evitar que un equipo más rápido desborde a uno más lento).  
    Los servicios que provee son:
    * Encapsulación de los paquetes de la capa de red en tramas.
    * Sincronización de marcos.
    * Control de errores, sumado al ARQ proporcionado por los protocolos de la capa de transporte, a técnicas de corrección de errores hacia adelante proporcionado en la capa física, y detección de errores y cancelación de paquetes proporcionado por todas las capas, incluyendo la capa de red.
    * Control de flujo.
    * Protocolos de acceso múltiple para control de acceso a canales.
    * MAC Addressing.
    * LAN Switching.
    * Encolamiento de paquetes de datos o planificador.
    * Técnicas de almacenamiento y reenvío y conmutación virtual cut-through.
    * Control de calidad de servicio.
    * Virtual LANs.

2. Compare los servicios de la capa de enlace con los de la capa de transporte.

3. Direccionamiento Ethernet:

    + ¿Cómo se identiﬁcan dos máquinas en una red Ethernet?

        Mediante las direcciones MAC.

    + ¿Cómo se llaman y qué características poseen estas direcciones?

        Son 48 bits de espacio. Cada uno de los tres sistemas numéricos usan el mismo formato y difieren tan solo en el tamaño del identificador. Las direcciones pueden ser direcciones "localmente administradas" o "universalmente administradas". Una dirección universalmente administrada es únicamente asignada a un dispositivo por su fabricante. Los tres primeros octetos identifican a la organización que publicó el identificador y son conocidas como "identificador de organización único". Con esto podemos determinar, como si fuera una huella digital, desde que dispositivo de red se emitió el paquete de datos aunque este cambie de dirección IP, ya que este código se ha acordado por cada fabricante de dispositivos.

    + ¿Cuál es la dirección de broadcast en capa de enlace? ¿Qué función cumple?

        La dirección de broadcast es FF:FF:FF:FF:FF:FF. Su funci´no es hacer que los marcos que sean dirigidos a ella alcancen todas las computadores de una LAN determinada. Los marcos de ethernet que contengan paquetes IP de broadcastson usualmente enviados a esta dirección. La dirección de broadcast es usada por el protocolo de resolución de direcciones(ARP) y el de neighbour discovery para traducir direcciones IP en direcciones MAC.

4. Sobre los dispositivos de capa de enlace:

    + Enumere dispositivos de capa de enlace y explique sus diferencias.

        - Bridge(Puente de red): es un dispositivo de interconexión de redes de ordenadores que opera en la capa de enlace. Un bridge conecta segmentos de red formando una sola subred(permite conexión entre equipos sin necesidad de routers). Funciona a través de una tabla de direcciones MAC detectadas en cada segmento al que está conectado.
        - Switch(Conmutador): es un dispositivo digital lógico de interconexión de redes de computadoras que opera en la capa de enlace. Su función es interconectar dos o más segmentos de red, pasando datos de un segmento a otro de acuerdo con la dirección MAC de destino de las tramas en la red.

    + ¿Qué es una colisión?

        Cuando los dispositivos intentan usar un medio simultáneamente podrían ocurrir colisión de marcos. Los protocolos de capa de enlace especifican cómo los dispositivos detectan y recuperan ante tales colisiones, y podrían proveer mecanismos que los reducen o los previenen. 

    + ¿Qué dispositivos dividen dominios de broadcast?

        Los routers.

    + ¿Qué dispositivos dividen dominios de colisión?

        Los routers y los switches.

5. Describa el algoritmo de acceso al medio en Ethernet ¿Es orientado a la conexión?

    Ethernet transmite solo mientras otro no esté transmitiendo. Puede darse que justo dos al mismo tiempo transmitan, en este caso se produce una colisión y se resuelve mediante un algoritmo que hace que los colisionantes reenvíen el paquete en tiempos distintos. Además, no es un protocolo orientado a conexión dado que no hay handshaking entre las NICs de emisor y receptor.