---
layout: default
title:  "Docker/k8s tips"
date:   2024-12-21 19:00:00 +0800
tags: docker k8s
---
Docker is for "how to run one container".

K8s is for "how to manage/run lots of containers".

# Docker

Docker is for "how to run one container"

## Docker components

- Docker Engine
  - The Docker client
  - The Docker server
- Docker Images
- Docker Containers
- Registries

## Installation on macOS

Goto docker [website](https://www.docker.com/get-started/)

## Practices with the "Docker Desktop"

## Practices with the CLI

### About the images

```shell
# list Images
docker Images

# pull the latest image
docker pull ubuntu

# pull 'ubuntu' image with specific version 'jammy'
docker pull ubuntu:jammy

# remove an image
docker rmi ubuntu
```

### About the containers

```shell
# list the running containers
docker ps

# show all the containers
docker ps -a

```

Create and run containers.

```shell
# create and run a container (with random name) from an image
docker run ubuntu

# create and run a container with specific name `ubuntu-container` from image ubuntu
docker run --name ubuntu-container ubuntu

# create and run a container with interactive mode
# --name ubuntu : container name
# -i : interactive Keep STDIN open even if not attached
# -t : Allocate a pseudo-TTY
docker run --name ubuntu -i -t ubuntu /bin/bash

# run a welcome-to-docker container and open the port 8080,
# so can access it from browser
# open a browser, open url `http://localhost:8080`
docker run -d -p 8080:80 docker/welcome-to-docker
```

Stop running containers

```shell
docker kill ubuntu
```

Remove containers

```shell
# remove the container ""
docker rm ubuntu-container
# or use the container ID which can be seen from the `docker ps -a`
docker rm 740ac834e823
```

Start existing containers

```shell
# start an existing container
docker start ubuntu

# Attach local standard input, output, and error streams to a running container
docker attach ubuntu

# display the logs of a container
docker logs ubuntu

# display the running processes of a container
docker top ubuntu
```

### Composing the image

```shell
cd ~/ws/demos/docker
mkdir static_web
cd static_web
touch Dockerfile
```

Here’s the content of the Dockerfile

```Dockerfile
# Version: 0.0.4
FROM ubuntu:24.04
LABEL maintainer="peter.2.song@outlook.com"
RUN apt update && apt install -y nginx
#RUN echo 'Hi, I am in your container' > /usr/share/nginx/html/index.html
RUN echo 'Hi, I am in your container' > /var/www/html/index.html
EXPOSE 80
# start the nginx when using `docker run`
# use ENTRYPOINT to start nginx
ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]
# use CMD to start nginx
#CMD nginx -g "daemon off;"
```

```shell
# build an image from the Dockerfile above
docker build -t "petesong/static_web" .

# run a container from the image
# if there's ENTRYPOINT in the Dockerfile, can run a container like this
docker run -d -p 8081:80 --name static_web petesong/static_web

# 访问
curl localhost:8081
```

# Kubernetes(K8s)

K8s is for "how to manage/run lots of containers"

Core functions:
核心功能

- Container orchestration: Automatically schedule and manage the deployment of containers.
      容器编排，自动调度和管理容器的部署
- Automatic scaling: Automatically expand or shrink container instances according to the change of load.
    自动伸缩，根据负载变化自动扩展或收缩容器实例
- High availability: Automatically restart failed containers and support multi-node deployment.
    高可用性，自动重启失败的容器，支持多节点部署
- Load balancing and service discovery
    负载均衡和服务发现
- Storage orchestration
    存储编排
- Rolling updates and rollbacks
    滚动更新和回滚

Kubernetes 架构

- Kubernets API
- Kuberctl 管理 Kubernetes 集群的命令行
- Kubernetes Cluster(集群)
  - Control Plane(控制平面)
    - Deployment
      - ReplicaSet
        - app:A in Pod 1
        - app:A in Pod n
  - Node 1
    - kubelet 位于各个节点内的小应用，用于和控制平面通信
    - container runtime, e.g. Docker, ContainerD
    - Pod 1
      - containerzed app 1
      - containerzed app n
      - volume
    - ...
    - Pod n
  - ...
  - Node n

## Installation on a local computer

<https://kubernetes.io/docs/tasks/tools/>

## Practices with `minikube` on macOS

### Install `minikube`

Install `minikube` per [link](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Farm64%2Fstable%2Fhomebrew) , the `kubectl` would also be installed.

```shell
brew install minikube

kubectl version
minikube version
```

### Start `minikube` and the cluster

Start the cluster after have started the docker desktop

```shell
minikube start --driver=docker

kubectl cluster-info

kubectl cluster-info dump
```

### Minikube addons

```shell
# list addons
minikube addons list

# enable an addon
minikube addons enable metrics-server

# disable an addon
minikube addons disable metrics-server
```

### Interact with the clusters

Interact with the clusters:

- use `minikube` dashboard
- use `kubectl` command line, it's interacting through an API endpoint to communicate with our applications.
- use Kubernetes APIs

### Use `minikube dashboard`

```shell
minikube addons enable metrics-server

minikube dashboard
```

The dashboard should be opened in the browser automatically.

### Use `kubectl` CLI

Use `kubectl` command line to interact with the cluster

```shell
# create a deployment
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080

# scale up a deployment
kubectl get deployments,replicasets,pods
kubectl scale deployments/hello-node --replicas=3
kubectl get deployments,replicasets,pods

# scale down a deployment
kubectl get deployments,replicasets,pods
kubectl scale deployments/hello-node --replicas=1
kubectl get deployments,replicasets,pods


# upgrade the version of the app
# first get the container name, e.g. `agnhost`
kubectl get deployment hello-node -o wide
# update the version
kubectl set image deployments/hello-node agnhost=registry.k8s.io/e2e-test-images/agnhost:2.40
# check the pods, can see one pod is terminating
kubectl get pods
# wait for a while, check the deployment
# can see the new version of app
kubectl describe deployment hello-node


# delete deployment
kubectl get deployments
kubectl delete deployment hello-node
kubectl get deployments

```

Want to see the resource types

```shell
# list all the resource types
kubectl api-resources
```

```shell
# list the deployment
kubectl get deployments

# list the nodes
kubectl get nodes

# list the pods
kubectl get pods
# show detailed information
kubectl describe pods

# list the events to see the
kubectl get events

```

Want to work with services

```shell
# by default, the Pod is only accessible by its internal IP address within the Kubernets cluster.
# to make the hello-node container accessible from outside the Kubernets virtual network
# you have to expose the Pods as a Kubernets service
kubectl expose deployment hello-node --type=LoadBalancer --port=8080

# get services
kubectl get services

# use the service
# it opens up a browser window the serves your app and show the app's response
minikube service hello-node
# alternatively, we can use the `kubectl` to forward the port
# then open http://localhost:7080 in the browser
kubectl port-forward service/hello-node 7080:8080


# delete service
kubectl get services
kubectl delete service hello-node
kubectl get services

```

```shell
# get logs of the specific pod with the pod name showed above.
kubectl logs hello-node-66d457cb86-g6xcb

# execute command on the container
# list the environment variables
kubectl exec hello-node-66d457cb86-g6xcb -- env
# run bash in the container
kubectl exec -ti hello-node-66d457cb86-g6xcb -- bash

# check the resources
kubectl top pods
kubectl top nodes
```

```shell
# view the config
kubectl config view
```

### Use the `curl` to interact with APIs

```shell
# by defaut, the pods are running inside Kubernets are running on a private, isolate networks.
# we need to open an second terminal window to run the proxy that will forward communications
# into the cluster-wide, private network
kuberctl proxy
```

then

```shell
curl http://localhost:8001/version

# get the pod name
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')

echo $POD_NAME

# access the Pod through the proxied API
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME:8080/proxy/

```

Use the `curl` to use the service

```shell
# it opens up a browser window the serves your app and show the app's response
minikube service hello-node --url
# can see the url, e.g. http://127.0.0.1:58210
# use the url above
curl http://127.0.0.1:58210

# alternatively, we can use the `kubectl` to forward the port
# then open http://localhost:7080 in the browser
kubectl port-forward service/hello-node 7080:8080
# then
curl http://localhost:7080
```

Run bash in the container

```shell
# run bash in the container
kubectl exec -ti hello-node-66d457cb86-g6xcb -- bash
# then we can use `curl` inside the container
curl http://localhost:8080
# then exit the container
exit
```

### Stop `minikube` and the cluster

```shell
minikube stop
```

### References

For K8s:

- <https://kubernetes.io/docs/tutorials/kubernetes-basics/>
- <https://jimmysong.io/book/kubernetes-handbook/architecture/perspective/>

For Docker:

- <https://ruanyifeng.com/blog/2018/02/docker-tutorial.html>
- <https://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html>
- "The Docker Book" CN and EN
