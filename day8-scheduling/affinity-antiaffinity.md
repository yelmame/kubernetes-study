
# Affinity & Antiaffinity

## affinity & anti-affinity(node/pod)

task: node affinity 

```bash

kubectl delete deploy 
```
```bash
vim affinity.yaml

apiVersion: v1
kind: Pod
metadata:
  name: node-affinity
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: env
            operator: In
            values:
            - dev            
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
```


 kubectl delete deploy.yaml (delete if any pod or deployment is present )

provide label to worker node as env = dev as in above affinity file 

kubectl label no worker1 env=dev now pods are created on 

```bash
kubectl create -f affinity.yaml
kubectl get all 
```
now pods are created on worker1  as node selector from yaml file match with labell on node 

--- requiredDuringSchedulingIgnoredDuringExecution:---

as its requireduring scheduling means at the time o
f scheduling label compalsary should be there 
and Ignored during execution means after scheduled pod if you change label on node it will igored and excute pods on node


task 2: now we use refferedduringscheduling

now make sure the label disktype=ssd should not be configured on any node 
still pod is  created on worker1 though  the label disktype=ssd is not available on any node 
because of preferredduring scheduling is there it will schedule pod 


vim affinity1.yaml

apiVersion: v1
kind: Pod
metadata:
  name: affinity1
spec:
  affinity:
    nodeAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd          
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent


***********************
# kubectl create -f affinity.yaml 
#kubectl get po -o wide 
affinity1 pod is created

********************************************



2> pod affinity
create a pod with label security=s1

```bash

vim pod.yaml

apiVersion: v1
kind: Pod55
metadata:
  name: nginx
  labels:
    security: s1
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```


#kubectl create -f pod.yaml

#kubectl get all -o wide

this pod schedule on worker2 node

```bash
# vim affinity.yaml
apiVersion: v1
kind: Pod
metadata:
  name: affinity-demo
  labels:
    security: s1
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchExpressions:
              - key: security
                operator: In
                values:
                  - s1
          topologyKey: "kubernetes.io/hostname"
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchExpressions:
              - key: app
                operator: In
                values:
                  - nginx
          topologyKey: "kubernetes.io/hostname"
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
```

# kubectl create -f affinity.yaml

# create pod on worker2 node where pod label i.e security = s1 matches
where due to aniaffinity it will reppeled


 

