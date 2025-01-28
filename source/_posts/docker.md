---
title: A Very Informal Introduction to Docker
---

### Container

- A way to package application with all necessary dependencies and configuration
- Portable artifact, easily shared and moved around
- Makes development and deployment more efficient
- Live in **container repositories** - [DockerHub](https://hub.docker.com) is a public repo 

### Before containers

- Installation process is different on each OS environment, which is a hassle for collaboration
- Lengthy documentation to install dependencies

### After containers

- Own isolated environment to run
- Contains are packaged with all needed configuration
- One command to install an app
- Even run same app with 2 different versions, with no conflict
- Only the Docker runtime has to be configured (one-time) on the server.

### What is a container?

- Technically a container is a layer of multiple images
- Base layer is a Linux image, usually `alpine` to keep the image size small
- Other application layers are stacked on top of it, based on requirements
- Public containers on Docker Hub can be pulled by using `docker pull`
- In case of a change in any image (layer), only the changes would be downloaded - this is an advantage of containers having multiple layers
- List all running containers using `docker ps`


### Docker vs. VMs

- Docker and VMs are both virtualization tools
- Docker, however, virtualizes only the applications layer of the OS
- VMs have their own OS kernel, meaning they're larger and slower

![Filesystem](/images/docker/filesystem.png)

### Container vs. Image

- Container is a running environment for image
- Every container has its own port assigned
- An application can only connect to a *container*, not an *image*
- Multiple versions of images can run in separate containers simultaneously, however, will run on the same *container* port
- Container ports are different from host ports, and if two containers with the same application are running, they need to be bound to different host ports.

```sh
docker run <container_name> -p<host_port>:<container_port>
```

### Docker commands

- Additionally, containers can be run in detached mode (which is the case for most applications), by passing the `-d` flag. 
- `docker run = docker pull + docker start` (works for public Docker containers on Docker Hub)
- `docker rm` removes a container, `-f` force deletes even if the container is active
- `docker logs <container_id>` or `docker logs <name>` shows the logs
- `docker run <container> --name my_container` creates a container with specified name, else a random string is used
- `docker exec -it <container> <command>` executes the given command in the specified container (`it -> interactive terminal`)
- `docker run` creates a new container by taking an image as a parameter, `docker start` starts an existing container
- The `-e <KEY=VALUE>` flag is used to pass environment variables to the container

### Docker workflow

![Workflow](/images/docker/workflow.png)

### Docker Network

- Containers inside the same network can communicate using just their container names
- Applications running outside the network can access the contains using `localhost:<container_port>`

```sh
# create new network
docker network create <network_name>

# list existing containers
docker network ls

# specify target network when creating a container
docker run <container_name> --net <network_name>
```

- For example, a MongoDB and a Mongo Express container can run side by side allowing for easy contact between each other just using their container names

### Docker Compose

- Running multiple containers with `docker run` is a tedious task, especially in a network
- We write `.yaml` files to automate starting dockers and passing arguments on container startup 

![Compose](/images/docker/run-vs-compose.png)

- Each entry under `services` maps to a container name, and `environment` contains environment variables
- `ports` contains the host port - container port mapping
- Docker Compose creates its own network between the containers listed in the `.yaml`file
- Indentation in compose file **matters**

```sh
# create a container network from example.yaml
docker-compose -f example.yaml up 
```

- When a container is restarted, all configuration is lost (no data persistence)
- In order to achieve data persistence, we use Docker Volumes

### Dockerfile 

- A `Dockerfile` is used to create our own Docker image
- Always start an image by basing it on a pre-existing image, like `node` or `postgres`
- Optionally define environment variables, although recommendation is to store them in a compose file because it allows for changes without having to recreate the entire image

```Dockerfile
ENV KEY VALUE
```

- `RUN` is used to run virtually any Linux command (within the container, not the environment)
- `COPY` executes on the **host machine** 

```Dockerfile
COPY <source_on_host> <destination_on_container>
```

- `CMD` serves as an entry point to the application. There can only be one `CMD` command in a `Dockerfile` but multiple `RUN` commands

```Dockerfile
CMD ["yarn", "start"]
```

 - `FROM` specifies a pre-existing image which the current image will be built on top of

```bash
docker build -t <image_name> . # from current dir
```

Delete images using `docker rmi <image_name>`

![Volume](/images/docker/volume.png)

### Further learnings

- Create an image of your app for running it on a container
- `alpine` uses `apk` as the package manager, not `apt`

```sh
apk update && apk add <package_list>
```
