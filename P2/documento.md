#Práctica 2

Se instalaron tres servidores Ubuntu Server en máquinas virtuales en VMWare Workstation, de los cuales uno tendrá funciones de balanceo de carga. Los servidores pertenecen a un segmento de red virtual privada además de una red externa para administración.

La configuración que se les asignó a los servidores es la siguiente:
-512 MB de memoria RAM
-1 procesador multithread con 2 hebras
-20 GB de almacenamiento 
- Sistema operativo Ubuntu Server 14.04 x64
-Dos tarjetas de red por cada servidor Fast Ethernet

Para la configuración de la red se creó un segmento de red en VMware al que se conectaron los tres servidores.
La red asignada a éste segmento es la 172.16.0.0/24.

Por otra parte en la segunda tarjeta de red de cada uno se le configuró conectividad Bridge para la red de administración y poder acceder remotamente a cada uno de ellos vía SSH. Ésta red cuenta con un servidor DHCP que asigna direcciones dinámicas a cada interfaz de red, en éste caso en la red 192.168.0.0/24.

Una vez que se haya comprobado la conectividad entre las máquinas, procedemos a realizar la generación de claves públicas y privadas

