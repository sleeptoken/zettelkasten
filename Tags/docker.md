
2025-04-23 20:38

Images can be downloaded using the `docker pull` .  Images have these labels called `tags`. These _tags_ are used to refer to variations of an image. 
- When specifying a tag, you must include a colon `:` between the image name and tag, for example, `ubuntu:22.04` `(image:tag)`

`docker image ls` - This command allows us to list all images stored on the local system
`docker image rm` - If we want to remove an image from the system
### Running containers 

to run a Docker container: `docker run [OPTIONS] IMAGE_NAME [COMMAND] [ARGUMENTS...]`.
example -  `docker run -it helloworld /bin/bash`
- `-it` - This will allow us to interact with the container directly.
- `/bin/bash` - I am going to spawn a shell

 After getting a shell, The hostname of a container is the container ID (which can be found by using `docker ps`)

[Running containers | Docker Docs](https://docs.docker.com/engine/containers/run/)
### DockerFiles (important)

Dockerfiles is a formatted text file which essentially serves as an instruction manual for what containers should do and ultimately assembles a Docker image.
- You use Dockerfiles to contain the commands the container should execute when it is built.
- Dockerfiles are formatted in the following way:  `INSTRUCTION argument`

```yml
# THIS IS A COMMENT
# Use Ubuntu 22.04 as the base operating system of the container
FROM ubuntu:22.04

# Set the working directory to the root of the container
WORKDIR / 

# Create helloworld.txt
RUN touch helloworld.txt
```
 don’t expect a command to be there from the start (even commands like _curl_, _ping_, etc., may need to be installed.)
### Building

`docker build -t helloworld .` 
1. We are going to name it ourselves, so we are going to use the `-t` argument.
2. We want to name the image.
3. The Dockerfile is located in our current working directory (`.`).
### Compose 

allows multiple containers (or applications) to interact with each other when needed while running in isolation from one another.

example -
1. Creating the network between the two containers: `docker network create ecommerce`
2. Running the Apache2 webserver container: `docker run -p 80:80 --name webserver --net ecommerce webserver`
3. Running the MySQL Database server: `docker run --name database --net ecommerce webserver`
 Do we want to do this for every container, every time? I certainly don’t.
 Instead, we can use Docker Compose via `docker-compose up` to run these containers together
#### Docker-compose.yml files

One file to rule them all. The formatting of a _docker-compose.yml_ file is different to that of a Dockerfile. It is important to note that YAML requires indentation
_Check out the [compose file](https://docs.docker.com/compose/compose-file/) documentation for all possible instructions._
### Sockets

When you install Docker, there are two programs that get installed:

1. The Docker Client
2. The Docker Server

Docker works in a client/server model. Docker achieves this communication using something called a socket. Sockets are an essential feature of the operating system that allows data to be communicated. 

 A socket can either be a network connection or what is represented as a file. What's important to know about sockets is that they allow for Interprocess Communication (IPC). This simply means that processes on an operating system can communicate with each other!

In the context of Docker, the Docker Server is effectively just an API. The Docker Server uses this API to **listen** for requests, whereas the Docker Client uses the API to **send** requests.

we can interact with the Docker Server using commands like `curl` or an API developer tool such as Postman.

### References
[TryHackMe | Intro to Docker](https://tryhackme.com/room/introtodockerk8pdqk)