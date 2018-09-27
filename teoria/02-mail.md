# Mail

1. ¿Qué es el mail?

Es uno de los primeros servicios de Internet. Utilizó uno de los primeros protocolos de transporte UUCP(Unix-to-Unix Copy).
Las Conexiones se realizaban vía modems.
En 1972, se escoge el símbolo “@” para separar el usuario del dominio o servidor al que iba dirigido. Ej: `USER@FQDN-DOMAIN` o `USER@FQDN-SERVER`.
En 1982, surge SMTP.

2. ¿Cuáles son los componentes del mail?

###### Componentes principales:

* **MUA(Mail User Agent)**: representa un cliente, es decir, el lector/emisor local de correo. Es el que se encarga de la escritura, edición y lectura de correos. Poseen una interfaz para comunicarse con el usuario y utilizan un LMTA(*Local Mail Transport Agent*) para cominicarse con el servidor de mail saliente. Utilizan protocolos SMTP, POP o IMAP dependiendo la interfaz y se comunican con el MTA y MDA. Puede ser Pidgeon, mutt, Mozilla Thunderbird, etc. También entran en esta categoría los web-mailers, por ejemplo Yahoo, Hotmail, Gmail, etc.
* **MTA(Mail Transport Agent)**: representan cliente y servidor. Se encargan de enviar el mail al servidor donde está almacenada la casilla de mail correspondiente. Almacena temporalmente el correo saliente. Podría servir a más de una casilla. Utiliza protocolo SMTP entre servidores MTA. Ejemplo: Postmail, Qmail, Sendmail, etc.
* **MDA(Mail Delivery Agent)**: representan cliente y servidor. Se encargan de tomar los mensaje recibidos por el MTA y llevarlos al usuario. Existen MDA locales(LMDA) y remotos(RMDA). Utilizan protocolos Pop y-o IMAP. Ej. de LMDA: procmail, maildrop, Sendmail, etc. Ej. de RMDA: Cyrus IMAPd, Courier IMAPd, Dovecot, etc.

###### Componentes Secundarias:

* Servidor de Autenticación
* Web Mail(Front-End WWW).
* Servidor de Anti-Virus, Anti-SPAM, ...
* Servidores de Listas.

3. ¿Qué protocolos de mail existen?

Hay dos tipos:

##### Protocolo para enviar y recibir *e-mails*

* **SMTP(*Simple Mail Transport Protocol*)**: es un protocolo Cliente/Servidor. Utiliza formato ASCII 7 bits en 8 NVT. Usa TCP para enviar mensajes en forma confiable y corre sobre el puerto servidor 25. Los LMTA de los MUA hablan SMTP con su servidor SMTP saliente. Los servidores SMTP hablan entre sí este protocolo. Requiere finalizar con CRLF(*Carriage Return, Line Feed*, es decir, el salto de línea). Usa conexiones persistentes. Trabaja de forma interactiva(requerimiento/respuesta) o pipeline. Cada respuesta se compone de un código de estado y una descripción. Puede o no requerir Autenticación. Puede o no trabajar de forma segura: SSL/TLS.
El SMTP tiene tres fases de transferencia: un saludo(handshaking), la propia transferencia de mensajes y una finalización. El saludo se realiza con el comando EHLO o HELO, dependiendo si es la versión extendida de SMTP o no, respectivamente. Durante la transferencia de mensajes, se procede a preparar el correo. Éste, a su vez, tiene otras tres partes: un envelope, un header y un body. El envelope está definido como `MAIL FROM:, RCPT TO:` y no es algo que el usuario vea, sino que es usado por MTAs. El header contiene la meta informacion del mail, esto es, `Subject:, From:, To:, Return-Path: X-Mailer:, X-...:`. El body representa el contenido del mail y se encuentra separado por una línea en blanco del header. Existe también un formato de mensaje extendido hecho con MIME para enviar datos binarios(lo que conocemos como archivos adjuntos). Utiliza tags especiales para indicar el tipo al sistema destino. Luego codifica el mensaje binario en formato que no viole US-ASCII. Finalmente se pasa a la fase de finalización en la que se envía el correo y luego se cierra la conexión TCP.

##### Protocolos acceso a correo

* **POP**: Post Office Protocol. Existe también la versión POPv3. Cuando se usa POP3, el mensaje es obtenido y luego se borra. Eso no significa que no sea posible obtener el mensaje sin borrar, pero vale la pena destacar que POP3 no tiene estado entre sesiones.
* **IMAP**: Internet Mail Access Protocol. Existe también la versión IMAPv4. En IMAP, se guardan todos los mensajes en el servidor. Resulta más flexible ya que permite el uso de carpetas y manipulación de mensajes en el servidor. IMAP mantiene estado entre sesiones: nombres de directorios, mensajes y directorios.

Ámbos requieren autenticación y utilizan formato ASCII 7 bits en 8 NVT(*Network Virtual Terminal*, un concepto asociado al telnet). Usan TCP sobre los puertos servidor 110 y 143. Permiten correr de forma segura sobre SSL/TLS.

4. ¿Cómo se envía un correo electrónico?

    1. Alicia usa aplicación para crear mensaje a roberto@fing.edu.uy.
    2. Alicia usa aplicación para enviar mensaje a su servidor de correo. El mensaje es puesto en una cola de mensajes.
    3. El servidor de Alicia abre una conexión TCP con el servidor de Roberto.
    4. El servidor SMTP de Alicia envia el mensaje por la conexión TCP.
    5. El servidor de Roberto coloca el mensaje en la casilla de Roberto.
    6. Roberto usa su aplicación para leer el mensaje.

5. ¿Qué es el spam?

Son todos aquellos e-mails no solicitados enviados a granel por diversas cuentas e-mail. Por lo general son publicidad o virus encubiertos. Utiliza el open relay, esto es, un server SMTP configurado de forma que permite que cualquiera en la internet pueda enviar e-mails a través de él, no solo mails destinados u originados por usuarios conocidos. Esto es lo que ha permitido la proliferación de los SPAMbots.
La forma para contrarrestarlo reside en bloquearlos a partir de la RBL(Realtime Blackhole List), una blacklist que contiene todas las direcciones que envían SPAM registradas al momento. También se puede apelar al concepto contrario y utilizar una whitelist, es decir, una lista que indica los e-mails de los que se desea recibir contenido. Existen además filtros de contenido que funcionan similar a las blacklist, idem las reglas dirigidas/entrenadas por el usuario.

6. HTTP vs. SMTP

|HTTP|SMTP|
|:---:|:---:|
|Se extraen datos del servidor|Se envían datos|
|Tiene comandos ASCII con estados|Tiene comandos ASCII con estados|
|Cada objeto esta encapsulado en su propio mensaje|Muchos objetos en un solo mensaje|
