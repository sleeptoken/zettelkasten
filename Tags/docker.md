
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




### References
[TryHackMe | Intro to Docker](https://tryhackme.com/room/introtodockerk8pdqk)