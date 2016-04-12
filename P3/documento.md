# Práctica 3

Para ésta práctica se va a realizar la configuración del servidor que realizará funciones de balanceo de carga, el primer paso es instalar Nginx como software de balanceo. Debemos asegurarnos de que no contamos con servidor web instalado en la máquina que pudiera apropiarse del puerto 80.

Para instalar el software en primer lugar se ha de descargar y añadir la clave del repositorio:

![Alt text](p3-1.JPG?raw=true)

El siguiente paso es añadir el repositorio a la lista de repositorios del sistema 

- echo "deb-src http://nginx.org/packages/ubuntu/ lucid nginx" >> /etc/apt/sources.list
- echo "deb http://nginx.org/packages/ubuntu/ lucid nginx" >> /etc/apt/sources.list

y finalmente actualizar la lista y descargar el paquete:

- apt-get update
- apt-get install nginx

Ya sólo queda configurar Nginx como balanceador de carga. Para configurar el servicio se ha de modificar el fichero de configuración situado en /etc/nginx/conf.d/default.conf.
Como siempre, el primer paso antes de modificar ficheros de configuración es realizar una copia de seguridad del mismo por los futuros problemas que pudieran encontrarse (y que con probabilidad se encontrarán).

- cp default.conf default.conf.bak

Eliminamos todo el contenido del fichero para crear nuestra propia configuración, la cual quedó de la siguiente manera

![Alt text](p3-2.JPG?raw=true)

Tras reiniciar el servicio de nginx procedemos a realizar una petición desde un navegador a la IP del balanceador, comprobando que resulta exitosa.
En cada petición debe alternarse el servidor al que se apunta al haber configurado el algoritmo round-robin con un sistema de prioridades basado en un parámetro de peso en el balanceador. Para comprobar ésto, antes de que se sincronice el espacio web en los servidores de la granja, realizamos modificaciones en los HTML que se sirven para comprobar a qué máquina se está apuntando.
Comprobamos con éxito que el balanceador muestra el HTML modificado correspondiente al primer servidor dos de cada tres peticiones como está configurado.

![Alt text](p3-3.JPG?raw=true)
![Alt text](p3-4.JPG?raw=true)

El siguiente paso es instalar y configurar el software haproxy. Procedemos por el mismo procedimiento modificando el fichero de configuración una vez instalado /etc/haproxy/haproxy.cfg, no sin antes realizarle una copia de seguridad.

![Alt text](p3-5.JPG?raw=true)

Como el servicio nginx sigue en funcionamiento en nuestra máquina escuchando en el puerto 80, para iniciar el servicio haproxy podemos o parar el servicio nginx para que haproxy use el puerto 80 o configurarlo para que use un puerto diferente, en éste caso el puerto 8080.
Reiniciamos el servicio y comprobamos el balanceo:

![Alt text](p3-6.JPG?raw=true)
![Alt text](p3-7.JPG?raw=true)

Comprobamos con éxito que el balanceo se realiza adecuadamente.
Adicionalmente se instalará el software Varnish para el balanceo de la carga, para ello:
- apt-get install varnish

Y luego configuramos el servicio de la siguiente manera en el fichero de configuración /etc/varnish/default.cfg

![Alt text](p3-8.JPG?raw=true)

Reiniciamos el servicio:

- service varnish restart

Y comprobamos cómo nos dirige alternativamente al cluster web. Por defecto este servicio utiliza el puerto 6081, ésto se puede modificar en el fichero de documentación localizado en /etc/default/varnish


