# Docker Etc...
### Doing stuff with Docker

-
-
# Unit: 1 Containers
### Configuration

-
##In Terminal: docker version
```
zcw$ docker version
Client: Docker Engine - Community
 Version:           18.09.0
 API version:       1.39
 Go version:        go1.10.4
 Git commit:        4d60db4
 Built:             Wed Nov  7 00:47:43 2018
 OS/Arch:           darwin/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.0
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.4
  Git commit:       4d60db4
  Built:            Wed Nov  7 00:55:00 2018
  OS/Arch:          linux/amd64
  Experimental:     true
```

-
##In Terminal: docker info
```
zcw$  docker info
Containers: 11
 Running: 0
 Paused: 0
 Stopped: 11
Images: 71
Server Version: 18.09.0
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: bridge host ipvlan macvlan null overlay
 Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
Swarm: inactive
...
```

-
##In Terminal: docker
```
zcw$ docker
Management Commands:
  builder     Manage builds
  checkpoint  Manage checkpoints
  config      Manage Docker configs
  container   Manage containers
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes
```
-
-
##In Terminal: docker

```Java
zcw$ docker
Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  deploy      Deploy a new stack or update an existing stack
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  ...
```
-
-
##In Terminal: docker

```Java
(continued from previous slide)
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```
-
##Docker command line structure

####Previous versions of docker used :<br>
syntax : `docker <mgt-command> (options)`<br> 
example: `docker run`<br>
####Newer versions :<br>
syntax : `docker <mgt-command> <sub-command> (options)` <br>example : `docker container run`

-
##What we covered
- Command: `docker version` <br>
  &nbsp;&nbsp;○ Verified cli can talk to engine
- Command: `docker info`<br>
  &nbsp;&nbsp;○ Most config values of engine<br>
- Docker command line structure<br>
  &nbsp;&nbsp;○ Old (Still works): `docker <mgt-command> (options)`<br>
  &nbsp;&nbsp;○ New: `docker <mgt-command> <sub-command> (options)`

-
#Unit: 1 Containers
###Web Servers
   
-
##Image vs. Container
- An image is the application we want to run<br>
- A Container is an instance of that image running as a process<br>
- You can have many containers running off the same image<br>
- In this unit our image will be the Nginx server<br>
- Docker’s default image “registry” is called Docker Hub (hub.docker.com)

-
##Let’s start a new container
```
zcw$ docker container run --publish 80:80 nginx Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx [...]
```
Then open up your browser of choice and go to http://localhost:80 (refresh your screen a few times and look at the logs)<br>

- Downloaded image ‘nginx’ from Docker hub<br>
- Started a new container from that image<br>
  Option: `--publish 80:80`<br>
- Opened port 80 on the host container IP<br>
- Routes that traffic to the container IP, port 80

-
##Let’s start a new DETACHED container
```
zcw$ docker container run --publish 80:80 --detach nginx b2096a61e9464c2a60bb702545e7d5d4466e0f1f06ea178431d9ff6aec4f8532
```
Then open up your browser of choice and go to http://localhost:80
 
-
##Lets list all of our containers
```
zcw$ docker container ls
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
b2096a61e946 nginx "nginx -g 'daemon ..." About a minute ago Up About a minute 0.0.0.0:80->80/tcp suspicious_jones
```
-
##Let’s stop our containers
```
zcw$ docker container stop b2
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
b2096a61e946 nginx "nginx -g 'daemon ..." About a minute ago Up About a minute 0.0.0.0:80->80/tcp suspicious_jones
```
To stop the container you only have to provide enough of the container id for it to be unique.
 
-
##Let’s list all of our containers
```
zcw$ docker container ls
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
```
Since no containers are currently running, none will be listed

-
##Let’s list all of our containers
```
zcw$ docker container ls -a
CONTAINER ID b2096a61e946 1d383263da25
IMAGE nginx nginx
COMMAND
"nginx -g 'daemon ..." "nginx -g 'daemon ..."
CREATED
5 minutes ago 22 minutes ago
STATUS PORTS Exited (0) 2 minutes ago Exited (0) 5 minutes ago
NAMES suspicious_jones
unruffled_mccarthy
```

Adding the -a flag will list all the containers , even the ones currently not running
-
##  Let’s start a new DETACHED container
```
zcw$ docker container run --publish 80:80 --detach --name ZCW nginx 83831fe7c024246e2ffb4f6caa0fdf7d7f35d5c632133b4d09908466d44e25cc
zcw$ docker container ls
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES 83831fe7c024 nginx "nginx -g 'daemon ..." 33 seconds ago Up 34 seconds 0.0.0.0:80->80/tcp ZCW
```
-
## Let’s look at our logs

(Before doing the next step, you will need to refresh a few times on http://localhost:80 to generate logs) 

```
zcw$ docker container logs ZCW
172.17.0.1 - - [13/Dec/2017:22:28:33 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36" "-"
172.17.0.1 - - [13/Dec/2017:22:29:21 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36" "-"
172.17.0.1 - - [13/Dec/2017:22:29:23 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36" "-"
172.17.0.1 - - [13/Dec/2017:22:29:25 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36" "-"
```

-
## Let’s look at our processes
```
zcw$ docker container top ZCW
PID 15287 15315
USER TIME root 0:00 101 0:00
COMMAND
nginx: master process nginx -g daemon off;
nginx: worker process
```
-
##  Let’s clean up
```
zcw$ docker container ls -a
CONTAINER ID 83831fe7c024 b2096a61e946 1d383263da25
IMAGE nginx
nginx nginx
COMMAND
"nginx -g 'daemon ..."
"nginx -g 'daemon ..." "nginx -g 'daemon ..."
CREATED
6 minutes ago
14 minutes ago 31 minutes ago
STATUS
Up 6 minutes
Exited (0) 11 minutes ago Exited (0) 14 minutes ago
PORTS 0.0.0.0:80->80/tcp
NAMES
ZCW suspicious_jones
unruffled_mccarthy
NAMES
ZCW suspicious_jones
unruffled_mccarthy
zcw$ docker container rm 838 b20 1d3
CONTAINER ID 83831fe7c024 b2096a61e946 1d383263da25
IMAGE nginx
nginx nginx
COMMAND
"nginx -g 'daemon ..."
"nginx -g 'daemon ..." "nginx -g 'daemon ..."
CREATED
6 minutes ago
14 minutes ago 31 minutes ago
STATUS
Up 6 minutes
Exited (0) 11 minutes ago Exited (0) 14 minutes ago
PORTS 0.0.0.0:80->80/tcp
```
(Some docker commands can be chained together to do multiple actions)

-
##  Let’s clean up : We have an error
```
zcw$ docker container rm 838 b20 1d3
Error response from daemon: You cannot remove a running container 83831fe7c024246e2ffb4f6caa0fdf7d7f35d5c632133b4d09908466d44e25cc.
```
You can not remove a running container , you must either stop the container , or use the -f flag to force the removal.

```
zcw$ docker container rm 838 -f
```
-
####  What happens in ‘docker container run’
- Looks for the image locally in image cache , to see if it's there<br>
- Then looks in remote image repository ( defaults to Docker Hub )<br>
- Downloads the latest version (Nginx:latest by default)<br>
- Creates new container based on that image and prepares to start<br>
- Gives it a virtual IP on a private network inside docker engine<br>
- Opens up port 80 on host and forwards to port 80 in container<br>
- Start container by using the CMD in the image Dockerfile

-
##  Example of Changing the Defaults

```
docker container run --publish 8080:80 --name ZCW -d nginx:1.11 nginx -T
8080:80 (change host listening port) Nginx:1.11 (change version of image) Nginx -T (change CMD run on start)
```
-
##  Containers aren’t Mini-VM’s
- They are just processes<br>
- Limited to what resources they can access<br>
- Exit when process stops

-
##  Let’s start up a mongo database
```
zcw$ docker container run --name mongo -d mongo
Unable to find image 'mongo:latest' locally latest: Pulling from library/mongo c4bb02b17bb4: Pull complete 3f58e3bb3be4: Pull complete a229fb575a6e: Pull complete 633f2d5861aa: Pull complete 7f55dbb7ff1d: Pull complete
[...]
```

-
##  Let’s list out all our running containers
```
zcw$ docker ps
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES 103411b8f3f4 mongo "docker-entrypoint..." 8 minutes ago Up 8 minutes 27017/tcp mongo```

-
##  This is a process on the host
```
zcw$ docker top mongo
USER PID %CPU %MEM VSZ RSS TT STAT STARTED TIME COMMAND 931 0.0 0.1 5117464 23524 ?? S 10:56AM 0:37.56
```

- This is not a VM, this is actually a process on the local system.

-
##  Let’s list the processes in our containers:

```
zcw$ docker top mongo
PID USER TIME COMMAND
2405 999 0:01 mongod --bind_ip_all
```
-
##  Lab: Manage Multiple Containers (Part 1)
- docs.docker.com and `--help` are your friends<br>
- Run a nginx,<br>
- Run it in `--detach` ( or `-d`), name them with --name<br>
- Nginx should listen on 80:80

-
##  Lab: Manage Multiple Containers (Part 2)
- docs.docker.com and `--help` are your friends
- Run a http(apache) server
- Run it `--detach` ( or `-d`), name them with `--name`
- http on 8080:80

-
##  Lab: Manage Multiple Containers (Part 3)
- docs.docker.com and `--help` are your friends<br>
- Run mysql<br>
- Run it `--detach` ( or `-d`), name them with `--name`<br>
- Mysql on 3306:3306<br>
- When running mysql, use the `--env` option (or `-e`) to pass in<br>`MYSQL_RANDOM_ROOT_PASSWORD=yes`<br>
- Use docker container logs on mysql to find the random password it created on startup

-
##  Lab: Manage Multiple Containers (Part 4)
- docs.docker.com and `--help` are your friends
- Clean it all up with `docker container stop` and `docker container rm`
(both can accept multiple names of ID’s)
- Use `docker container ls` to ensure everything is correct before and after
clean up

