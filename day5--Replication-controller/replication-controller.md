uum ## Replication Option

### ● What is RC

A **Replication Controller (RC)** is a legacy Kubernetes object used to ensure that a specified number of pod replicas are running at any given time.

---

### ● Why You Need Replication

- ✅ Reliability  
- 🌐 Load Balancing  
- 🔁 High Availability  
- 📈 Scaling  

---

### ● Types of Kubernetes Replication

- Replication Controller
- Replica Set *(Replaced RC)*
- Deployments

> 🔎 **Concept of Selector and Label**  
Selectors and labels are used to group and manage pods under replication strategies.

---

## Replication Controller Deployment Strategy
vi rc.yaml
***************
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
******************************

```bash
# Create RC in namespace 'qa'
kubectl create -f rc.yml -n qa

# Get the ReplicationController
kubectl get rc -n qa

# View detailed description
kubectl describe rc -n qa | less
# delete 1 pod from rc
kubectl deleete pod podname
it just create a new pod before delete as desired state is maintained
if want to deelete pod permnently delete the rc not pod


# Delete the ReplicationController
kubectl deelete rc     
kubectl delete rc -n qa

# Delete a pod to test High Availability (HA)
kubectl delete pod <podname> -n qa
```

---

### 🔄 3 Ways to Scale Up/Down

1. **Scale using CLI:**
   ```bash
   kubectl scale rc nginx --replicas=5     (nginx - name of replica) 
   ```

2. **Edit directly (via etcd update):**
   ```bash
   kubectl edit rc nginx -n qa
   # Change replica count and save (usually with :wq)
   ```

3. **Modify YAML and re-apply:**
   ```bash
   kubectl get rc nginx -n qa -o yaml > is.yml
   # Edit `replicas:` value to desired number (e.g., 4)
   kubectl apply -f is.yml
   ```

> 📌 After applying, the new **desired state** will be reflected in the cluster.


### Drawback of Replication controller 
delete above rc 
kubectl delete rc nginx 
create yaml file by command   
# kubectl run pod1 --image httpd --dry-run=client -o yaml > pod.yaml
pod.yaml file trough pod of httpd is created 
edit pod.yaml file  and add label app: nginx

run rc nginx where update  selector as app: nginx

kubetcl create -f rc.yaml

kubectl get po --show-labels

it will creeate 2 pods as it count the orphan pod which created by pod.yaml because tha label and selector for pod & rc pod are same i.e 3

to check whether the pod is member of RC 
kubectl describe po 
this is drwback of RC 
kubectl delete rc nginx

selector and label both should be same and its consider last label and last selector





