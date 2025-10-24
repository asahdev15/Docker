```shell

docker version
docker system info

docker pull IMAGE:[TAG]

docker run IMAGE:[TAG]
docker run -d IMAGE:[TAG]
docker attach ID

docker run redis
docker run redis:4.0
docker run jenkins/jenkins
docker run -it kodekloud/simple-prompt-docker
docker run -p 80:5000 kodekloud/webapp
docker run -p 8000:5000 kodekloud/webapp
docker run -p 8001:5000 kodekloud/webapp
docker run -v /opt/datadir:/var/lib/mysql mysql
docker run ubuntu
docker run ubuntu cat /etc/*release*
docker run ubuntu:17.10 cat /etc/*release*
docker run ubuntu sleep 15
docker run -d ubuntu sleep 1500
docker run timer
docker run -d timer

docker pull jenkins/jenkins
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins
docker run -p 8080:8080 -p 50000:50000 -v /root/my-jenkins-data:/var/jenkins_home -u root jenkins/jenkins
docker inspect <jenkins-container-id>
# http://172.17.0.2:8080

docker logs blissful_hopper

docker inspect ID/NAME

docker ps
docker ps -a

docker stop ID/NAME

docker rm ID/NAME

docker rmi ID/NAME

docker exec ID/NAME CMD

docker images


docker run -it ubuntu bash
apt-get update
apt-get install -y python python-setuptools python-dev build-essential python-pip python-mysqldb
pip install flask
cat > /opt/app.py
vi /opt/app.py
FLASK_APP=app.py flask run --host=0.0.0.0
history
# create docker file

docker build . -t mmumshad/my-simple-webapp
docker history IMAGE
docker login
docker push mmumshad/my-simple-webapp
docker pull mmumshad/my-simple-webapp
docker images
docker run -p 5000:5000 my-simple-webapp
# http://<HOST_IP>:5000

export APP_COLOR=blue; python app-2.py
docker run -e APP_COLOR=blue simple-webapp-color
docker run -e APP_COLOR=green simple-webapp-color
docker run -e APP_COLOR=pink simple-webapp-color
docker inspect blissful_hopper


docker run ubuntu sleep 5
docker build -t ubuntu-sleeper .
docker run ubuntu-sleeper
docker run ubuntu-sleeper sleep 10

docker run --entrypoint sleep2.0 ubuntu-sleeper 10

# docker compose

docker-compose up



docker network create \
  --driver bridge \
  --subnet 182.18.0.0/16 \
  custom-isolated-network

docker network ls


docker login private-registry.io
docker pull private-registry.io/apps/internal-app

docker run -d -p 5000:5000 --name registry registry:2

docker image tag my-image localhost:5000/my-image
docker push localhost:5000/my-image
docker pull localhost:5000/my-image
docker pull 192.168.56.100:5000/my-image



export DOCKER_HOST="tcp://192.168.1.10:2375"
docker ps
dockerd \
  --host=unix:///var/run/docker.sock \
  --host=tcp://192.168.1.10:2375

dockerd \
  --host=unix:///var/run/docker.sock \
  --host=tcp://192.168.1.10:2376 \
  --tls=true \
  --tlscert=/var/docker/server.pem \
  --tlskey=/var/docker/serverkey.pem

docker --tlsverify \
  --tlscacert=ca.pem \
  --tlscert=client.pem \
  --tlskey=client-key.pem \
  -H=tcp://192.168.1.10:2376 info


docker [object] [command] [options] [arguments]

docker image ls
docker container run -it ubuntu
docker image build .
docker container attach <container>
docker container kill <container>
docker container create httpd
ls /var/lib/docker/


# Command Description
docker container ls	    # Show running containers
docker container ls -a	# Show all containers (running & stopped)
docker container ls -l	# Show the most recently created container
docker container ls -q	# Display only container IDs (running)
docker container ls -aq	# Display all container IDs

docker container start 36a391532e10

docker container run ubuntu
docker container run -d httpd
docker container attach 11cbd7fe7e65
docker container run -it ubuntu
docker container run -itd --name webapp ubuntu
docker container rename intelligent_almeida webapp2


#Action	Command	Description
docker run -it ubuntu	# Launches Ubuntu with an interactive TTY shell
#exit	Terminates the shell and stops the container
#Detach without stopping	Press Ctrl-p Ctrl-q	Leaves the container running in the background 
docker exec CONTAINER COMMAND	#Runs a command inside a running container
docker exec -it CONTAINER /bin/bash	#Starts a new shell session for troubleshooting
docker attach CONTAINER	#Reconnects your terminal to the container’s main shell

docker container ls	                # List active containers
docker container inspect <id>	    # Show detailed JSON metadata
docker container stats	            # Stream live resource usage
docker container top <name>         # List processes inside the container
docker container logs <id>	        # Retrieve container logs
docker container logs -f <id>	    # Follow logs in real time
docker system events --since 60m	# Stream Docker engine events from last hour



#Signal	Action	Description
#SIGSTOP	Pause	Suspends the process, cannot be caught.
#SIGCONT	Resume	Continues a paused process.
#SIGTERM	Graceful shutdown	Allows process cleanup before exit.
#SIGKILL	Forceful kill	Immediately terminates; cannot be trapped.

# Pause Apache httpd
kill -SIGSTOP 11663
# Resume it
kill -SIGCONT $(pgrep httpd)
# Graceful shutdown
kill -SIGTERM $(pgrep httpd)
# Forceful kill
kill -SIGKILL $(pgrep httpd)
# shorthand
kill -9 $(pgrep httpd)

#Action	Linux Signal	Docker Command
#Pause	SIGSTOP/SIGCONT	docker pause / docker unpause
#Stop	SIGTERM → SIGKILL	docker stop
#Kill	SIGKILL	docker kill --signal=SIGKILL
#Remove	—	docker rm


docker run --name web httpd
docker pause web
docker unpause web
docker stop web
# Send SIGKILL by name
docker kill --signal=SIGKILL web
# Or by number
docker kill --signal=9 web

docker stop web
docker rm web

# Stop all running containers
docker stop $(docker ps -q)
# Remove all containers (running & exited)
docker rm $(docker ps -aq)

docker container prune
docker image prune

docker run --rm ubuntu expr 4 + 5

docker container exec -it 65 cat /etc/lsb-release

docker container run -it --name webapp --hostname webapp ubuntu

docker run --name test-no --restart=no ubuntu expr 3 + 5
docker run --name test-fail --restart=on-failure:3 ubuntu expr three + 5
docker run --name test-always --restart=always ubuntu sleep 5
docker run --name test-unless --restart=unless-stopped ubuntu sleep 5

/etc/docker/daemon.json
{
  "debug": true,
  "hosts": ["tcp://192.168.1.10:2376"],
  "tls": true,
  "tlscert": "/var/docker/server.pem",
  "tlskey": "/var/docker/serverkey.pem",
  "live-restore": true
}
sudo systemctl restart docker
# or
sudo systemctl reload docker

# With live restore NOT enabled:
docker run --name web httpd
sudo systemctl stop docker
# Containers stop when daemon stops.
sudo systemctl start docker
docker ps
# web is not running

# With live restore enabled:
docker run --name web httpd
sudo systemctl stop docker
# web stays running.
sudo systemctl start docker
docker ps
# web is still running

docker container cp [HOST_PATH] [CONTAINER_NAME]:[CONTAINER_PATH]
docker container cp /tmp/web.conf webapp:/etc/web.conf
docker container cp ./config/ webapp:/etc/config

docker container cp /path/on/host container_name:/path/in/container	# Copy files or directories into container
docker container cp container_name:/path/in/container /path/on/host	# Copy files or directories to host

docker inspect simple-webapp --format '{{json .NetworkSettings.Ports}}'
iptables -t nat -S DOCKER



pid=$(pgrep dockerd)
sudo kill -SIGHUP $pid


docker run -d \
  --name myapp \
  --log-driver=fluentd \
  --log-opt fluentd-address=localhost:24224 \
  nginx

docker container inspect -f '{{.HostConfig.LogConfig.Type}}' nginx

docker container inspect test-container --format='{{json .HostConfig.LogConfig}}'


# Verify the current logging driver
docker system info | grep -i "logging driver"
# Output: Logging Driver: json-file
docker info --format '{{.LoggingDriver}}'


# HTTPD images with at least 10 stars
docker search httpd --filter stars=10
# Only official HTTPD images
docker search httpd --filter is-official=true
# Combine multiple filters
docker search httpd --filter stars=10 --filter is-official=true
docker search --limit 2 httpd
docker search --filter stars=10 httpd
docker search --filter stars=10 --filter is-official=true httpd
docker image pull httpd
docker image ls

## Google Container Registry (GCR)
#image: gcr.io/your-project/httpd
#
#
## Private registry
#image: registry.example.com/your-namespace/httpd

docker login docker.io
docker login gcr.io

docker image tag httpd:alpine httpd:customv1
docker image tag httpd:customv1 gcr.io/organization/httpd:customv1
docker push gcr.io/organization/httpd:customv1

docker system df

docker image rm httpd:customv1
docker image rm httpd:alpine

docker image prune -a
docker image history ubuntu

docker image inspect ubuntu
docker image inspect httpd -f '{{.Os}}'
docker image inspect httpd -f '{{.Architecture}}'
docker image inspect httpd -f '{{.Architecture}} {{.Os}}'
docker image inspect httpd -f '{{range $p := .ContainerConfig.ExposedPorts}}{{printf "%s " $p}}{{end}}'

docker pull alpine:latest
docker image save alpine:latest -o alpine.tar
docker image load -i alpine.tar

docker export <container-name> > testcontainer.tar
docker image import testcontainer.tar newimage:latest
docker image ls

docker tag httpd:kodekloudv1 <username>/httpd:kodekloudv1
docker push <username>/httpd:kodekloudv1

docker image rm httpd:alpine
docker image rm <username>/httpd:kodekloudv1

docker build -t your-dockerhub-username/flask-app:latest .

docker history your-dockerhub-username/flask-app:latest





# Client
docker --tlscacert=/path/to/cacert.pem \
       --tlscert=/path/to/client.pem \
       --tlskey=/path/to/client-key.pem \
       --tlsverify \
       -H tcp://192.168.1.10:2376 ps

# Server
/etc/docker/daemon.json
{
  "hosts": ["tcp://192.168.1.10:2376"],
  "tls": true,
  "tlsverify": true,
  "tlscacert": "/var/docker/cacert.pem",
  "tlscert": "/var/docker/server.pem",
  "tlskey": "/var/docker/server-key.pem"
}


docker run --cpu-shares=512 --name webapp4 webapp

docker run --cpuset-cpus="0-1" --name webapp1 webapp
docker run --cpuset-cpus="0-1" --name webapp2 webapp
docker run --cpuset-cpus="2"   --name webapp3 webapp
docker run --cpuset-cpus="2"   --name webapp4 webapp

docker run --cpus=2.5 --name webapp4 webapp


docker run --memory=512m my-webapp
docker run --memory=512m --memory-swap=768m my-webapp
docker container run -itd --name=testthree --memory=200m --memory-swap=-1 ubuntu # unlimited swap
docker container run -itd --name=testfour  --memory=200m --memory-reservation=100m --memory-swap=-1 ubuntu
  
docker container run -itd --name=testone ubuntu
docker container inspect testone | grep -i mem

docker container run -itd --name=testtwo --memory=200m ubuntu
docker container inspect testtwo | grep -i mem

#Memory Flags Summary
#Flag	Description	Example
#--memory	Hard limit for container memory	--memory=200m
#--memory-reservation	Soft (guaranteed) memory reservation	--memory-reservation=100m
#--memory-swap	Total memory + swap limit (-1 = unlimited)	--memory-swap=-1
#--memory-swappiness	Tendency to swap (0–100)	--memory-swappiness=10



#Network Name	Driver	Scope	Description
#bridge	        bridge	local	Default network for standalone containers
#host	        host	local	Containers share the host’s network namespace
#none	        null	local	Containers have no network interfaces

docker network ls

docker run --network=<network_name> ubuntu
docker run --network=host ubuntu
docker run --network=none ubuntu

docker network create \
  --driver bridge \
  --subnet 182.18.0.0/16 \
  custom-isolated-network

docker container run -itd --name=testtwo --net=custom-isolated-network ubuntu

docker inspect <container_id_or_name> | jq '.[0].NetworkSettings'

docker network inspect bridge

# Connect a container to a custom network
docker network connect custom-net my-container
# Disconnect a container from a network
docker network disconnect custom-net my-container

# Remove a specific network
docker network rm custom-net
# Remove all unused networks
docker network prune

docker plugin install vieux/sshfs
docker volume create \
  --driver vieux/sshfs \
  --opt sshcmd=user@host:/remote/path \
  ssh_volume

docker volume inspect data_volume

docker volume rm data_volume
docker volume prune
docker container inspect my-container

docker container run \
  --mount type=volume,source=data_vol1,target=/var/www/html,index.html,readonly \
  httpd
  
docker run -itd --name test -v testvol:/yogesh centos:7
docker exec -it test df -h | grep /yogesh
docker volume inspect testvol

docker stop test
docker rm test
docker volume ls

docker volume create testvol
docker run -itd --name test \
    --mount source=testvol,destination=/yogesh \
    centos:7

docker exec -it test ls /yogesh

docker volume rm testvol
docker volume prune

docker run -itd --name volume_rw \
    --mount source=data_vol1,destination=/yogesh \
    centos:7
docker inspect volume_rw --format '{{json .Mounts}}' [{"Destination":"/yogesh","Mode":"rw", ...}]

docker run -itd --name volume_ro \
    --mount source=data_vol1,destination=/yogesh,readonly=true \
    centos:7

docker inspect volume_ro --format '{{json .Mounts}}'
[{"Destination":"/yogesh","Mode":"ro", ...}]






```


