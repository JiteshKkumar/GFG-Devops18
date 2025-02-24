### Basic Docker Commands

- **docker inspect <container_name>** - This command will allow you to inspect a container. It will give you detailed information about the container such as the IP address, the volumes, the environment variables, etc.
- **docker exec -it <container_name> bash** - This command will allow you to execute bash program inside your container, you can also run any other program like date, python3 etc.
- **docker run -it -p 80:80 -v /local_dir:/container_dir <image_name>** - This command will allow you to run a container with the given image. The `-it` flag will allow you to run the container in interactive mode with a terminal. The `-p` flag will allow you to map the port of the container to the port of the host machine. The `-v` flag will allow you to mount a local directory/volume to a directory inside your container. So we can have a persistent storage for our containers.
- **docker cp <container_name>:<container_dir> <local_dir>** - This command will allow you to copy files from a container to your local machine.
- **docker cp <local_dir> <container_name>:<container_dir>** - This command will allow you to copy files from your local machine to a container.

---

### Dockerfile

- It is a file that contains instructions on how to build an image.

**Example Dockerfile**

```dockerfile
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install -y nginx
CMD ["nginx", "-g", "daemon off;"]
```

- After creating a Dockerfile, you can build an image with the help of the following command:

```bash
docker build -t <image_name:version> .
```

**Dockerfile Instructions**

- **FROM** - This instruction is used to specify the base image.
- **RUN** - This instruction is used to run commands at the time of building the image.
- **LABEL** - This instruction is used to add metadata to the image. You can specify any key-value pair as metadata such as maintainer, description, version, etc.
- **COPY** - This instruction is used to copy files from the local machine to the docker image.
- **ENV** - This instruction is used to set environment variables inside of your image.
- **WORKDIR** - This instruction is used to set the working directory for the instructions that follow it.
- **CMD** - This instruction is used to specify the command that needs to be executed when a container is created from the image.
- **ENTRYPOINT** - This instruction is used to specify the command that needs to be executed when a container is created from the image. You can specify any command that you would normally run on a Linux machine. The difference between CMD and ENTRYPOINT is that CMD can be overridden by passing arguments to the docker run command. Whereas, ENTRYPOINT cannot be overridden by passing arguments to the docker run command.

**CMD** vs **ENTRYPOINT** - The difference between CMD and ENTRYPOINT is that CMD can be overridden by passing arguments to the docker run command. Whereas, ENTRYPOINT cannot be overridden by passing arguments to the docker run command. For example, if you have a Dockerfile with the following CMD instruction:

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

You can override the CMD instruction by passing arguments to the docker run command like this:

```bash
docker run -it <image_name> bash
```

But if you have a Dockerfile with the following ENTRYPOINT instruction:

```dockerfile
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

You cannot override the ENTRYPOINT instruction by passing arguments to the docker run command like this, you can just pass an extra argument to the mentioned command in Entrypoint:

```bash
docker run -it <image_name> bash
```

---

### Docker Compose

It is a tool for defining and running multi-container Docker applications. With Compose, We can use a YAML file to configure our application’s services. Then, with a single command, We can create and start all the services from our configuration.

**Example docker-compose.yml of Wordpress Application**

```yaml
version: '3.1'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```

- After creating a docker-compose.yml file, you can run the following command to start the application:

```bash
docker-compose up -d
```
- To delete all the containers created by docker-compose, you can run the following command:

```bash
docker-compose down
```
---

### Docker Hub

Docker Hub is a container registry built for developers and open source contributors to find, use, and share their container images. With DockerHub, developers can host public repos that can be used for free, or private repos for teams and enterprises.

To push an image to Docker Hub, you can run the following commands:

```bash
docker login
docker tag <image_name> <docker_hub_username>/<image_name>
docker push <docker_hub_username>/<image_name>
```

To pull an image from Docker Hub, you can run the following command:

```bash
docker pull <docker_hub_username>/<image_name>
```

To Push image into any other registry, you can run the following commands:

```bash
docker login <registry_url>
docker tag <image_name> <registry_url>/<image_name>
docker push <registry_url>/<image_name>
```
---