# # Capa de Transporte - Parte II

1. Utilizando la máquina virtual, use Wireshark para capturar paquetes enviados y recibidos en cada uno de los siguientes casos. Para ello, arranque la captura en la interfaz con IP 172.28.0.1 antes de realizar los incisos A, B, C y D.

    1. Abra un navegador e ingrese a la URL: www.redes.unlp.edu.ar. Analice la secuencia de segmentos TCP que permiten la apertura del canal de comunicación por el cual posteriormente viajarán los mensajes HTTP intercambiados ¿Con qué nombre se conoce a dicha secuencia? ¿Qué flags se utilizan en cada uno de los segmentos intercambiados? ¿Qué indica cada uno de estos flags?

        El nombre de esa secuencia se conoce como _3-way-handshake_. La IP fuente envía un SYN a la IP destino para iniciar la conexión. La IP destino, luego responde con un SYN-ACK, indicando que se ha abierto ese extremo y que desea abrir el extremo restante. La IP fuente reponde con un ACK para indicar que se ha abierto el extremo que quedaba.

    2. Cierre el navegador. Analice la secuencia de segmentos TCP que ocurren al hacerlo ¿Cuál es el objetivo de éstos? ¿Qué flags se utilizan en cada uno de dichos segmentos? ¿Qué indica cada uno de estos flags?

        Cuando cierro el navegador, se cierra la conexión TCP con los flags FIN y ACK. La secuencia para cerrar la conexión es muy similar a la correspondiente para abrirla:
        * La IP fuente envía un FIN a la IP destino para iniciar el cierre.
        * La IP destino envía un FIN-ACK a la IP fuente, indicando que ha cerrado ese extremo y que desea cerrar el extremo restante.
        * La IP fuente responde con con ACK indicando que ha cerrado el extremo que faltaba.

    3. Para este ejercicio debe usar tanto el navegador Chromium como Iceweasel. Utilice Chromium para ingresar a la URL: www.redes.unlp.edu.ar y seguidamente utilice Iceweasel para ingresar nuevamente a la URL: www.redes.unlp.edu.ar

        1. Observe la información de Puerto Origen y Puerto destino de cada una de las comunicaciones. En base a lo observado, responda ¿Es posible conectarse 2 veces en forma simultánea al mismo lugar? ¿Qué distingue una conexión de otra? Capture el tráfico de red si considera necesario para observar dicha información.

            Si, se puede. Como se puede observar en las imágenes, lo que distingue a las conexiones es el puerto de origen.

        2. Identifique lo observado en el punto anterior utilizando el comando ss.

            Como se puede observar en la captura, hay dos procesos corriendo simultáneamente: el iceweasel y el chromium. Ámbos acceden al mismo lugar. Lo que hace posible que ámbos puedan acceder al mismo lugar simultáneamente son los puertos, específicamente el puerto de origen.