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