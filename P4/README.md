#Practica 4


En ésta práctica vamos a comprobar el rendimiento de nuestros servidores web, para ello vamos a hacer distintas pruebas con diferentes benchmark para contrastar los resultados.

![Alt text](c3.PNG?raw=true)

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

	
	