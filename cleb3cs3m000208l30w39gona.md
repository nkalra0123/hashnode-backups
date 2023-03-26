---
title: "Learning Kubernetes - Part 6"
seoTitle: "10 Essential kubectl Commands for Managing Kubernetes Clusters"
seoDescription: "we explore ten essential kubectl commands that every Kubernetes user should know. These commands allow you to efficiently manage your cluster"
datePublished: Sun Feb 19 2023 07:52:41 GMT+0000 (Coordinated Universal Time)
cuid: cleb3cs3m000208l30w39gona
slug: learning-kubernetes-part-6
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/jOqJbvo1P9g/upload/09fd91fdbad94a387ddf9e4c3f5bf946.jpeg
tags: kubernetes, k8s

---

Kubernetes is a popular container orchestration tool used for deploying and managing containerized applications at scale. As a Kubernetes user, it's essential to know some useful `kubectl` commands that can help you manage your cluster more efficiently. Here are ten useful `kubectl` commands that you can use:

Ten useful `kubectl` commands for working with Kubernetes clusters:

1. `kubectl get pods`: This command lists all the running pods in the current namespace.
    
2. `kubectl get nodes`: This command lists all the nodes in the cluster.
    
3. `kubectl describe pod <pod-name>`: This command provides detailed information about a specific pod.
    
4. `kubectl logs <pod-name>`: This command displays the logs of a specific pod.
    
5. `kubectl apply -f <filename>`: This command creates or updates resources defined in a YAML or JSON file.
    
6. `kubectl delete pod <pod-name>`: This command deletes a specific pod.
    
7. `kubectl exec <pod-name> <command>`: This command allows you to run a command inside a specific pod.
    
8. `kubectl port-forward <pod-name> <local-port>:<pod-port>`: This command forwards traffic from a local port to a specific port in a pod.
    
9. `kubectl rollout status <deployment-name>`: This command checks the status of a deployment rollout.
    
10. `kubectl scale deployment <deployment-name> --replicas=<number>`: This command scales the number of replicas for a deployment.
    

In some of these commands, you can specify namespaces.

```yaml
kubectl get pods -n <namespace>
```

If you want to list all pods in all namespaces, use the following command:

```yaml
kubectl get pods --all-namespaces
```

Namespaces in Kubernetes are a way to create virtual clusters within a physical Kubernetes cluster. A namespace provides a way to group and organize resources in a Kubernetes cluster, and it acts as a boundary for these resources.

Each namespace is a unique environment where Kubernetes resources can be created, such as pods, services, and deployments. Resources in one namespace are isolated from resources in other namespaces, so they don't interfere with each other.

By default, Kubernetes has three built-in namespaces: `default`, `kube-system`, and `kube-public`. The `default` namespace is the default namespace that resources are created in if a namespace is not specified. The `kube-system` namespace is used for Kubernetes system resources, and the `kube-public` namespace is used for resources that should be publicly available in the cluster.

If you want to learn more about kubectl commands, you can check this article : [Kubectl Cheat Sheet - 15 Kubernetes Commands & Objects (](https://spacelift.io/blog/kubernetes-cheat-sheet)[spacelift.io](http://spacelift.io)[)](https://spacelift.io/blog/kubernetes-cheat-sheet)