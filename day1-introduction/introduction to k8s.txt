
#  ☸️ What is Kubernetes?

Kubernetes (often called K8s) is an open-source container orchestration platform.
It automates deploying, scaling, and managing containerized applications.

If you have a lot of containers running across multiple servers, Kubernetes makes sure they are running properly, scaling up/down when needed, and recovering if something fails — all automatically!

### 🌟 Why Kubernetes?

Handles container deployment (no manual start/stop)

Scales apps automatically based on traffic/load

Self-heals (restarts crashed containers, reschedules them if a server fails)

Load-balances traffic between containers

Manages secrets and config securely

Rolls out updates to apps without downtime

### 🛠️ Key Concepts:


Concept	Meaning
Pod	Smallest unit in Kubernetes — it can hold one or more containers
Node	A single machine (physical or virtual) in the Kubernetes cluster
Cluster	A group of nodes managed by Kubernetes
Deployment	Defines how to create/update Pods
Service	Exposes your app to other apps or the outside world
Namespace	Virtual clusters within a cluster (for organizing resources)

### 📈 Quick Example:

You want 5 replicas of your app.

Kubernetes creates 5 Pods running your app across available Nodes.

If one Pod crashes, Kubernetes automatically spins up a new one.

If your app gets lots of traffic, Kubernetes can scale up by creating more Pods.

