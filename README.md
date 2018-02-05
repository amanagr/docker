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

	`docker rm 'docker ps -a -q'`_Delete all containers_

- **List docker images available on host**

	`docker images <OPTIONAL:PULLED REPO>`

- **Download images by tag names**

	`sudo docker run --name ubuntu_container -i -t ubuntu:xenial /bin/bash`_"-t <\REPOSITORY>:<\TAG>"_

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
