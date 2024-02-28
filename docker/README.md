# Before containerization 

# docker architecture 

# docker component 

### docker images 


1. Pull an image from a Docker registry like Docker Hub:
   ```
   docker pull IMAGE_NAME
2. List all existing images in the local repository:
   ``` 
   docker images 
3. Remove an image from the local repository:
   ```
    docker rmi IMAGE_NAME (or IMAGE_ID)
4. Tag an image: Tagging an image allows us to identify it and push it to our Docker Hub account for later use by other users.
   ``` 
   docker tag IMAGE_NAME soulredouane/IMAGE_NAME:TAG
5. Push an image to a Docker registry:
   ```
   docker tag IMAGE_NAME soulredouane/IMAGE_NAME:TAG
-----

### Containers

1. Run a container from an image (e.g., Ubuntu image):
   ``` 
   docker run ubuntu (or IMAGE_ID)
2. List all running containers:
   ```
   docker ps 
3. List all containers (running and stopped):
   ```
   docker ps -a
4. Remove a container (a running container should be stopped before removal):
   ``` 
   docker rm CONTAINER_NAME (or CONTAINER_ID)
5. Remove a running container (force remove a container even if it is running):
   ``` 
   docker rm -f CONTAINER_NAME (or CONTAINER_ID)

6. Get the logs of a running container (useful for monitoring application status):
   ``` 
   docker logs CONTAINER_NAME (or CONTAINER_ID)
7. Access the operating system inside a container:
   ```
   docker exec -it CONTAINER_NAME (or CONTAINER_ID) bash (or sh) # sh or bash option depends on wich shell the OS exist inside the container 
   ex: docker run -d --name my-ubuntu-container ubuntu sleep infinity


8. Execute Linux commands inside the container:
   ``` 
   docker exec CONTAINER_NAME (or CONTAINER_ID) command_name
   ex: docker exec contaienr-name  ls 

9. Stop a running container
   ```
    docker stop CONTAINER_NAME(or CONTAINER_ID)
10. Restart a container 
    ```
    docker restart CONTAINER_NAME(or CONTAINER_ID)

11. Copy a file from your machine to a Docker container :
    ```
    docker cp myfile.txt CONTAINER_NAME:/path/in/container
12. Copy a file from a container to host this time 
    ```
    docker cp CONTAINER_NAME:/path/in/container /path/on/host
13. Run a container by exposing its port 
    ```
    docker run --name CONTAINER_NAME -p 8080:8080  jenkins
14. Run a container with environment variables, port and volume 
    ```
    docker run --name my-postgres-container \
     -e POSTGRES_PASSWORD=soul-redouane -e POSTGRES_USER=soul \
     -p 5430:5432 \
     -d postgres
15. Take a snapshot of a container: Sometimes, you may have a running container, and you want to capture its current state as an image to share it with a friend, for example.
    ``` 
    docker commit CONTAINER_NAME IMAGE_NAME


-----------
### Docker Volumes

One of the container properties is that they are ephemeral, and when deleted, there is a risk of losing all data in that container. To tackle this problem, Docker provides a solution called Docker volumes.

Docker volumes work as follows: when you create a Docker volume, you reserve some space on your machine to be used by Docker. But to use this volume, you should attach it to a container.



There are three types of Docker volume:
- **Anonymous Volume**: -v /var/lib/mysql/data  # /var/lib/docker/volumes/random_hash/_data
- **Named Volume**:  -v VOLUME_NAME:/var/lib/mysql/data
- **Host Volume**: -v /home/mount/data:/var/lib/mysql/data
-----

#### The following commands are specific to Named Volume type except  number 6 is for Host volume.

1. Create a Docker volume:
   ```
   docker volume create my_volume
2. List all volumes:
   ``` 
   docker volume list 
3. Inspect a volume: To obtain detailed information about a volume.
   ``` 
   docker volume inspect my_volume
4. Remove an existing volume:
   ```
   docker volume rm my_volume 

5. Run a container with a **Named Volume** (attach my_volume to the container):
   ```
   docker run -v my_volume:/path/in/container image_name
   ex :
   docker run --name my-postgres-container-ftp \
   -e POSTGRES_PASSWORD=soul-redouane \
   -e POSTGRES_USER=soul \
   -p 5430:5432 \
   -v my_volume:/var/lib/postgresql/data \
   -d postgres

6. Example: Running a PostgreSQL instance with a **Host Volume**:
   ```
   docker run --name my-postgres-container-ftp \
   -e POSTGRES_PASSWORD=soul-redouane \
   -e POSTGRES_USER=soul \
   -p 5430:5432 \
   -v ~/docker-volumes-ftp:/var/lib/postgresql/data \
   -d postgres

----------
### Dockerfile Overview

A Dockerfile is utilized to construct custom Docker images. It is commonly employed to dockerize (build a Docker image) applications such as React apps or Spring Boot applications. To create a Dockerfile, one must script a series of commands tailored to the specific requirements or nature of the application intended for dockerization.

#### Dockerfile for a React App

You can find an example of a Dockerfile for a React app [here](https://github.com/SOULREDOUANE/initial-react-app-dockerized-configured-for-runtime-env-variables/blob/main/Dockerfile).

This Dockerfile is configured to dockerize a React application. It includes steps to set up the environment, install dependencies, build the application, and specify runtime environment variables.

* **FROM**: This directive indicates the base image upon which the image we are building should be based.
  ```Dockerfile
  FROM node:14-alpine
* **WORKDIR**: This directive tells Docker that all data coming from our host will be placed in this working directory.
  ```Dockerfile
  WORKDIR /app
* **COPY**: This command is used to copy data from the host machine to the image we want to build.
  ```Dockerfile
  COPY package.json /app
* **ADD**: The `ADD` command performs a similar function to `COPY`, but it can also copy data from the internet to the operating system of the image we want to build.
  ```Dockerfile
  ADD https://google.com/file.tar.gz /app
* **RUN**: The `RUN` command is used to execute commands in the operating system of the image we want to build.
  ```Dockerfile
  RUN apt-get update && apt-get install -y \
      package1 \
      package2 \
      && rm -rf /var/lib/apt/lists/*
* **USER**: The `USER` directive tells Docker which user should run commands in the image after they are executed.

  ```Dockerfile
  USER soul
* **EXPOSE**: The `EXPOSE` directive tells Docker which ports should be exposed to the local host.
  ```Dockerfile
  EXPOSE 3000
* **CMD**: The `CMD` directive specifies the main command that will be executed whenever the container starts. It's important to note that if this command fails, the container will also fail.
  ```Dockerfile
  CMD ["npm", "start"]
* **MAINTAINER**: The `MAINTAINER` directive was previously used to specify the name and email address of the person maintaining the Dockerfile. However, it is now considered deprecated in favor of using the `LABEL` directive to provide metadata about the image, including information about the maintainer.

* **LABEL**: The `LABEL` directive provides metadata about the image, including information about the maintainer.

  ```Dockerfile
  LABEL maintainer="Your Name <your.email@example.com>"
  


