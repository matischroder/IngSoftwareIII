Ejercicios:

1. La version que tenia instalada en docker (docker version) es la 19.03.12

3. La version que obtuve de busybox (docker images) fue la ultima subida.

4. Cuando ejecutamos con 'docker run busybox', no se obtiene ningun resultado concreto porque simplemente se ejecuto una vez busybox y luego se dejo de ejecutar. 
Cuando ejecutamos con 'docker run busybox echo "Hola Mundo"', se obtiene el echo del Hola Mundo realizado por busybox y nuevamente deja de ejecutarse.
La salida del comando 'docker ps -a' muestra las imagenes ejecutadas (multiples contenedores o procesos que se crearon) ultimamente sin importar si siguen o no en ejecucion. Por eso mismo podemos observar en la imagen que hace 3 segundos en busybox fue ejecutado el comando 'echo "Hola Mundo"', el cual tiene un id de contenedor distinto al que habia ejecutado anteriormente.

![alt text](https://github.com/matischroder/IngSoftwareIII/blob/master/TP2/dockerPs-a.png?raw=true)

5. Al ejecutar con 'docker run -it busybox sh' podemos ver que se ejecuto dentro de busybox lo que es el Shell-script de linux, es decir se abre la consola del busybox. El '-it' hace referencia a que se ejecute de forma interactiva, para que esta consola de busybox este disponible para el usuario. Entonces, si abrimos otra consola e introducimos 'docker ps -a', nos va a figurar que se esta corriendo el proceso sh dentro de un contenedor busybox distinto a los otros busybox que creamos anteriormente. Para poder ingresar nuevamente al contenedor creado se utiliza el comando start con la flag i que indica un ingreso interactivo nuevamente 'docker start -i strange_raman'.

Resultados de los comandos ps, uptime y free:

![alt text](https://github.com/matischroder/IngSoftwareIII/blob/master/TP2/comandosBusybox.png?raw=true)

9. En este ejercicio, con el docker run logramos ejecutar la imagen de postgres y de esta forma crear un contenedor de postgres en nuestra computadora (se puede crear mas de un contenedor postgres al mismo tiempo). Luego, con el docker exec pudimos ejecutar la aplicacion bin/bash que se encuentra dentro del postgres. A traves de esa consola ingresamos a postgres sql y creamos una nueva tabla.

