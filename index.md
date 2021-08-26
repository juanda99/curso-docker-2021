# Curso de Docker



# Virtualización: introducción


## Virtualización tradicional
- Es una abstracción de la capa de hardware
- Despliegues más sencillos
- Ahorro costes
- Aislamiento
- ...

![](images/traditional-arquitecture.png)  


## Hypervisor

- Es un monitor que orquesta el acceso de varios SO a los recursos de un servidor físico.
  ![](./images/hypervisor-types.png)


## Hypervisor type 1
- Nativo o Bare Metal Hypervisor
- Corre directamente en el hardware de la  máquina, hacen la  función de HAL (Hardware Abstraction Layer)
- Ej: VMWare ESXI, Microsoft Hyper-V, Citrix/Xen Server 


## Hypervisor type 2

- Host OS Hypervisor. 
- Corre sobre el sistema operativo, como una aplicación más.
- Ej: VMware Workstation, VMware Player, VirtualBox, Parallel Desktop (MAC), ¿KVM?
- No es adecuado cuando hay un workload elevado: Active Directory, bbdd...
- Adecuados para entornos de test
  - Más baratos
  - Instalación más sencilla 


## Contenedores

- Son una abstracción de la capa de aplicación

![](./images/containers-arquitecture.png)


## Ventajas contenedores

- Más ligeros
  - Portabilidad
    - Contrato entre el sysadmin y el developer
    - Despliegues más rápidos
  - Mas eficientes -> menor coste
- Son efímeros


## Desventajas

- Menor seguridad y aislamiento
  - Depende del host  
- Snapshots
- Migraciones en caliente (VMWare vMotion)
- Son efímeros


## Qué es Docker

Herramienta **open-source** que nos permite realizar una **virtualización ligera**, con la que poder **empaquetar entornos y aplicaciones** que posteriormente podremos **desplegar** en cualquier sistema que disponga de esta tecnología


## Arquitectura Docker

- Es una arquitectura cliente-servidor
  - El servidor es el daemon (container engine) al que se acccede mediante una API REST
  - Existen SDKs y clientes de la API para distintos lenguajes
  - El cliente habitual es el comando **docker**
- ![](images/docker-architecture.svg)


-  Por defecto usa UNIX sockets:
   - Comunicacción entre procesos de la misma  máquina
   - Se maneja  por el kernel

- Podemos configurarlo para que use TCP
  - Cliente y dockerd en máquinas distintas
  - Se cambia la variable de entorno ```DOCKER_HOST=tcp://X.X.X.X:2375```
  - Es recomendable  habilitar -tls (por defecto puerto 2376 para TLS) y poner un WebProxy delante para controlar el acceso.


## Objetos de Docker

- Los objetos principales en Docker son las **imágenes**, los **contenedores** y los **servicios**
- Hay otros conceptos relacionados  con ellos como **volúmenes**, **registro de imágenes** que los veremos conforme los necesitemos.


## Imagen

- Plantilla que define todas las dependencias de mi aplicación
- Es habitual que las imágenes se creen en base a otras (herencia)
- Ejemplo:

```
FROM ubuntu
MAINTAINER username (email@domain.com)
RUN apt-get update
RUN apt-get install -y nginx
CMD ["nginx", "-g", "daemon off;"]
EXPOSE 80
```


## Contenedor

- Instancia ejecutable de una imagen
- Son **efímeros**, la persistencia se logra mediante el uso de **volúmenes**
- Cada contenedor se ejecuta en un entorno aislado (podemos controlar el nivel de aislamiento):
  - variables de entorno
  - Volúmenes montados
  - Interfaces de red
- Podemos crear una imagen a  partir de  un estado del contenedor.


## Servicios
- Los servicios permiten escalar contenedor a través de múltiples demonios de Docker, los cuales trabajarán conjuntamente como un enjambre (swarm).


##  Docker en Linux (I)

- En Linux Docker no es virtualizado, no hay un hipervisor. 
- Los procesos que corren dentro de un contenedor de docker se ejecutan con el mismo kernel que la máquina anfitrión.


##  Docker en Linux (II)

- Linux aisla los procesos, ya sean los propios de la máquina anfitrión o procesos de otros contenedores. 
- Controla los recursos que se le asignan a contenedores pero sin la penalización de rendimiento de los sistemas virtualizados.    


##  Docker en Windows (I)

- El ecosistema de contenedores fundamentalmente utiliza Linux
- Como los  contenedores comparten el kernel con el host, no se  pueden ejecutar directamente en Windows, [hace falta virtualización](https://stackoverflow.com/questions/48251703/if-docker-runs-natively-on-windows-then-why-does-it-need-hyper-v )
- HyperV en Windows 10 pero versiones PRO o ENTERPRISE


##  Docker en Windows (II)

- Docker Client se ejecuta en Windows pero llama al Docker Daemon de una máquina virtual Linux

![](images/mobyvm.png)



# Aplicación en los módulos de FP Informática


## Grado medio (I)

- Seguridad informática
  - Cortafuegos
  - Zonas desmilitarizadas
  - Copias de seguridad
  - Servicio  SSH


## Grado medio (II)

- Servicios en red  
  - Servicios de red orientados a aplicación
  - Instalación de servicios de transferencia de ficheros
  - Instalación  de un servidores Web

- Aplicaciones web
  - Instalación de aplicaciones web (Wordpress, Moodle, app a medida)


## Grado superior ASIR

- Servicios de red e Internet.
- Implantación de aplicaciones web. 
- Administración de sistemas gestores de bases de datos. 
- Seguridad y alta disponibilidad. 


## Grado superior DAM

- Sistemas de gestión empresarial
- "Montar un workflow de desarrollo"


## Grado superior DAW

-  Despliegue de aplicaciones Web.
-  "Montar un workflow de desarrollo"



# Instalación


## Versiones de Docker
-  Hasta el 2019 había dos productos: 
   -  Docker Enterprise Edition
   -  Docker Community  Edition
- La versión Enterprise [la compró la empresa Mirantis](https://www.mirantis.com/software/mirantis-kubernetes-engine/) 
- Nos centraremos en la versión libre, que es la única a la que hace referencia ahora la [web de Docker](https://www.docker.com/)  


## Instalación

- Ir a la [web de Docker](https://docs.docker.com/get-docker/)
- Se puede instalar el Docker Desktop en Windows
- Se puede instalar el Docker Engine en Linux
- Se puede desplegar una imagen mediante Vagrant que tenga todo
- Se puede usar una OVA.
- Es importante instalar también **Docker Compose** 


## Instalación en Linux

- Lo más usual es usar imágenes basadas en Linux
  https://docs.docker.com/engine/install/ubuntu/
- El proceso habitual de instalación es:
  - Añadir  el repositorio de  docker
  - Instalar Docker Engine
  - Configurar usuarios para uso de docker (sin  privilegios root). Ver [Linux PostInstall](https://docs.docker.com/engine/install/linux-postinstall/)
  - Instalar [Docker Compose](https://docs.docker.com/compose/install/)


## Trabajar con Docker

- Utilizaremos la terminal (cliente docker)
- Utilizaremos Visual Studio Code
  - Terminal integrada
  - Extensión Docker
  - Debug  dentro de contenedores



# Comandos básicos


## Contenedores

- Ejecutar un contenedor nuevo:
  - *docker run*
  - *docker container run*
- Iniciar un contenedor existente:
  - *docker start*
  - *docker container start*
- Parar un contenedor:
  -  *docker stop*
  -  *docker container stop*


- Borrar contenedor
  - *docker rm*	
  - *docker container rm*
- Inspeccionar contenedor
  - *docker inspect*
  - *docker container inspect*
- Ejecutar comando en un contenedor:
  - *docker exec*
  - *docker container exec*
- Ver logs:
  - *docker logs*
  - *docker container logs*


## Imágenes
- Ver listado de imágenes
  - *docker images*
- Borrar una imágen
  - *docker rmi*
- Descargar imágen:
  - *docker pull*
- Publicar  imágen:
  - *docker push* 


## Descargar imágenes

- Traemos la última versión de la imagen de redis:

```
docker pull --help
docker pull redis
```


##  Listado de imáges locales

```
docker images
```
- Aparecen datos útiles:
  -  Órigen de la imágen
  -  Versión
  -  Tamaño  que ocupa
  -  Fecha de creación


## Ejecución imágen (I)

- Recordemos que una imagen en ejecución es un contenedor

```
docker run redis
docker  ps
```
CTRL  + D para pararlo y podemos comprobar que  ya  no  está


##  Ejecución imagen (II)

- Mejor ejecutar en modo detached:
```
docker run -d redis
docker  stop  <container-id>
```


Si queremos arrancarlo de nuevo (no sabemos  id)
```
docker ps  -a
docker start <container-id>
```


## Ejecución imagen con versión

- Por defecto descargamos imágen con versión *latest*
- **docker pull redis** es equivalente a **docker pull redis:latest**
- Vamos a descargar la versión  4.0 de redis:

  ```
  docker run redis:<version>  
  # hace directamente docker pull & docker start
  ```


## redis-comander

- [Paquete de npm](https://www.npmjs.com/package/redis-commander) para acceder a Redis desde el navegador
- Instalación:
  - Debemos instalar  nodejs, la versión de la distribución de Ubuntu es antigua (v10)
  -  Instalamos usando el PPA oficial

```
sudo apt-get install npm

sudo curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt-get install -y nodejs
npm --version
sudo npm i -g redis-commander
redis-commander
```


## Comprobamos acceso a nuestro redis

- No nos conecta, ¿qué pasa?
  - Los contenedores funcionan por defecto en una red interna, ajenos al host
  - Veamos como funciona la configuración de red de docker


## Varios redis a la vez

- Redis funciona por una red interna
- Podríamos lanzar varios redis, y aunque escuchan  por el mismo puerto funcionaría:
  ```
    docker run  -d redis
    docker run -d redis:4.0   
  ```

## Configuración  de redes

- Internamente usa las regla de enrutado del  servidor
  - [En Linux mediante IPtables](https://docs.docker.com/network/iptables/)
- Es un sistema abierto, mediante el uso de drivers. 


##  Drivers - bridge

- Por defecto, no es necesario especificarlo.  Los contenedores se comunican entre sí.
-  Son las más utilizadas
-  Déjemos que docker se encargue de todo :-)
-  Ejecutar comandos:
    ```
    ip  address
    docker network ls
    docker network inspect bridge
    ```


 ![](images/docker-bridge-network.jpg)


## ¿Cómo  se accede  al exterior?
  ```
  $ sudo iptables -t nat -L –n
  ...
  Chain POSTROUTING (policy ACCEPT) target prot opt
  source destination MASQUERADE all -- 172.17.0.0/16
  !172.17.0.0/16
  ...
  ```


##  Drivers - host

  - Usa  la red del  host directamente, elimina  el aislamiento del contenedor con el host. 
  - Comparte namespace de red con el host
  - Tiene acceso a todas las interfaces del Host
  - Evita el uso de NAT 
  - Puede provocar conflictos de puertos


##  Drivers - overlay

- Para conectar múltiplos demonios docker y permitir la comunición entre servicios swarm.


##  Drivers - macvlan

- Permite asignar direcciones MAC a los contenedores, haciendo que aparezcan como dispositivos físicos en la red.


##  Drivers - otros

- **none**:

- **Network plugins**:  

  - Se pueden instalar y  usar  plugins  de  red  de terceros. 


## Debug de un contenedor

- La forma más evidente es entrar a la  terminal  del  contenedor:
  - Variables de entorno
  - Estructura  ficheros
  - pocos comandos disponibles!

```
docker exec -it <name  or container-id> /bin/bash
```


## Ejercicio

- Comprueba que efectivamente los contenedores se pueden comunicar entre sí.


## Ejercicio - solución

- Accede al fichero /etc/host de cada contenedor para ver su ip 
- Haz un ping entre contenedores
  ```
  docker exec -it <container-name> /bin/bash
  apt update
  apt install iputils-ping
  # para ver la ip también podíamos haber utilizado el comando ip
  # apt install iproute2
  # ip address
  ping <ip-container>
  ```


## ¿Cómo dar acceso a  redis-commander?

- En modo bridge, podemos mapear puertos del contenedor al host:

  ```
  docker run -p5000:6379 -d redis
  # error siguiente línea,  cambiar   puerto!! 
  docker run -p5000:6379 -d redis:4.0  
  ```

- Otra opción es arrancar el contenedor en modo  host:
  ```
  docker run  --network host -d redis
  # ¿Qué sucede si lo  arrancas dos veces?
  docker run  --network host -d redis
  ```


  
## Logs de un contenedor

```
docker logs <name or container-id>
```

- Los contenedores tienen  un nombre aleatorio, pero se puede dar de forma explícita:
```
docker run -p5000:6379 --name  redis-new -d redis
docker run -p5001:6379 --name  redis-old -d redis:4.0
```




## Versiones imágenes

- No es bueno utililzar imágenes con etiqueta latest
  - Se pueden producir breaking changes
  - No está claro el versionado de la imágen. ¿docker inspect?


## Ejercicio

- Borra todas las imágenes previas en tu equipo
  ``` 
  docker images
  docker rmi $(docker images -a -q)
  ```
- Descarga redis:latest y comprueba su versión
- Descarga esa versión específica
- Comprueba que tienes dos imágenes al hacer un listado pero que:
  - Ambas ocupan el mismo tamaño
  - Son idénticas:
    - Mismo **IMAGE ID**
    - No se ha producido ninguna descarga de layers adicionales.




# Docker Hub

- Podemos crear una imagen de cero pero lo normal es usar o partir de una ya creada.
- Hay un registro oficial de imágenes llamado  [Docker Hub](https://hub.docker.com)
- También podemos crearnos nuestro propio registro.
- Los registros pueden ser públicos o privados.


## Registro  de docker

- Lugar donde se almacenan las imágenes.
  - Si vamos a desarrollar por ej con php buscaremos una imagen pública de php con una determinada versión
- **Docker Hub** es un registro público, gratuito para imágenes públicas, y con versiones de pago del servicio para registros privados. - En el registro público, cualquiera puede hacer push de sus imágenes, o pull de las imágenes creadas por otros usuarios.
Cuando se ejecuta docker pull o docker run, las imágenes requeridas se descargan del registro. Cuando es hace docker push, la imagen se sube al registro.
- Docker viene configurado por defecto para buscar las imágenes de Docker Hub (docker.io/)


## Otros registros

- Es posible usar nuestro propio registro
  - Podemos usar la misma implementación vanilla del Docker registry que usar Docker Hub
  - Otros más avanzados como Harbor (Open Source, era VMWare), Artifactory (JFrog), Nexus (Sonatype), etc.
- Docker también ofrece una versión enterprise de su registro llamada Docker Trusted Registry (DTR)
- Google Container Registry (GCR), Amazon Elastic Container Registry (ECR), Azure Container Registry (ACR), Quay (Redhat, versión On-Prem y versión cloud), Gitlab Container Registry, Github Packages, etc.



# Docker images

- Aprender a crear ficheros Dockerfile
- Aprender  a crear y publicar imágenes
- Entender el concepto de layers en imágenes.



## Dangling  images
- Build de una  imagen pero no le damos nombre, se queda sin etiquetar mostrada como  <none>.
- Se borran mediante el comando:
  ```docker system  prune ```


## Imágenes sin uso
- Conforme vamos usando  docker, descargando imágenes... empezamos a ocupar  espacio
- Podemos comprobar el espacio usado mediante el comando: 
  ```docker system df ```
  
- Las eliminaremos con el  comando:
  ```docker system prune  -a``

## Etiquetar imágenes

```docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]```


## Ver uso  de los contenedores

```docker stats --no-stream```


## Hello World
```
$ docker image ls
$ docker run hello-world
Hello from Docker!
```


## Creacción y acceso a un contenedor

```
docker run -it --name <container-name> -d <image-name>
docker exec -it <container-name> /bin/bash
```

- Navegando desde el plugin Docker de Visual Code


## Ejemplo

```$ docker run -it ubuntu /bin/bash ```
- Docker hace un ```docker pull ubuntu:latest```  
  - Descarga la imagen ubuntu (ubuntu:latest) del registro por defecto (normalmente Docker Hub), a no ser que ya estuviera localmente. 
- Docker crea un nuevo contenedor (docker container create).

  - Docker crea una nueva capa sólo lectura, con archivos especiales requeridos por el contenedor, y otra más en lectura-escritura, y la asocia como última capa del contenedor.
  - Se crea un interfaz de red que conecta el contenedor con la red por defecto. 
    - Se asigna una dirección IP al contenedor. 
    - Se establecen mediante iptables unas reglas de NAT para que el contenedor pueda conectarse a Internet.
- Docker ejecuta el contenedor, y lanza el proceso /bin/bash. 





# Dockerfile

## ¿Qué es un dockerfile?

- Plantilla en texto plano que define las dependencias de mi aplicación
- Las imágenes se construyen base a estas plantillas
- Cada línea del fichero Dockerfile genera una layer (capa) en la imágen
  - La caché funciona por cada línea o capa.
- Es habitual que las imágenes se creen en base a otras (herencia)


# Ejemplo servidor web con docker


## Crear dockerfile

```
# Pull the httpd image from DockerHub repository to build our application as a base
FROM httpd:2.4
# Copy the static page from the target directory to apache2 docs
COPY ./public-html/ /usr/local/apache2/htdocs/
```


## Construir imágen

```
# docker build -t my-apache2 
```


## Ejecutar contenedor

```
docker run  -p 80:80 --name my-apache2-1  my-apache2
```



## Docker-compose


## Entornos de desarrollo

- Se instalan mediante contenedores
- Aseguramos versiones:
  - Compiladores y herramientas de  builld tipo Maven, Gradle
  - BBDD
- El código fuente  se mantiene en un directorio físico asociado mediante un volumen a un  contenedor
- El código compilado se lleva a otra carpeta del host o a una nueva imagen que se utiliza para el despliegue.


Desarrollo directamente dentro del contenedor con Visual Studio Code:
https://code.visualstudio.com/docs/remote/containers  


Pipelines de CI/CD
Jenkins: https://jenkins.io/doc/book/pipeline/docker/
Azure Pipelines Container Jobs:
https://docs.microsoft.com/en-us/azure/devops/pipelines/process/container-phases?view=az ure-devops
Github actions:
● https://github.com/features/actions
● https://help.github.com/en/actions/building-actions/creating-a-docker-container-action
Tekton: CI/CD nativo para Kubernetes - https://cloud.google.com/tekton



## Limitación de recursos
- Los contenedores comparten los recursos del host
- No existe ninguna limitación en el consumo de los mismos.
- Se limitan mediante Docker Dashboard

## Seguridad
-



## Almacenamiento

- En docker, las imágenes se construyen a base de capas
- Cada capa contiene únicamente las diferencias respecto a la capa padre. 
- Docker utiliza mecanismos de union filesystems para montar en una carpeta la combinación de las distintas capas.
- Al crear un contenedor, Docker añade una capa adicional (la capa de contenedor), que es la única sobre la que es posible escribir. De esta forma el contenedor puede modificar aparentemente la imagen base, como si tuviera una copia real, pero únicamente está modificando esta última capa. 
- Podemos crear múltiples contenedores sobre una misma imagen, reutilizando todas las capas excepto la capa de contenedor.
- Al destruir un contenedor, esta capa con las modificaciones se destruye, a no ser que la convirtamos en una nueva imagen con el comando docker commit.




## Workflow con docker

- Development
- CI/CD
- Deployment

Aplicación JavaScript con MongoDB
Git commit -> CI push a private repository con mi  códdigo
Uso también  de  Docker hub para el mongoDB,  MongoExpress para no lidiar con terminal


Mongo   y  MongoExpress en la missma red:
docker network ls
docker network create mongo-network


docker run -p 27017:27017  -d --network mongo-network --name mongodb \
    -e MONGO_INITDB_ROOT_USERNAME=admin \
    -e MONGO_INITDB_ROOT_PASSWORD=password \
    mongo

// ojo  que coincida red, nombre servidor y usr/pwd!!!!!

$ docker run -d \
    --network mongo-network \
    --name mongo-express \
    -p 8081:8081 \
    -e ME_CONFIG_MONGODB_SERVER=mongodb \
    -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
    -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
    mongo-express


    docker logs mongo-express para ver  un  "connected" y  luego probamos con localhost


    docker-logs -f y ver que mongodb introduce nuevos datos de la app que haga. 


    Ejecutat varios contenedores... mejor con docker-compose!!!

    ojo, y  faltaban los volúmenes!!!

    yaml: ojo con indentation!  usar  plugins en  Visual  Studio Code

  docker-compose.yaml:

    version: '3'
    services:
      # my-app:
      # image: ${docker-registry}/my-app:1.0
      # ports:
      # - 3000:3000
      mongodb:
        image: mongo
        ports:
          - 27017:27017
        environment:
          - MONGO_INITDB_ROOT_USERNAME=admin
          - MONGO_INITDB_ROOT_PASSWORD=password
        volumes:
          - mongo-data:/data/db
      mongo-express:
        image: mongo-express
        ports:
          - 8080:8081
        environment:
          - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
          - ME_CONFIG_MONGODB_ADMINPASSWORD=password
          - ME_CONFIG_MONGODB_SERVER=mongodb
    volumes:
      mongo-data:
        driver: local


!!  añadir  fichero  .env y hacer docker-compose config
No hace falta network  porque crea  una para todos ellos.

Ejecución
Comprobamos  trazas al hacer el up,  creación de networkks y  containers, logs mezclados al arrancar ambos a la vez. connection refused porque todavía no ha arrancado mongodb. -> se puede configurar en el docker-compose para que espere.

docker-compose up -f [<file-name>] -d

docker-coompose down  -> rm de todo

docker-compose stop/start


dockerfile:
```
FROM node:13-alpine

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

RUN mkdir -p /home/app

COPY ./app /home/app

# set default dir so that next commands executes in /home/app dir
WORKDIR /home/app

# will execute npm install in /home/app because of WORKDIR
RUN npm install

# no need for /home/app/server.js because of WORKDIR
CMD ["node", "server.js"]
```

Mejor variables de entorno fuera,  en el docker-compose!!!
Si cambian  no  hay  que hacer rebuild de  la  imagen!!!        

