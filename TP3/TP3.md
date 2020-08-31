###1. Verificar el estado de los contenedores y redes en Docker, describir:

    **Cuales puertos están abiertos?**
    	Los puertos que estan abiertos son el 5000 por la web y el 6379 el cual utiliza Redis.
    **Mostrar detalles de la red mybridge con Docker.**
    [
    {
        "Name": "mybridge",
        "Id": "8f08b5c04027335a21c76a52fce61f3b0ac18d364a2a47f8e8f480d4e61c1963",
        "Created": "2020-08-24T13:59:25.393281528-03:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "1d3be3f16978c62e16e9601689187f49b2131ab853ffafe593c829cd70efec77": {
                "Name": "web",
                "EndpointID": "fc14c2cce272f18703f2942f9e6c535c7a0b4573713de5ea546997c671cc3f7a",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "cf43fb6001f7bab826af4e3599f0073066ce332be97291d53ba3e0d2ee5bf823": {
                "Name": "db",
                "EndpointID": "0d6a5126ac9f5de1237836779a352a4d3bad07f4c8c8ad08e7347a5e83b4036e",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
    **Qué comandos utilizó?** 
    	Para ver los puertos abiertos se utilizo "docker ps", y para visualizar la red mybridge, se utilizo el comando "docker network inspect mybridge".
    	
###2. Siendo el código de la aplicacion web el siguiente:

import os

from flask import Flask
from redis import Redis


app = Flask(__name__)
redis = Redis(host=os.environ['REDIS_HOST'], port=os.environ['REDIS_PORT'])
bind_port = int(os.environ['BIND_PORT'])


@app.route('/')
def hello():
    redis.incr('hits')
    total_hits = redis.get('hits').decode()
    return f'Hello from Redis! I have been seen {total_hits} times.'


if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True, port=bind_port)
    
    **Explicar como funciona el sistema.**
    	En el sistema podemos observar que tenemos dos contenedores conectadas por una red. En la app web podemos observar que se accede a la base de datos de redis para almacenar la cantidad de veces que se entro a la web ("redis.incr('hits')") y para recibir tambien este dato (redis.get('hits').decode()) y luego mostrarlo ({total_hits}) en la pagina. 
    	
   **¿Para qué se sirven y porque están los parámetros -e en el segundo Docker run del ejercicio 1?**
   	Variable de entorno utilizada por el script `client.js` para crear un nuevo cliente Redis (enviroment).
      El nombre del host utilizado para comunicarse con el contenedor del servicio Redis
      REDIS_HOST: db
      El puerto Redis predeterminado
      REDIS_PORT: 6379
      
   **¿Qué pasa si ejecuta docker rm -f web y vuelve a correr docker run -d --net mybridge -e REDIS_HOST=db -REDIS_PORT=6379 -p 5000:5000 --name web alexisfr/flask-app:latest?**
   	Cuando borramos la pagina web y luego la volvemos a correr, con el mismo comando, tenemos que tener en cuenta que nunca cambiamos los datos del redis, entonces el contador de cantidad de veces de la pagina visitada sigue siendo el mismo que antes.
   	
   **¿Qué occure en la página web cuando borro el contenedor de Redis con docker rm -f db?**
   	Al borrar el contenedor de redis, obtenemos un error en la pagina web, ya que no puede realizar ninguna conexion con la base de datos redis.
   	
   **Y si lo levanto nuevamente con docker run -d --net mybridge --name db redis:alpine?**
   	Ahora si, el contador inicio de vuelta en cero, porque la base de datos fue eliminada y comenzada nuevamente.
   	
   **¿Qué considera usted que haría falta para no perder la cuenta de las visitas?**
   	Para no perder la cuenta de las visitas, se podria realizar backups de la base de datos. Se puede crear un volumen que contenga estos datos, guardandose de esta forma, cuando la pagina web o la base de datos esten caidos.
 
###3. Utilizar docker compose

  **¿Qué hizo Docker Compose por nosotros? Explicar con detalle.**
  	Lo que hizo docker compose fue crear un nuevo bridge, el cual se llama dockercompose_default. Este bridge sirve para unir la pagina web que se mapea en el puerto 5000 con la base de datos que almacena la cantidad de veces que la pagina fue visitada. Tambien se puede observar que esa base de datos esta almacenada en un volumen especifico, de esta forma, si hacemos un down y un up nuevamente a docker-compose, la cantidad de visitas en la base de datos no se pierde. Esto es porque se almacena en nuestra maquina, a traves de los volumes de docker.
  	
###4- Aumentando la complejidad, análisis de otro sistema distribuido.
	
  **Explicar como está configurado el sistema, puertos, volumenes componenetes involucrados, utilizar el Docker compose como guía.**
    Una aplicación web de Python que te permite votar entre dos opciones: esta app utiliza el puerto 5000 de nuestra computadora que refleja el puerto 80 del contenedor docker. La eleccion del usuario la manda a redis.
    Una cola de Redis que recolecta nuevos votos: se accede a traves del puerto 6379.
    Un trabajador .NET o Java que consume votos: depende de redis y de la base de datos, esto porque toma los elementos que se encuentran en la cola de redis y luego los almacena a los votos en la base de datos Postgres.
    Una base de datos de Postgres respaldada por un volumen de Docker: esta base de datos en postgress almacena todos los votos realizado en la aplicacion de python y tiene un respaldo en el volume "db-data"
    Una aplicación web Node.js que muestra los resultados de la votación en tiempo real: esta alplicacion web se corre en el puerto 80 de su contenedor, el cual se refleja en nuestro puerto 5001. Necesita para esto, los datos almacenados en la base de datos de Postgres.

###5- Análisis detallado
  	
  	
	


	
   	


    	 

