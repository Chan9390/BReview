# Working with Containers

COMMAND | DESCRIPTION
---|---
`docker --help` | Full list of docker commands
`docker COMMAND --help` | Additional information about using the command
`docker version` | Displays docker version

### Using Docker images

- Docker images can be viewed using : `Docker images`
- Image ID is unique 64 hexadecimal digit string of characters

### Searching Docker images

- Search anything on DockerHub using the `docker search` command
- An automated build is a Docker image that builds automatically when a linked Git repository is updated.
- If you find an image that you want to use - `docker pull <image name>`. Ex: `docker pull tutum/ubuntu`
- To see the list of downloaded images - `docker images`
- To remove unwanted images use - `docker rmi`
- To force remove the image use `-f`
- If two or more containers have the same Image ID, deleting any one using their name, will also delete others.

### Manipulating Docker images

- Most basic way to run a container is :  `docker run -i -t <image_name>:<tag> /bin/bash`
- Example : `docker run -i -t nginx:latest /bin/bash`
- `-i` gives interactive shell into running container
- `-t` will allocate a pseudo tty
- The options could be used together, example: `-it`
- To run in *daemon* mode: `docker run -d <image_name>:<tag>`

- To see the running containers: `docker ps`
- To see all the containers (which includes those which are not running) : `docker ps -a`
- The names of each container is created randomly. You can also specify it using `--name=` switch
- To expose ports on the container, i.e. for **port forwarding** use : `docker run -p <host_port>:<container_port> <image>:<tag>`
- To see the logs, use `docker logs`. To see the logs specific to one container: `docker logs <tag>` or `docker logs <container_name>`

### Stopping containers

- `docker kill` - kills the container immediately
- `docker stop` - graceful shutdown of the container
- `docker rename` - changes the name of the container. Ex: `docker rename <current_container_name> <new_container_name>`
- `docker stats <container_name>` - gives information about CPU utilization, Memory usage, etc.
- `docker top <container_name>` - gives information on list of processes running
- `docker rm <container_name>` - removes unwanted containers
