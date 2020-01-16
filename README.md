# Docker-Learning
- Some notes I made while learning basic Docker functionality.

![Git](https://nickjanetakis.com/assets/blog/cards/differences-between-a-dockerfile-docker-image-and-docker-container-001320c81dd8d2989df10d0bec36341fd6a94b043f6f9de1c26ee79eaf16e566.jpg)

## Let's get started
- https://hub.docker.com/
- Download "Docker Desktop for Mac"
- Custom and official Docker IMAGES are on DockerHub, e.g - https://hub.docker.com/_/ubuntu/

## Some core lingo
- Docker IMAGES are used to create CONTAINERS
- A CONTAINER is an instance of an IMAGE
- A CONTAINER is a slimmed down version of an OS
- A DockerFile is basically a type of config file
- VOLUMES allow CONTAINERS to see files from the host machine

## Basic commands
```
docker images = lists all offline images on your machine
docker ps -a = display current running containers
docker ps = display current running containers
docker rmi <imageID> = Deleting an image from your computer
```

## Juicy commands
- **-it** pops us into a container upon creating 
- **bash** commands are set

```
docker run ubuntu = creates an ubuntu container with random name (will download if image is not on machine already)
docker run --name LOL ubuntu = runs an ubuntu container with my custom name "LOL"
docker run -it ubuntu bash = if no --name, a random name will be assigned from docker
```

## Juicier commands
- **-v** mounts a file or drive
- **--rm** deletes a container upon exiting it

```
docker rm $(docker ps -a -f status=exited -q) = delete all local containers
docker run -it --name my-linux-container --rm -v notes.txt ubuntu bash
docker run -it --name my-linux-container --rm -v /c/Users/:/my-data ubuntu bash 
```

## Creating a container from my own image file

This is the basic setup
```
MC-S104581:docker-tutorial dalyw01$ ls
Dockerfile	notes.txt
```

Build an IMAGE from the Dockerfile
- **.** indicates DockerFile is in same directory

```
docker build -t my-ubuntu-image . = build an image from my local Dockerfile
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

## Creating a node JS container, running it locally and viewing on my host machine

Make a directory called my-node-app and go into it

```
MC-S104581:Desktop dalyw01$ mkdir my-node-app
MC-S104581:Desktop dalyw01$ cd my-node-app/
MC-S104581:my-node-app dalyw01$ ls
Dockerfile	index.js	package.json
```

index.js, package.json and DockerFile are located in this repo, copy them into the my-node-app folder

Inside this folder have a file called "Dockerfile" with the following - 

```
    # Dockerfile  
    FROM node:8  
    WORKDIR /app  
    COPY package.json /app  
    RUN npm install  
    COPY . /app  
    EXPOSE 8081  
    CMD node index.js
```

Now create a new IMAGE from it, calling it "wills_node_image"

```
docker build -t wills_node_image .
```

Once you run that command you get should get a lot of output, it ends with something like

```
Step 7/7 : CMD node index.js
 ---> Running in 60b8cefe8344
Removing intermediate container 60b8cefe8344
 ---> 891ddb571099
Successfully built 891ddb571099
Successfully tagged wills_node_image:latest
```

Check the IMAGE has been built successfully

```
MC-S104581:my-node-app dalyw01$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
wills_node_image    latest              891ddb571099        15 seconds ago      904MB
```

Now you can run it!

```
docker container run -p 4000:8081  wills_node_image
```

Verify it's running

```
MC-S104581:my-node-app dalyw01$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
e69acf6e20f6        wills_node_image    "docker-entrypoint.s…"   13 minutes ago      Up 13 minutes       0.0.0.0:4000->8081/tcp   fervent_wing
```

Visit http://localhost:4000/ on your host machines browser, you should see "Hello World!"

However, any changes we make to index.js will not be reflected in our container

So we will kill it

```
docker kill e69acf6e20f6
```

Verify it's been killed successfully

```
MC-S104581:my-node-app dalyw01$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

So if you want to make changes to index.js,make the changes then run the following

re-build the image

```
docker build -t wills_node_image .
```

re-run the container

```
docker container run -p 4000:8081  wills_node_image
```

You should see the changes in your browser!


![An image displaying HTML output of our nodeJS app](browser.jpg)

## Commands summarized


- *name* 
- *-it* pops us into a container upon creating 
- *bash* commands are set
- *-v* mounts a file or drive
- *--rm* deletes a container upon exiting it
- *.* indicates DockerFile is in same directory

```
Listing docker info

docker images
docker ps
docker ps -a 

Creating containers

--name = gives container a name
-it = pops us into a container upon creating 
bash = set bash terminal

docker run ubuntu
docker container run ubuntu
docker container run -it ubuntu bash
docker container run ubuntu bash
docker container run --name HAHAHAHAHAHAHAHAH ubuntu

-v = mounts a file or drive [path to local]:[path inside container]
--rm = deletes a container upon exiting it

docker container run --name my-linux-container -v notes.txt ubuntu bash
docker container run -it --name my-linux-container --rm -v notes.txt ubuntu bash 
docker container run -it --name water123123 --rm -v /Users/dalyw01/Desktop/docker-tutorial/my-data:/my-data ubuntu bash
docker container run -p 4000:8081 wills_node_image

Building custom Images from Dockerfile

-t = names an image

docker build -t my-ubuntu-image .
docker build -t wills_node_image .

Destroying images and containers

docker image rm wills_node_image --force
docker kill e69acf6e20f6 = kill a specific container
docker rm $(docker ps -a -f status=exited -q) = delete all local containers
```
