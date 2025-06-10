## Replication Option

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

```bash
# Create RC in namespace 'qa'
kubectl create -f rc.yml -n qa

# Get the ReplicationController
kubectl get rc -n qa

# View detailed description
kubectl describe rc -n qa | less

# Delete the ReplicationController
kubectl delete rc r1

# Delete a pod to test High Availability (HA)
kubectl delete pod <podname> -n qa
```
# creat a pod with command(dry run )
```
kubctl run pod1 --image httpd --dry-run=client -o yaml >pod.yaml
ls
vim pod.yaml
metadata:
   labels:
      app: nginx
 kubectl creat -f pod.yaml

pod is created
run replication controller also
now check how many pods crated
kubectl get po --show-labels
pod1
nginx1
nginx2


totally it creats total 3 pods as labels of pod is app: nginx & selector app: nginx rc has aquird pod from pod label is same
 kubectl describe po
kubectl delete rc
 
---

### 🔄 3 Ways to Scale Up/Down

1. **Scale using CLI:**
   ```bash
   kubectl scale --replicas=no_of_replica replication-controller/name_of_rc
   kubectl scale --replicas=3 rc/rc -n qa
   ```

2. **Edit directly (via etcd update):**
   ```bash
   kubectl edit rc webserver -n qa
   # Change replica count and save (usually with :wq)
   ```

3. **Modify YAML and re-apply:**
   ```bash
   kubectl get rc rc -n qa -o yaml > is.yml
   # Edit `replicas:` value to desired number (e.g., 4)
   kubectl apply -f is.yml
   ```

> 📌 After applying, the new **desired state** will be reflected in the cluster.
