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
kubectl delete rc -n qa

# Delete a pod to test High Availability (HA)
kubectl delete pod <podname> -n qa
```

---

### 🔄 3 Ways to Scale Up/Down

1. **Scale using CLI:**
   ```bash
   kubectl scale --replicas=3 rc/webserver -n qa
   ```

2. **Edit directly (via etcd update):**
   ```bash
   kubectl edit rc webserver -n qa
   # Change replica count and save (usually with :wq)
   ```

3. **Modify YAML and re-apply:**
   ```bash
   kubectl get rc webserver -n qa -o yaml > is.yml
   # Edit `replicas:` value to desired number (e.g., 4)
   kubectl apply -f is.yml
   ```

> 📌 After applying, the new **desired state** will be reflected in the cluster.
