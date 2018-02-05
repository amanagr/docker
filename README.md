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
