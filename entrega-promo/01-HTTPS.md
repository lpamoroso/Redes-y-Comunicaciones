<center><font size="6"><u>Grupo K</u></font></center>
<center><font size="5"><u>Laboratorio de HTTP</u></font></center>
<center><font size="4"> Amoroso, Lihuel Pablo 13497/2; Gasquez, Federico Ramón 13598/6</font></center>

1. **Con las premisas dadas en clase, debe resolver**
    1. **Utilizando la herramienta *curl* debe inscribirse en el <a href="http://redes.catedras.linti.unlp.edu.ar/loginregister">sitio</a>. Establezca como *User-Agent* un nombre de grupo que lo identifique. Se recomienda previamente inspeccionar con el navegador las variables a enviar. Cuando la inscripción sea satisfactoria, recibirá un *mail* para confirmar su clave. Ingrese con el *browser*, registre y guarde el *header* de autenticación que le da el sitio.**

        Se utilizó el comando:

        ```
        curl --data "username=fgasquez&email=lihueamoroso@hotmail.com&password=23456789&passwordConfirm=23456789&submit=Registro" -H "User-Agent: GrupoA" -i http://redes.catedras.linti.unlp.edu.ar/loginregister/ >> http_1
        ```

        El header obtenido fue el que sigue:

        ```
        HTTP/1.1 302 Found  
        Server: nginx  
        Date: Wed, 27 Mar 2019 22:54:02 GMT  
        Content-Type: text/html; charset=UTF-8  
        Transfer-Encoding: chunked  
        Connection: keep-alive  
        Set-Cookie: PHPSESSID=83l2fku1ovbctuhb8ou0fbvef1; path=/  
        Expires: Thu, 19 Nov 1981 08:52:00 GMT  
        Cache-Control: no-store, no-cache, must-revalidate  
        Pragma: no-cache  
        Location: index.php?action=joined  
        ```

    2. **Con el *token*, acceder utilizando *curl* los siguientes recursos protegidos:**

        1. **<a>http://redes.catedras.linti.unlp.edu.ar/acceso/</a>**

            **Incluir el *header* obtenido y, nuevamente, acceder usando el nombre de grupo como *User-Agent*. Le dará un *hash* que es necesario incluir en la entrega.**

            Para acceder se usó el comando de abajo:

            ```
            curl -H "authorization:Basic Zmdhc3F1ZXo6MjM0NTY3ODk=" -H "User-Agent: GrupoA" -i http://redes.catedras.linti.unlp.edu.ar/acceso/
            ```

            El header obtenido fue el que sigue:

            ```
            HTTP/1.1 200 OK  
            Server: nginx  
            Date: Mon, 01 Apr 2019 15:18:59 GMT  
            Content-Type: text/html; charset=UTF-8  
            Connection: keep-alive  
            Set-Cookie: PHPSESSID=62dh81nuk93ppjd3rmob8cm7v4; path=/  
            Expires: Thu, 19 Nov 1981 08:52:00 GMT  
            Cache-Control: no-store, no-cache, must-revalidate  
            Pragma: no-cache  
            ```

        2. **<a>http://redes.catedras.linti.unlp.edu.ar/proxy/</a>**

            **Haga lo mismo que antes, incluyendo la opcion **—noproxy** de *curl*.**

            El comando que utilizamos fue el siguiente:

            ```
            curl -I -H "authorization:Basic Zmdhc3F1ZXo6MjM0NTY3ODk=" -H "User-Agent: GrupoA" --noproxy "*" -i http://redes.catedras.linti.unlp.edu.ar/proxy/
            ```
            
            Con la opción **--noproxy**, obtuvimos este header:

            ```
            HTTP/1.1 301 Moved Permanently  
            Server: nginx  
            Date: Mon, 01 Apr 2019 15:20:22 GMT  
            Content-Type: text/html  
            Content-Length: 178  
            Location: http://redes.catedras.linti.unlp.edu.ar/proxy  
            Connection: keep-alive  
            ```

            Como se puede apreciar arriba, el *header* devuelve una redirección hacia <a>http://redes.catedras.linti.unlp.edu.ar/proxy</a>.  
            Si, en cambio, el comando que utilizamos es el que sigue:

            ```
            curl -I -H "authorization:Basic Zmdhc3F1ZXo6MjM0NTY3ODk=" -H "User-Agent: GrupoA" --noproxy "*" -i http://redes.catedras.linti.unlp.edu.ar/proxy
            ```

            Lo que obtenemos, entonces, es esto:

            ```
            HTTP/1.1 301 Moved Permanently  
            Server: nginx  
            Date: Mon, 01 Apr 2019 15:39:29 GMT  
            Content-Type: text/html; charset=iso-8859-1  
            Connection: keep-alive  
            Location: http://redes.catedras.linti.unlp.edu.ar:8080/proxy/  
            X-proxy: get_hash  
            ```

            Lo obtenido es también una redirección, esta vez, hacia el puerto 8080 del servidor.

        **En todos los casos, debe registrar los comandos enviados y los *headers* recibidos. Explique las redirecciones realizadas y los *header* de autorización. Identifique los *header* personalizados. Debe incluir el *header* de autenticación y el *hash* obtenido. Con los datos obtenidos de los *header* ¿Puede decodificar el contenido del *header* de autorización? ¿Cómo?**

        Si, se puede decodificar usando <a href="base64decode.org">un decodificador de base-64</a>. El resultado es la concatenación usando el formato *usuario:contraseña*.

2. **Usando *curl* haga un requerimiento a <a>https://unlp.edu.ar</a>. Seguidamente, haga un requemiento cambiando el header de host por otro cualquiera ¿Qué observa?**

    Usamos el siguiente comando apra realizar un requerimiento a <a>https://unlp.edu.ar</a>:

    ```
    curl --head -i https://unlp.edu.ar
    ```

    Lo que obtuvimos fue lo siguiente:

    ```
    HTTP/1.1 200 OK  
    Content-Type: text/html;charset=utf-8  
    Cache-Control: public, max-age=120, s-maxage=240  
    X-XSS-Protection: 1; mode=block  
    X-Content-Type-Options: nosniff  
    X-Frame-Options: SAMEORIGIN  
    Vary: Accept-Encoding  
    Date: Tue, 02 Apr 2019 23:48:09 GMT  
    X-Varnish: 3377129 3316532  
    Age: 114  
    Via: 1.1 varnish-v4  
    Accept-Ranges: bytes  
    ```

    Hasta acá todo perfecto. Luego hicimos un requerimiento al mismo sitio pero esta vez cambiando el header de host por "<a>google.com</a>". Este fue el comando:

    ```
    curl -H 'host: google.com' --head -i https://unlp.edu.ar
    ```

    Este fue el resultado:

    ```
    HTTP/1.0 503 Service Unavailable  
    Cache-Control: no-cache  
    Connection: close  
    Content-Type: text/html  
    ```

    Lo que se puede observar es que el comando falló debido a la naturaleza de la cabecera host. Lo que el comando plantea es que se haga una petición al servidor de <a>https://unlp.edu.ar</a> pero indicando que queremos acceder al VirtualHost llamado <a>google.com</a>. Aparentemente, el dominio no provee tal servicio y falló arrojando un error 503.