# E-Mail

## Preguntas generales

1. ¿Qué es el e-mail?

    Es uno de los primeros servicios de Internet. Utilizó uno de los primeros protocolos de transporte UUCP(Unix-to-Unix Copy).
    Las Conexiones se realizaban vía modems. En 1972, se escoge el símbolo "@" para separar el usuario del dominio o servidor al que iba dirigido. Ej: `USER@FQDN-DOMAIN` o `USER@FQDN-SERVER`. En 1982, surge SMTP.

2. ¿Cuáles son los componentes del mail?

   + Componentes principales:
     * MUA (Mail User Agent): representa un cliente, es decir, el lector/emisor local de correo. Es el que se encarga de la escritura, edición y lectura de correos. Poseen una interfaz para comunicarse con el usuario. Utilizan protocolos como SMTP, POP3 o IMAP4 dependiendo de si buscan enviar los e-mails (SMTP) o consultarlos (POP3 o IMAP4) y se comunican con el MTA y MDA. Puede ser Pidgeon, mutt, Mozilla Thunderbird, etc. También entran en esta categoría los web-mailers, por ejemplo Yahoo, Hotmail, Gmail, etc.
     * MTA (Mail Transport Agent): se encargan de enviar el mail al servidor donde está almacenada la casilla de mail correspondiente. Almacena temporalmente el correo saliente. Utiliza protocolo SMTP entre servidores MTA. Ejemplo: Postmail, Qmail, Sendmail, etc.
     * MDA (Mail Delivery Agent): se encargan de tomar los mensaje recibidos por el MTA y llevarlos al usuario. Existen MDA locales(LMDA) y remotos (RMDA). Utilizan protocolos POP3 y/o IMAP4. Ej. de LMDA: procmail, maildrop, Sendmail, etc. Ej. de RMDA: Cyrus IMAPd, Courier IMAPd, Dovecot, etc.
   + Componentes Secundarias:
     * Servidor de Autenticación
     * Web-Mail (Front-End WWW).
     * Servidor de Anti-Virus, Anti-SPAM, ...
     * Servidores de Listas.

3. ¿Qué protocolos de mail existen?

    Hay dos tipos. El protocolo para enviar y recibir e-mails (SMTP) y los protocolos de acceso a correo (POP3 o IMAP4):

    * SMTP (Simple Mail Transport Protocol): es un protocolo cliente/servidor que transfiere mensajes desde los servidores de correo de los emisores a los servidores de correo de los destinatarios. Utiliza formato ASCII 7 bits, una restricción que tenía sentido cuando el protocolo fue creado (dada la escasa capacidad de transmisión), pero que hoy es algo que genera muchos problemas, dado que requiere que todos los datos binarios sean convertidos a ASCII previo a la transmisión y luego reconvertidos un vez realizado el transporte SMTP. Usa TCP para enviar mensajes en forma confiable y corre sobre el puerto servidor 25. Requiere finalizar con CRLF (*Carriage Return, Line Feed*, es decir, el salto de línea). Usa conexiones persistentes. Trabaja de forma interactiva (requerimiento/respuesta) o pipeline. Cada respuesta se compone de un código de estado y una descripción. Puede o no requerir autenticación. Puede o no trabajar de forma segura con SSL/TLS.

        El SMTP tiene tres fases de transferencia: un saludo (handshaking), la propia transferencia de mensajes y una finalización. El saludo se realiza con el comando EHLO o HELO, dependiendo si es la versión extendida de SMTP o no, respectivamente. Durante la transferencia de mensajes, se procede a preparar el correo. Éste, a su vez, tiene otras tres partes: un envelope, un header y un body. El envelope está definido como `MAIL FROM:, RCPT TO:` y no es algo que el usuario vea, sino que es usado por MTAs. El header contiene la meta informacion del mail, esto es, `Subject:`, `From:`, `To:`, `Return-Path:`, `X-Mailer:`, etc. No todos los headers son obligatorios (por ejemplo, el `Subject:` no lo es). El body representa el contenido del mail y se encuentra separado por una línea en blanco del header. Existe también un formato de mensaje extendido hecho con MIME para enviar datos binarios (lo que conocemos como archivos adjuntos). Utiliza tags especiales para indicar el tipo al sistema destino. Luego codifica el mensaje binario en formato que no viole US-ASCII. Finalmente se pasa a la fase de finalización en la que se envía el correo. El cliente repetirá todo el proceso tantas veces como mensajes haya en el servidor todo a través de la misma conexión TCP. Si no quedan más mansajes al servidor receptor, la conexión TCP se cierra.

        Algo interesante de SMTP es que no utiliza servidores de correo intermedios. Incluso aunque el receptor estuviera en el otro extremo del mundo, la conexión TCP será siempre entre servidor emisor y receptor. Algo que también podría pasar es que el servidor no estuviera disponible. En este caso, el servidor emisor intentará más tarde.
    * POP: Post Office Protocol. Existe también la versión POPv3. Cuando se usa POP3, el mensaje es obtenido y luego se borra. Eso no significa que no sea posible obtener el mensaje sin borrar, pero vale la pena destacar que POP3 no tiene estado entre sesiones. Utiliza el puerto 110. Permite correr de forma segura sobre SSL/TLS. Requiere autenticación y formato ASCII 7 bits en 8 NVT.
    * IMAP: Internet Mail Access Protocol. Existe también la versión IMAPv4. En IMAP, se guardan todos los mensajes en el servidor. Resulta más flexible ya que permite el uso de carpetas y manipulación de mensajes en el servidor. IMAP mantiene estado entre sesiones: nombres de directorios, mensajes y directorios. Utiliza el puerto 143. Permite correr de forma segura sobre SSL/TLS. Requiere autenticación y formato ASCII 7 bits.

4. ¿Cómo se envía un correo electrónico?

    1. Alicia usa su aplicación para crear un mensaje a roberto@fing.edu.uy.
    2. Alicia abre una conexión TCP para comunicarse con su servidor de correo.
    3. Alicia usa su aplicación para enviar un mensaje a su servidor de correo a través de SMTP. El mensaje es puesto en una cola de mensajes.
    4. El servidor de Alicia abre una conexión TCP con el servidor de Roberto.
    5. El servidor SMTP de Alicia envia el mensaje por la conexión TCP.
    6. El servidor de Roberto coloca el mensaje en la casilla de Roberto.
    7. Roberto utiliza un protocolo de acceso a correo para consultar sus mensajes. Este puede ser IMAP4, POP3 o HTTP. Si IMAP4 es utilizado, el puerto es el 143; por su parte, si se utiliza POP3, el puerto elegido será el 110. Por último, también podrá emplearse un WebMail utilizando un navegador. En este caso, será necesario contar con un protocolo de acceso (IMAP4 o POP3) sumado al webmail que utilizará el puerto 80 o el 443 (el de HTTP o HTTPS, respectivamente).
    8. Roberto lee el mensaje.

5. HTTP vs. SMTP

    |HTTP|SMTP|
    |:---:|:---:|
    |Transfiere archivos de un servidor web a un cliente web|Transfiere archivos de un servidor de correo a otro|
    |Puede utilizar conexiones persistentes (solo en HTTP 1.1)|Siempre utiliza conexiones persistentes|
    |Protocolo pull, es decir, extrae datos de un servidor|Protocolo push, el servidor de correo emisor introduce un archivo en el servidor de correo receptor|
    |No tienen restricciones en los mensajes en cuanto a ASCII|Requiere que cada mensaje esté en formato ASCII de 7 bits|
    |Cada objeto esta encapsulado en su propio mensaje|Muchos objetos en un solo mensaje|

6. POP3 vs. IMAP4

    |POP3|IMAP4|
    |:---:|:---:|
    |Descarga y almacena los mensajes de forma local|Almacena los mensajes en el servidor de correo|
    |Consume espacio local|Solo utiliza el servidor de correo|
    |No se puede ver el correo desde diferentes dispositivos (por defecto, POP3 descarga y elimina los mensajes, aunque es posible configurarlo para que solo los descargue)|El correo es accesible desde cualquier dispositivo con acceso a Internet|
    |Los cambios solo son locales|Al trabajar directamente con el servidor de correo, cualquier cambio es visible desde todos los dispositivos que accedan a la cuenta|
    |Si el dispositivo al que se descargan los mensajes sufre una avería, el correo podría llegar a perderse (por defecto, POP3 descarga y elimina los mensajes, aunque es posible configurarlo para que solo los descargue)|Si el dispositivo desde el que se conecta a la cuenta tiene una avería, no se pierden los correos al estar almacenados directamente en el servidor|

7. Comandos de POP3

    Hay 5 comandos a tener en cuenta:
    + user: indica el usuario.
    + pass: indica la contraseña.
    + list: lista los e-mails.
    + retr: descarga los e-mails.
    + dele: borra los e-mails del servidor.
    + quit: termina la conexión.

    Ejemplo de uso de POP3 en el caso que se busca descargar y borrar los e-mails. Se marcará como C al cliente y S a la respuesta del servidor:

    ```POP3
    C: telnet algunMailserver 110
    S: +OK POP3 server ready
    C: user roberto
    S: +OK
    C: pass r0b3rt0
    S: +OK user succesfully logged on
    C: list
    S: 1 498
    S: 2 983
    S: .
    C: retr 1
    S: (bla bla bla ....
    S: ... .... ... ....
    S: ...... bla)
    C: dele 1
    C: retr 2
    S: (bla bla bla ....
    S: ... .... ... ....
    S: ...... bla)
    C: dele 2
    C: quit
    S: +OK POP3 server signing off
    ```

## Preguntas de yapa que vi en otros parciales

1. A través de un ejemplo describa brevemente las componentes de un e-mail: envelope, encabezado y cuerpo.

    Envelope (a saber, estos datos son propios del paquete SMTP, no es algo que el usuario vea):

    ```envelope-smtp
    MAIL FROM: <alicia@fing.edu.uy>
    RCPT TO: <bob@hamburg.ger>
    ```

    Encabezado (esto aparece en cualquier e-mail. Los obligatorios son el from y el to, el resto -asunto, fecha, etc- son opcionales):

    ```encabezado-smtp
    From: alicia@fing.edu.uy
    To: bob@hamburg.ger
    Subject: Significado de la vida.
    ```

    Cuerpo: toda la data del mensaje

2. En la especificacion MIME, en particular en correo electronico, que indica un header "Content-Type: Multipart/Mixed"?

    Un mensaje MIME Multipart/Mixed se compone de una combinación de diferentes tipos de datos. Cada parte del cuerpo está delimitada por un límite. El parámetro de límite es una cadena de texto que se utiliza para delimitar una parte del cuerpo del mensaje de otra. Todos los límites comienzan con dos guiones (-). El límite final también concluye con dos guiones (-). El límite puede estar compuesto por cualquier carácter ASCII excepto por un espacio, un carácter de control o caracteres especiales.

    Un encabezado de mensaje MIME de varias partes / mixto de un mensaje enviado con un archivo adjunto de Microsoft Word puede tener un aspecto similar al siguiente:

    ```SMTP
    Content-type: multipart/mixed;
    boundary="Boundary_any ascii character except some of the following special characters:

    <BR/> ( )< > @ , ; : \ / [ ] ? = "
    "
    --Boundary_any ASCII character, except some special characters below:
    content-Type: text/plain;----
    charset=iso-8859-1
    Content-transfer-encoding: 7BIT
    --Boundary_ASCII characters  
    Content-type: application/msword;
    name="message.doc"
    Content-Transfer-Encoding: base64
    ```
