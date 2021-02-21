# Capa de Red - Ruteo

#### Aclaración importante

1. En CORE no se guardan los cambios realizados en una topología al detenerla. Por ello, es deseablecompletar todo el ejercicio una vez empezado, para no tener que volver a configurar todo. Alternativa-mente se puede utilizar el script que se encuentra en este repositorio https://github.com/RYSAEI/Save-RestoreScripts para forzar que se guarden los cambios.

#### Ruteo

2. En las redes IP el ruteo puede configurarse en forma estática o en forma dinámica. Indique ventajas y desventajas de cada método.

    * Ruteo estático: no tienen en cuenta el estado de la subred al tomar las decisiones de ruteo. Las tablas de ruteo se configuran de forma manual y permanecen inalterables hasta que no se vuelve a actuar sobre ellas. Por tanto, la adaptación en tiempo real a los cambios de las condiciones de la red es nula. Los algoritmos de este tipo se caracterizan por ser rígidos, rápidos y de diseño simple, pero, además, son los que suelen tomar peores decisiones.
    * Ruteo dinámico: hacen que la red sea más tolerante a cambios tales como variaciones en el tráfico, incremento del retardo o fallas en la topología. Tiene 3 categorías:
        + Adaptativo centralizado: todos los nodos de la red son iguales excepto un nodo central que es quien recoge la información de control y los datos de los demás nodos para calcular con ellos la tabla de ruteo. Este método tiene el inconveniente de que consume abundantes recursos de la propia red.
        + Adaptativo distribuido: este tipo de ruteo se caracteriza porque el algoritmo correspondiente se ejecuta por igual en todos los nodos de la subred. Cada nodo recalcula continuamente la tabla de ruteo a partir de dicha información y de la que contiene en su propia base de datos. A este tipo pertenecen dos de los más utilizados en internet que son los algoritmos por vector de distancias y lo de estado de enlace.
        + Adaptativo aislado: se caracterizan por la sencillez del método que utilizan para adaptarse al estado cambiante de la red. Su respuesta a los cambios de tráfico o de topología se obtiene a partir de información propia y local de cada nodo. Un caso típico es el ruteo por inundación, cuyo mecanismo consiste en reenviar cada paquete recibido con destino a otros nodos, por todos los enlaces excepto por el que llegó.

3. Una máquina conectada a una red pero no a Internet ¿Tiene tabla de ruteo?

    Si, tiene tabla de ruteo en la que aparecen, al menos, las interfaces de la máquina.

5. Con la máquina virtual con acceso a Internet realice las siguientes observaciones respecto de la auto-configuración IP vía DHCP:

    1. Inicie una captura de tráfico Wireshark utilizando el filtro bootp para visualizar únicamente tráfico de DHCP.

    2. En una terminal de root, ejecute el comando sudo _/sbin/dhclient eth0_ y analice el intercambio de paquetes capturado.

        Se generaron 4 paquetes: DHCP discover, DHCP offer, DHCP request, DHCP ACK.

    3. Analice la información registrada en el archivo /var/lib/dhcp/dhclient.leases ¿Cuál parece su función?

        Tiene todos los contratos de arrendamiento.

    4. Ejecute el siguiente comando para eliminar información temporal asignada por el servidor DHCP.rm /var/lib/dhcp/dhclient.leases

    5. En una terminal de root, vuelva a ejecutar el comando sudo /sbin/dhclient eth0 y analice el intercambio de paquetes capturado nuevamente ¿A qué se debió la diferencia con lo observado en el punto b?

        Se debe a que ya estaba iniciado dhcp.

    6. Tanto en b como en e ¿Qué información es brindada al host que realiza la petición DHCP, además de la dirección IP que tiene que utilizar?

6. ¿Qué es NAT y para qué sirve? De un ejemplo de su uso y analice cómo funcionaría en ese entorno. Ayuda: analizar el servicio de Internet hogareño en el cual varios dispositivos usan Internet simultáneamente.