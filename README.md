# Docker
Fundamentos de Docker

- [🐳 Docker - Introducción](#🐳-docker---introducción)
  - [Las 3️⃣ áreas y problemas en el desarrollo de software profesional](#las-3️⃣-áreas-y-problemas-en-el-desarrollo-de-software-profesional)
    - [1️⃣ Construcción](#1️⃣-construcción)
    - [2️⃣ Distribución](#2️⃣-distribución)
    - [3️⃣ Ejecución](#3️⃣-ejecución)
    - [¿Cuál es la solución a todos estos problemas?](#cuál-es-la-solución-a-todos-estos-problemas)
  - [🖥️ Virtualización](#️-virtualización)
    - [💻Máquina Virtual (VM)](#máquina-virtual-vm)
    - [📦Contenedores](#contenedores)
  - [¿Qué es Docker?](#qué-es-docker)
    - [⚙️ Docker Engine](#️-docker-engine)
    - [📑 Registros Docker](#-registros-docker)
    - [🧊 Componentes de Docker](#-componentes-de-docker)
- [🐳 Docker - Contenedores](#-docker---contenedores)
  - [¿Qué es un contenedor?](#qué-es-un-contenedor)
    - [Detalles importantes de los contenedores.](#detalles-importantes-de-los-contenedores)
  - [Primeros comandos...](#primeros-comandos)
  - [🏳️ Modo interactivo, -it flag](#️-modo-interactivo--it-flag)
    - [😎 Ubuntu desde un contenedor](#-ubuntu-desde-un-contenedor)
  - [❤️ El ciclo de vida de un contenedor](#️-el-ciclo-de-vida-de-un-contenedor)
    - [🏛️ Main process](#️-main-process)
  - [🏳️ Modo background, -d flag](#️-modo-background--d-flag)
    - [♾️ Ejecutando indefinidamente Ubuntu](#️-ejecutando-indefinidamente-ubuntu)
    - [▶️ Comando docker exec](#️-comando-docker-exec)
  - [⛔ Stop \& Kill 🔫](#-stop--kill-)
  - [Exponiendo Contenedores: Puertos 🚢](#exponiendo-contenedores-puertos-)
  - [🏳️ Modo publicando, -p flag](#️-modo-publicando--p-flag)
    - [Publicando un contenedor Nginx](#publicando-un-contenedor-nginx)
    - [🧾 Docker logs](#-docker-logs)
- [🐳 Docker - Datos](#-docker---datos)
  - [📂Bind Mounts](#bind-mounts)
    - [Desventajas de Bind Mounts](#desventajas-de-bind-mounts)
  - [💽 Volúmenes](#-volúmenes)
    - [Comandos](#comandos)
  - [-v flag Versus --mount flag](#-v-flag-versus---mount-flag)
  - [Insertar y extraer archivos](#insertar-y-extraer-archivos)



# 🐳 Docker - Introducción

## Las 3️⃣ áreas y problemas en el desarrollo de software profesional


### 1️⃣ Construcción

> 💡*Escribir código es sólo una pequeña parte del problema. Los problemas complejos necesitan equipos* 👥

Cuando estamos en la etapa de construcción nos encontramos con problemas como:

1. Entornos de desarrollo.
2. Dependencias.
3. Entornos de ejecución.
4. Equivalencia con entorno productivo.
5. Servicios externos.

Ya que generalmente en proyectos grandes nunca trabajamos un código individualmente. Siempre se trabaja en equipo y es común encontrarse con problemas como: *- en mi computadora funciona no sé por qué en la tuya no -* o *- tienes que tener estas versiones de paquetes para poder que funciona -*, etc. Todo esto son problemas que vienen dados por falta de compatibilidad en los entornos de construcción.

### 2️⃣ Distribución

Cuando estamos en la etapa de distribución nos encontramos con problemas como:

1. Divergencia de repositorios.
2. Divergencia de artefactos.
3. Versionamiento.

A veces las aplicaciones tienen diferentes artefactos y esto implica que a veces se tengan diferentes repositorios para cada componente por lo tanto la administración de estos a veces se vuelve difícil. También es común en contrarse problemas a la hora de versionar nuestras aplicaciones, asegurarnos que la versión que se está distribuyendo sea la correcta.

### 3️⃣ Ejecución
> 💡*La máquina donde se escribe el software siempre es distinta a la máquina donde se ejecuta de manera productiva.*

Cuando estamos en la etapa de ejecución nos encontramos con problemas como:

1. Compatibilidad con el entorno productivo.
2. Dependencias.
3. Disponibilidad de servicios externos.
4. Recursos de hardware.

### ¿Cuál es la solución a todos estos problemas?

> 🐳*Docker te permite construir, distribuir y ejecutar cualquier aplicación en cualquier lado.*

## 🖥️ Virtualización

> 💡*Versión virtual de algún recurso tecnológico, como (...) hardware, un sistema operativo, un dispositivo de almacenamiento o (...) recurso de red.*

La virtualización permite atacar en simultáneo los tres problemas (construcción, distribución y ejecución) del desarrollo de software profesional. Para esto existen dos conceptos:
1. Máquina Virtual.
2. Contenedor.

### 💻Máquina Virtual (VM)
*Una máquina virtual (VM, por sus siglas en inglés) es un software que simula un entorno de hardware de una computadora y permite ejecutar sistemas operativos y aplicaciones como si fuera un equipo físico. La máquina virtual actúa como un contenedor aislado del sistema operativo y hardware subyacente del host, lo que significa que una VM se puede ejecutar en cualquier host que tenga el software de virtualización apropiado instalado.*

Las máquinas virtuales podrían "solucionar" los tres problemas de dasarrollo de software profesional pero tienen algunos problemas:

1. **Mayor consumo de recursos:** Las VM necesitan un sistema operativo completo y sus propios recursos de hardware, lo que significa que tienen un mayor consumo de recursos que los contenedores, lo que puede hacer que las VM sean menos eficientes en términos de uso de recursos.
2. **Mayor tiempo de arranque:** Debido a que las VM necesitan arrancar todo el sistema operativo y las aplicaciones, el tiempo de arranque de una VM suele ser más largo que el de un contenedor.
3. **Menor portabilidad:** Las VM pueden ser más difíciles de mover entre diferentes sistemas operativos y arquitecturas de hardware debido a sus dependencias de sistema operativo y hardware.
4. **Mayor tamaño:** Las VM son más grandes que los contenedores debido a que contienen todo el sistema operativo y sus recursos. Esto puede hacer que la distribución y el almacenamiento de las VM sean más difíciles.

En resumen, las VM son una tecnología útil para la virtualización, pero también tienen algunas desventajas en comparación con los contenedores, como un mayor consumo de recursos y tiempo de arranque, menor portabilidad y mayor tamaño.

### 📦Contenedores 
*Un contenedor es una tecnología de virtualización a nivel de sistema operativo que permite la ejecución de aplicaciones de forma aislada y portátil. En lugar de virtualizar todo el sistema operativo como lo hacen las máquinas virtuales (VM), los contenedores permiten que varias aplicaciones compartan el mismo sistema operativo y utilicen sólo los recursos necesarios para su funcionamiento.*

Cada contenedor se ejecuta en un espacio de usuario aislado, lo que significa que tiene su propia vista del sistema de archivos, variables de entorno, bibliotecas y otros recursos. Además, los contenedores pueden ser fácilmente empaquetados, distribuidos y escalados, lo que los hace ideales para la implementación de aplicaciones en la nube.

En comparación con las máquinas virtuales, los contenedores son más livianos, más rápidos de arrancar y ocupan menos espacio en el disco. Además, los contenedores pueden ejecutarse en cualquier sistema operativo que tenga soporte para la tecnología de contenedores utilizada, lo que los hace altamente portátiles.

Los contenedores tienen grandes ventajas como:
1. Flexibes.
2. Livianos.
3. Portables.
4. Bajo acoplamiento.
5. Escalables.
6. Seguros.

---
> ¡ Docker usa contenedores ! 😎

![Docker vs vm](https://i0.wp.com/k21academy.com/wp-content/uploads/2020/10/vm-vs-containers.png)

Como podemos observar en una VM se tiene un sistema operativo por cada App, mientras que en Docker solo usamos un sistema operativo que comparte los recursos con todas las Apps.

## ¿Qué es Docker?

Es una plataforma que permite construir, ejecutar y compartir aplicaciones mediante **contenedores**.

![infraestructura Docker](https://k21academy.com/wp-content/uploads/2020/05/2020-05-12-16_36_49-PowerPoint-Slide-Show-Azure_AZ104_M01_Compute_ed1-1-1024x599.png)

La arquitectura Docker es una arquitectura cliente-servidor, dónde el cliente habla con el servidor (que es un proceso daemon) mediante un API para poder gestionar el ciclo de vida de los contenedores y así poder construir, ejecutar y distribuir los contenedores.

El hecho de que el cliente se comunique con el servidor mediante el API hace que el cliente y servidor puedan estar en la misma máquina comunicándose mediante sockets de UNIX o bien en máquinas diferentes comunicándose mediante un end-point en la red.

Docker está escrito en GO, aunque también se aprovecha de muchas de las capacidades del kernel Linux, como namespaces, cgroups, y el sistema de ficheros UnionFS.

Dentro de los elementos de la Arquitectura Docker encontramos dos, por un lado el elemento principal de la arquitectura Docker que es el Docker Engine y por otro el Registro Docker.

### ⚙️ Docker Engine
El Docker Engine es la aplicación cliente-servidor que implementa Docker. Esta aplicación tiene tres componentes:

1. **Servidor (Docker daemon)**: es el proceso principal de Docker y que funciona como proceso demonio del sistema. Es el encargado de gestionar los objetos que hay en Docker como imágenes, contenedores, redes y volúmenes. Se representa mediante el comando `dockerd`.
2. **API Rest**: es un API Rest que nos permite acceder a las capacidades del servidor y ejecutar comandos sobre él. Podemos utilizar un simple curl para acceder a las capacidades del API de Docker
3. **Cliente**: es la línea de comandos representada por el comando Docker. El cliente habla vía el API Rest para poder ejecutar los comandos. Es lo que utilizaremos para poder ir gestionando el ciclo de vida de nuestras imágenes y contenedores.

### 📑 Registros Docker
Los registros Docker (Docker Registry) son los que almacenan imágenes Docker. El Docker Hub es un registro público que almacena múltiples imágenes, algunas de ellas certificadas por Docker.

### 🧊 Componentes de Docker 
1. **Container**: es donde se aloja y corren los aplicativos.
2. **Images**: artefacto que utiliza Docker para empaquetar y distribuir los contenedores.
3. **Data Volumenes** es la forma que tiene Docker para acceder al sistema de archivos del anfitrión de forma segura sin comprometerlo.
4. **Network**: Permite la comunicación de contenedores entre si o en el mundo exterior.

# 🐳 Docker - Contenedores

Con el siguiente comando en la terminal podemos ejecutar nuestro primer contenedor 😎

`docker run hello-world`

## ¿Qué es un contenedor?

El contenedor es el concepto más importante que existe en el mundo de Docker. Un contenedor Docker es un paquete de software que contiene todo lo necesario para ejecutar una aplicación, incluyendo el código, las bibliotecas y las dependencias, todo empaquetado en un único objeto. Docker utiliza una tecnología de virtualización a nivel de sistema operativo para crear entornos de ejecución aislados y portátiles, lo que significa que los contenedores Docker pueden ser fácilmente movidos de un entorno de desarrollo a uno de producción sin cambios en la configuración.Algunas de las características principales de los contenedores Docker son:
1. **Aislamiento**: Los contenedores Docker ofrecen un alto grado de aislamiento entre aplicaciones y servicios, lo que significa que pueden ser ejecutados en el mismo host sin interferir entre sí.
2. **Portabilidad**: Los contenedores Docker son altamente portátiles, ya que incluyen todo lo que se necesita para ejecutar una aplicación, incluyendo el código, las bibliotecas y las dependencias.
3. **Ligereza**: Los contenedores Docker son muy ligeros en comparación con las máquinas virtuales, ya que utilizan el kernel del sistema operativo del host en lugar de virtualizar todo el hardware.
4. **Escalabilidad**: Docker permite escalar fácilmente aplicaciones y servicios en función de la carga, lo que significa que pueden manejar grandes volúmenes de tráfico sin tener que preocuparse por problemas de rendimiento.
5. **Flexibilidad**: Docker es una plataforma muy flexible que puede ser utilizada en una variedad de entornos, incluyendo la nube, los centros de datos locales y los entornos de desarrollo.
6. **Control**: Docker ofrece una gran cantidad de herramientas y APIs que permiten controlar el proceso de implementación y ejecución de los contenedores, lo que facilita la gestión de aplicaciones y servicios.

### Detalles importantes de los contenedores.
* Los procesos que se ejecutan dentro de un contenedor Docker solo pueden ver los recursos y la configuración que se han definido en el contenedor y no pueden acceder a los recursos fuera del contenedor, incluyendo los recursos del host en el que se está ejecutando el contenedor.
* Docker se ejecuta de forma nativa en sistemas operativos basados en Linux, como Ubuntu, Debian, CentOS, entre otros, debido a que utiliza algunas características del kernel de Linux para implementar la virtualización a nivel de contenedores. Sin embargo, existen versiones de Docker para otros sistemas operativos como Windows y macOS, pero en estos casos Docker se ejecuta en una máquina virtual Linux en segundo plano, lo que significa que se necesita una capa adicional de virtualización para emular el sistema operativo Linux en el que Docker se ejecuta. En resumen, aunque Docker se ejecuta de forma nativa en sistemas operativos Linux, también se puede utilizar en otros sistemas operativos utilizando una máquina virtual Linux en segundo plano.
* Cada contenedor Docker tiene un ID único generado automáticamente por Docker y también puede tener un nombre asignado por el usuario para identificarlo de forma más amigable. El nombre debe ser único dentro del sistema Docker y, si no se especifica, Docker generará uno automáticamente (generalmente suelen ser nombres graciosos 😅).
  
## Primeros comandos...

Es importante tener en cuenta que: 

Cada vez que ejecutas el comando `docker run`, Docker crea un nuevo contenedor a partir de la imagen especificada. Si no se especifican opciones adicionales, se crea un contenedor nuevo con una configuración predeterminada. Es importante tener en cuenta que los contenedores son entidades efímeras, lo que significa que son diseñados para ser temporales y descartables. Esto permite a los desarrolladores y operadores de sistemas trabajar con aplicaciones de manera aislada y segura sin preocuparse por las dependencias del sistema operativo o las bibliotecas compartidas.

* `docker run <image>`: Ejecuta la imagen en un contenerdor. Ejemplo, `docker run hello-world` ejecuta la imagen hello-world en un contenedor.
* `docker ps`: Muestra los contenedores activos (que están actualmente corriendo).
* `docker ps`: Muestra todos los contenedores, tando los activos como los no activos (ya ejecutados).
* `docker inspect <container ID or container name>`: Muestra toda la información detallada del contenedor.
* `docker run --name <custom name> <image>`: Ejecuta una imagen en un contenedor con un nombre de contenedor asignado por el usuario.
* `docker rename <container name> <new container name>`: Renombra un contenedor.
* `docker rm <container ID or container name>`: Elimina un contenedor.
* `docker container prune`: Elimina todos los contenedores que no están en un estado de ejecución (parados).

## 🏳️ Modo interactivo, -it flag
`-it` es la abreviatura de `--interactive + --tty  (consola/terminal)`. Cuando ejecuta `docker run` con este comando, este lo lleva directamente dentro del contenedor. Es decir, te permite acceder a la consola del contenedor.
>`-it` indica que se debe utilizar una sesión de shell interactiva y que la entrada y salida deben estar conectadas a la consola actual. 
### 😎 Ubuntu desde un contenedor 
Para demostrar la funcionalidad del comando (o bandera) `-it` correremos Ubuntu en un contenedor. Para esto ejecutamos el comando `docker run ubuntu`. Una vez ejecutado el comando se observa que no sucede "nada" aparentemente. Si ejecutamos el comando `docker ps -a` vemos que un contenedor fue creado con la imagen `ubuntu` pero esto se encuentra finalizado, con un **STATUS** `Exited (0)`. El número 0 indica que todo funcionó correctamente (un número diferente a 0 indicará un error).

Si ejecutamos el comando `docker run -it ubuntu` este nos permitirá ejecutar la imagen en ubuntu en un contenedor y adicionalmente acceder a este (modo interactivo). Una vez ejecutado el comando anterior hemos accedido a la terminal de Ubuntu el cual se ejecuta a través del contenedor de Docker; podemos ejecutar el comando `docker ps` en una nueva terminal y observamos que el **STATUS** de este nuevo contenedor es `up`.

El comando `cat /etc/lsb-release` nos permite observar que distribución de Ubuntu estamos ejecutando a través del contenedor.

> Básicamente estamos corriendo Linux Ubuntu en Docker, que a su vez está corriendo en Linux Ubuntu con WSL, que a su vez está corriendo en mi máquina Windows 😅

## ❤️ El ciclo de vida de un contenedor

El ciclo de vida de un contenedor Docker comienza al crear una instancia de una imagen, lo que da lugar a un contenedor en ejecución. El proceso principal en un contenedor es el que se inicia al ejecutar el contenedor y mantiene en marcha el contenedor. Si el proceso principal (conocido como *main process*) finaliza, el contenedor también se detendrá, lo que significa que el contenedor se considerará "muerto". En este punto, el contenedor aún existe en el sistema de Docker, pero ya no está en ejecución. Para reiniciar el contenedor, es necesario iniciar el proceso principal nuevamente o crear una nueva instancia del contenedor a partir de la imagen.

### 🏛️ Main process
El *main process* en Docker se refiere al proceso principal que se ejecuta dentro de un contenedor. Es el proceso que se inicia cuando el contenedor se inicia y que se ejecuta en primer plano. El proceso principal se define en el comando `docker run` mediante el parámetro `CMD` o `ENTRYPOINT`. Es importante tener en cuenta que si el proceso principal termina, el contenedor se detendrá automáticamente, por lo que es crucial asegurarse de que el proceso principal no termine a menos que se desee detener el contenedor.

Cuando se ejecuta el comando `docker ps`, la columna **COMMAND** muestra el comando que se utilizó para iniciar el contenedor. Esto suele ser el proceso principal que se ejecuta dentro del contenedor, aunque no siempre es el caso. En algunos casos, la columna **COMMAND** puede mostrar información adicional, como argumentos de línea de comando o variables de entorno utilizadas para configurar el contenedor.

En el ejemplo anterior [😎 Ubuntu desde un contenedor](#😎-ubuntu-desde-un-contenedor) podemos ver que al ejecutar el comando `docker run -it ubuntu` inmediatamente entramos a la consola de Ubuntu. Si ejecutamos el comando `docker ps` (en una nueva terminal) podemos ver que el contenedor se encuentra vivo (`up`) y **COMMAND** tiene asociado `"/bin/bash"`, es decir, el proceso principal que se está ejecutando es `"/bin/bash"` si por algún motivo este proceso finaliza inmediatamente el contenedor muere. Ejecutando `exit` en la terminal de Ubuntu y luego ejecutando `docker ps -a` podemos ver que el contenedor está muerto (estado `Exited (0)`)

## 🏳️ Modo background, -d flag
El parámetro `-d` en Docker se utiliza para indicar que se desea ejecutar un contenedor en segundo plano (modo **detach**). Esto significa que el contenedor se ejecutará en segundo plano, sin bloquear la línea de comandos en la que se ejecutó el comando `docker run`. Al utilizar este parámetro, Docker inicia el contenedor y luego devuelve el control al usuario para que pueda seguir usando la línea de comandos para realizar otras tareas. El contenedor sigue ejecutándose en segundo plano, lo que permite al usuario interactuar con él a través de comandos adicionales de Docker o acceder a su shell de línea de comandos si así lo desea.

### ♾️ Ejecutando indefinidamente Ubuntu
En el caso de que queramos ejecutar un contenedor Ubuntu y además de esto que no finalicé su ejecución cuando ejecutamos `exit` en la terminal, debemos cambiar el *main process* `"/bin/bash"`.

El comando `tail -f /dev/null` es una técnica común para mantener un contenedor en ejecución sin que haga nada.

* `tail` es un comando de Linux que se utiliza para imprimir las últimas líneas de un archivo. En este caso, se está especificando el archivo `/dev/null`, que es un archivo especial de Linux que se utiliza para descartar la salida, es decir, no hace nada con los datos que recibe.

* El flag `-f` le indica a tail que siga monitoreando el archivo y que no finalice la ejecución. Por lo tanto, al ejecutar `tail -f /dev/null`, el comando permanecerá en ejecución indefinidamente, lo que es útil para mantener en ejecución un contenedor que no tiene ninguna tarea activa que realizar.

 El comando `docker run --name always_up -d ubuntu tail -f /dev/null` me permite:
 1. Ejecutar una imagen de Ubuntu en un contenedor.
 2. Asociarle al contenedor el nombre `always_up`.
 3. Correr el contenedor en modo **detach** a través del comando `-d`.
 4. Ejecutar el proceso `tail -f /dev/null` como **main process**.

Una vez ejecutado el comando, podemos ver con `docker ps` en campo **COMMAND** que el proceso que se está ejecutando es `tail -f /dev/null`.

### ▶️ Comando docker exec
`docker exec` es un comando que se utiliza para ejecutar comandos dentro de un contenedor Docker en ejecución. Esto es útil para interactuar con el sistema operativo del contenedor y realizar tareas como la instalación de software, la depuración y la monitorización. Con `docker exec`, se puede acceder a la terminal del contenedor y ejecutar comandos como si se estuviera trabajando en una máquina virtual o en una computadora física.

`docker exec -it always_up bash` este comando inicia una nueva sesión de shell interactiva en el contenedor `always_up`. El argumento `-it` indica que se debe utilizar una sesión de shell interactiva y que la entrada y salida deben estar conectadas a la consola actual. La palabra `bash` indica que se debe utilizar el **Shell Bash** dentro del contenedor.

---
Una vez ejecutado el comando `docker exec -it always_up bash` no encontramos con la consola de Ubuntu. Si ejecutamos el comando `ps -aux` en la terminal podemos ver los procesos que se están ejecutando solamente en el contenedor. En este caso veríamos el proceso `bash` con el `PID = 7` y el proceso `tail -f /dev/null` con el `PID = 1` que significa que es el proceso principal. Si salimos de la consola de Ubuntu con el comando `exit` podemos observar que el contenedor no morirá ya que el proceso principal no es `bash` sino `tail -f /dev/null`, caso contrario sucedería si hubiésemos ejecutado la imagen de Ubuntu con el comando `docker run -it ubuntu` cuyo proceso principal por defecto es `/bin/bash`.

## ⛔ Stop & Kill 🔫
Para detener un contenedor existe el **stop** y el **kill**.

* `docker stop <container id o container name>`: envía una señal SIGTERM al proceso principal dentro del contenedor, lo que permite que el proceso se detenga correctamente y realice una salida ordenada. Si el proceso no se detiene dentro de un período de tiempo predeterminado (por defecto 10 segundos), Docker enviará una señal SIGKILL para forzar la terminación del proceso.
* `docker kill <container id o container name>`: envía una señal SIGKILL directamente al contenedor, lo que causa la terminación inmediata del proceso y no permite una salida ordenada.

En resumen, `docker stop` es un método más suave de detener un contenedor, mientras que `docker kill` es más agresivo y no permite que el proceso termine correctamente.

## Exponiendo Contenedores: Puertos 🚢
Los contenedores pueden exponer puertos a través de los cuales las aplicaciones que se ejecutan dentro del contenedor pueden ser accesibles desde el exterior. Los puertos se pueden configurar durante la creación del contenedor o se pueden agregar posteriormente. Por ejemplo, si se ejecuta un contenedor de Nginx, el puerto 80 puede ser expuesto para que la aplicación web que se ejecuta dentro del contenedor sea accesible a través de un navegador web en el host o en una red externa.

> Nginx es un servidor web y proxy inverso de alto rendimiento que se utiliza para servir contenido web estático y dinámico, así como para enrutar solicitudes de tráfico web entre diferentes servidores y aplicaciones. Además de su capacidad de manejar grandes cantidades de solicitudes simultáneas, es conocido por su configuración flexible y escalabilidad, lo que lo hace popular en la industria web.

## 🏳️ Modo publicando, -p flag
En Docker, el parámetro `-p` (publish) se utiliza para publicar (mapear) los puertos de un contenedor en los puertos de la máquina host. La sintaxis para usar este parámetro es `-p <host_port>:<container_port>`, donde `host_port` es el número de puerto en la máquina host que se desea utilizar para acceder al contenedor, y `container_port` es el número de puerto en el contenedor que se desea exponer al host.

### Publicando un contenedor Nginx

Un ejemplo de cómo correr un contenedor de nginx en Docker y publicarlo sería:

`docker run --name my_nginx -d -p 8080:80 nginx`

Este comando descargará la imagen de nginx si aún no la tienes, creará un nuevo contenedor con el nombre my_nginx, y lo ejecutará en segundo plano `-d`. Además, el parámetro `-p` indica que el puerto 8080 del host (tu máquina) está mapeado al puerto 80 del contenedor, lo que permitirá que puedas acceder al servidor web que se está ejecutando dentro del contenedor.

Una vez que el contenedor se está ejecutando, abre un navegador web y accede a http://localhost:8080. Verás la página de bienvenida de nginx que indica que todo está funcionando correctamente.

Si ejecutamos el comando `docker ps` podemos observar en el atributo **PORTS** la configuración de los puertos `0.0.0.0:8080->80/tcp`

### 🧾 Docker logs
Para ver los logs de un contenedor, puedes usar el comando `docker logs <container_name_or_id>`. Esto mostrará todos los logs generados por el contenedor desde su inicio. Si deseas seguir los nuevos logs en tiempo real, puedes agregar el parámetro `-f` al comando, es decir, `docker logs -f <container_name_or_id>`. También puedes usar opciones adicionales como `--tail <num_lines>` para especificar cuántas líneas de logs quieres ver.

`docker logs -f --tail 5 my_nginx` para este comando se mostrarían las últimas 5 líneas de los logs de manera continua.

#  🐳 Docker - Datos
Uno de los problemas comunes de los contenedores es la persistencia de los datos. Cuando se elimina un contenedor, se pierden los cambios realizados dentro del contenedor, a menos que se hayan guardado de forma explícita fuera del contenedor. Esto se debe a que los datos se almacenan dentro del sistema de archivos del contenedor, que se elimina junto con el contenedor.

Para solucionar este problema se puede usar:

*  Bind Mounts.
*  Volúmenes.

## 📂Bind Mounts

Bind Mounts es una técnica en Docker que permite montar un directorio o archivo del host en un contenedor. En otras palabras, un directorio en el host es enlazado o montado en un contenedor Docker. Esto significa que los archivos en el directorio montado en el host son accesibles desde el contenedor, y cualquier cambio realizado en el contenedor también se reflejará en el directorio montado en el host. Esta técnica es útil para compartir archivos y datos entre el host y los contenedores de Docker, y permite persistir los datos más allá del ciclo de vida de un contenedor.

`docker run -d -v </my/local/folder:/container/folder> <docker_image>`

Donde:

* `-d`: indica que se desea ejecutar el contenedor en segundo plano (detach mode)
* `-v`: indica el Bind Mount y se compone de la ruta en el host y la ruta dentro del contenedor separadas por `:`.
* `/my/local/folder`: es la ruta en el host, es decir, la ruta local donde se encuentra el archivo o directorio que se desea compartir con el contenedor.
* `/container/folder`: es la ruta dentro del contenedor, es decir, la ruta donde se desea que se monte el directorio o archivo del host.
* `docker_image`: es la imagen de Docker que se desea ejecutar.

Por ejemplo el comando `docker run -d --name mongodb -v /my/local/folder:/data/db mongo` enlaza la dirección host `/my/local/folder` con la dirección en donde Mongo guarda los datos que es `/data/db mongo`.

Si eliminamos el contenedor `mongodb` (`docker rm -f mongodb`) no perderemos la información registrada en Mongo ya que los datos estarán en la ruta host. En caso de que se necesite usar los datos registrados en el host en un nuevo contenedor, basta con lazar un contenedor mongo con la misma ruta `-v` con el que se lanzó el contenedor eliminado `/my/local/folder:/data/db`.

### Desventajas de Bind Mounts
Una desventaja de usar Bind Mounts es que los permisos y configuraciones de los archivos en el host pueden afectar al contenedor, lo que puede generar problemas de seguridad. Además, si se eliminan los archivos en el host, también se eliminarán en el contenedor, lo que puede ocasionar la pérdida de datos. Otra desventaja de usar Bind Mounts es que podría comprometer la seguridad del contenedor y de la máquina anfitriona si se comparte un directorio o archivo sensible desde la máquina anfitriona al contenedor. Esto puede permitir que un atacante acceda y modifique archivos importantes del sistema en la máquina anfitriona a través del contenedor. Por lo tanto, es importante tener cuidado al utilizar Bind Mounts y asegurarse de que se estén compartiendo solo los directorios o archivos necesarios y que no se compartan datos sensibles o críticos del sistema.

## 💽 Volúmenes
Los volúmenes fueron una evolución que Docker desarrolló para darle más seguridad a las personas que ejecutan Docker en etornos productivos.

En Docker, los volúmenes son un mecanismo para persistir los datos más allá del ciclo de vida de un contenedor. Se crean y administran independientemente del contenedor, lo que permite que los datos sean compartidos entre varios contenedores. Los volúmenes se pueden crear de diferentes formas y pueden ser utilizados para almacenar archivos de configuración, bases de datos, logs, entre otros. 

Cuando se crea un volumen en Docker, Docker crea un directorio en el sistema de archivos del host y lo utiliza para almacenar los datos del contenedor. El contenedor puede acceder a este directorio a través de un enlace simbólico o una ruta de montaje y puede leer y escribir datos en él. Además, los datos del volumen persisten incluso después de que el contenedor haya sido eliminado, lo que significa que pueden ser compartidos y reutilizados entre contenedores.

### Comandos

* `docker volume ls`: me permite listar todos los volúmenes. Por defecto, Docker ya tiene creado algunos volúmenes.

* `docker volume create <volume name>`: me permite crear un volumen.

* `docker run --mount src=<volume name>,target=<container path> <container image>`
  * `src`: **source** es el nombre del volumen que se va a montar.
  * `target`: es el directorio dentro del contenedor donde se montará el volumen.
  * `container image`: es el nombre de la imagen que se utilizará para crear el contenedor.

Por ejemplo, si ejecutamos el comando `docker run -d --name mongodb --mount src=dbdata,target=/data/db mongo` inmediatamente vamos a crear un contenedor mongo con un volumen llamado `dbdata` y ese volumen va a estar asociado a la dirección `/data/db` del contenedor.

## -v flag Versus --mount flag

En realidad es posible montar un Bind Mounts o un Volumen tanto con `-v` o `--mount` flag. 

> In general, --mount is more explicit and verbose. The biggest difference is that the -v syntax combines all the options together in one field, while the --mount syntax separates them. Here is a comparison of the syntax for each flag.
> 
> If you need to specify volume driver options, you must use --mount.
>
> [Para más informacion](https://docs.docker.com/storage/volumes/)

---

La principal diferencia entre los volúmenes y los bind mounts es que los volúmenes son gestionados por Docker, mientras que los bind mounts son directorios o archivos existentes en el host que se montan en un contenedor. Los volúmenes se crean y se gestionan desde el daemon de Docker, lo que proporciona algunas ventajas adicionales, como una mayor seguridad y portabilidad, ya que los volúmenes pueden ser usados por diferentes contenedores en diferentes hosts de Docker, independientemente de su ubicación en el sistema de archivos del host. Además, los volúmenes permiten la realización de tareas como la copia de datos de un contenedor a otro o la realización de backups de datos de forma más fácil y segura que los bind mounts.

## Insertar y extraer archivos


## Example [](#){name=example}

## [Third Example](#){name=third-example}

a
sdfasdfasd
asdfa
sdfasdfasdf

## Example2 [](#){name=example2}
