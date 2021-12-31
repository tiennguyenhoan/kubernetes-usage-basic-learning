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
>   Please let me know if any part of this guideline have incorrect, all of contributions are really appreciated to make this guideline better


<!-- vim-markdown-toc GFM -->

* [Requirement](#requirement)
* [Docker - Basic knowleged before we start](#docker---basic-knowleged-before-we-start)
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

## Docker - Basic knowleged before we start

If you want to learn of use Kubernetes, you must know how to work with Docker first. It's because Kubernetes will host your service/application as an Docker containers from Docker image.

By that, we will need to understand how to Dockerize an application and understand to work/troubleshoot a Docker container first 

### What is Docker

> " Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

Basically, Docker is an open source platform that we can package (or we can say "Image" in docker) our application with its whole environment need, this can be a few libraries or any filesystem of an installed operating system. 

To run your application with this image, we just need to deploy it to docker environment in the Serve, and it will be executed as Container
Or we can transfer image(s) to a storage to store it for the future usage or backuping (we call it's "Registry" in Docker)

There are three main Docker components we will forcus on this tutorials:

**Images** :
- Basically, a Docker image has everything needed to run a containerized application, including code, config files, environment variables, libraries and runtimes. 
  To make an Docker image, we must have a Dockerfile which including a base image and a set of commands to copy application source code to the base image or installing needed libraries

**Containers** :
- We can understand the docker container is virtualized runtime environment of a Docker image, which means we start running our application under docker environment.
  We can have multiple containers running on one hosting server and it's completely isolated from both host and all other conainers. 
  However, we also can setup container's network to container or between containers to containers or expose ports to open the connection between the host


**Registry** :
- A Docker Registry is a place that stores your Docker images and facilitates easy sharing of those images between different users and computers.
  When you build your image, you can either run it on the computer you’ve built it on, or you can push (upload) the image to a registry and then pull (download) it on another computer and run it there.
  Certain registries are public, allowing anyone to pull images from it, while others are private, only accessible to certain people or machines.

![Docker-flow](./readme/docker-flow.png)

### Learning Resource

I have create an python application to desmonstrate this guideline and I call it "[dummy-api](./dummy-api)".
If you want to run it directly in your local to know how it's working, please check [dummy-api Readme]('./dummy-api/README.md')

In the project, I have created a [Dockerfile][./dummy-api/Dockerfile] which contains steps to build an docker image

Then we in the next step I will show how can we dockerize this application to prepare deploy it to Kubernetes env.
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

## Kubernetes




### What is Kubernetes


### Hardware level concept

### Software level concept


## Software level practical

#### Ex-1: Simple deployment sample

#### Ex-2: containers comunication between deployment

#### Ex-3: Deployment with multiple Containers in a pod

#### Ex-4:

