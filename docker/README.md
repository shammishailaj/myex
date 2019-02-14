[Home](../) | [Alpine Linux](../alpine-linux)
# Installing and running Tuleap via Docker

- Create a docker Volume

		docker volume create --name tuleap-data

		docker run -ti -e VIRTUAL_HOST=tuleap.example.com -p 8001:80 -p 8002:443 -p 8000:22 -p 8003:3306 -v tuleap-data:/data enalean/tuleap-aio

- Run Daemonized ( -d )

		docker run -d -p 8001:80 -p 8002:443 -p 8000:22 -p 8003:3306 -v tuleap-data:/data enalean/tuleap-aio

- List docker images

		docker image ls

		root@alm:~# docker image ls
		REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
		pm-tuleap            v1                  eb227c7410f1        6 hours ago         947 MB
		enalean/tuleap-aio   latest              75a7929f366c        8 days ago          830 MB
		hello-world          latest              48b5124b2768        3 weeks ago         1.84 kB


- List running containers

		docker ps

		CONTAINER ID        IMAGE               COMMAND              CREATED             STATUS              PORTS                                                                                       NAMES
		0443890cb26f        pm-tuleap:v1        "/root/app/run.sh"   3 days ago          Up 2 days           0.0.0.0:8000->22/tcp, 0.0.0.0:8001->80/tcp, 0.0.0.0:8002->443/tcp, 0.0.0.0:8003->3306/tcp   quizzical_booth

- Commit current docker image

		docker commit <container-id> <repository-name>:<tag>


- Stop a docker container

		docker stop -t <time-in-seconds> <container-name/container-id>

- Run a container from committed image

		docker run -d -p 8001:80 -p 8002:443 -p 8000:22 -p 8003:3306 -v tuleap-data:/data pm-tuleap:v1

- Detached mode

		docker run -ti -d true -e VIRTUAL_HOST=&lt;hostname&gt; -p 8001:80 -p 8002:443 -p 8000:22 -v tuleap-data:/data enalean/tuleap-aio

		docker run -ti true -e VIRTUAL_HOST=&lt;hostname&gt; -p 8001:80 -p 8002:443 -p 8000:22 -v tuleap-data:/data enalean/tuleap-aio



- Running openproject-ce via Docker

		docker run -it -p 8080:80 -e SECRET_KEY_BASE=secret openproject/community:6

		login: admin, password: admin

	Normal usage:

		docker run -d -p 8080:80 -e SECRET_KEY_BASE=secret openproject/community:6


		mkdir -p /var/lib/openproject/{pgdata,logs,static}


		docker run -d -p 8080:80 --name openproject -e SECRET_KEY_BASE=secret \
		-v /var/lib/openproject/pgdata:/var/lib/postgresql/9.4/main \
		-v /var/lib/openproject/logs:/var/log/supervisor \
		-v /var/lib/openproject/static:/var/db/openproject \
		openproject/community:6


	Since we named the container, you can now stop it by running:

		docker stop openproject

	And start it again by running:

		docker start openproject


	If you want to destroy the container, run the following command

		docker stop openproject && docker rm openproject

	Just another sample command for running openproject

		docker run -d --name openproject -e SECRET_KEY_BASE=secret -v /var/lib/openproject/pgdata:/var/lib/postgresql/9.4/main -v /var/lib/openproject/logs:/var/log/supervisor -v /var/lib/openproject/static:/var/db/openproject -e EMAIL_DELIVERY_METHOD=smtp -e SMTP_ADDRESS=smtp.sendgrid.net -e SMTP_PORT=587 -e SMTP_DOMAIN=&lt;hostname&gt; -e SMTP_AUTHENTICATION=login -e SMTP_ENABLE_STARTTLS_AUTO=true -e SMTP_USER_NAME="apikey" -e SMTP_PASSWORD="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" -p 8000:22 -p 8001:80 -p 587:587 openproject/community:6


- Running PHP Redmin - a redis administration panel in PHP via Docker

	[Cf. https://github.com/sasanrose/phpredmin](https://github.com/sasanrose/phpredmin)

		docker run -p 8080:80 -d --name phpredmin -e "PHPREDMIN_DATABASE_REDIS_0_HOST=192.168.1.6" -e "PHPREDMIN_AUTH_USERNAME=root" sasanrose/phpredmin

		docker run -p 8080:80 -d --name phpredmin -e "PHPREDMIN_DATABASE_REDIS_0_HOST=192.168.1.6" -e "PHPREDMIN_AUTH_USERNAME=root" sasanrose/phpredmin

		docker run -p 8100:22 -p 8101:80 -d --name phpredmin -e "PHPREDMIN_DATABASE_REDIS_0_HOST=cache1.example.com" -e "PHPREDMIN_DATABASE_REDIS_1_HOST=cache2.example.com" -e "PHPREDMIN_AUTH_USERNAME=root" sasanrose/phpredmin



- Running MySQL v5.7.17 via Docker

		docker run --name &lt;name-of-container&gt; -e MYSQL_ROOT_PASSWORD=<mysql-root-password> -p 8000:22 -p 8003:3306 -d mysql:5.7.17


- Running MSSQL Linux via Docker

	Pull the Docker image from Docker Hub.

		docker pull microsoft/mssql-server-linux:latest

	To run the Docker image, you can use the following commands:

		docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -d microsoft/mssql-server-linux

	To persist the data generated from your Docker container, you must map volume to the host machine. To do that, use the **run** command with the **-v <host directory>:/var/opt/mssql** flag. This will allow the data to be restored between container executions.


		docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d microsoft/mssql-server-linux


	_NOTE:_ The _ACCEPT_EULA_ and _SA_PASSWORD_ environment variables are required to run the image.
	
	_IMPORTANT:_ Volume mapping for Docker-machine on Mac with the SQL Server on Linux image is not supported at this time.


- Docker RUN vs CMD vs ENTRYPOINT

		debconf: unable to initialize frontend: Dialog
		debconf: (TERM is not set, so the dialog frontend is not usable.)
		debconf: falling back to frontend: Readline
		debconf: unable to initialize frontend: Readline
		debconf: (This frontend requires a controlling tty.)
		debconf: falling back to frontend: Teletype
		dpkg-preconfigure: unable to re-open stdin: 

	Cf. [http://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/](http://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/)



- TO LOGIN to canister.io

		docker images

		REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
		eventql             0.4.0               3cfb53e56b0c        2 days ago          1.37 GB
		ubuntu              16.04               0ef2e08ed3fa        13 days ago         130 MB
		ubuntu              14.04               7c09e61e9035        13 days ago         188 MB

		docker push --help

		Usage:	docker push [OPTIONS] NAME[:TAG]

		Push an image or a repository to a registry

		Options:

		--disable-content-trust   Skip image verification (default true)
		--help                    Print usage

- Log of pushing a docker image to canister.io

		docker push cloud.canister.io:5000/shammishailaj/eventqleventql:0.4.0
		
		docker tag evenql cloud.canister.io:5000/shammishailaj/eventql:0.4.0		
		Error response from daemon: no such id: evenql


		docker tag eventql cloud.canister.io:5000/shammishailaj/eventql:0.4.0
		Error response from daemon: no such id: eventql
		
		docker tag eventql:0.4.0 cloud.canister.io:5000/shammishailaj/eventql:0.4.0
		
		docker push cloud.canister.io:5000/shammishailaj/eventql:0.4.0

		The push refers to a repository [cloud.canister.io:5000/shammishailaj/eventql]
		9076f4977d5f: Preparing 
		1fea87a50b15: Preparing 
		0dec235624f2: Preparing 
		287cd734e2b7: Preparing 
		cee26d796e38: Preparing 
		20d927306b88: Waiting 
		a054dc08c511: Waiting 
		72d3cda2d13a: Waiting 
		30d746277548: Waiting 
		85bcb3a517a3: Waiting 
		a062897e8a4f: Waiting 
		664ea11bd30a: Waiting 
		bd00cdbae641: Waiting 
		af43131c4039: Waiting 
		9bd4c7af882a: Waiting 
		04ab82f865cf: Waiting 
		c29b5eadf94a: Waiting 
		authorization server did not include a token in the response


		docker push cloud.canister.io:5000/shammishailaj/eventql
		
		The push refers to a repository [cloud.canister.io:5000/shammishailaj/eventql]
		9076f4977d5f: Preparing 
		1fea87a50b15: Preparing 
		0dec235624f2: Preparing 
		287cd734e2b7: Preparing 
		cee26d796e38: Preparing 
		20d927306b88: Waiting 
		a054dc08c511: Waiting 
		72d3cda2d13a: Waiting 
		30d746277548: Waiting 
		85bcb3a517a3: Waiting 
		a062897e8a4f: Waiting 
		664ea11bd30a: Waiting 
		bd00cdbae641: Waiting 
		af43131c4039: Waiting 
		9bd4c7af882a: Waiting 
		04ab82f865cf: Waiting 
		c29b5eadf94a: Waiting 
		authorization server did not include a token in the response

	Login to docker hub

		docker login
		Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
		Username: 


		docker login --help

		Usage:	docker login [OPTIONS] [SERVER]

		Log in to a Docker registry

		Options:
		      --help              Print usage
		  -p, --password string   Password
		  -u, --username string   Username


		docker login cloud.canister.io:5000
		
		Username: shammishailaj
		Password: 
		Login Succeeded

		docker push cloud.canister.io:5000/shammishailaj/eventql
		
		The push refers to a repository [cloud.canister.io:5000/shammishailaj/eventql]
		9076f4977d5f: Pushed 
		1fea87a50b15: Pushed 
		0dec235624f2: Pushed 
		287cd734e2b7: Pushed 
		cee26d796e38: Pushed 
		20d927306b88: Pushed 
		a054dc08c511: Pushed 
		72d3cda2d13a: Pushed 
		30d746277548: Pushed 
		85bcb3a517a3: Pushed 
		a062897e8a4f: Pushed 
		664ea11bd30a: Pushed 
		bd00cdbae641: Pushed 
		af43131c4039: Pushed 
		9bd4c7af882a: Pushed 
		04ab82f865cf: Pushed 
		c29b5eadf94a: Pushed 
		0.4.0: digest: sha256:0e342bc7c268f4614c8a5fc056becb5d208f17b073dcf5eb6c03a820723a7269 size: 3865

		docker push cloud.canister.io:5000/shammishailaj/eventql:0.4.0

		The push refers to a repository [cloud.canister.io:5000/shammishailaj/eventql]
		9076f4977d5f: Layer already exists 
		1fea87a50b15: Layer already exists 
		0dec235624f2: Layer already exists 
		287cd734e2b7: Layer already exists 
		cee26d796e38: Layer already exists 
		20d927306b88: Layer already exists 
		a054dc08c511: Layer already exists 
		72d3cda2d13a: Layer already exists 
		30d746277548: Layer already exists 
		85bcb3a517a3: Layer already exists 
		a062897e8a4f: Layer already exists 
		664ea11bd30a: Layer already exists 
		bd00cdbae641: Layer already exists 
		af43131c4039: Layer already exists 
		9bd4c7af882a: Layer already exists 
		04ab82f865cf: Layer already exists 
		c29b5eadf94a: Layer already exists 
		0.4.0: digest: sha256:0e342bc7c268f4614c8a5fc056becb5d208f17b073dcf5eb6c03a820723a7269 size: 3865


- An open source alternative to Docker Hub supporting ARM architectures.

	[https://github.com/cloudfleet/marina](https://github.com/cloudfleet/marina)

- Keeping docker containers running even after crashes and system reboots

		docker run -p 8100:22 -p 8101:80 -d --restart unless-stopped --name phpredmin -e "PHPREDMIN_DATABASE_REDIS_0_HOST=cache1.example.com" -e "PHPREDMIN_DATABASE_REDIS_1_HOST=cache2.example.com" -e "PHPREDMIN_AUTH_USERNAME=root" 0f65cd69f147


- Running a MySQL conatiner 

		docker run --name some-mysql -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag


	Cf. [Docker auto-restart](https://docs.docker.com/engine/reference/run/#restart-policies---restart)

		docker run -d --restart unless-stopped --name db.example.com -v /root/docker/etc/mysql/conf.d:/etc/mysql/conf.d -v /root/docker/mysqlData:/var/lib/mysql -e TZ=Asia/Kolkata -e MYSQL_ROOT_PASSWORD=<root-user-password> -p 8000:22 -p 8003:3306 mysql:5.7.17


- Using Docker and Docker Compose for Local Development and Small Deployments

	Cf. [https://www.codementor.io/jquacinella/docker-and-docker-compose-for-local-development-and-small-deployments-ph4p434gb](https://www.codementor.io/jquacinella/docker-and-docker-compose-for-local-development-and-small-deployments-ph4p434gb)


- Running a MySQL docker container with customized configuration with configuration files residing on the host machine

		docker run -d --restart unless-stopped --name db.example.com -v /root/docker/etc/mysql/conf.d:/etc/mysql/conf.d -v /root/docker/mysqlData:/var/lib/mysql -e TZ=Asia/Kolkata -e MYSQL_ROOT_PASSWORD=<MySQL-root-user-password> -p 8000:22 -p 8003:3306 mysql:5.7.17

		docker run -d --restart unless-stopped --name db.example.com -v /root/docker/etc/mysql/conf.d:/etc/mysql/conf.d -v /root/docker/mysqlData:/var/lib/mysql -e TZ=Asia/Kolkata -e MYSQL_ROOT_PASSWORD=<MySQL-root-user-password> -p 8000:22 -p 3306:3306 mysql:5.7.17


- Installation of Let's Chat via Docker

	Run a MongoDB Container

		docker run -d --name letschat mongo:latest

		docker run  --name letschat-server --link letschat:mongo -p 8001:8080 -d sdelements/lets-chat

- Installation of Rocket Chat via Docker

		docker run --name rocketchat --link letschat:db -p 8002:3000 -e "ROOT_URL=http://rocketchat.example.com" -d rocket.chat:latest


- Installation of Mattermost via Docker

		docker run --name mattermost-preview -d -p 8003:8065 mattermost/mattermost-preview


- Installation of shammishailaj/gearmand

		docker run -d --restart unless-stopped --name gearmand -p 4730:4730 -p 8300:8080 shammishailaj/gearmand


- Deploying your own Docker Registry Server

	Cf. [https://docs.docker.com/registry/deploying/](https://docs.docker.com/registry/deploying/)


- Running SonarQube via Docker

		docker run -d --restart unless-stopped --name sonarqube \
		-p 9100:9000 -p 9192:9092 \
		-e SONARQUBE_JDBC_USERNAME=<your-db-user> \
		-e SONARQUBE_JDBC_PASSWORD=<your-db-user-password> \
		-e SONARQUBE_JDBC_URL=jdbc:mysql://db.example.com/sonar\?useUnicode=true\&characterEncoding=utf8 \
		sonarqube


- Running MySQL via Docker with auto-restart across crashes and reboots

		docker run -d --restart unless-stopped --name db.example.com -v /root/docker/etc/mysql/conf.d:/etc/mysql/conf.d -v /root/docker/mysqlData:/var/lib/mysql -e TZ=Asia/Kolkata -e MYSQL_ROOT_PASSWORD=<MySQL-root-password> -p 8000:22 -p 8003:3306 mysql:5.7.17


- Manage or Run Docker as a non-root user
	
	To create the docker group and add your user:

	1. Create the docker group

		sudo groupadd docker

	2. Add your user to the docker group

		sudo usermod -aG docker $USER

	3. Log out and log back in so that your group membership is re-evaluated

	4. Verify that you can run docker commands without sudo

		docker run hello-world

	Cf. [https://docs.docker.com/install/linux/linux-postinstall/](https://docs.docker.com/install/linux/linux-postinstall/)


- Installing Docker CE on Ubuntu

	1. Uninstall old versions. Older versions of Docker were called docker, docker.io , or docker-engine. If these are installed, uninstall them:

		sudo apt-get remove docker docker-engine docker.io containerd runc

- Install using the repository

	A. SET UP THE REPOSITORY

	1. Update the apt package index:

		sudo apt-get update

	2. Install packages to allow apt to use a repository over HTTPS:
		
		sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common

	3. Add Docker’s official GPG key:

		curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
	
	Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.
		
		sudo apt-key fingerprint 0EBFCD88

	4. Use the following command to set up the stable repository:
		
		sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

	B. INSTALL DOCKER CE

	1. Update the apt package index
		
		sudo apt-get update && sudo apt-get install docker-ce

	Cf. [https://docs.docker.com/install/linux/docker-ce/ubuntu/](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
