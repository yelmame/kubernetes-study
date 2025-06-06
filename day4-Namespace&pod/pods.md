****  PODS  ***

-smallest unit of resources is pod we cannot interact with container , we create pods
 
``bash 
kubectl get pods -n amazon 

kubectl get pods -n kube-system

kubectl get pods --all-namespaces

kubectl run podname --image=nginx:1.14.2

kubectl get pods

kubectl get pods -n default 

kubectl describe pod pod1

kubectl edit pod pod1
(if you change name of pod then it create pod & delete )

kubectl get pods --show-labels

kubectl run pod1 --image=nginx:1.14.2 -n amazon 

kubectl get ev 

kubectl logs pod1

kubectl exec -it <podname> /bin/bash

env 

cat /etc/os-release
```

Method-2   create pod with yaml 
vi  pod2.yaml
```bash
apiVersion: v1
kind: Pod
metadata:
	name: pod2
	labels:
		app: nginx
spec:
	containers:
		- name: con1
		  image: nginx:1.14.2
		  ports: 
			- containerPort: 80

```
```bash

kubectl create -f pod2.yaml
```
modify file with new label add

```bash
kubectl apply -f pod2.yaml

kubectl create -f pod
```

