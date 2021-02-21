# Practica integradora

1. En base al diagrama y a la salida de los dos comandos que se muestran debajo, y considerando que ambos comandos fueron ejecutados en PC-A, responda los items que siguen.

    a. ¿Qué protocolo de capa de aplicación se está utilizando?

    DNS.

    b. ¿Qué protocolo de capa de transporte se está utilizando?

    UDP.

    c. Complete, donde aparecen los guiones bajos, el registro por el que se hizo la consulta y se recibió la respuesta.

    La opción por la que pregunta el dig es **ns**. Excepto el *additional section*, es *ns*. El *additional section* es **a**.

    d. ¿A qué servidor se le realizó la consulta? Justifique.

    La consulta fue realizada al servidor 200.40.165.80 dado que, cuando el comando *cat /etc/resolv.conf* es ejecutado, lo devuelve, indicando que es el *resolver*.

    e.  ¿La consulta fue recursiva? Justifique.

    La consulta fue recursiva ya que aparece el flag **rd**(*recursive desired*).

    f. ¿La respuesta fue recursiva? Justifique.

    La respuesta fue recursiva ya que aparece el flag **ra**(*recursive answer*).

    g. ¿La respuesta fue autoritativa? Justifique. Si considera que no fue autoritativa, explique cómo haría para obtener una respuesta autoritativa.

    No, no fue autoritativa dado que el flag que lo indica no está activado. Para obtener una respuesta autoritativa habría que cambiar el *resolver* al root server para que cuando se realice la consulta pregunte directamente al root. 

    h. ¿Qué significa el número 600? ¿Para qué sirve? ¿Qué ocurrirá mientras ese registro no llegue a 0? ¿Y cuándo llegue a 0? ¿Quién establece ese número?

    Representa el *time to live*. Lo define el servidor DNS autoritativo para ese dominio. Mientras no llegue a 0 seguirá cacheado allí. Cuando llega a 0, habrá que consultarlo a los *root servers*.

2. Responda los incisos que siguen, en base a la salida del comando debajo y lo que ya sabe sobre la PC-A y el diagrama.

    a. En el comando anterior, ¿Qué protocolo de capa de aplicación y qué método de ese protocolo está utilizando? Justifique.

    El protocolo es HTTP 1.1 y el método es el **HEAD**.

    b.  ¿Qué significa la respuesta recibida? Justifique.

    Significa que fue permanentemente mudada a *https://www.redes.unlp.edu.ar/*. Porque así lo indica el código de error(301 Moved Permanently).

    c. Si quisiera obtener el contenido de la página, ¿Qué método utilizaría? Justifique.

    El **GET** ya que es el que es usado para pedir datos desde un lugar determinado.

    d. Si estuviera usando un navegador web, ¿qué ocurriría al acceder a http://www.redes.unlp.edu.ar?
    ¿Por qué cree que ocurre eso?

    Interpreta el error y redirige a *https://www.redes.unlp.edu.ar/*.

    e.  ¿Qué protocolo de capa de transporte y qué puertos utilizaría en el caso anterior? Justifique.

    El de TCP, dado que requiero una conexion segura y confiable en la que no se pierdan datos. El puerto es el *default* 80.

    f. Liste, en orden, todas las consultas DNS que deberán realizarse, indicando el registro de la consulta, quién la realiza, a quién se la realiza, cuál es la respuesta recibida y qué tipo de consulta es (recursiva o iterativa). Tenga en cuenta que cuando consulta por el servidor de nombres de un dominio, podría recibir un nombre en lugar de una dirección IP. ¿Implicaría eso alguna consulta adicional? Justifique.

3. Suponga que en la PC-A hay un cliente de correo configurado con la cuenta lionel@gmail.com que desea enviar un mail a diego@redes.unlp.edu.ar. Su cliente de correo tiene la siguiente configuración para el servidor saliente:

Indique, en orden, los pasos para que el correo que envía Lionel llegue al servidor de correo de Diego.
Considere
todos
los protocolos de aplicación y transporte que intervienen. Para cada paso especifique:
Protocolo de aplicación y datos relevantes (si los hubiera).
Protocolo de transporte y puerto.
IP destino de la conexión (si la conoce).