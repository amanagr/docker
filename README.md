# Docker Info
#### [Why use docker?](https://dzone.com/articles/5-key-benefits-docker-ci)
1. Isolated Environment - Solves the issue "Works on my machine!"
2. Environment Standardization and Version Control
3. Continuous Deployment and Testing
4. Multi-Cloud Platforms
5. Security - Different Images running separately
6. *Cost savings*
7. Deployment using Micro-services

### Docker Setup
#### Server :
1. Login to [Digital Ocean](digitalocean.com)
2. Create a new droplet with *Docker with Ubuntu* selected.
3. Setup [SSH key](https://docs.docker.com/docker-cloud/cloud-swarm/ssh-key-setup/)
4. Login through your terminal using :
	`ssh root@<ip-of-droplet>`
5. Run:
	```
	#create an "deploy" user with Docker privileges
	useradd --user-group --create-home --shell /bin/bash deploy
	usermod -aG docker deploy
	```
6. [Copy root authorized_keys to "deploy" user](https://www.digitalocean.com/community/questions/error-permission-denied-publickey-when-i-try-to-ssh) :

	```
	mkdir /home/deploy/.ssh
	cp ~/.ssh/authorized_keys /home/deploy/.ssh/authorized_keys
	chmod 700 /home/deploy/.ssh
	chmod 644 /home/deploy/.ssh/authorized_keys
	chown -R deploy:deploy /home/myuser/.
	chown deploy /home/deploy/.ssh/authorized_keys

	#log out and log back in as deploy@<ip address>
	docker login
	```

### What is a Docker Image?

	A Docker image is made up of filesystems layered over each other. At the
	base is a boot filesystem, 'bootfs', which resembles the typical Linux/Unix boot
	filesystem. A Docker user will probably never interact with the boot filesystem.
	Indeed, when a container has booted, it is moved into memory, and the boot
	filesystem is unmounted to free up the RAM used by the initrd disk image.
	So far this looks pretty much like a typical Linux virtualization stack. Indeed,
	Docker next layers a root filesystem, 'rootfs' , on top of the boot filesystem. This
	rootfs can be one or more operating systems.

	In the Docker world, however, the root filesystem stays in read-only mode, and Docker
	takes advantage of a union mount to add more read-only filesystems onto the
	root filesystem. A union mount is a mount that allows several filesystems to be
	mounted at one time but appear to be one filesystem. The union mount overlays
	the filesystems on top of one another so that the resulting filesystem may contain
	files and subdirectories from any or all of the underlying filesystems. Docker calls
	each of these filesystems images.


## Docker CheatSheet:

- **Run an Interactive ubuntu shell**:

	`sudo docker run --name ubuntu_container -i -t ubuntu /bin/bash`
+ Use -d flag to detach the process to background

- **Start a docker image**

	`docker start ubuntu_container`

- **Stop a docker image**

	`docker stop ubuntu_container` _SIGTERM_

	`docker kill ubuntu_container` _SIGKILL_

- **Ineract with the container**

	`docker attach ubuntu_container`

- **To show container logs**

	`docker logs ubuntu_container`

- **To see the programs running within a container**

	`docker top ubuntu_container`

- **To see details about a container**

	`docker inspeact ubuntu_container`

	`sudo docker inspect --format='{{ .State.Running }}' daemon_dave`

- **Delete containers**

	`docker rm ubuntu_container`

	`docker rm 'docker ps -a -q'` _Delete all containers_

- **List docker images available on host**

	`docker images <OPTIONAL:PULLED REPO>`

- **Download images by tag names**

	`sudo docker run --name ubuntu_container -i -t ubuntu:xenial /bin/bash` _REPO:TAG_

- **PULL an image**

	`sudo docker pull fedora`

- **Searching for pulicly available docker images**

	`sudo docker search puppet`

##### Creating Docker Images

- **Using 'docker commit' to create images [_Not Recommended_]**

	```
	$ docker run --name uc -i -t ubuntu /bin/bash
	# Install whatever you want
	# exit
	$ docker commit -m="A new custom image" --author="Aman Agrawal" uc <DOCKERUSERNAME/NAMEOFTHEIMAGE>
	```

**Building images with a Dockerfile[_Recommended_]**

###### Why?

- Docker runs a container from the image.
- An instruction executes and makes a change to the container.
- Docker runs the equivalent of docker commit to commit a new layer.
- Docker then runs a new container from this new image.
- The next instruction in the file is executed, and the process repeats until all instructions have been executed.
- This is highly useful when debugging


Create Dockerfile with the necessary settings.

```
# Version 0.0.1
FROM ubuntu:trusty
MAINTAINER Aman Agrawal "f2016561@pilani.bits-pilani.ac.in"
ENV REFRESHED_AT 2018-02-03
RUN yum -y -q upgrade
RUN apt-get update
RUN apt-get install -y nginx
RUN echo 'Hi, I am in your container' \
        > /usr/share/nginx/html/index.htmnl
EXPOSE 80
```
Then do:

```
docker build -t "<USERNAME/NAMEOFIMAGE>:<TAG:EG:V1>" <FOLDER OF THE Dockerfile>
```
Additional Commands:

```
docker history <IMAGE>
docker build --no-cache -t "<USERNAME/NAMEOFIMAGE>:<TAG:EG:V1>" <FOLDER OF THE Dockerfile>
sudo docker run -d -p 80 --name static_web jamtur01/static_web \ ↩  nginx -g "daemon off;"


```


### Dockerfile Instructions



- RUN : Runs the command when the container is build.

- CMD : Runs the command after the container is build.
_Commands within array run 'as-is' otherwise docker prepends `/bin/sh -c` to it._
_We can override the CMD instruction on the docker run command line._

- ENTRYPOINT : The ENTRYPOINT instruction provides
a command that isn't as easily overridden. Instead, any arguments we specify
on the docker run command line will be passed as arguments to the command
specified in the ENTRYPOINT .*Any arguments we specify
on the docker run command line will be passed as arguments to the command
specified in the ENTRYPOINT .*

- WORKDIR : The WORKDIR instruction provides a way to set the working directory for the container and the ENTRYPOINT and/or CMD to be executed when a container is launched from the image. _Override the working directory at runtime with the -w flag._

- ENV : The ENV instruction is used to set environment variables during the image build process.

- USER : The USER instruction specifies a user that the image should be run as._The default user if you don't specify the USER instruction is root._

- VOLUME : The VOLUME instruction adds volumes to any container created from the image.

- ADD : The ADD instruction adds files and directories from our build environment into our image

- COPY : The COPY instruction is closely related to the ADD instruction. The key difference is that the COPY instruction is purely focused on copying local files from the build context and does not have any extraction or decompression capabilities.

- ONBUILD : The ONBUILD instruction adds triggers to images. A trigger is executed when the image is used as the basis of another image.
