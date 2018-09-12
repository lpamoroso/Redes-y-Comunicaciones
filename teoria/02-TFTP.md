# TFTP

1. ¿Qué es el TFTP?

El protocolo TFTP(Trivial File Transfer Protocol) es un protocolo utilizado para copia de archivos desde o hacia un servidor. Consta solamente de dos funcionalidades, requerimiento de escritura(WRQ) o de lectura(RRQ), los cuales se mapean a dos comandos en las aplicaciones: put y get. A los comandos mencionados se agrega la funcionalidad de ACK y notificación de Errores. Es habitualmente utilizado en proceso de arranque de equipos sin disco trabajando conjuntamente con BOOTP/DHCP. Este servicio es montado sobre UDP. Sin bien TFTP puede trabajar tanto sobre una red LAN como una WAN, su uso en general se limita a las redes LAN.

2. ¿Cómo funciona el protocolo TFTP?

El TFTP trabaja en un modelo cliente/servidor. El cliente construye un socket UDP y genera luego una solicitud de escritura o de lectura hacia el servidor. El servidor, si acepta la petición, responde con un mensaje UDP desde otro puerto(un nuevo puerto para la nueva transferencia) al puerto efı́mero desde el cual se inicio la “conexión”. El cliente podría utilizar cualquier puerto efı́mero. ***Desde ese momento la transferencia se producirá desde o hacia este nuevo puerto. El servidor podrı́a notificar un error al intento de transferencia. Como es un protocolo montado sobre UDP debe manejar timers para cancelar sesiones que han quedado sin actividad. El protocolo no utiliza autenticación y, ante una copia, por ejemplo, intentaría realizarla directamente. Si el archivo no existiera, se mostraría un error; caso contrario, el resultado es exitoso. Eventualmente, este protocolo además, utiliza permisos, es decir, que, una escritura por ejemplo, se llevará a cabo si el archivo existe y si se tienen los permisos para hacerlo; caso contrario, la operación falla. Lo anterior es una forma de dar algo de seguridad a un servicio que, de forma nativa, no la provee.

3. ¿Qué codigos utiliza TFTP?

Los códigos de operaciones de TFTP son los siguientes:

|Código de Operación|Operación|
|:---:|:---:|
|1|Read request(RRQ)|
|2|Write request(WRQ)|
|3|Data(DATA)|
|4|Acknowledgment(ACK)
|5|Error(ERROR)|

El canal secundario solo puede ser creado por la recepción de un RRQ o un WRQ válido. Solo el servidor TFTP puede iniciar tráfico sobre el canal secundario y a lo sumo solo un canal incompleto secundario puede existir entre el servidor y el cliente. Un error de notificación desde el servidor cierra el canal secundario.

4. ¿Qué modos existen de TFTP?

De forma similar a FTP, TFTP tiene la capacidad de transmitir los archivos en modo ASCII o BINARIO. El modo ASCII realiza las conversiones de CR a CR,LF y es el default. Para transmitir información sin que sea posiblemente alterada, usar modo binario.

5. ¿Qué usos se le da al TFTP?

En la actualidad es ampliamente utilizado por los fabricantes de dispositivos de networking como mecanismo de actualización y backup de los firmware de los productos que ofrecen. Este protocolo es de muy sencilla implementación y por esto tiene la capacidad de ocupar muy poco espacio en memorias. Es habitual encontrarlo grabado en ROM de placas de red permitiendo a los sistemas arrancar sin necesidad de disco. La capacidad de poder trabajar con varias sesiones simultaneas(multiplexar) es permitida debido a la utilización de diferentes ports por el servidor para cada nueva transferencia.
