# 🌟 DOCKER 🌟
## ¿Qué es un contenedor?

Unidad de software que encapsula una aplicación y todas sus dependencias en un entorno aislado. Proporciona una forma de empaquetar, distribuir y ejecutar aplicaciones de manera consistente en diferentes entornos, ya sea en un entorno de desarrollo, pruebas o producción.

Los contenedores utilizan tecnologías de virtualización a nivel de sistema operativo para crear entornos ligeros y portátiles. Cada contenedor se ejecuta de forma independiente pero comparte el mismo kernel del sistema operativo subyacente.

Ventajas: portabilidad, escalabilidad y eficiencia en el uso de recursos.

## Conceptos básicos
Contenedor: Es una instancia de una imagen. Es un proceso que se ejecuta en un entorno aislado.

Imagen: Es un archivo binario que contiene todos los elementos necesarios para ejecutar un contenedor. Es como una plantilla que se utiliza para crear contenedores.

Dockerfile: Es un archivo de texto que contiene las instrucciones necesarias para crear una imagen.

Docker Hub: Es un repositorio de imágenes de contenedor. Es como un GitHub pero de imágenes de contenedor.
## Instalación en Linux
Utilizar el script de instalación oficial de Docker, que se encarga de instalar todas las dependencias necesarias y configurar el sistema para que Docker funcione correctamente
  
  curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh

Este comando, descarga el script de instalación y lo ejecuta. Este comando, deberás lanzado con permisos root o añadir sudo delante del comando sh que ejecuta el script.

## Comprobación de la instalación
    docker -- version

    docker run hello-world

Si todo ha ido bien, deberías ver un mensaje de bienvenida de Docker.

## Iniciar un contenedor
Para arrancar un contenedor, utilizamos el comando docker run. Este comando, nos permite arrancar un contenedor a partir de una imagen. Su sintaxis básica es la siguiente:

    docker run <imagen>

Por ejemplo, si queremos arrancar un contenedor nginx (un conocido servidor web), podríamos hacerlo con el siguiente comando:

    docker run nginx

Aunque no hayamos descargado una imagen, Docker se encargará de buscar la imagen en Docker Hub, descargarla y arrancar el contenedor de forma automática.

Si queremos ver los contenedores que tenemos en ejecución, podríamos hacerlo con el siguiente comando:

    docker ps

Podemos ver todos los contenedores, tanto los que están en ejecución como los que están parados, con el comando 
    
    docker ps -a.

Junto al comando docker run, podemos utilizar una serie de opciones para personalizar el comportamiento del contenedor. Algunas de las opciones más comunes son:

1. -d: Arranca el contenedor en segundo plano.

2. -p: Mapea un puerto del contenedor al puerto del host.

3. -v: Mapea un volumen del host al contenedor.

4. --name: Asigna un nombre al contenedor.

5. --rm: Elimina el contenedor al pararlo.

6. -e: Define una variable de entorno.

7. --env-file: Define un archivo de variables de entorno.

## Política de reinicio

Por defecto, cuando un contenedor falla o termina su proceso, Docker no lo reinicia por defecto. Si queremos que un contenedor se reinicie automáticamente, podemos utilizar la opción --restart. Esta opción nos permite definir una política de reinicio. Algunas de las políticas más comunes son:

1. no: No reinicia el contenedor.

2. always: Reinicia el contenedor siempre.

3. unless-stopped: Reinicia el contenedor siempre que no lo paremos.

4. on-failure: Reinicia el contenedor solo si falla.

5. on-failure:<n>: Reinicia el contenedor solo si falla n veces.

Por ejemplo, si queremos que un contenedor se reinicie siempre que falle o se reinicie el servidor anfitrión, podríamos hacerlo con el siguiente comando:

    docker run --restart always nginx

## Mapeo de puertos
El parámetro -p, mencionado anteriormente, nos permite mapear un puerto del host a un puerto del contenedor. En este caso, si queremos acceder al servidor web que hemos arrancado, necesitamos mapear el puerto 80 del contenedor un puerto del host. 

En este caso he optado por el 8080, ya que en windows y mac el 80 a veces esta en uso. Para ello, podemos utilizar el siguiente comando:

    docker run -d -p 8080:80 nginx

Ahora si, si accedemos a localhost:8080, podremos ver el servidor web de nginx respondiendo perfectamente. 

## Gestion de contenedores
Por razones de depuración, a veces necesitamos conectarnos a un contenedor en ejecución y ejecutar un comando en él que no sea el principal. Para ello, podemos utilizar el comando exec.

Por ejemplo, si quisieramos listar el contenido de un contenedor en ejecución, podríamos hacerlo con el siguiente comando:

    docker exec <id_contenedor> ls

Lo más común, ejecutar el entorno de bash, sh o zsh en un contenedor y así obtener un terminal interactivo. Para poder permitir hay que utilizar los parámtros i, interactive y t, de terminal. Por ejemplo:

    docker exec -it <id_contenedor> bash

Podemos detener un contenedor en ejecución con el comando 
    docker stop

Por ejemplo, si queremos parar el contenedor vigorous_noether, podríamos hacerlo con el siguiente comando:

    docker stop vigorous_noether

También podríamos parar el contenedor por ID:

    docker stop e9267a9edf3d

O, incluso podríamos pararlo solo especificando las primeras letras del ID:

    docker stop e92

Este truco del id parcial se puede utilizar en la mayoría de los comandos de Docker.

Podemos iniciar un contenedor que hemos parado con el comando 
    
    docker start

Recordemos que el comando run crearía un nuevo contenedor, mientras que el comando start iniciará un contenedor que ya ha sido creado previamente y que se encuentra parado.

Podemos reiniciar un contenedor con el comando 
    
    docker restart

Reiniciar un contenedor es equivalente a pararlo y volver a iniciarlo. Es decir, es como hacer un docker stop seguido de un docker start.

Podemos eliminar un contenedor con el comando 
  
    docker rm

    docker rm -f <id_contenedor> (para forzar la eliminación)

Podemos eliminar todos los contenedores parados con el comando 
  
    docker container prune

Este comando eliminará todos los contenedores que estén parados. Es útil para limpiar el sistema de contenedores que ya no necesitamos.

## Imagenes de contenedores 
Dos conceptos imporantes: una vez que creamos una imagen no se puede modificar (inmutabilidad) y están formadas por capas, cada una de ellas con una funcionalidad concreta.
Para descargar manualmente una imagen usamos:

    docker pull <nombre_imagen>

Una vez descargada la imagen podemos ver las capas de las imagenes con el comando:

    docker history

Y si queremos ver las capas de la imagen, podemos hacerlo con el comando:

    docker history <nombre_imagen>

Todo el proceso de capas hace muy eficiente el almacenamiento de imágenes y la creación de los contenedores. Docker solo necesita descargar las capas que no tenga en local y montarlas en el contenedor. 

Para borrar una imagen necesitamos el ID o nombre de la imagen. Podemos ver las imagenes que tenemos con:

    docker images

Y borrarlas con 

    docker rmi <nombre_imagen>
    docker rmi <ID_imagen>

Si queremos borrar todas las imágenes que no estén en uso usamos el comando:

    docker image prune

## Dockerfile y Docjer build (construir imagenes, optimizar caché)
Un Dockerfile es un archivo de texto que contiene una serie de instrucciones que Docker leerá y ejecutará para construir una imagen de contenedor. Cada instrucción en un Dockerfile crea una capa en la imagen. 

Un Dockerfile se compone de una serie de instrucciones, cada una en una linea diferente. Las más comunes son:

    FROM: Indica la imagen base que se utilizará para construir la nueva imagen.
    RUN: Ejecuta comandos en la imagen.
    COPY: Copia archivos o directorios desde el host a la imagen.
    WORKDIR: Establece el directorio de trabajo por defecto para el resto de     
    instrucciones.
    CMD: Define el comando que se ejecutará cuando se inicie un contenedor a partir
    de la imagen.
    ENTRYPOINT: Define el comando que se ejecutará cuando se inicie un contenedor a
    partir de la imagen, pero no se puede sobreescribir.

Es importante tener en cuenta el orden de las instrucciones en un Dockerfile. Se ejecutan de arriba a abajo, secuencialmente, y Docker intenta reutilizar capas de imágenes anteriores para optimizar el proceso de construcción.

Para probar los ficheros utilizamos el comando

    docker build 

El comando construye una imagen a partir de un Dockerfile y un contexto. El contexto es el directorio desde el que se construye la imagen y se utiliza de referencia para el copiado de archivos y directorios. 

## Entrypoints, argumentos y variables de entorno
Las instrucciones ENTRYPOINT y CMD son las que definen qué comando se ejecutará cuando se inicie un contenedor. Tienen una funcionalidad similar pero con una diferencia importante.

La instrucción ENTRYPOINT define el comando que se ejecutará cuando se inicie un contenedor. Si se especifica un ENTRYPOINT, este comando se ejecutará siempre que se inicie el contenedor, y cualquier comando que se pase al contenedor se ejecutará como argumentos del ENTRYPOINT

La instrucción CMD también define el comando que se ejecutará cuando se inicie un contenedor, pero si se especifíca un CMD, será sobreescrito por cualquier comando que se pase al contenedor.

Esto nos da flexibilidad de modificar el comportamiento de un contenedor sin tener que modificar el Dockerfile.

Ademas de lo visto anteriormente, podemos pasar agrumentos a nuestro proceso de construcción y definir variables de entorno en nuestro Dockerfile. 

Para pasar argumentos podemos usar la instrucción ARG:

    FROM alpine

    Declarar un argumento con un valor predeterminado
    ARG NOMBRE="Mundo"

    Crear un archivo que contenga el mensaje personalizado
    RUN echo "Hola $NOMBRE" > /message 

    Comando predeterminado para mostrar el contenido del archivo
    CMD ["cat", "/message"]

También podemos sobreescribir el argumento NOMBRE, durante el proceso de construcción con:

    docker build --build-arg NOMBRE=dockermaniatico -t saludador .

Por último, vamos a ver cómo pasar variables de entorno a nuestros contenedores en el momento de ejecutarlos:

    docker run -e DEBUG=1 mi_aplicacion

La filosofía de los contenedores es ser lo más agnóstico posible, es decir, que no dependan de un entorno concreto y se puedan ejecutar en cualquier entornos, cliente y caso de uso. Por eso, es importante configurar nuestras imágenes para que no dependan de variables estáticas y que se puedan adaptar a diferentes situaciones.

## Docker compose
Es una herramienta que nos permite definir y ejecutar aplicaciones formadas por múltiples contenedores. Podemos definir un archivo YAML con la configuración de los servicios que forman nuestra aplicación y luego ejecutarla con un solo comando.

Su lógica es simple, nos permite definir en un mismo fichero múltiples servicios, volúmenes, redes, secretos y configuraciones, algo que hasta ahora habríamos trabajado de forma individual y usando el cli. Por defecto, se suele usar un fichero llamado "compose.yaml". El archivo contará con los siguientes puntos:

*Servicios*: Contiene la definición de la imagen que van a ejecutar, los puertos que exponen, volúmenes, redes, variables de entorno, etc. Todo lo que hacíamos con el comando docker run y que lo podemos definir en un solo bloque.
Y luego, todos los recursos que necesitamos para que nuestra aplicación funcione:

*Volúmenes*: que antes definíamos con docker volume create o con el flag -v en docker run, y que ahora automatizaremos su uso (y su creación si no existe).

*Redes*: que antes definíamos con docker network create o con el flag --network en docker run, y que ahora automatizaremos su uso (y su creación si no existe).

*Secretos y configuraciones*: que antes definíamos con docker secret create o con el flag --secret en docker run. 

Los comandos más comunes para usar docker compose son:

    docker compose up: Levanta todos los servicios definidos en el fichero compose.
    docker compose up -d: Levanta todos los servicios en segundo plano.
    docker compose down: Detiene y elimina todos los servicios.
    docker compose ps: Muestra el estado de los servicios.
    docker compose logs: Muestra los logs de los servicios.
    docker compose exec <servicio> <comando>: Ejecuta un comando en un servicio
