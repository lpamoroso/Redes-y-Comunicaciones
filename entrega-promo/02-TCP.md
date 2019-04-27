<center><font size="6"><u>Grupo A</u></font></center>
<center><font size="5"><u>Práctica/Laboratorio de TCP</u></font></center>
<center><font size="4"> Amoroso, Lihuel Pablo 13497/2; Gasquez, Federico Ramón 13598/6</font></center>

Para la captura dada analizar con el siguiente cuestionario utilizando una herramienta como wireshark.

1. Cuántos intentos de conexiones TCP hay en la captura?

    Hay un intento exitoso.

2. Cuales son la fuente y el destino (IP:port) para c/u?

    Para el único intento exitoso, la fuente es 10.0.1.10 y el destino es 10.0.3.10.

3. Cuántas conexiones TCP exitosas hay en la captura? Cómo se identiﬁcan las exitosas de las no exitosas, que ﬂags se encuentran en estas?

    Hay una conexión exitosa. Una conexión exitosa se diferencia de otra que no lo es en cómo se desarrolla la conexión. Para establecer una conexión exitosa en TCP son necesarios tres pasos(el también llamado _three-way handshake_):
    1. SYN: La apertura activa es realizada por el cliente enviando un SYN al servidor. El cliente establece el número de secuencia del segmento a un valor aleatorio, supongamos A.
    2. SYN-ACK: en respuesta, el servidor responde con SYN-ACK. El número de confirmación es establecido en uno más que el número de secuencia recibido, en este caso, A+1, y el número de secuencia que el servidor elige para el segmento es otro número aleatorio, llamémoslo B.
    3. ACK: finalmente, el cliente envía un ACK al servidor. El número de secuencia es establecido según el valor de confirmación, en este caso, A+1, y el valor de confirmación es establecido a uno más que el número de secuencia recibido, en nuestro caso, B+1.

    Ese es el desarrollo de una conexión exitosa. Una conexión fallida se puede reconocer a simple vista por el flag **RST** que indica el reinicio de la conexión cuando falla el intento.

Dada la primera exitosa responder:

1. Quién inicia la conexión, quien sería el servidor y quién el cliente? Qué ﬂags se ven activados? En que segmentos se ve el 3-way hand-shake?

    La IP que inicia la conexión es la 10.0.1.10. Esta misma IP es el cliente dado que es la única que *pushea* datos. La IP 10.0.3.10 se limita solo a recibirlos, por lo que solo actúa de servidor.

2. Qué ISNs se intercambian?

    El ISN de 10.0.1.10 es 3436242234, el de 10.0.3.10 es 2219352712.

3. Qué opciones se negocian. Qué signiﬁca c/u? Cuál es el MTU negociado?

    
