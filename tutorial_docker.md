**WIP**

# Uso de Docker en el servidor `simidat-apps`

## Instalación de Docker en local

Lo primero es instalar Docker. Para ello, podemos comprobar si `docker`o `docker-ce` ya están disponibles desde nuestro gestor de paquetes favorito (`apt`, `pacman`, `dnf`...) o bien seguir [las instrucciones oficiales](https://www.docker.com/community-edition).

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

~~~Dockerfile
# Use an official Ruby runtime as a parent image
FROM ruby:2.4.2-onbuild

# Make port 80 available to the world outside this container
EXPOSE 80

# Run app.py when the container launches
CMD ["bundle", "exec", "jekyll", "serve", "-H", "0.0.0.0", "-P", "80"]
~~~

~~~bash
docker build -t mi_container .
docker run -p 8080:80 mi_container
~~~

## Carga del contenedor a Docker Cloud

~~~bash
docker build -t user/container:latest .
docker push user/container:latest
~~~

## Instalación en `simidat-apps`

### Creación de un *virtual host*

~~~xml
<VirtualHost *:80>
  ServerName cometa.ml
  ServerAlias cometa.ml
  ProxyPass "/"  "http://localhost:4040/"
  ProxyPassReverse "/"  "http://localhost:4040/"
</VirtualHost>
~~~

### Instalación del contenedor

~~~bash
docker run -p 8080:80 user/container
~~~

### Actualizaciones

~~~bash

~~~
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MTYwMjI1MF19
-->