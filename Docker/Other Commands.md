# Docker commands: 

Running containers : docker ps

All containers : docker ps -a

Pull the image : docker pull [image-name]:[tag/version]

Shows the images : docker images

inspect the image : docker inspect [image-id]

create the container out of image : docker create [image-id] 

start the container created : docker start [container-id]

instead of pull the image, create the container and start the container you can directly : docker run [image-name]:[tag/version]
docker run = pull + create + start

Options:
-d = detach mode
-P = assigns a random host port to container port

-p = you can choose the host port
hostport:container-port

docker run -d -p 8080:80 nginx
curl http://[IP-Address]:[host-port]


Enter the terminal of running container : docker exec -it [container-id] bash

Just execute the command no need to enter inside container. : docker exec [container-id] command

remove the container : docker rm [container-id]

-f = force

remove all containers : docker rm -f `docker ps -a -q`

environment variables
docker run -d -e key=value [container-id]
to check docker exec [container-id] env

docker volumes
by default docker removes the data when you remove the container, but you can create any folder in the host machine and map it to docker container using -v.

Now even you remove the container, data will not be removed you can attach it to another container again.
mkdir mysql-data
docker run -d -v ~/mysql-data:/var/lib/mysql mysql




### Stopping Containers:
docker stop <container-id> - Stop a running container with the specified ID.
docker stop $(docker ps -aq) - Stop all running containers on the host.

### Removing Containers:
docker rm <container-id> - Remove a stopped container with the specified ID.
docker rm $(docker ps -aq) - Remove all stopped containers on the host.

### Force Removing Containers:
docker rm -f <container-id> - Forcefully remove a container with the specified ID, even if it's still running.
docker rm -f $(docker ps -aq) - Forcefully remove all containers on the host, including running containers.

### Removing Images:
docker rmi <image-id> - Remove the image with the specified ID.
docker rmi <image-name> - Remove the image with the specified name and tag.
docker rmi $(docker images -aq) - Remove all images on the host.

### Force Removing Images:
docker rmi -f <image-id> - Forcefully remove the image with the specified ID, even if it's being used by a running container.
docker rmi -f <image-name> - Forcefully remove the image with the specified name and tag, even if it's being used by a running container.
docker rmi -f $(docker images -aq) - Forcefully remove all images on the host, even if they're being used by running containers.


docker run <image> - Start a new container based on the specified image.
docker run -d <image> - Start a new container in the background (detached mode).
docker run -it <image> - Start a new container in interactive mode (with a shell).
You can also specify additional options when starting a container. Here are some examples:

docker run -p <host-port>:<container-port> <image> - Map a host port to a container port.
docker run -v <host-path>:<container-path> <image> - Mount a host directory as a volume in the container.
docker run --name <name> <image> - Specify a name for the container.
