# Pr�ctica 6

En �sta pr�ctica se va a proceder a la configuraci�n e implementaci�n de un sistema RAID a partir de un sistema ya instalado. Para ello se va a comenzar
con la creaci�n de dos nuevos discos virtuales de 1 GB de capacidad cada uno, para instalar sobre ellos un sistema RAID 1 o espejo.

![Alt text](c1.PNG?raw=true)

Con el comando 'fdisk -l' comprobamos que el sistema ha reconocido los dispositivos f�sicos de almacenamiento:

![Alt text](c2.PNG?raw=true)

Y a continuaci�n procedemos a crear el dispositivo RAID 1 con la herramienta 'mdadm' , la cual se ha tenido que instalar mediante el gestor de paquetes
que se prefiera en caso de no estar previamente instalada en el servidor.


![Alt text](c3.PNG?raw=true)

Una vez creado, y antes de reiniciar el servidor vamos a dotar al dispositivo RAID de un sistema de ficheros, para �ste caso el sistema ext2

![Alt text](c4.PNG?raw=true)

El siguiente paso es crear un directorio el cual se utilizar� como punto de anclaje para nuestro almacenamiento en RAID, para �ste caso se ha creado
un directorio en el ra�z del sistema llamado /dat, en el cual montaremos el dispositivo

![Alt text](c5.PNG?raw=true)

Si dejamos la configuraci�n tal cual est�,necesitaremos realizar el montaje en cada inicio del sistema. Para evitar �sto hemos de modificar el fichero 
'/etc/fstab' y a�adir una l�nea identificando nuestro dispositivo RAID con el punto de montaje que se elija. Para ello se necesitar� conocer con antelaci�n
el UUID del dispositivo, el cual podemos consultar en /dev/:

![Alt text](c7.PNG?raw=true)

Tras realizar �sta secuencia, reiniciaremos la m�quina para comprobar que la configuraci�n permanece s�lida. El nombre del dispositivo habr� cambiado de md0
a md127. 
En el siguiente paso vamos a simular un fallo en uno de los dos discos del RAID, para �ste caso ser� el disco /dev/sdc.
En el punto de montaje se han creado sendos documentos y directorios,para comprobar tras las pruebas de simulaci�n que la integridad y disponibilidad
de los datos no se han visto comprometidas.

![Alt text](c8.PNG?raw=true)

Como puede observarse, uno de los dispositivos figura como 'faulty spare'. Efectivamente es el disco sobre el cual hemos provocado el fallo.
Ahora corresponde eliminar el disco del dispositivo RAID y a continuaci�n reemplazarlo, para ello se usaron las siguientes secuencias de comandos:


![Alt text](c9.PNG?raw=true)

Como se aprecia en las capturas de pantalla, al a�adir el nuevo disco y consultar el estado del RAID puede que nos aparezca su estado como 'spare rebuilding',
�sto es porque la informaci�n entre ambos est� siendo sincronizada. Tras aguardar unos segundos se comprueba que la sincronizaci�n se realiz� 
satisfactoriamente y comprobamos que el estado de ambos discos es 'active sync'. 
Si tras �sto accedemos a nuestro punto de montaje y comprobamos que la informaci�n permanece tal cual la creamos, la configuraci�n se realiz� con �xito.

![Alt text](c10.PNG?raw=true)