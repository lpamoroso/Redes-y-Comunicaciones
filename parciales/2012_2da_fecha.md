1. Realice el subnetting con los rangos IP disponibles para asignar direcciones IP a las distintas redes del gráfico. Es importante tener en cuenta que tanto las redes de los servidores de DNS, el Web Server y los servidores de Mail deben tener direccionamiento público.

    Dadas las condiciones que el ejecicio plantea, las redes B, C y D deberán tener direccionamiento público. De los bloques de IP disponibles hay solo dos que cumplen esas condiciones: el 20.0.0.0/22 y el 30.0.0.0/28. Por el contrario, tanto 10.0.0.0/8 y 192.168.10.64/26 son rangos de IP privadas de clase A y C, respectivamente.  
    Volviendo a las IP que cumplen las condiciones, hay una de ellas que no podemos usar dado que no nos permite subnetear la cantidad de host de la red C(la red con más hosts): es la IP 30.0.0.0/28 y es la que descartaremos. Por lo tanto, usaremos la 20.0.0.0/22.  
    Para empezar, tomaremos 7 bits para representar los host de la red con mayor cantidad de hosts(la red C), por lo que nos quedarán 3 bits para la subred. Esto supone que tengo 8 subredes que pueden representar hasta 128 hosts, descontando 2(el primero, el reservado y el último, el de broadcast), es decir, 126 hosts.

    00010100 00000000 00000000 00000000 IP.  
    11111111 11111111 11111100 00000000 Máscara actual.  
    11111111 11111111 111111**11 1**0000000 Máscara nueva.  

    Red C = 20.0.0.0/25.
    Red B = 20.0.0.128/25.
    Red A = 20.0.1.0/25.
    Próximo bloque a subnetear = 20.0.1.128/25.

    Los bits en negrita representan la subred. Tanto la red A como B y C necesitaban 7 bits para representar sus hosts, por lo que se le dio una subred a cada una. Podría darle una subred completa a D que tiene 50 hosts, pero estaría desperdiciando mucho, por lo que voy a recortar un poco la red para ajustarla más a lo que D requiere. La idea es tomar 6 bits para los hosts y el resto que quede como subred, es decir, voy a tener 2 subredes con 64 hosts cada una(62, si descontas el host reservado y el de broadcast).

    00010100 00000000 00000001 10000000 IP.  
    11111111 11111111 11111111 10000000 Máscara actual.  
    11111111 11111111 11111111 1**1**000000 Máscara nueva.  

    Red D = 20.0.1.128/26.

    El bit en negrita representa la subred. Ahora hay que asignar IP a los enlaces punto-a-punto. Podría usar esta misma red para hacerlo, pero lo cierto es que conviene dejar esta red exclusivamente para hosts en caso de crecimiento. Para este asunto, conviene usar una red de naturaleza más ajustada, una red como 30.0.0.0/28. Dado que necesito 2 bits para los hosts, los otros 2 bits irán a la subred. Por lo cual tengo 4 subredes para asignar con 4 hosts cada una(2 si descuento el host reservado y el de broadcast).

    00011110 00000000 00000000 00000000 IP.  
    11111111 11111111 11111111 11110000 Máscara actual.  
    11111111 11111111 11111111 1111**11**00 Máscara nueva.

    Enlace RA-RB = 30.0.0.0/30.
    Enlace RB-RD = 30.0.0.4/30.
    Enlace RA-RC = 30.0.0.8/30.
    Enlace RC-RD = 30.0.0.12/30.

    Los bits en negrita son los de subred. Ahora toca asignar los hosts.

    Red C = 20.0.0.0/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;RouterC = 20.0.0.1/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;PCC = 20.0.0.2/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;mail2.com = 20.0.0.3/25.  
    Red B = 20.0.0.128/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;RouterB = 20.0.0.129/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;mail1.com = 20.0.0.130/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;DNS2 = 20.0.0.131/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;PCD = 20.0.0.132/25.  
    Red A = 20.0.1.0/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;RouterA = 20.0.1.1/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;PCA = 20.0.1.2/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;PCB = 20.0.1.3/25.  
    Red D = 20.0.1.128/26.  
    &nbsp;&nbsp;&nbsp;&nbsp;RouterD = 20.0.1.129/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;DNS1 = 20.0.1.130/25.  
    &nbsp;&nbsp;&nbsp;&nbsp;Web Server = 20.0.1.131/25.  
    Enlace RA-RB = 30.0.0.0/30.  
    &nbsp;&nbsp;&nbsp;&nbsp;RA = 30.0.0.1/30.  
    &nbsp;&nbsp;&nbsp;&nbsp;RB = 30.0.0.2/30.
    Enlace RB-RD = 30.0.0.4/30.  
    &nbsp;&nbsp;&nbsp;&nbsp;RB = 30.0.0.5/30.  
    &nbsp;&nbsp;&nbsp;&nbsp;RD = 30.0.0.6/30.  
    Enlace RA-RC = 30.0.0.8/30.  
    &nbsp;&nbsp;&nbsp;&nbsp;RA = 30.0.0.9/30.  
    &nbsp;&nbsp;&nbsp;&nbsp;RC = 30.0.0.10/30.  
    Enlace RC-RD = 30.0.0.12/30.  
    &nbsp;&nbsp;&nbsp;&nbsp;RC = 30.0.0.13/30.  
    &nbsp;&nbsp;&nbsp;&nbsp;RD = 30.0.0.14/30.  

2. Suponiendo que existe conectividad entre la PCA y el resto de la topología y que todas las redes con direccionamiento publico tienen conexión a Internet:
    1. Escriba las tablas de rutas de RA, RB, RC, RD, DNS1, PCA y "Web Server". Elija las direcciones IP que necesite de acuerdo a la asignación del punto anterior.

        ### Tabla de rutas de RA

        | Destination | Gateway | Genmask | Iface |
        |:---:|:---:|:---:|:---:|
        | 20.0.0.128 | 0.0.0.0 | 255.255.255.128 | eth1 |
        | 20.0.0.0 | 0.0.0.0 | 255.255.255.128 | eth2 |
        | 20.0.1.128 | 30.0.0.10 | 255.255.255.128 | eth2 |
        | 30.0.0.0 | 0.0.0.0 | 255.255.255.252 | eth1 |
        | 30.0.0.4 | 30.0.0.2 | 255.255.255.252 | eth1 |
        | 30.0.0.8 | 0.0.0.0 | 255.255.255.252 | eth2 |
        | 30.0.0.12 | 30.0.0.10 | 255.255.255.252 | eth2 |
        | 220.0.0.0 | 30.0.0.2 | 255.255.255.252 | eth1 |
        | 0.0.0.0 | 30.0.0.5 | 0.0.0.0 | eth1 |

        ### Tabla de rutas de RB

        | Destination | Gateway | Genmask | Iface |
        |:---:|:---:|:---:|:---:|
        | 20.0.1.0 | 0.0.0.0 | 255.255.255.128 | eth0 |
        | 20.0.0.0 | 30.0.0.1 | 255.255.255.128 | eth0 |
        | 20.0.1.128 | 0.0.0.0 | 255.255.255.128 | eth3 |
        | 30.0.0.0 | 0.0.0.0 | 255.255.255.252 | eth0 |
        | 30.0.0.4 | 0.0.0.0 | 255.255.255.252 | eth3 |
        | 30.0.0.8 | 30.0.0.6 | 255.255.255.252 | eth3 |
        | 30.0.0.12 | 30.0.0.1 | 255.255.255.252 | eth0 |
        | 220.0.0.0 | 0.0.0.0 | 255.255.255.252 | eth2 |
        | 0.0.0.0 | 192.10.36.86 | 0.0.0.0 | eth2 |

        ### Tabla de rutas de RC

        | Destination | Gateway | Genmask | Iface |
        |:---:|:---:|:---:|:---:|
        | 20.0.1.0 | 0.0.0.0 | 255.255.255.128 | eth1 |
        | 20.0.0.128 | 30.0.0.9 | 255.255.255.128 | eth1 |
        | 20.0.1.128 | 0.0.0.0 | 255.255.255.128 | eth2 |
        | 30.0.0.0 | 30.0.0.9 | 255.255.255.252 | eth1 |
        | 30.0.0.4 | 30.0.0.9 | 255.255.255.252 | eth1 |
        | 30.0.0.8 | 0.0.0.0 | 255.255.255.252 | eth1 |
        | 30.0.0.12 | 0.0.0.0 | 255.255.255.252 | eth2 |
        | 220.0.0.0 | 30.0.0.9 | 255.255.255.252 | eth1 |
        | 0.0.0.0 | 30.0.0.14 | 0.0.0.0 | eth2 |

        ### Tabla de rutas de RD

        | Destination | Gateway | Genmask | Iface |
        |:---:|:---:|:---:|:---:|
        | 20.0.1.0 | 30.0.0.13 | 255.255.255.128 | eth2 |
        | 20.0.0.128 | 0.0.0.0 | 255.255.255.128 | eth1 |
        | 20.0.0.0 | 0.0.0.0 | 255.255.255.128 | eth2 |
        | 30.0.0.0 | 30.0.0.5 | 255.255.255.252 | eth1 |
        | 30.0.0.4 | 0.0.0.0 | 255.255.255.252 | eth1 |
        | 30.0.0.8 | 30.0.0.13 | 255.255.255.252 | eth2 |
        | 30.0.0.12 | 0.0.0.0 | 255.255.255.252 | eth2 |
        | 220.0.0.0 | 30.0.0.5 | 255.255.255.252 | eth1 |
        | 0.0.0.0 | 30.0.0.5 | 0.0.0.0 | eth1 |

        ### Tabla de rutas de DNS1

        | Destination | Gateway | Genmask | Iface |
        |:---:|:---:|:---:|:---:|
        | 20.0.1.128 | 0.0.0.0 | 255.255.255.128 | eth0 |
        | 0.0.0.0 | 20.0.1.129 | 0.0.0.0 | eth0 |

        ### Tabla de rutas de PCA

        | Destination | Gateway | Genmask | Iface |
        |:---:|:---:|:---:|:---:|
        | 20.0.1.0 | 0.0.0.0 | 255.255.255.128 | eth0 |
        | 0.0.0.0 | 20.0.1.1 | 0.0.0.0 | eth0 |

        ### Tabla de rutas de Web Server

        | Destination | Gateway | Genmask | Iface |
        |:---:|:---:|:---:|:---:|
        | 20.0.1.128 | 0.0.0.0 | 255.255.255.128 | eth0 |
        | 0.0.0.0 | 20.0.1.129 | 0.0.0.0 | eth0 |

    2. Suponga ahora que en el RA se corta el enlace configurado para alcanzar la red D ¿Deben modificarse las tablas de rutas que decribió antes para que el PCA siga llegando a todos los elementos de la topología? Indique los cambios necesarios en las tablas de enrutamiento.



























    3. Para evitar la configuración manual de las tablas se utilizan protocolos de enrutamiento dinámicos. Mencione los distintos tipos de algoritmos de enrutamiento. Explique características de los mismos. Mencione al menos una implementación de cada uno.

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