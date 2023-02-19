# Learning Kubernetes - Part 3

**Worker nodes**

Worker nodes, also known as "minions," are the nodes in a Kubernetes cluster that are responsible for running the containerized applications. They communicate with the Master node to receive instructions on what to do and report back on their status.

Each Worker node is composed of a number of components, including:

* **kubelet**: This is the primary component that runs on each Worker node and is responsible for communicating with the Master node. It receives instructions from the Master node on which pods to run and ensures that they are running on the Worker node.
    
* **kube-proxy**: This is a network proxy that runs on each Worker node and is responsible for implementing the networking rules for the pods on the Worker node. It enables the pods to communicate with each other and with the outside world.
    
* **Container runtime**: This is the software that is responsible for running the containers on the Worker node. Kubernetes supports a number of container runtimes, including Docker, rkt, and containerd.
    

Worker nodes are responsible for running the containerized applications and communicating with the Master node to ensure that the desired state of the application is maintained. They are also responsible for implementing the networking rules for the pods and enabling communication between the pods and the outside world.