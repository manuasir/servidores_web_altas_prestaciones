# Práctica 6

En ésta práctica se va a proceder a la configuración e implementación de un sistema RAID a partir de un sistema ya instalado. Para ello se va a comenzar
con la creación de dos nuevos discos virtuales de 1 GB de capacidad cada uno, para instalar sobre ellos un sistema RAID 1 o espejo.

![Alt text](c1.PNG?raw=true)

Con el comando 'fdisk -l' comprobamos que el sistema ha reconocido los dispositivos físicos de almacenamiento:

![Alt text](c2.PNG?raw=true)

Y a continuación procedemos a crear el dispositivo RAID 1 con la herramienta 'mdadm' , la cual se ha tenido que instalar mediante el gestor de paquetes
que se prefiera en caso de no estar previamente instalada en el servidor.


![Alt text](c3.PNG?raw=true)

Una vez creado, y antes de reiniciar el servidor vamos a dotar al dispositivo RAID de un sistema de ficheros, para éste caso el sistema ext2

![Alt text](c4.PNG?raw=true)

El siguiente paso es crear un directorio el cual se utilizará como punto de anclaje para nuestro almacenamiento en RAID, para éste caso se ha creado
un directorio en el raíz del sistema llamado /dat, en el cual montaremos el dispositivo

![Alt text](c5.PNG?raw=true)

Si dejamos la configuración tal cual está,necesitaremos realizar el montaje en cada inicio del sistema. Para evitar ésto hemos de modificar el fichero 
'/etc/fstab' y añadir una línea identificando nuestro dispositivo RAID con el punto de montaje que se elija. Para ello se necesitará conocer con antelación
el UUID del dispositivo, el cual podemos consultar en /dev/:

![Alt text](c7.PNG?raw=true)

Tras realizar ésta secuencia, reiniciaremos la máquina para comprobar que la configuración permanece sólida. El nombre del dispositivo habrá cambiado de md0
a md127. 
En el siguiente paso vamos a simular un fallo en uno de los dos discos del RAID, para éste caso será el disco /dev/sdc.
En el punto de montaje se han creado sendos documentos y directorios,para comprobar tras las pruebas de simulación que la integridad y disponibilidad
de los datos no se han visto comprometidas.

![Alt text](c8.PNG?raw=true)

Como puede observarse, uno de los dispositivos figura como 'faulty spare'. Efectivamente es el disco sobre el cual hemos provocado el fallo.
Ahora corresponde eliminar el disco del dispositivo RAID y a continuación reemplazarlo, para ello se usaron las siguientes secuencias de comandos:


![Alt text](c9.PNG?raw=true)

Como se aprecia en las capturas de pantalla, al añadir el nuevo disco y consultar el estado del RAID puede que nos aparezca su estado como 'spare rebuilding',
ésto es porque la información entre ambos está siendo sincronizada. Tras aguardar unos segundos se comprueba que la sincronización se realizó 
satisfactoriamente y comprobamos que el estado de ambos discos es 'active sync'. 
Si tras ésto accedemos a nuestro punto de montaje y comprobamos que la información permanece tal cual la creamos, la configuración se realizó con éxito.

![Alt text](c10.PNG?raw=true)