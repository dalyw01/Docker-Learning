# Docker-Learning
Some notes I made while learning basic Docker commands! 

## To start

- https://hub.docker.com/
- Download "Docker Desktop for Mac"

## Super basic commands

```
docker images = lists all offline images on your machine
docker ps -a = display current running containers
```

## Little bit more juicy

```
docker run ubuntu = creates an ubuntu container with random name (will download if image is not on machine already)
docker run --name LOL ubuntu = runs an ubuntu container with my custom name "LOL"
docker run -it ubuntu bash = "-it" pops us into a container upon creating, "bash" commands are set
```
## Little bit juicier again
```
docker rm $(docker ps -a -f status=exited -q) = delete all local containers
docker run -it --name my-linux-container --rm -v notes.txt ubuntu bash 	= rm deletes container when you exit, -v copies code to container
docker run -it --name my-linux-container --rm -v /c/Users/:/my-data ubuntu bash = rm deletes container when you exit, -v copies code in folder
```

## Creating and running own Image file

Setup
```
MC-S104581:docker-tutorial dalyw01$ ls
Dockerfile	notes.txt
```
Build an IMAGE from the Dockerfile
```
docker build -t my-ubuntu-image . = build my local image, "." indicates DockerFile is in same directory
```
Now running the new IMAGE to create a CONTAINER
```
docker run -it my-ubuntu-image bash
```
Verifying Python is actually installed
```
root@29aacfd9b882:/# python3
Python 3.6.9 (default, Nov  7 2019, 10:44:02) 
```
