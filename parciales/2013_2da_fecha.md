1. La organización X necesita asignar direcciones IPs teniendo en cuenta que:

    * Los enlaces punto-a-punto deben usar direccionamiento privado y desperdiciar la menor cantidad posible de direcciones IPs.
    * Todas las redes de hosts y servidores de la casa matriz deben tener direccionamiento público y poder alojar al menos 20 hosts en cada una.

    Para la asignación de IPs evalue la validez de los siguientes bloques así como también la utilidad de los mismos. Use el o los bloques que crea conveniente.

    <table align="center">
        <tr>
            <td align="center">127.48.93.128/25</td>
            <td align="center">60.256.80.0/24</td>
            <td align="center">128.0.0.0/25</td>
            <td align="center">10.0.0.0/28</td>
        </tr>
        <tr>
            <td align="center">163.10.0.0/29</td>
            <td align="center">80.46.0.0/29</td>
            <td align="center">172.23.58.0/24</td>
            <td align="center">225.16.40.0/24</td>
        </tr>
    </table>

    *A priori*, hay direcciones que tenemos que ir descartando ya que no se pueden usar:
    
    + 127.48.93.128/25: es una dirección de *loopback* y por ende no es posible asignarla en la red.
    + 60.256.80.0/24: no es válida(como mucho se llega a 255).
    + 225.16.40.0/24: es de clase D y por ende no se puede usar(en este caso).

    Entonces, de las 8 direcciones solo podemos utilizar 5:

    <table align="center">
        <tr>
            <td align="center">128.0.0.0/25</td>
            <td align="center">10.0.0.0/28</td>
            <td align="center">163.10.0.0/29</td>
            <td align="center">80.46.0.0/29</td>
            <td align="center">172.23.58.0/24</td>
        </tr>
    </table>

    Lo que primero conviene resolver es el direccionamiento de redes, citando lo que dice arriba:

    ```
    Todas las redes de hosts y servidores de la casa matriz deben tener direccionamiento público y poder alojar al menos 20 hosts en cada una.
    ```

    Si las redes deben tener direccionamiento público, entonces primero hay que separar las direcciones públicas de las privadas. Eso nos deja con la siguiente tabla:

    <table align="center">
        <tr>
            <td align="center">128.0.0.0/25</td>
            <td align="center">163.10.0.0/29</td>
            <td align="center">80.46.0.0/29</td>
    </table>

    De esos tres bloques hay dos que vamos a descartar:

    + 163.10.0.0/29: el /29 hace que solo tenga 2 bits para los hosts(suponiendo que un bit lo dedico a la subred), lo cual hace que, como mucho pueda tener 2^2(4) hosts, 2 si descontamos el primer host reservado y el último utilizado para el broadcast.
    + 80.46.0.0/29: idem 163.10.0.0/29.

    Entonces tengo solo un bloque para utilizar: el 128.0.0.0/25.  
    La idea es la siguiente: de los 7 bits que tengo para usar del bloque, voy a darle a los *hosts* 5, de modo que pudieren haber como mucho 32, 30 si descuento el primer host reservado y el último utilizado para el broadcast. A la subred le quedarán 2 bits, por lo que tendré 4 subredes, dos de ellas serán utilizadas, las otras dos me sobrarán. Entonces:

    10000000 00000000 00000000 00000000 IP.  
    11111111 11111111 11111111 10000000 Máscara actual.  
    11111111 11111111 11111111 1**11**00000 Máscara nueva.  

    Los dos bit en negrita son los que representan la subred, los bits a la izquierda de ésta son los de red, los ceros a la derecha son los hosts.  
    Finalmente, asignando las redes nos queda:

    Red X3: 128.0.0.0/27  
    Red X2: 128.0.0.32/27

    Asignando los hosts a cada red:



    10000000 00000000 00000000 00000010
    10000000 00000000 00000000 00000011
    11111111 11111111 11111111 1111111
    128.0.0.2/31

    10000000 00000000 00000000 00000100
    10000000 00000000 00000000 00000101
    11111111 11111111 11111111 111111
    128.0.0.4/30



    Red X3: 128.0.0.0/27  
    &nbsp;&nbsp;&nbsp;&nbsp; Router X3: 128.0.0.1/27  
    &nbsp;&nbsp;&nbsp;&nbsp; mailX: 128.0.0.2/27  
    &nbsp;&nbsp;&nbsp;&nbsp; dnsX: 128.0.0.3/27  
    &nbsp;&nbsp;&nbsp;&nbsp; wwwX: 128.0.0.4/27  
    &nbsp;&nbsp;&nbsp;&nbsp; dbX: 128.0.0.5/27  

    Red X2: 128.0.0.32/27  
    &nbsp;&nbsp;&nbsp;&nbsp; Router X2: 128.0.0.33/27  
    &nbsp;&nbsp;&nbsp;&nbsp; pcX1: 128.0.0.34/27  
    &nbsp;&nbsp;&nbsp;&nbsp; pcX2: 128.0.0.35/27  
    &nbsp;&nbsp;&nbsp;&nbsp; pcX3: 128.0.0.36/27  
    &nbsp;&nbsp;&nbsp;&nbsp; pcX4: 128.0.0.37/27  
    &nbsp;&nbsp;&nbsp;&nbsp; pcX5: 128.0.0.38/27  

    Ya solucionamos el asunto del direccionamiento público de las redes de la casa matriz, pero todavía falta algo:

    ```
    Los enlaces punto-a-punto deben usar direccionamiento privado y desperdiciar la menor cantidad posible de direcciones IPs.
    ```

    Por lo cual, volvemos a la tabla que nos habia quedado antes de descartar las privadas:

    <table align="center">
        <tr>
            <td align="center">128.0.0.0/25</td>
            <td align="center">10.0.0.0/28</td>
            <td align="center">163.10.0.0/29</td>
            <td align="center">80.46.0.0/29</td>
            <td align="center">172.23.58.0/24</td>
        </tr>
    </table>

    Filtramos solo las privadas y entonces obtenemos:

    <table align="center">
        <tr>
            <td align="center">10.0.0.0/28</td>
            <td align="center">172.23.58.0/24</td>
        </tr>
    </table>

    De esas dos no es posible utilizar 172.23.58.0/24 dado que en el enunciado se especifica que deben desperdiciarse la menor cantidad de direcciones. Los enlaces punto-a-punto requieren solo de 4 hosts(2 que usará cada router involucrado en el enlace y los otros dos serán el primer host reservado y el último utilizado para el broadcast), es decir, 2 bits. Si subneteamos el /24, de los 8 bits a usar, usaremos 2 bits para los hosts(los requeridos) y 6 bits para las subredes(quedándonos 64 subredes disponibles, de las cuales usaríamos 4, por lo cual desperdiciaríamos 60). Si subneteamos 10.0.0.0/28, entonces, de los 4 bits a utilizar, 2 van a subredes y dos a hosts, por lo que nos queda el esquema justo, sin desperdiciar ninguna subred y ningún host. Pasando al subneteo:

    00001010 00000000 00000000 00000000 IP.  
    11111111 11111111 11111111 11110000 Máscara actual.  
    11111111 11111111 11111111 1111**11**00 Máscara nueva.  

    Los dos bits en negrita representan la subred. A la izquierda están los bits de red; a la derecha, los de hosts. Asignando las redes nos quedaría:

    Enlace RouterX3-RouterX1: 10.0.0.0/30.  
    Enlace RouterX3-RouterX2: 10.0.0.4/30.  
    Enlace RouterX1-RouterY1: 10.0.0.8/30.  
    Enlace RouterX2-RouterY1: 10.0.0.12/30.  

    Asignando los hosts a cada red:

    Enlace RouterX3-RouterX1: 10.0.0.0/30  
    &nbsp;&nbsp;&nbsp;&nbsp; Router X3 - eth1: 10.0.0.1/30  
    &nbsp;&nbsp;&nbsp;&nbsp; Router X1 - eth1: 10.0.0.2/30  

    Enlace RouterX3-RouterX2: 10.0.0.4/30  
    &nbsp;&nbsp;&nbsp;&nbsp; Router X3: 10.0.0.5/30  
    &nbsp;&nbsp;&nbsp;&nbsp; Router X2: 10.0.0.6/30  

    Enlace RouterX1-RouterY1: 10.0.0.8/30  
    &nbsp;&nbsp;&nbsp;&nbsp; Router X1: 10.0.0.9/30  
    &nbsp;&nbsp;&nbsp;&nbsp; Router Y1: 10.0.0.10/30  

    Enlace RouterX2-RouterY1: 10.0.0.12/30  
    &nbsp;&nbsp;&nbsp;&nbsp; Router X2: 10.0.0.13/30  
    &nbsp;&nbsp;&nbsp;&nbsp; Router Y1: 10.0.0.14/30  

2. Asigne direcciones IP a todos los elementos de red de la Sucursal 1 que crea necesarios de manera tal que todos los servidores y hosts tengan internet. Para cada asignación, indique: (Dispositivo; Nombre interfaz de red; Dirección IP; Máscara).

    En la red Sucursal 1 hay dos mini-redes: la de 13.22.33.0/24 y la de 160.40.0.16/29. La de 13 no tiene ningún defecto; la de 160 tiene algunos detalles interesantes:
    * El host del router tiene asignada la dirección 160.40.0.19/29: el router es el primer host al que se le asigna IP, por lo que, si la red es la 160.40.0.16/29, entonces el router debería ser 160.40.0.17/29.
    * wwwY2 y mailY1 no tienen direcciones asignadas: procederé a asignarle a wwwY2 la dirección 160.40.0.18/29 y, habiendo liberado ya la 160.40.0.19/29, será la que asigne a mailY1.

3. Escriba las tablas de rutas de routerX1, routerX2, routerX3 y routerY1 teniendo en cuenta que:
* Tanto la casa matriz como la sucursal salen a internet a través de su enlace externo.
* La comunicación entre la casa matriz y la sucursal se realiza a través del enlace punto-a-punto(enlace principal).
    ### Tabla de routerX1

    | Destination | Gateway | Genmask | Interface |
    |:---:|:---:|:---:|:---:|
    | 160.40.0.16 | 10.0.0.10 | 255.255.255.248 | eth3 |
    | 128.0.0.1 | 10.0.0.1 | 255.255.255.224 | eth1 |
    | 13.22.33.1 | 10.0.0.10 | 255.255.255.0 | eth3 |
    | 0.0.0.0 | 200.0.1.1 | 0.0.0.0 | eth2 |

    ### Tabla de routerX2

    | Destination | Gateway | Genmask | Interface |
    |:---:|:---:|:---:|:---:|
    | 128.0.0.33 | 0.0.0.0 | 255.255.255.224 | eth2 |
    | 160.40.0.16 | 10.0.0.14 | 255.255.255.248 | eth4 |
    | 128.0.0.1 | 10.0.0.5 | 255.255.255.224 | eth0 |
    | 13.22.33.1 | 10.0.0.14 | 255.255.255.0 | eth4 |
    | 0.0.0.0 | 10.0.0.14 | 0.0.0.0 | eth0 |

    ### Tabla de routerX3

    | Destination | Gateway | Genmask | Interface |
    |:---:|:---:|:---:|:---:|
    | 128.0.0.2 | 0.0.0.0 | 255.255.255.254 | eth2|
    | 128.0.0.4 | 0.0.0.0 | 255.255.255.252 | eth3 |
    | 160.40.0.16 | 10.0.0.2 | 255.255.255.248 | eth1 |
    | 128.0.0.33 | 10.0.0.6 | 255.255.255.224 | eth0 |
    | 13.22.33.1 | 10.0.0.2 | 255.255.255.0 | eth1 |
    | 0.0.0.0 | 10.0.0.2 | 0.0.0.0 | eth1 |

    ### Tabla de routerY1

    | Destination | Gateway | Genmask | Interface |
    |:---:|:---:|:---:|:---:|
    | 160.40.0.16 | 0.0.0.0 | 255.255.255.248 | eth1 |
    | 13.22.33.1 | 0.0.0.0 | 255.255.255.0 | eth4 |
    | 128.0.0.1 | 10.0.0.9 | 255.255.255.224 | eth3 |
    | 128.0.0.33 | 10.0.0.13 | 255.255.255.224 | eth0 |
    | 0.0.0.0 | 193.14.38.49 | 0.0.0.0 | eth2 |

4. En un principio, considerando que todas las tablas ARP y las tablas de los *switches* están vacías. Indique cómo quedarán las tablas ARP de dbY y pcX1 y la tabla de asociaciones del Switch-Y-1 luego de que se ejecuten los siguiente comandos:

    1. pcX1 realiza un ping a dbY.

        Suponiendo que las tablas CAM de los *switches* estuvieren vacías, entonces pcX1, según su tabla de ruteo, como dbY no forma parte de la red de pcX1, va a lanzar un ARP *request* preguntando a qué MAC está asociada la IP de su router. El ARP *request* va a llegar a todos los dispositivos de la red y, eventualmente, el router responderá que él es quién tienen tal IP mediante un ARP *reply* adjuntando su MAC, la cual será guardada en la tabla CAM del switch para futuras referencias junto con la interfaz a través de donde llegó y en la tabla ARP de pcX1. El próximo paso será especificarle a la MAC del router que le haga un ping a dbY y éste, mediante su tabla de rutas, lo pasará al router Y1. El router Y1 

    2. dbY realiza un ping a mail Y1.

        Suponiendo que las tablas CAM de los *switches* estuvieren vacías, entonces dbY, según su tabla de ruteo, sabe la IP de mailY1. Entonces, procede a lanzar un ARP *request* que, eventualmente, contestará mailY1 a dbY, adjuntando su MAC que será guardada en la tabla CAM del switch-Y-1 con la interfaz e0 y en la tabla ARP de dbY.

5. Describa qué sucede cuando desde pcX1 se intenta acceder a una IP inexistente de la red 13.22.33.0/24 indicando:

    1. El camino que toma el datagrama IP.

    2. En caso de que se genere un mensaje de respuesta indicando que la IP no existe, indique qué protocolo se utiliza, quién la envía y quién la recibe

6. Indique verdadero o falso según corresponda. Justifique en ambos casos.

    1. Agregar *hubs* en un dominio de *broadcast* mantiene la cantidad de dominios de colision mientras que agregar *switches* los aumenta.

        Falso. Agregar *hubs* en un dominio de *broadcast* aumenta la cantidad de dominios de colisión, ya que éstos están determinados por la cantidad de hubs. Agregar *switches* en un dominio de broadcast no afecta(mantiene) la cantidad de dominios de colisión.

    2. Las tablas de los *switches* asocian direcciones IP con puertos de *switch*. Las direcciones IP salen de los datagramas que pasan por el.

        Falso. Las tablas de los *switches* asocian MACs con interfaces del *switch*. Las MAC salen de los diferentes *ARP reply* que van pasando por allí.

    3. Es posible que dos aplicaciones en un servidor estén escuchando en el mismo número de puerto.

        Falso. Se asigna un puerto por aplicación. Dos aplicaciones no pueden estar escuchando de un mismo puerto.
    
    4. El protocolo HTTP puede utilizar tanto TCP como UDP como protocolo de transporte, en cambio IMAP y POP3 funcionan solo con TCP.

        Falso. HTTP, por definición, requiere de una capa de transporte confiable, por lo que TCP resulta más adecuado. Hay implementaciones de HTTP usando UDP pero no es HTTP en sí mismo(véase HTTPU). IMAP y POP3 usan solo TCP.

    5. Sin el servicio de DNS no funciona el envío de correo electrónico entre los servidores.

        Verdadero. El DNS es requerido por el envío de correo electrónico para buscar el *nameserver* que coincida con el nombre de dominio extraído por el MSA. Una vez encontrado el nameserver se procederá a filtrar los servidores mx para proceder con el envío del *mail*.