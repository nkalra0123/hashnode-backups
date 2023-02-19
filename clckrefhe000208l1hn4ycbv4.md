# Learning Kubernetes - Part 5

**Services**

In Kubernetes, a Service is an abstraction that allows you to access your applications from outside the cluster. It enables you to access your applications using a stable, virtual IP address and DNS name, regardless of where the actual application is running.

Services are created and managed by other Kubernetes objects, such as Deployments, ReplicaSets, and StatefulSets. These objects are responsible for ensuring that the desired number of replicas of the application are running and handling the deployment and scaling of the application.

Services are typically created in one of two ways:

* **ClusterIP**: This type of service exposes the application within the cluster, but not to the outside world. It is only accessible from within the cluster using its virtual IP address.
    
* **NodePort**: This type of service exposes the application to the outside world by opening a specific port on each node in the cluster. It is accessible from the outside using the node's IP address and the designated port.
    

In addition to these two types of services, Kubernetes also supports **LoadBalancer** and **ExternalName** services. LoadBalancer services create an external load balancer in the underlying infrastructure, such as a cloud provider, that routes traffic to the service. ExternalName services allow you to access external DNS names that are not managed by Kubernetes.

Services are an important part of the Kubernetes ecosystem and provide a stable, consistent way to access your applications from outside the cluster. They enable you to abstract away the underlying infrastructure and access your applications using a virtual IP address and DNS name, regardless of where the actual application is running.