# Other Docker Feature Sets

### Storing images on Docker registries

- Docker Hub comes in 3 flavors. They are used to store the images.
- **Docker Hub**: Free service hosted by Docker. Private image repositories is not free - paid service. URL: https://hub.docker.com/
- **Docker Trusted Registry**: This solution could be hosted or used in premise. Backend infrastructure is maintained by Docker.
- **Docker Registry**: Gives you ability to run your own Docker registry on your own hardware or in cloud environment to store images.

### Docker Machine

- Tool that can setup and manage your Docker hosts
- Can be used to provision Docker hosts on Mac or Windows machines and also manage/provision remote Docker hosts
- Can be accessed interminal using: `docker-machine`
- `docker-machine create` - Will create Docker hosts that your containers will run on
- Example: `docker create -d virtualbox node1`. This will create a new Docker host on locally installed VirtualBox with name node1
- `docker-machine ls` - List of Docker hosts that are currently running. If you were a part of Docker Swarm cluster, we could seethe information of the swarm cluster too.
- `docker-machine restart <host_name>` - restarts the host

### Docker Compose

- Used to create multiple containers with a single command. Ex: spin up web server, database server and/or file server
- Utilizes `docker-compose.yml` file to start up and configure all the containers that you have specified
- Could access in the terminal using the command: `docker-compose`
- `docker-compose` could be executed only in directory which contains `docker-compose.yml` file
- `docker-compose ps` - used to display information on containers running within a particular Docker Compose folder
- The output of the above command has a naming pattern. Ex: if the output is `galeracompose_master_1`, then it means the following:
  - `galeracompose` is the folder name
  - `master` part of the service name that is being used in the `docker-compose.yml` file
  - `1` is the index of the first service instance
- `docker-compose restart` - restarts all the containers that are being used in `docker-compose.yml` file
- `docker-compose restart <service>` - restarts the containers that are currently working
- `docker-compose restart` will only restart the containers that are currently running. It doesnt start the containers with EXIT status.
- If a container is in EXIT status, then it could be turned on using the `docker-compose up`. It starts all the containers specified in the compose file.
- `docker-compose up -d` will start container in daemon mode
- If you don't use `-d` switch and hit Ctrl+C, it will start shutting down the running containers.

### Docker Swarm

- allows you to create and manage clustered Docker servers
- Installation for Docker Swarm actually launches a container that is used as the Swarm Manager master to communicate to all the nodes in a Swarm cluster.

### Docker UCP

- Docker UCP (Universal Control Plane) allows to control various aspects of your Docker environment through web interface.
- Use Docker UCP to deploy various cloud solutions, tie into existing authentication infrastructure, and in turn control user access.
