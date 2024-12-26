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



