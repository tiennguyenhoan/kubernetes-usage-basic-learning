# Kubernetes usage basic learning

## Inspiration:

I saw many people who want to learn with Kubernetes but they don't have much time for reading a bunch of definitions to understand how it works.
Or just because many tutorials focus too much on explaining all the definitions of Kubernetes and examples aren't focused on what they exactly want, deploy a simple application from End to End

Therefore, I made this small project with the aiming to support those, who want to learn k8s but don't have much time, to have the basic mindset of how Kubernetes works and how to work with it.
In this guideline, we will cover basic knowledge of K8s with step by step examples and I hope this can help other beginners

- We won't go depth into Docker in this guideline, but some basic knowledge in Docker and dockerize will be required. You can go to [Docker get started](https://docs.docker.com/get-started/) for a quick review about docker before we can move on
- I will try to avoid as much definition as I can in this tutorials, and I will focus more on practical examples with commands and explainations at that state, so if you need to know the definition and stuff, please the link **Kubernetes documentation** above

If you want to learn fully about Kubernetes please read [Kubernetes documentation](https://kubernetes.io/docs/tutorials/)

> **Importants**:
>
>   When you go through this guideline, you may see many explanations that may not correct 100% with the definitions on the official Documentation, since the target of this project is helping people have the general/basic knowledge on K8s.
>   Therefore, I made this as simple as I can, and some of them are the simple conclusion about my experience in Kubernetes.
>
>   Please let me know if any part of this guideline have incorrect, all of contributions are appreciated to make this guideline better


<!-- vim-markdown-toc GFM -->

* [Requirement](#requirement)
* [Docker - Basic knowledge before we start](#docker---basic-knowledge-before-we-start)
  * [What is Docker](#what-is-docker)
  * [Learning Resource](#learning-resource)
* [Kubernetes](#kubernetes)
  * [What is Kubernetes](#what-is-kubernetes)
  * [Hardware level concept](#hardware-level-concept)
  * [Software level concept](#software-level-concept)
* [Software level practical](#software-level-practical)
    * [Ex-1: Simple deployment sample](#ex-1-simple-deployment-sample)
    * [Ex-2: containers comunication between deployment](#ex-2-containers-comunication-between-deployment)
    * [Ex-3: Deployment with multiple Containers in a pod](#ex-3-deployment-with-multiple-containers-in-a-pod)
    * [Ex-4:](#ex-4)

<!-- vim-markdown-toc -->

## Requirement

To follow this guideline, you must have:
- Docker installed in your machine, check [Docker install](https://docs.docker.com/engine/install/) if you don't have it

- Docker Desktop with Kubernetes activated or Minikube
  - Docker Desktop: Read [Docker Deskop - Kubernetes](https://docs.docker.com/desktop/kubernetes/) for how to enable K8s
  - Minikube: If you don't have kubernetes in your Docker Desktop, then follow [Minikube](https://github.com/kubernetes/minikube) to install it.

- kubectl command which will use most of the time to work with Kubernetes, check [Kubernetes install tools](https://kubernetes.io/docs/tasks/tools/)

## Docker - Basic knowledge before we start

If you want to learn to use Kubernetes, you must know how to work with Docker first. It's because Kubernetes will host your service/application as a Docker container from Docker image.

By that, we will need to understand how to Dockerize an application and understand to work/troubleshoot a Docker container first 

**why do we use docker and why do we need it**:

Years ago, most software applications were big and they included many services and jobs, including serving external connections as Hub, processing logical requests as a backend, or even serving frontend pages. those software applications development used to call with name `monolithic` application.

These monolithic applications are used to run a single process with all components on a handful server, this development system can work but it's hard to scale up and have many potential harmful to the server itself or the company business.

For instance, with many components running inside one server, this is challenging for scaling up when the server cannot handle more works.
or when we have an issue with the running server, it will take time to troubleshoot and identify which components cause the issue and how can we keep the service online asap
besides, we may face many problems related to libraries installed in the server when having conflict between multiple applications deployed in the same server.
and there are still have many issues with the security or automating processes.

with the development of software development, these big monolithic legacy applications are slowly being broken down into smaller, independently running components called `microservices`. 

the microservices can understand as an architectural pattern that we have many independent applications, which play certain roles or serve a single service. 
these services communicate with each other via a well-defined interface using lightweight apis and because they are independently run, each service can be updated, deployed, and scaled to meet the demand for specific functions of an application.

![monolithic-microservice](./readme/monolithic-microservice.png)

The microservice replacing the old architectural (monolithic application) and resolve many problems in architecture, but with the developing of the product or expanding of business, 
the number of components requires to deploy increases time by time, and it becomes difficult to configure, manage and keep the whole system running smoothly.
however, microservices also bring other problems such as making it hard to debug and trace execution calls, because they span multiple processes and machines.
or have more complexity on deploying components to server for not conflict on required libraries and get the high resource utilization.

![mono-micro-complex](./readme/mono-micro-complex.png)

with those difficult, docker come up with a high advantage in packaging source code and deploying it in an isolated environment which is not related much to the hosting machine.

### What is Docker

> " Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

Basically, Docker is an open-source platform that we can package (or we can say "Image" in docker) our application with its whole environment need, this can be a few libraries or any filesystem of an installed operating system. 

To run your application with this image, we just need to deploy it to docker environment in the Serve, and it will be executed as Container
Or we can transfer image(s) to storage to store it for future usage or backing up (we call it's "Registry" in Docker)

With this mechanism, we can deploy to multiple servers at the same time without doing any re-configuration on hosting servers.
Or we can only restart the docker container to make it back to the original state when happing any urgent issue and impact directly to business

There are three main Docker components:

**Images** :
- Basically, a Docker image has everything needed to run a containerized application, including code, config files, environment variables, libraries, and runtimes. 
  To make a Docker image, we must have a Dockerfile that including a base image and a set of commands to copy application source code to the base image or install needed libraries

**Containers** :
- We can understand the docker container is virtualized runtime environment of a Docker image, which means we start running our application under the docker environment.
  We can have multiple containers running on one hosting server and it's completely isolated from both host and all other containers. 
  However, we also can set up container's network to a container or between containers to containers or expose ports to open the connection between the host

**Registry** :
- A Docker Registry is a place that stores your Docker images and facilitates easy sharing of those images between different users and computers.
  When you build your image, you can either run it on the computer you’ve built it on, or you can push (upload) the image to a registry and then pull (download) it on another computer and run it there.
  Certain registries are public, allowing anyone to pull images from it, while others are private, only accessible to certain people or machines.

![Docker-flow](./readme/docker-flow.png)

### Learning Resource

I have created a python application to demonstrate this guideline and I call it "[dummy-api](./dummy-api)".
If you want to run it directly in your local to know how it's working, please check [dummy-api Readme]('./dummy-api/README.md')

In the project, I have created a [Dockerfile][./dummy-api/Dockerfile] which contains steps to build a docker image

Then we in the next step I will show how can we dockerize this application to prepare to deploy it to Kubernetes env.
Make sure your Docker server is up and running. Now we will create a Docker image in our local machine.

1. Open terminal and go to folder [dummy-api](./dummy-api)

1. Execute command below:
    ```bash
      docker build -t dummy-api:1 .
    ```

1. When it's finished, we can check created docker image with:
    ```bash
      docker images
    ```

    You will get response like this
    ``` 
      REPOSITORY             TAG             IMAGE ID       CREATED         SIZE
      dummy-api              1               f1060ccb9b7e   6 seconds ago   58.9MB
    ```

- In case  you want to remove the image too
```bash
  docker rmi dummy-api:1
```

**Test running this docker image (Optional)**

Execute command below, it will run your built image as a docker container and we will able to access it

```bash
  docker run -d --name dummy-container -p 105:105 dummy-api:1

  # Args explain
  #  -d      : The container will run in background
  #  --name  : Set the name of creating container, will random if we dont set
  #  -p      : Forward internal port of container (left) to external port of our machine (right). Cannot access to container if we dont set

```

Then you can test it by open your browser and navigate to http://localhost:105

- To view all of your docker container
    ```bash 
      docker ps -a
    ```

- To get the realtime log of the container
    ```bash
      docker logs -f dummy-container
    ```

- To run shell inside a container
    ```bash
      docker exec -it dummy-container sh   # Some container have bash shell so we can use it instead of "sh"
    ```

-  To stop the container and all of it changes content still there
    ```bash
      docker stop dummy-container
    ```

- We can start the stopped container again
    ```bash
      docker start dummy-container
    ```

- And remove it, all its contents are removed and it can’t be started again.
    ```bash
      docker rm dummy-container
    ```

> **Note**: The container loaded from your docker image, so if we stop or remove the container it will not affect to the built image.

## Kubernetes

### What is Kubernetes


### Hardware level concept

### Software level concept


## Software level practical

#### Ex-1: Simple deployment sample

#### Ex-2: containers comunication between deployment

#### Ex-3: Deployment with multiple Containers in a pod

#### Ex-4:

