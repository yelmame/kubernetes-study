
# Minikube with Docker on RHEL 9

  ## Prerequisites
  ### System Requirements
    1. RHEL 9 with 2+ CPUs and 4GB RAM
    2. 20GB free disk space
    3. Internet connectivity
    4. Sudo privileges

  ### Docker Version Requirements
    1. Docker 18.09 or higher (20.10+ recommended) for optimal compatibility.
  
  ## Step-by-Step Installation

  ## Step 1: Install Docker CE

  ### Add Docker repository
``` 
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

  ### Install Docker CE with compatibility flags
``` bash 
sudo dnf install docker-ce docker-ce-cli containerd.io --nobest -y
```

  ### Start and enable Docker service
 ``` bash
sudo systemctl start docker
sudo systemctl enable docker
 ```
  ### Verify Docker installation
``` bash 
sudo systemctl status docker
docker --version
```

  ## Step 2: Configure Docker for Non-Root Access

  ### Add current user to docker group
  ``` bash
  sudo usermod -aG docker $USER
  
  ```
  ### Apply group changes (logout/login or use newgrp)
  ```bash
  newgrp docker
  ```

  ### Test Docker without sudo
  ```bash 
  docker run hello-world
  ```
  ## Step 3: Install kubectl
  ### Download latest stable kubectl

  ```bash
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  ```
  ### Make executable and install
  ```bash
  chmod +x kubectl
  sudo mv kubectl /usr/local/bin/
  ```
  
  ### Verify installation
  ```bash
  kubectl version --client --output=yaml
  ```
  ## Step 4: Install Minikube
  ### Download latest Minikube binary
  ```bash 
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  ```
  ### Install Minikube
  ```bash
  sudo install minikube-linux-amd64 /usr/local/bin/minikube
  ```
  ### Verify installation
  ```bash 
  minikube version
  ```

Starting Minikube with Docker Driver
Basic Cluster Startup

  ### Start Minikube with Docker driver
  ```bash
  minikube start --driver=docker
  ```
  ### Set Docker as default driver for future clusters
  ```bash
  minikube config set driver docker
  ```
  ### Verify cluster status
  ```bash
  minikube status
  ```
  Advanced Configuration Options
  ### Start with custom resource allocation
  ```bash 
  minikube start --driver=docker --memory=4096 --cpus=2 --disk-size=20GB
  ```
  ### Start with specific Kubernetes version
  ```bash
  minikube start --driver=docker --kubernetes-version=v1.28.3
  ```
  ### Start with container runtime specification
  ```bash
  minikube start --driver=docker --container-runtime=containerd
  
  ```
  ## Cluster Verification and Testing
  ### Verify Cluster Components
  ### Check cluster information
  ```bash
  kubectl cluster-info
  ```
  ### List nodes
  ```bash
  kubectl get nodes -o wide
  ```
  ### Check system pods
  ```bash
  kubectl get pods -n kube-system
  ```
  ### Minikube specific status
  ```bash
  minikube status
  ```
  ### Deploy Test Application
  ### Create nginx deployment
  ```bash
  kubectl create deployment nginx-test --image=nginx:latest --replicas=2
  ```
  ### Expose deployment as NodePort service
  ```bash
  kubectl expose deployment nginx-test --type=NodePort --port=80 --target-port=80
  ```
  ### Get service details
  ```bash
  kubectl get services nginx-test
  ```
  ### Access the service through Minikube
  ```bash
  minikube service nginx-test --url
  ```
  ### Test the service
  ```bash
  curl $(minikube service nginx-test --url)
  ```




