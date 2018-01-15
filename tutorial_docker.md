# Uso de Docker en el servidor `simidat-apps`

## Instalación de Docker en local

Lo primero es instalar Docker en nuestro ordenador personal. Para ello, podemos comprobar si `docker`o `docker-ce` ya están disponibles desde nuestro gestor de paquetes favorito (`apt`, `pacman`, `dnf`...) o bien seguir [las instrucciones oficiales](https://www.docker.com/community-edition).

Una vez instalado, podemos comprobar que la instalación es correcta ejecutando
~~~bash
$ docker run hello-world
~~~
Nos debería aparecer un mensaje de confirmación. Si no llega a aparecer este mensaje, hay varias posibilidades:

- El servicio de Docker no está arrancado. Lo lanzamos con `systemctl start docker`. Es conveniente arrancarlo al inicio del sistema, para ello ejecutamos `systemctl enable docker`.
- No tenemos permiso para comunicarnos con el socket. Añadimos nuestro usuario al grupo `docker` :
~~~bash
$ sudo groupadd docker
$ usermod -aG docker $USER
~~~
- [Otros posibles problemas se detallan en la documentación](https://docs.docker.com/engine/installation/linux/linux-postinstall/)

## Creación de nuestro servicio en un contenedor

Los servicios que se ejecutan en un contenedor Docker vienen determinados por el *Dockerfile*. Generalmente estos consisten en una imagen básica (una distro de Linux o una plataforma de un lenguaje), la instalación de paquetes necesarios, y la ejecución de un comando.

Por ejemplo, el siguiente sería un Dockerfile útil para servir un blog creado con Jekyll (sobre Ruby):

~~~Dockerfile
# Use an official Ruby runtime as a parent image
FROM ruby:2.4.2-onbuild

# Make port 80 available to the world outside this container
EXPOSE 80

# Run Jekyll when the container launches
CMD ["bundle", "exec", "jekyll", "serve", "-H", "0.0.0.0", "-P", "80"]
~~~

En [Docker Hub](https://hub.docker.com/explore/) podéis encontrar una lista de imágenes oficiales de las que partir. En GitHub se pueden encontrar Dockerfiles personalizados para lanzar todo tipo de servicios.

Una vez escrito el Dockerfile, nos situamos con la terminal en el directorio donde se ha creado y ejecutamos:

~~~bash
docker build -t mi_container .
docker run -p 8080:80 mi_container
~~~

La opción `-p` permite redirigir el puerto 8080 del anfitrión al 80 del contenedor. Así, accediendo a `localhost:8080

## Carga del contenedor a Docker Cloud

Tras personalizar nuestro Dockerfile y comprobar su funcionamiento

~~~bash
docker build -t user/container:latest .
docker push user/container:latest
~~~

## Instalación en `simidat-apps`

### Acceso al daemon Docker

Pide al administrador del servidor que añada tu usuario al grupo `docker` del servidor. Consulta también en qué puerto del servidor puedes colocar tu servicio.

### Creación de un *virtual host*

~~~xml
<VirtualHost *:80>
  ServerName cometa.ml
  ServerAlias cometa.ml
  ProxyPass "/"  "http://localhost:6001/"
  ProxyPassReverse "/"  "http://localhost:6001/"
</VirtualHost>
~~~

### Instalación del contenedor

~~~bash
docker run -p 6001:80 user/container
~~~

### Actualizaciones

~~~bash
docker pull user/container
docker ps # consulta el id de tu contenedor
docker stop <id>
docker run -p 6001:80 user/container
~~~
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExODYwNjEzNTQsMTgzMTE1NzQ2N119
-->