## Learning Docker
- [Learning Docker](#learning-docker)
- [Commands](#commands)
- [Containers](#containers)
- [Share an image](#share-an-image)
- [Resources used](#resources-used)

## Commands
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
- Containers are the bottom of the hierarchy of an app. Above that is a service, defines how containers behave in production. Top level is a stack, defines interactions of all services:
    - STACK
    - SERVICES
    - CONTAINER
- Portable images (`Dockerfile`) ensure that your app, dependencies and runtime all travel together.
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

## Resources used
- [Docker docs](https://docs.docker.com/get-started/)
- [Docker classroom](https://training.play-with-docker.com/)
- [Docker for Node](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)
