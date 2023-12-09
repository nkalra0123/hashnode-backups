---
title: "Learning Kubernetes - Part 3"
datePublished: Fri Jan 06 2023 16:49:00 GMT+0000 (Coordinated Universal Time)
cuid: clckr501j000108l5ccys11ke
slug: learning-kubernetes-part-3
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/tjX_sniNzgQ/upload/853617f9e56a248905360eab0ff637ef.jpeg
tags: kubernetes, k8s

---

**Worker nodes**

Worker nodes, also known as "minions," are the nodes in a Kubernetes cluster that are responsible for running the containerized applications. They communicate with the Master node to receive instructions on what to do and report back on their status.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702108283193/205afb4c-bf65-457c-9a59-a3ce036e6221.png align="center")

Each Worker node is composed of a number of components, including:

* **kubelet**: This is the primary component that runs on each Worker node and is responsible for communicating with the Master node. It receives instructions from the Master node on which pods to run and ensures that they are running on the Worker node.
    
* **kube-proxy**: This is a network proxy that runs on each Worker node and is responsible for implementing the networking rules for the pods on the Worker node. It enables the pods to communicate with each other and with the outside world.
    
* **Container runtime**: This is the software that is responsible for running the containers on the Worker node. Kubernetes supports a number of container runtimes, including Docker, rkt, and containerd.
    

Worker nodes are responsible for running the containerized applications and communicating with the Master node to ensure that the desired state of the application is maintained. They are also responsible for implementing the networking rules for the pods and enabling communication between the pods and the outside world.