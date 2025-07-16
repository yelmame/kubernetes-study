
# Namespace in kubernetes 

In Kubernetes, a namespace is a logical partition that allows you to organize and isolate resources (like pods, services, and deployments) within a cluster. Namespaces are especially useful in large environments where multiple teams or applications share the same Kubernetes cluster.


## Why Use Namespaces?

- Resource Isolation: Different teams or applications can have their own namespace to avoid conflicts.

- Access Control: You can set role-based access control (RBAC) per namespace.

- Quota Management: Limit CPU, memory, and other resources per namespace.

- Environment Separation: Use different namespaces for dev, staging, and production.

### Default Namespaces in Kubernetes
Kubernetes comes with some built-in namespaces:

- default - The default namespace if you don’t specify one.

- kube-system - Contains system-related components (e.g., kube-dns, controller-manager).

- kube-public - Used for public resources accessible across the cluster.

- kube-node-lease - Stores heartbeats of nodes for faster failure detection.

#### Common Namespace Operations

1. Listing all namespaces
``` bash 

kubectl get namespaces
kubectl get ns 
kubectl get pods
kubectl get pods -n kube-system  

```

2. Creating a new namespace
``` bash
kubectl create namespace my-namespace
kubectk create ns nginx
kubectl get ns 
kubectl run nginx --image nginx    (nginx pod created )
kubectl delete pod nginx
kubectl run nginx --image nginx -n nginx (to create pod in ns nginx)
kubectl get po
kubectl get ns --show-labels
kubectl label po podname app=web
kubectl label po podname app-
 

```

3. Deploying resources into a namespace
``` bash
kubectl apply -f my-app.yaml --namespace=my-namespace
```
4. Switching to a namespace
``` bash 
kubectl config set-context --current --namespace=my-namespace
```
5. Deleting a namespace
``` bash
kubectl delete namespace my-namespace
```
##### Example YAML to Create a Namespace
mkdir nginx 
cd nginx 
vi nginx.yaml
``` bash
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
```
Apply it using

``` bash
kubectl apply -f nginx.yaml
```

