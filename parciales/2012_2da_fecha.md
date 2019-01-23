3. Suponga que PCA y PCB utilizan un cliente pesado de correo electrónico. Desde PCA, el usuario Juan, con cuenta en mail1.com, desea enviar un correo a pedro@mail2.com.

    1. Explique cómo se lleva a cabo el envío de mail, explicando detalladamente qué protocolos de aplicación intervienen y cómo los mismos se interrrelacionan.

        El envío de mail se lleva a cabo de la siguiente manera:

        1. Desde el *MUA*(**mail user agent**), el mensaje es formateado en formato de email y usa el protocolo de envío, un perfil del *SMTP*(**simple mail transfer protocol**), para enviar el contenido del mensaje al *MSA*(**mail submission agent**), en este caso *smtp.mail1.org*.
        2. El MSA determina la dirección de destino dada por el protocolo SMTP(**no por el encabezado del mensaje**), en este caso *pedro@mail2.com*, el cual es un *FQDA*(**fully qualified domain address**). Lo que está antes del *@* es la **parte local** de la dirección, por lo general el usuario del receptor, y la parte luego del *@* es el **nombre del dominio**. El MSA resuelve el nombre del dominio para determinar el FQDA del servidor de mail en el *DNS*(**domain name system**).
        3. El servidor de DNS para el dominio *mail2.com*(*ns.mail2.com*) responde con cualquier registro MX listando los servidores de intercambio de mail para ese dominio, en este caso *mx.mail2.com*.
        4. *smtp.mail1.com* envía el mensaje al MTA(**mail transfer agent**) *mx.mail2.com* usando SMTP mediante el MSA. El MTA quizá necesitara de reenviar el mensaje a otros MTAs antes de que, finalmente, el mensaje alcance el *MDA*(**mail delivery agent**) del receptor.
        5. El MDA lleva el mensaje al buzón de correo del usuario *pedro*.
        6. El MUA de *pedro* toma el mensaje o bien usando *POP3*(**post office protocol 3**) o *IMAP*(**internet message access protocol**).

    2. Explique cómo Pedro, desde PCB, lleva a cabo la lectura de sus mails. Explique detalladamente qué protocolos de capa de aplicación intervienen y cómo están relacionados.

        Para consultar sus e-mails, Pedro, desde su MUA, accederá al MDA mediante IMAP o POP3. Hay numerosas diferencias entre cada uno de estos protocolos:
        * Si Pedro decide usar IMAP, entonces, entre muchas funcionalidades, podrá:
            + Trabajar en modo de conexión permanente, por lo que Pedro será inmediatamente avisado si se produciere la llegada de un nuevo correo.
            + Decidir inmediatamente el borrado de un mensaje, ya que solo se transmiten las cabeceras. Se baja el mensaje solo cuando Pedro quiera leerlo.
            + Gestionar carpetas, plantillas y borradores en el servidor.
            + Buscar mensajes por medio de palabras claves.
        * Si Pedro, en cambio, utilizara POP3, entonces podrá:
            + Ver los mensajes estando desconectado(los mensajes se descargan y se eliminan del servidor).
            + Ahorrar espacio en el servidor(POP3 elimina los mensajes una vez descargados).

5. Responda si las siguientes oraciones son verdaderas o falsas. Justifique cada una en ambos casos.

    1. Utilizar la misma red IP en distintos dominios de broadcast no produce ningún problema de comunicación.

        Esto es verdadero, hay situaciones en que esto es así y no hay problemas de comunicación. Por otro lado, no se me ocurrieron casos en los que se hubiera problema. El caso que se me ocurrió fue el de NAT(**network address translation**). Cuando se traduce una ip pública a una ip privada  que llega a mi router doméstico, hay muchos otros routers que pueden estar usando esa dirección y es algo que no me afecta dado que es algo interno que realiza mi router.

    2. Utilizar la misma IP en distintos dominios de colisión dentro del mismo dominio de broadcast no produce ningún problema de comunicación.

        Esto sí que es falso. Si dos dispositivos tienen la misma IP en un mismo dominio de broadcast, incluso aunque sea distinto dominio de colisión, cuando se pregunte ¿Quíen tiene esta IP? mediante broadcast, responderán dos dispositivos y eso es algo que no debe pasar.

    3. En caso de tener MAC duplicadas, utilizarlas en distintos dominios de broadcast no produce ningún problema.

        Es verdadero. Las MAC address intervienen en el ARP o Neighbor discover/advertisement, por lo que si son diferentes dominios de broadcast, ante los distitos mensajes de ARP, no le veo problema.

    4. En caso de tener MAC duplicadas, utilizarlas en distintos dominios de colisión dentro del mismo dominio de broadcast no produce ningún problema.

        Es verdadero. Las MAC address intervienen en el ARP o Neighbor discover/advertisement, por lo que si es el mismo dominios de broadcast, ante los distitos mensajes de ARP, no le veo problema.

    5. Utilizar en un servidor el mismo número de puerto TCP y UDP para distintos servicios no produce ningún problema en el funcionamiento de los mismo.

        Esto es falso dado que cada puerto solo puede abrirse un tipo de conexión determinada en un momento dado. Esto significa que no puede darse que, en algún instante de tiempo, un puerto se asocie a TCP Y UDP simultáneamente.