# Meet Docker

### The emergence of Docker

- Began as internal project for dotCloud organization (PaaS) - open sourced on 2013
- Version 1.0 launched in June 2014

### What is containerization ?

- Docker utilizes Linux Containers (LXC), rely on Linux kernel **cgroups**
- Operating System virtualization method that can be utilized to run multiple isolated Linux systems in a single host
- They all utilize the host's kernel version

### Docker Differences

- Docker - Container management system
- lets you create containers in virtual environments (on Mac and Windows)
- Commands in the development and production are pretty much the same
- *Theres no need for a full OS everytime we bring up a new container, which cuts down the overall size and resource footprint of containers*
- Another advantage is size of images when they are born - they dont contain kernel / OS. Incredibly small, compact and easy to ship

### Docker Benefits

#### Overall Benefits

- **Portability** - You can ship environments to all kinds of infrastructure without worrying about building VMs.
- **Quick deployment/teardown** - Very few commands required. Single command to spin up new / destroy container. Consumes a few minutes only.
- **Managing Infrastructure-like code** - To change the environment simply update the Dockerfile
- **Open Source** - Highly customizable
- **Consistency** - When you setup a container using a Dockerfile, the container will act the same in any given system
