#Práctica 4


En ésta práctica vamos a comprobar el rendimiento de nuestros servidores web, para ello vamos a hacer distintas pruebas con diferentes benchmark para contrastar los resultados.
Para este fin se ha creado un script que automatice el proceso, realizando las peticiones pertinentes a los distintos balanceadores configurados así como de una máquina servidora de la granja (máquina 1). 


	#!/bin/bash
	for i in `seq 1 10`;do
			ab -n 1000 -c 5 http://192.168.0.156/index.html >> maquina1-ab.txt
			ab -n 1000 -c 5 http://192.168.0.162/index.html >> balanceador-ab-nginx.txt
			ab -n 1000 -c 5 http://192.168.0.162:6081/index.html >> balanceador-ab-varnish.txt
			siege -b -t5S -v http://192.168.0.156/index.html 2> maquina1-siege-ngix.txt
			siege -b -t5S -v http://192.168.0.162/index.html 2> balanceador-siege-nginx.txt
			siege -b -t5S -v http://192.168.0.162:6081/index.html 2> balanceador-siege-varnish.txt
	done
	cat maquina1-ab.txt | grep "Time taken for tests" >> maquina1-ab.csv
	cat maquina1-ab.txt | grep "Failed requests" >> maquina1-ab.csv
	cat maquina1-ab.txt | grep "Requests per second" >> maquina1-ab.csv

	cat balanceador-ab-nginx.txt | grep "Time taken for tests" >> balanceador-ab-nginx.csv
	cat balanceador-ab-nginx.txt | grep "Failed requests" >> balanceador-ab-nginx.csv
	cat balanceador-ab-nginx.txt | grep "Requests per second" >> balanceador-ab-nginx.csv
	cat balanceador-ab-haproxy.txt | grep "Time taken for tests" >> balanceador-ab-haproxy.csv
	cat balanceador-ab-haproxy.txt | grep "Failed requests" >> balanceador-ab-haproxy.csv
	cat balanceador-ab-haproxy.txt | grep "Requests per second" >> balanceador-ab-haproxy.csv
	cat maquina1-siege-nginx.txt | grep "Transactions" >> maquina1-siege-nginx.csv
	cat maquina1-siege-nginx.txt | grep "Throughput" >>  maquina1-siege-nginx.csv
	cat maquina1-siege-nginx.txt | grep "Successful" >> maquina1-siege-nginx.csv


	cat balanceador-siege-nginx.txt | grep "Transactions" >> balanceador-siege-nginx.csv
	cat balanceador-siege-nginx.txt | grep "Throughput" >> balanceador-siege-nginx.csv
	cat balanceador-siege-nginx.txt | grep "Successful" >> balanceador-siege-nginx.csv

	cat balanceador-siege-varnish.txt | grep "Transactions" >> balanceador-siege-varnish.csv
	cat balanceador-siege-varnish.txt | grep "Throughput" >> balanceador-siege-varnish.csv
	cat balanceador-siege-varnish.txt | grep "Successful" >> balanceador-siege-varnish.csv

Los resultados se muestran en las diferentes gráficas adjuntadas a continuación:

Ejecuciones de AB sobre el balanceador NGINX
![Alt text](c1.PNG?raw=true)


Ejecuciones de AB sobre el balanceador VARNISH:

![Alt text](c2.PNG?raw=true)

Ejecuciones de AB sobre el servidor 1:

![Alt text](c3.PNG?raw=true)

## Para la aplicación siegue se utilizó 15 segundos como parámetro

Ejecuciones de Siege sobre el balanceador NGINX:

![Alt text](c4.PNG?raw=true)

Ejecuciones de Siege sobre el balanceador VARNISH:

![Alt text](c5.PNG?raw=true)

Ejecuciones de Siege sobre la maquina 1:

![Alt text](c6.PNG?raw=true)
