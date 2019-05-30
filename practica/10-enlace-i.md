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