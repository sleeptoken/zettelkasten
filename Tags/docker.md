
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







### References
[TryHackMe | Intro to Docker](https://tryhackme.com/room/introtodockerk8pdqk)