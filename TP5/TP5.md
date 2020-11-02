#### 3- Introducción a Maven
- Qué es Maven?
	Maven es una herramienta de software utilizada para la gestion y construccion de proyectos JAVA. Su modelo de configuracion (POM) esta basado en XML. Utiliza el POM (Proyect Object Model) para describir el proyecto de software.
- Qué es el archivo POM?
	El POM es el archivo que describe como se va a construir el software, cuales son sus dependecias de otros modulos, componentes externeos y el orden de construccion de los elementos.
    1. modelVersion: especifica la version del modelo. En Maven 2 y 3 es siempre 4.0.0.
    2. groupId: sirve para identificar de forma unica un proyecto, respecto el grupo de proyectos al que pertenece.
    3. artifactId: es el nombre del archivo jar sin version.
    4. versionId: aca se elige la version que se va a utilizar.
    
- Repositorios Local, Central y Remotos:
    1. Repositiorio Local: directorio en la computadora donde se corre Maven. Almacena en cache las descargas remotas.
    2. Repositorio Central: repositorio donde se encuentran las principales librerias de Maven.
    3. Repositorio Remoto: se accede a traves de protocolos como https. Se utilizan para proporcionar artefactos en descargas.
    
- Entender Ciclos de vida de build
    1. default: toma el deploy del proyecto. Realiza todas las tareas, desde validacion, inicializacion, compilacion, tests, instalacion, hasta el deploy.
    2. clean: realiza trabajos de limpieza del proyecto. Por ejemplo, borrar todos los archivos generados previamente al realizar un nuevo build.
    3. site: se centra en la creacion de un sitio. Deploya en un webserver especificado todo lo indicado.
    
    
- Ejecutar mvn clean install y sacar conclusiones del resultado.
    Este comando lo que realiza es una limpia instalacion de todo lo que indica el archivo pom creado. Al decir que lo realiza de una manera limpia, nos referimos a que si se encotraban otros archivos ya descargados, se eliminaban y se volvian a descargar.
    
- Ejecutar mvn clean package, que significa el comando?
    El comando lo que hace es crear un archivo jar ejecutable para luego poder correrlo en Java.
    
- Crear un nuevo proyecto con artifactId ejemplo-uber-jar:
    Comando utilizado: mvn archetype:generate -DgroupId=ar.edu.ucc -DartifactId=ejemplo-uber-jar -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

- Compilar el código e identificar el problema.
    El error que figura es el siguiente: package org.slf4j does not exist


