## Learning Docker
- [Learning Docker](#learning-docker)
- [Basic Commands](#basic-commands)
- [Containers](#containers)
- [Share an image](#share-an-image)
- [Services](#services)
- [Swarms](#swarms)
- [Stacks](#stacks)
- [Resources used](#resources-used)

## Basic Commands
- List Docker CLI commands
    - `docker`
    - `docker container --help`

- Display Docker version and info
    - `docker --version`
    - `docker version`
    - `docker info`

- Execute Docker image
    - `docker run hello-world`

- List Docker images
    - `docker image ls`

- List Docker containers (running, all, all in quiet mode)
    - `docker container ls`
    - `docker container ls --all`
    - `docker container ls -aq`

## Containers
- Containers are the bottom of the hierarchy of an app. Above that is a service, 
- defines how containers behave in production. Top level is a stack, defines 
- interactions of all services:
    - STACK
    - SERVICES
    - CONTAINER
- Portable images (`Dockerfile`) ensure that your app, dependencies and runtime 
all travel together.
- `Dockerfile` defines what goes on in the environment inside your container. 
- Create `Dockerfile`, run `docker build -t <NAME> .` in directory. 
- `docker image ls` should show new Repository.
- `docker run -p 4000:80 friendlyhello`
- Nav to localhost:4000 to view app.
- Can also use curl to view raw html content: `curl http://localhost:4000`
- Run the app in the background, in detached mode:
    - `docker run -d -p 4000:80 friendlyhello`
    - You get the long container ID
- `docker container stop <CONTAINER_ID>`

## Share an image
- upload a built image and run it somewhere else
- Push to registries when you want to dep to prod
- registy is a collection of repo's, a repo is a collection of images. Like a 
github repo, but the code is already built. An account on a registry can build
many repo's. 
1 - Login (`docker login`) to Docker public registry on local machine
2 - Tag the Image
    - notation for associtating a local image with a repo: `username/repository:tag`
    - tag is optional, but recommended to give Docker images a version i.e., `get-started:part2`
    - `docker tag image username/repository:tag`
    - `docker tag friendlyhello mlars84/docker-python:v1`
3 - Publish the image
    - `docker push username/repository:tag`
    - `docker push mlars84/docker-python:v1`
4 - Pull and run from remote repo:
    - `docker run -p 4000:80 username/repository:tag`

## Services
- Really just containers in production. Runs one image, but it codifies the way
the image runs--what ports it should use, how many replicas of the container should
run so the service has the capacity it needs, etc.
- `docker-compose.yml`
    - pulls the uploaded image
    - run 5 instances of that image as a service called web, limiting each one to 
    use  at most 10% of the CPU (across all cores), and 50MB RAM.
    - immediately restart containers if one fails
    - Map port 4000 on the host to web's port 80
    - instruct web containers to share port 80 via a load-balanced network called   
    `webnet`
    - define the `webnet` network with the default settings
- `docker swarm init`
- `docker stack deploy -c docker-compose.yml <APP_NAME>`
- `docker service ls`
- A single container running in a service is called a `task`.
- `Tasks` have unique id's that numerically increment up to the number of replicas.
- `docker service ps dockerPython_web` to view all replica `tasks`
- OR: `docker container ls -q` to list task id's 
- Scale the app by changing the replicas value and re-running `docker stack deploy`
- Take app/stack down: `docker stack rm <APP_NAME>`
- Take down swarm: `docker swarm leave --force`

## Swarms
- group of machines running docker and joined in a "cluster".
- continue to run normal docker commands, but now they are executed on a 
cluster by a swarm manager.
- Can be physical or virtual. After joining swarms, they are called nodes.
- `docker swarm init`
- Create VMs:
    - `docker-machine create --driver virtualbox <VM_NAME>` X 2
- The first machine acts as the manager, executes commands and auths workers to
join the swarm. The second is the worker.
- `docker-machine ssh` to send commands to VMs.
- `docker-machine ssh myvm1 "docker swarm init --advertise-addr <myvm1 ip:2377>"`
- Make sure to use `2377` for `docker swarm init` AND `docker swarm join`
- Use command that is returned to join a worker to the manager.
- `docker node ls` on manager to view nodes
-  `docker-machine env <VM_MANAGER>` to config docker-machine shell env variables.
-  `docker stack deploy -c docker-compose.yml <APP_NAME>` to deploy on swarm manager.
-  `docker stack ps <APP_NAME>` to view swarm distribution.
-  `<VM_IP:port>` in browser to access app: `http://192.168.99.100:8080/`
-  `docker stack rm <APP_NAME>` to teardown.
-  `docker-machine start <machine-name>` to restart a machine
-  `eval $(docker-machine env -u)` to unset the env variables.
-  `docker-machine ssh myvm2 "docker swarm leave"` to remove worker.
-  `docker-machine ssh myvm1 "docker swarm leave --force"` to remove manager.

## Stacks
- A group of interrelated services that share dependencies, and can be orchestrated
and scaled together.
- single stack is capable of defining and coordinating the functionality of an
entire application (for larger/more complex, can have multiple stacks).
- Add a service in the docker-compose.yml: `dockersamples/visualizer:stable`
- Make sure shell is configured to myvm1 (env config)
- Run `docker stack deploy -c docker-compose.yml dockerPython` to re-deploy and add
new service in.
- open in browser: `http://192.168.99.100:8080/` OR check with `docker stack ps dockerPython`
- 

## Resources used
- [Docker docs](https://docs.docker.com/get-started/)
- [Docker classroom](https://training.play-with-docker.com/)
- [Docker for Node](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)
