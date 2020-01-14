# Docker-Learning
Some notes I made while learning basic Docker commands! 

To start

https://hub.docker.com/
Download "Docker Desktop for Mac"

Super basic commands

```
docker images									= lists all offline images on your machine
docker run hello-world 							= runs a hello-world container
docker run --name LOL hello-world				= runs a hello-world container with my custom name "LOL"
docker run -it ubuntu bash						= -it pops us into a container upon creating, bash commands set (downloads Ubuntu if not installed)
docker ps -a  									= display current running containers
```
Little bit more juicy
```
docker rm $(docker ps -a -f status=exited -q)   = delete all local containers
docker run -it --name my-linux-container --rm -v notes.txt ubuntu bash 			= rm deletes container when you exit, -v copies code to container
docker run -it --name my-linux-container --rm -v /c/Users/:/my-data ubuntu bash = rm deletes container when you exit, -v copies code in folder
```

Setup for a new image file I created 
```
MC-S104581:docker-tutorial dalyw01$ ls
Dockerfile	notes.txt
docker build -t my-ubuntu-image . 												= build my local image, "." indicates DockerFile is in same directory
```
Now running the new image to create a container
```
docker run -it my-ubuntu-image bash
```
Verifying Python is actually installed
```
root@29aacfd9b882:/# python3
Python 3.6.9 (default, Nov  7 2019, 10:44:02) 
```

MC-S104581:docker-tutorial dalyw01$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
my-ubuntu-image     latest              4500489ba57a        About a minute ago   127MB
<none>              <none>              93aa485029f8        24 hours ago         127MB
ubuntu              latest              549b9b86cb8d        3 weeks ago          64.2MB
docker-whale        latest              e0fc5a9836fc        11 months ago        278MB
hello-world         latest              fce289e99eb9        12 months ago        1.84kB
docker/whalesay     latest              6b362a9f73eb        4 years ago          247MB

MC-S104581:docker-tutorial dalyw01$ docker ps -a

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
129c0282096c        ubuntu              "bash"              40 seconds ago      Exited (0) 38 seconds ago                       nifty_hermann
a417f749326d        ubuntu              "bash"              3 minutes ago       Exited (0) 44 seconds ago                       agitated_hermann
e36f859db9dd        hello-world         "/hello"            6 minutes ago       Exited (0) 6 minutes ago                        LOL
722a0fa3ef6e        hello-world         "/hello"            8 minutes ago       Exited (0) 8 minutes ago                        musing_wescoff
b2a0385b6985        hello-world         "/hello"            9 minutes ago       Exited (0) 9 minutes ago                        my-hello























