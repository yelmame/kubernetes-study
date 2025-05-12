
# Installation of kubernetes cluster 

1 Basic requirement -. 2x CPU & 2GB RAM

2 Disable Swap memory
   
   ``` bash 
   	swapoff --a
```

Disable SELinux

``` bash 
    setenforce 0
```
Disable Firewall
```bash 
	systemctl stop firewalld
```
3) Host entry

  	vim /etc/hosts
	Both master & worker node entry on all node
	{ip addddress domain_name  shortname}
                            
4) Install docker

```bash 
yum remove docker docker-client-latest docker-common docker-latest docker-latest-logrotate  docker-engine podman runc
yum install yum-utils -y 
yum-config-manager –add-repo https://download.docker.com/linux/centos/docker-ce.repo

yum install docker-ce

systemctl start docker

systemctl enable docker

docker run hello-word
```
5 containerd error

If there is error related to containerd 

vim /etc/containerd/config.toml
#Disabled_plugin =[cri]

Add these  lines below disabled_plugin=[cri]

version = 2

[plugins]

  [plugins."io.containerd.grpc.v1.cri"]

   [plugins."io.containerd.grpc.v1.cri".containerd]
   
        [plugins."io.containerd.grpc.v1.cri".containerd.runtimes]

	[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
        
	  runtime_type = "io.containerd.runc.v2"
          
	  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]


Systemctl restart containerd

6) Install Kubernetes tools

Kubeadm install  (follow documentation on Kubernetes.io documentation& follw install kubeadm on all node)
Minikube: 1master & 1 worker node
Kubeadm --> install largest cluster

NowOnly on master node run 
Kubeadm init
Login to cluster node
Connect worker node to master node
Kubectl get nodes
Mkdir .kube in roots home dir 
Cp /etc/Kubernetes/admin.conf /root/.kube/config

Connect to worker node 
You have to take token from master node and paste to worker node with which you can connect 
Token can be generate with following command

Kubeadm token generate 
Kubeadm token list 
Kubeadm token create 
Kubeadm token create –print-join-command

Copy token from kudeadm init output from master node (to create new token again you can reset kubeadm reset then again kubeadm init )
Paste that command on worker node 

4: install calico on matsre node
   Calico.yaml search follow documentation for  kubenetes on 50 less than node 
 curl https://raw.githubusercontent.com/projectcalico/calico/v3.29.1/manifests/calico.yaml -O
Kubectl apply -f calico.yaml
Kubectl get pods -n kube-system
Kubectl get nodes 
After calico installation on master node





