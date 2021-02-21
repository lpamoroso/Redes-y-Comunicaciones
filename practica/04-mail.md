# Correo electrónico

1. ¿Qué protocolos se utilizan para el envío de mails entre el cliente y su servidor de correo? ¿Y entre servidores de correo?

    Para el envío de mails entre el cliente y su servidor de correo se utilizan HTTP, DNS y SMTP. Se utiliza HTTP(o HTTPS) cuando se va a acceder a la página del correo. DNS para buscar la página del correo y para buscar el servidor de mail al que se va a enviar el correo. SMTP es el protocolo que se utiliza para enviar el mail una vez obtenido el servidor destino a través del DNS.

2. ¿Qué protocolos se utilizan para la recepción de mails? ¿Incluiría a HTTP en dichos protocolos? Enumere y explique características y diferencias entre las alternativas posibles.

    IMAP y POP, como protocolos que se conectan directamente al servidor de mail. HTTP podría incluirse si se accede primero al sitio web y luego al servidor de mail, es decir, no es en sí mismo un protocolo que accede directamente al servidor de mail.
    IMAP es un protocolo de mail que accede al servidor de mail y descarga solo las cabeceras; de modo que, si el usuario quiere ver un mail, debe hacer click en la cabecera para descargue el cuerpo. Una ventaja de IMAP es que la conexión con el servidor no se cierra hasta que el usuario lo especifique, de modo que, si llega un nuevo e-mail, el usuario se ve notificado casi instantáneamente. Otras ventajas de IMAP es la posibilidad de establecer reglas sobre los e-mails o filtros y la posibilidad de crear carpetas.
    POP es un protocolo de mail que accede al servidor de mail, descarga todos los mails en el dispositivo desde el cual se esté accediendo y luego borra todo en el servidor de mail. Una ventaja es que el servidor siempre estará limpio(siempre y cuando sea accedido periódicamente) y se podrán acceder los mensajes offline.

3. //FALTA HACER

4. En base a lo observado ¿Qué protocolo le parece mejor? ¿POP o IMAP? ¿Por qué? ¿Qué protocolo considera que utiliza más recursos del servidor? ¿Por qué?

    Depende, no considero que haya un protocolo mejor que otro, dado que son protocolos distintos. En cuanto a funcionalidades, es preferible IMAP ya que permite filtrar, ordenar y establecer reglas sobre los emails; POP no posee un abanico de funcionalidades, analizándolo desde esa perspectiva. Si, en cambio, se mira desde el punto de uso del servidor, POP solo lo utiliza para guardar mensajes y, eventualmente, borrarlos. IMAP, en cambio, posee variadas funcionalidades y conserva los mails, por lo que el servidor debe tener la capacidad para operar con ellos y el espacio para guardarlos, por lo que, en ese sentido, POP consume muchos menos recursos.

5. Suponga que el servidor de correo mail1.example.com tiene para enviar un correo a pepe@gmail.com. Indique y justifique en todos los casos:

    * Primer consulta de DNS que debe hacer mail1.example.com.

        El MSA(Mail Submission Agent) consulta con el DNS el servidor de mail de la dirección destino que aparece en el paquete SMTP. Tal dirección se encuentra en formato FQDA(Fully Qualified Domain Address). El "@" separa la parte local de la dirección(por lo general el usuario) y el nombre del dominio. El MSA resuelve el nombre del dominio para luego, a través de una consulta DNS, determinar los registros MX asociados.

    * Suponiendo que la consulta anterior devolviera varios resultados ¿De qué forma elegiría mail1.example.com el servidor al cuál entregar el correo? ¿Y si ese servidor no estuviera disponible?

        Usaría la prioridad. Cada registro MX tiene una prioridad. Cuanto menor es el número, mayor la prioridad. Si tal servidor no estuviera disponible por la razón que fuera, eligiría el siguiente conforme a la prioridad.

    * Considerando que la consulta anterior retorna un listado de nombres de servidores de correo para gmail.com ¿Será necesario realizar una consulta adicional? En caso de responder afirmativamente, indique el registro por el cuál será la consulta.

        En tal caso si, deberá realizarse una consulta adicional para conocer los registros MX de uno de esos NS. //PREGUNTAR

    * El protocolo de aplicación y de transporte, junto con el puerto correspondiente, que deberá utilizar el servidor mail1.example.com para entregar el correo a pepe@gmail.com.

        Utilizará HTTP para conectarse al sitio web, DNS para buscar el nombre del dominio a quien debe enviarse el mail y SMTP para enviar el mail. Se utilizará un puerto mayor o igual a 1024.