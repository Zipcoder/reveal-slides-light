#Unit: 1 Containers
###CLI Monitoring
-
##What’s going on in Containers:
- `docker container top` - process list in one container
- `docker container inspect` - details of one container config
- `docker container stats` - performance stats for all containers

-
##Let’s start a nginx container
- `docker container run -d --name nginx nginx`
- `docker container top nginx`

-
##Let’s start a mysql container
- `docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql`
- `docker container top mysql`
 
-
##Docker container inspect
- `docker container inspect mysql`
- This will return a JSON array of all the data involved in starting up the container

-
##Docker container stats
- `docker container stats`
- This will give you a running play on the processes running in containers on
your machine
- This is not what you would use in production
- It’s great for when you are working on your local machine
- Getting a Shell inside Containers
- `docker container run -it`
	- starts new containers interactively
- `docker container exec -it`
	- run additional command in existing container
	- Different Linux distros in containers
	
-

##Container flags -i , -t or -it
- `-t pseudo-tty`
	- Simulates a real terminal, like what SSH does
- `-i, --interactive`
	- Keep STDIN open even if not attached
- This allows us to keep the session open even when there are no commands
-
##Container additional commands
- usage : `docker container run [OPTIONS] IMAGE [COMMAND] [ARGS…]`
- `docker container run -it --name proxy nginx bash`
	- Here by placing bash after the image name ‘nginx’ we are overriding the default action of this container
	- This will log you in as the root user of the container
	- Try using `ls`
	- Using the `exit` command to leave the container and end.
	- Containers only run as log as the command on startup

-
##Let’s pull down a full distribution
- `docker container run -it --name ubuntu ubuntu`
	- Once logged in run `apt-get update`
	- This is a stripped down version of Ubuntu , and would not contain all the things that comes with a full distribution by default.
	- You can even install things normally: `apt-get install -y curl`
	- Once you exit the container again… it will stop the container
	- If we restarted the container CURL would be installed

-
###Docker container start
- `docker container start --help`
- `-a, --attach :: Attach STDOUT/STDERR` and forward signals
- `docker container start -ai ubuntu`

-
###Docker container exec
- Let's say we want to look inside of an container already running a process
- `docker container exec -it mysql bash`
	- Will place you in a container inside of sql
	- In this shell we can jump directly into the mysql command line
	- Try `ps aux`
	- When you finally exit the process will continue
	- `run docker ps`

-
###Linux Alpine
- A small security focused distribution of linux
- Lets pull down a copy of alpine and take a look
	- `docker pull alpine`
	- `docker image ls`
- Let’s try :
	- `docker container run -it alpine bash`
	- The above will not work because bash is not part of the distribution
- Lets try:
	- `docker container run -alpine sh`
	- The above will work because sh is include in this image, although it has less features
available than bash.

-

#Unit: 1 Containers
###Networking Concepts
-

##Docker Networks: Concepts
- Review of `docker container run -p`
- For local dev/testing, networks usually “just work”
	- Dockers motto Batteries are included but removable
- Quick port check with `docker container port <container>`
- Learn concepts of Docker Networking

-
##Docker Network Defaults
- Each container connected to a private virtual network “bridge”
	- This is the default docker system network
- Each virtual network routes through NAT firewall on host IP
	- The docker daemon configures the host ip address on its default interface so that
containers can get out to the internet
- All containers on a virtual network can talk to each other with `-p`
- Best practice is to create a new virtual network for each app
	- Network “zcw_web_app” for mysql and php/apache containers
	- Network “zcw_api” for mongo and nodejs containers 

-
##Docker Networks Cont.
- “Batteries Included, But Removable”
	- Default work well in many cases, but easy to swap out parts to customize it
- Things you can change
	- Make new virtual networks
	- Attach containers to more than one virtual network (or none)
	- Skip virtual networks and use host IP (--net=host)
■ You lose contanerization benefits but it’s unavoidable
	- Use different Docker network drivers to gain new abilities

-
###-p (--publish)
- Publishing ports is always in HOST:CONTAINER format
- RUN: `docker container run -p 80:80 --name webhost -d nginx`
- RUN: `docker container port webhost`
	- `80/tcp -> 0.0.0.0:80`

-
###Inspect --format
- `docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost`
	- Will return the ip address of the container ‘172.17.0.3’
- Run: `ifconfig en0`
	- Will return the ip address of local machine ‘10.0.0.92’
	- Notice that the two machines are not on the same network
	- There is an edge firewall that blocks calls in and out
	- Docker has a default bridge that maps to our local ethernet interface
	- Using the `-p` on docker will allow external traffic into the docker virtual network
	- Containers on the same network have access to each other, unless you use `-p` there will be no incoming calls. 

-

##Docker Networks: Concepts recap
- Review of `docker container run -p`
- For local/dev testing, networks usually “just work”
- Quick port check with `docker container port <container>`
- Learn concepts of Docker networking

-
#Unit: 1 Containers 
###CLI Management

-
##Docker Networks : CLI Management
- Show networks: `docker network ls`
- Inspect a network: `docker network inspect`
- Create a network: `docker network create --driver`
- Attach a network to a container: `docker network connect`
- Detach a network from container: `docker network disconnect`

-
##Docker Networks
- Run : `docker network ls`
- Run : `docker network inspect bridge`
	- Will list the containers that are attached to this network
- Three default networks:
	- Host network - a special network that skips virtual networks but sacrifices security of a
container
	- Bridge network - default network for docker host
	- None network - it has not attachment
	
-
##Docker Networks
- Run : `docker network create zcw_app_network`
- Run : `docker network ls`
	- We can now see our new network with a driver of bridge
		- Bridge is the default network driver
- Run : `docker network create --help`
- Run : `docker container run -d --name new_nginx --network zcw_app_network nginx`
- Run : `docker network inspect zcw_app_network`

-

##Docker Networks :
- Docker network connect
	- Dynamically creates a NIC (networking interface card) in a container on an existing virtual
network
- Run : `docker network ls`
- Run : `docker network inspect bridge`

-

##Lab : CLI Testing
- Use different Linux distro containers to check curl cli tool versions
- Use two different terminal windows to start bash in both centos:7 and
ubuntu:14.04, using `-it`
- Use the docker container `--rm` options so you can save cleanup
- Ensure curl is installed and on latest version for that distro
	- ubuntu : `apt-get update` && `apt-get install curl`
	- Centos : `yum update curl`
- check curl `--version`

-

#Unit 2: Container Images

-

##Unit 2 - Container Images : Agenda
- Define what an Image is
- Docker HUB
- Image Cache
- Tagging
- Docker File
	- Building
	- Extending

-
##Unit 2 - Container Images : Overview
- All about images, the building blocks of containers
- What's in an image (and what isn't)
- Using Docker Hub registry
- Managing our local image cache
- Building our own images

-
##What’s an Image?
- Application binaries and dependencies
- Metadata about the image data and how to run the image
- Not a complete OS. No Kernel… the host provides the Kernal
- Small as one file (your application binary) like in golang
- Or something like Ubuntu

-
##Docker Hub
- Basics of Docker Hub (hub.docker.com)
	- Github for docker images
	- Create an account on hub.docker.com
- Find official and other good public images
	- Official images are the only ones without forward slashes on them
- Download images and basics of image tags

-
##Union file system
- Unionfs is a filesystem service for Linux, FreeBSD and NetBSD which
implements a union mount for other file systems.
- It allows files and directories of separate file systems, known as branches,
to be transparently overlaid, forming a single coherent file system.
- Contents of directories which have the same path within the merged
branches will be seen together in a single merged directory, within the new,
virtual filesystem.

-
##Docker Images and Layers
- Image layers
- Union file system
- History and inspect commands
- Copy on write
	- How containers run as expansions on top of images

-
##Creating and Storing Container Images
- Run : `docker image ls`
- Run : `docker history nginx:latest`
	- This is not a history of what’s in the container
	- This is a history of the image changes over time
	- Every Image starts off as a basic layer called a scratch file
- Run : `docker history mysql`
	- Look at the difference
- Some points
	- Every layer gets its own unique sha
	- Docker uses this to check to see what changes are between each version

-

##Docker inspect
- Run : `docker image inspect nginx`
	- This will show you all the default commands in the image
	- Remember most of these can be changed after the fact

-
##Container Images
- Images are made up of file system changes and metadata
- Each layer is uniquely identified and only stored once on a host
- This saves storage space on hos and transfer time on push/pull
- A container is just a single read/write layer on top of image
- docker image history and inspect command 

-
##Image Tagging and Pushing to docker hub
- Run : `docker image tag --help`
- Run : `docker image ls`
	- Images don’t have a name
		- They have an ID and a Tag
		- They exist inside of a repository and are listed by their Tag and Image
- Tags are a pointer to an specific image commit
	- Let’s navigate to docker hub and look at images and how they are tagged
- Run : `docker pull nginx:mainline`
- Run : `docker image ls`

-
##Image Tagging and Pushing to docker hub
- Run : `docker image tag --help`
- Run : `docker image ls`
	- Images don’t have a name
		-  They have an ID and a Tag
		-  They exist inside of a repository and are listed by their Tag and Image
- Tags are a pointer to an specific image commit
	- Let’s navigate to docker hub and look at images and how they are tagged
- Run : `docker pull nginx:mainline`
- Run : `docker image ls`

-
##Tagging images
- docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
- Run : `docker image tag nginx <your-username>/nginx`
- Try to run : `docker image push <your-username>/nginx`
	- Since you are not logged in you will not be able to
- Create an account on docker hub
- Run : `docker login`
- Run : `cat .docker/config.json`
- Run : `docker image push <your-username>/nginx`

-
##Retagging Images
- Run : `docker image tag <your-username>/nginx <your-username>/nginx:testing`
- Run : `docker image ls`
	- Pay attention to the image id
- Run : `docker image push <your-username>/nginx:testing`

-
##Dockerfile
- Navigate to the dockerfile-sample-1 in the git repo
- Open up the file ‘Dockerfile’ and review it
- Run : `docker image build -t localimage .`
- Run : `docker image ls`
- Let’s make a update and the expose port 8080 to the Dockerfile
- Rebuild the file
	- Notice the Using cache tag line in the build process
	- Its critically important the ordering of your flies
		- Keep the things you change the least at the top

-
##Copying files to build
- Run: `docker container run -p 80:80 --rm nginx`
- Open up browser and webhost
- Run : `docker image build -t nginx-with-html .`
- Navigate to browser

-
##Push to docker hub
- Let’s retag and send to docker hub
- Run :`docker image tag nginx-with-html:latest <your-username>/nginx-with-html:latest`
- Run : `docker push <your-username>/nginx-with-html:latest`

-
##Lab : Build your Own Image
- Take existing Node.js app and Dockerize it
- Make Dockerfile.
	- Build it
	- Test it
	- Push it
	- Run it
- Expect this to be iterative. You seldom get it right the first time
- Details in dockerfile-assignment-1/Dockerfile
- Use the Alpine version of the official ‘node’ 6x image
- Expected result is the web site at http://localhost
- Remove from local cache, run again from Hub

-
#Unit 2: Container Images
###Docker HUB
-

##Docker Hub
To access Docker HUB, browse to: hub.docker.com (ie, nginx official)
Official images, advantages, versions etc. 
-
##2:2
Commands:
- `docker search`
- `docker pull`
- `docker login`
- `docker push`
Download images: hub.docker.com (ie, nginx official)
-
##docker search
`docker search [OPTIONS] TERM`
-
##docker pull
`docker pull [OPTIONS] NAME[:TAG|@DIGEST]`
-
##docker login
`docker login [OPTIONS] [SERVER]`
-
##docker push
`docker push [OPTIONS] NAME[:TAG]`
-
##Image Cache
`docker image ls`
`docker pull nginx`
`docker pull nginx:1.11.9`
(when testing, specify the version)
`docker history nginx:latest`
`docker image inspect nginx`
-
#Unit 2: Container Images
###Tagging
-
#Tagging
-
##Tagging
`<repository>/<tag_name>` (by default is “latest”>

```
docker image tag --help
docker pull mysql/mysql-server
docker pull nginx:mainline
docker image tag nginx doliothesleuth/nginx
docker image image push doliothesleuth/nginx
``` 
(if denied, then:)
`docker login`
(then push again) (to add a verion to the tag, use `:<tag_version>`)
Dockerfile:
-
#Unit 2: Container Images
###Docker File
-
##Docker File
- A recipe for creating your image
File’s name is alway “Dockerfile”, contains the recipe for building the image.
`docker image build -t <image_name>` . (“.” if called from inside the
directory that contains the desired Dockerfile)
Tips: things that change the least at top of file, things that change the most
towards the bottom
-

##Docker File: Build
Building:

```
“FROM <repo>:<tag>”
“EXPOSE <portnumbers>”
“RUN apk add --update tini”
“WORKDIR <path the workdir>”
“COPY <host files or dir> <destination>”
“CMD []“
```
-
#Unit 3: Container Life Cycle
-
##Unit 3-Container Life Cycle : Agenda
- Container Lifetime
- Persistent Data
	- Volumes
	- Bind mounting

-
##Unit 3-Container Life Cycle : Overview
- Defining the problem of persistent data
- Key concepts with containers: immutable, ephemeral
- Learning and using Data Volumes
- Learning and using Bind Mounts

-
##3: Container Lifetime & Persistent Data
Containers are usually immutable and ephemeral, giving us an "immutable
infrastructure", where we only re-deploy containers, which never change.
This is the ideal scenario, however in order to accommodate databases or other
unique data, Docker gives us features to ensure "separation of concerns". This
collection of features are known as as "persistent data", handled in two ways:
Volumes and Bind Mounts.
Volumes make a special location outside of the container Union File System,
while Bind Mounts link a container path to a host path.

-
##The Twelve Factors
1. Codebase
2. Dependencies
3. Config
4. Backing services
5. Build, release, run
6. Processes
7. Port binding
8. Concurrency
9. Disposability
10. Dev/prod parity
11. Logs
12. Admin processes


-
##The Twelve Factors (continued)
**1. Codebase**<br>
	One codebase tracked in revision control, many deploys
	
**2. Dependencies**<br>
	Explicitly declare and isolate dependencies
	
**3. Config**<br>
	Store config in the environment
	
**4. Backing services**<br>
	Treat backing services as attached resources

-
##The Twelve Factors (continued)

**5. Build, release, run**<br>
	Strictly separate build and run stages
	
**6. Processes**<br>
	Execute the app as one or more stateless processes
	
**7. Port binding**<br>
	Export services via port binding
	
**8. Concurrency**<br>
	Scale out via the process model


-
##The Twelve Factors (continued)

**9. Disposability**<br>
	Maximize robustness with fast startup and graceful shutdown
	
**10. Dev/prod parity**<br>
	Keep development, staging, and production as similar as possible
	
**11. Logs**<br>
	Treat logs as event streams
	
**12. Admin processes**<br>
	Run admin/management tasks as one-off processes


-
#Unit 3: Container Life Cycle
###Persistent Data
-
##3: Persistent Data: Volumes
- This usage is achieved by using the `VOLUME` command in the Dockerfile
- Can also be implemented as an override using the `–v` option in the docker `run` command. (`docker run -v /path/in/container` )
- Bypasses Union File System and stores in alternative location on host
- Includes its own management commands under docker volume
- Connect to none, one, or multiple containers at once
- Not subject to commit, save, or export commands
- By default they only have a unique ID, but you can assign name, making it a
"named volume".
-
##Creating a Data Volume:
This method uses a container as a shared data volume and called data only
container.
```
zcw$ docker run --name datavolume -v /var_volume1 -e POSTGRES_USER=pgsql -e
POSTGRES_PASSWORD=mypass postgres
zcw$ docker run --name client1 --volumes-from datavolume postgres
zcw$ docker run --name client2 --volumes-from datavolume postgres
```
(creates two postgres containers that share the same data volume container)
####*Remember to use the same base image

-
##Creating a Data Volume:
- Maps a host file or directory to a container file or directory
- Essentially, it’s two locations pointing to the same file(s)
- Like Data Volumes, skips Union File System, and host files overwrite any in
container
- Can't be enacted by the Dockerfile, must be used with container run:
`... run -v /Users/<username>/stuff:/path/container` (mac/linux)
`... run -v //c/Users/<username>/stuff:/path/container` (windows)
-
##Mount a host directory as a data volume
By using the –v option in the run command, you can create a directory or map an
already existing one to a container.
Example:
`zcw$ docker run --name myname -v Shared:/SharedData -i -t ubuntu /bin/bash`

(This creates a container with a shared folder with the host `Shared` Folder and
will be mounted as `/SharedData` in the container, and if the folder didn’t exist in the host it will be automatically created.)

-

##Mount a host directory as a data volume:

The other option is giving a name to the container, opening its command shell with bash shell.
```
zcw$ docker inspect myname
…
"Mounts": [
 {
 "Source": Shared",
 "Destination": "/SharedData",
 "Mode": "",
 "RW": true
 }
 ]
…
```

-
##Mount a host directory as a data volume:
```
zcw$ ls
…
SharedData
…
```
If anything is written in the **SharedData** Volume, it will be written in the **Shared** volume
-

##Lab: Working with Named Volumes
- Task: Performing a PostGres Database Upgrade using Named Volumes, starting with version 9.6.1
(See docker hub if needed)
- Database upgrade with containers
- Create a Postgres container with name volume sql-data using version 9.6.1
- Use Docker hub to learn the VOLUME path and versions needed top run it
- Check the logs, and stop the container.
- Create a new postgres container with the same named volume using version 9.6.2
- Check the logs to validate that the container is using the newer version.<br>
**(*this only works with patch versions, most SQL databases require manual commands to upgrade
to major/minor versions--it’s a limitation of databases not containers)**

-
##Lab: Working with Bind Mounts
- Use a Jekyll "Static Site Generator" to start a local web server
- Don't have to be a web developer: this is an example of bridging the gap between local file access
and apps running in containers.
- Source code provided in **bindmount-lab**
- Edit it with your editor of choice
- The Container detect changes with the host files and updates the same on the web server
- Start container using: `docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve`
(The repo for Jekyll server in a docker container)
- Refresh browser to see changes
- Change the file in the **_posts** directory and refresh browser to see the changes.
- Look at logs to observe the operations

-
