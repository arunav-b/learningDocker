# Learning Docker
> Docker concepts

<br/>

## Docker Image

A Docker image is a file used to execute code in a Docker container. Docker images act as a set of instructions to build a Docker container, like a template. Docker images also act as the starting point when using Docker. An image is comparable to a snapshot in virtual machine (VM) environments.

## Container

It is the running version of image. Multiple containers can be running from the same image.

## Docker Networks

There are three types of docker networks -

  1. Bridge network(default)
  2. `none` network 
  3. `host` network

<br/> 

### **Bridge Network:**

- When a docker container is created without specifying any particular network, its assigned under default network which is bridge network. This is private internal network on docker host in range of **172.17...** series.

  ```
    docker run ubuntu
  ```

- Containers within bridge network can talk to each other using internal ip address. If these containers were to be accessed by outside bridge network then that can be done by mapping its port to any of the docker host port(port for machine on which docker is running) by using **-p option**. Another way is to associate a host port with the container.

### **`none` network:** 

- When a container is created with `none`, its not associated with any network. Neither it can be accessed by anyone, nor can it access any other container. They run in an isolated network.

  ```
    docker run --network=none ubuntu
  ```

### **`host` network:**

- Container created with `host` option is hosted on a host network not docker internal network. Like a container can be hosted on 5000 port on host network. This way without port mapping container can be accessed by outside networks.

  ```
    docker run --network=host ubuntu
  ```
  
> **Note** : 
>
> - By default docker will create one bridge network but we can create another one using below command :
>
>   ```
>     docker network create 
>     --driver bridge 
>     --subnet 182.18.0.0/16
>     custom-isolated-network
>   ```
>
>  - List networks: `docker network ls` 
>  - Inspect a container's network:`docker inspect containerid`

<br/>

## Embedded DNS server: 

- Docker has **DNS server** running on ip **127.0.0.11**. 
- Every container has a container name which can be used to resolve a container by embedded DNS. So if a webserver wants to connect to a mysql container, it can do by providing the container name and in turn it uses DNS to resolve the ip address using DNS server.

<br/>

## Docker Registry

- Docker registry is a cloud space where docker images are maintained and stored.  
- If no path is specified the default registry is **docker.io**. For example if you are pulling a nginx image then actully path is: docker.io/nginx/nginx : here 1st nginx is user account, 2nd is image name. But this is public registry. 
- Likewise docker's own , there is **google's public registry** as well **gcr.io** where kubernetes images are available. 
- One can create its own private registry as well and to access that, you need to first login using `docker login` command. When you create an account in AWS or GCP (Google cloud), they by default provide a registry to maintain docker images. 

<br/>

## Deploy private registry:

- Docker registry docker.io is itself a image called registry. If we want to run an instance of the private registry in local then execute below command:
  ```
    docker run -d --name registry -p 5000:5000 registry:2 
  ``` 
  This will run and deploy private registry to port `5000` on docker host.

- Tagging your own image to docker registry by using Docker image tag command and attach the image to private registry url:
  ```
    docker image tag my-image localhost:5000/my-image 
  ```

- Pushing the image to local private registry:
  ```
    docker push localhost:5000/my-image
  ```

- Pulling image from same host or different host:
  ```
    docker pull localhost:5000/my-image 
    //or 
    docker pull 192.168.56.100:5000/my-image 
  ```

<br/>

## Common Docker Commands

```dockerfile

# run a particular image on docker
  docker run <image-name>	

# By default will run latest version unless a specific version is specified
  docker run <image-name>:<version>	

# list of images  
  docker images	

# remove an image. If a container is running on that image then it will throw an error
  docker rmi <image-name>	

# pull a image from docker-hub repository  
  docker pull <image-name>	

# list of containers (processes)
  docker ps
  docker ps -a 	# list all containers, all running and previously exited containers

# to stop a particular container
  docker stop <container-id> / <name> 

# remove a stopped/exited/running container. If you do a ps -a it should not appear again
  docker rm <container-id> / <name>

# remove the container that matches the chars. 
# Multiple containers can be removed by providing unique characters with spaces in same command  
  docker rm <first unique characters of container-id>

# container starts and then sleeps for 1000 secs
  docker run <image-name> sleep 1000	

# execute a particular command inside a container
  docker exec <container-id> / <name> "command"	

# details about the particular container
  docker inspect <container-id> 

# remove all stopped containers
  docker container prune

# list all networks on docker host  
  docker network ls	

```


