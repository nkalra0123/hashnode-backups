---
title: "Learning Kubernetes - Part 2"
datePublished: Fri Jan 06 2023 16:46:04 GMT+0000 (Coordinated Universal Time)
cuid: clckr188y000008mgghlvasdn
slug: learning-kubernetes-part-2
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/qMehmIyaXvY/upload/52b8b584bc226707773e9dbe0963efcc.jpeg
tags: kubernetes, k8s

---

**Master node**

The Master node in a Kubernetes cluster is the central control plane that is responsible for the overall management of the cluster. It is composed of a number of components, each with its own unique role to play in the orchestration of the cluster.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702108200890/b92da1fd-43a1-4bd5-bfb8-14f5bd26f03c.png align="center")

Some of the key components of the Master node include:

* **etcd**: This is a distributed key-value store that is used to store the cluster's configuration data. It is responsible for storing the desired state of the cluster and ensuring that the current state of the cluster matches the desired state.
    
* **kube-apiserver**: This is the API server that exposes the Kubernetes API and allows other components to communicate with the cluster. It is responsible for processing API requests and ensuring that they are carried out by the appropriate component in the cluster.
    
* **kube-scheduler**: This is the component that is responsible for scheduling pods on the Worker nodes. It takes into account the resource requirements of the pods and the available resources on the Worker nodes when making scheduling decisions.
    
* **kube-controller-manager**: This is the component that is responsible for the overall management of the cluster. It includes a number of controllers that are responsible for different aspects of the cluster, such as replicasets, services, and endpoints.
    

The Master node is responsible for the overall management of the cluster and communicates with the other nodes in the cluster to ensure that the desired state of the application is maintained. It is also responsible for exposing the Kubernetes API, which allows other components to communicate with the cluster and carry out their various functions.