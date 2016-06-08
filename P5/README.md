# Práctica 5

En ésta práctica se va a implementar un sistema de bases de datos Mysql redudante entre dos máquinas, de forma que la información de un servidor maestro se vea replicada en otro esclavo.
Para ello en primer lugar hay que proceder a instalar Mysql, para ésta instancia se utilizará la versión 5.5.
En primer lugar se creó una base de datos llamada 'contactos' , la cual almacenará una tabla con dos campos, nombre y teléfono.
Insertaremos algunos registros en ella:

	mysql>  insert into datos(nombre,tlf) values ("manu",660773539);
	Query OK, 0 rows affected (0,01 sec)

La primera configuración que se realizará será un duplicado manual de ésta con la herramienta mysqldump

	root@1server:~#mysqldump contactos -u root -p > /root/contactos.sql

De ésta forma obtenemos la base de datos en un fichero de texto lista para ser importada manualmente en el servidor de copias.
Para ello basta con el comando "secure copy" (scp):

	swap@2server:~$ sudo scp swap@maquina1:/root/contactos.sql /root/

Una vez contamos con el volcado de la base de datos, se ha de crear la base de datos que contendrá la réplica:

	mysql -u root -p contactos < /root/contactos.sql

Con un simple "select * from datos" se puede comprobar que la información que se refleja es la misma en ambas bases de datos.
Ya que ésta no es la manera más óptima de proceder, se debe automatizar el proceso. Para ello haremos uso de los mecanimos que nos provee Mysql.

![Alt text](c1.PNG?raw=true)

Tras haber realizado una copia de seguridad del fichero /etc/mysql/my.cnf, procedemos a modificar las siguientes líneas: 
	bind-address 127.0.0.1 -> debe ser comentada para permitir conexiones externas	
	log_error = /var/log/mysql/error.log -> directorio donde se guardarán logs
	server-i -> identificador de cada servidor, en nuestro caso 1 y 2
	log_bin -> directorio donde se almacenarán los logs relacionados con las actualizaciones.
	
Tras modificar, reiniciamos el servicio con #service mysql restart

En el maestro creamos las credenciales necesarias para la copia, conviene un usuario dedicado exclusivamente a ésta tarea, para ello:

	mysql> CREATE USER esclavo IDENTIFIED BY 'esclavo';
	mysql> GRANT REPLICATION SLAVE ON *.* TO 'esclavo'@'%'
	IDENTIFIED BY 'esclavo';
	mysql> FLUSH PRIVILEGES;
	mysql> FLUSH TABLES;
	mysql> FLUSH TABLES WITH READ LOCK;

![Alt text](C2.PNG?raw=true)

Resultado del comando SHOW MASTER STATUS en mysql en el servidor maestro.
Es el turno de configurar el esclavo, para ello ejecutamos un prompt de mysql y en él configuramos lo siguiente:

	mysql> CHANGE MASTER TO MASTER_HOST='172.16.0.100',
	MASTER_USER='esclavo', MASTER_PASSWORD='esclavo',
	MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=501,
	MASTER_PORT=3306;

Tras realizar el comando SHOW SLAVE STATUS\G; debemos apreciar la información de replicación, en particular el campo #Seconds_Behind_Master está a 0, lo que nos indica que la configuración es correcta.

![Alt text](c3.PNG?raw=true)

Como última comprobación añadiremos nuevos datos en el maestro, comprobando que satisfactoriamente se replican en el esclavo de inmediato.

