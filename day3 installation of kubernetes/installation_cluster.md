
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
follow this link
https://docs.docker.com/engine/install/rhel/

```bash
subscription-manager register
yum remove docker docker-client-latest docker-common docker-latest docker-latest-logrotate  docker-engine podman runc
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo


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

``` bash 

version = 2

[plugins]

  [plugins."io.containerd.grpc.v1.cri"]

   [plugins."io.containerd.grpc.v1.cri".containerd]
   
        [plugins."io.containerd.grpc.v1.cri".containerd.runtimes]

	[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
        
	  runtime_type = "io.containerd.runc.v2"
          
	  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]

```
``` bash

systemctl restart containerd
```

6) Install Kubernetes tools

kubeadm install 
follow documentation on Kubernetes.io & follw install kubeadm on all node

Minikube: 1master & 1 worker node
Kubeadm --> install largest cluster

Installing kubeadm, kubelet and kubectl

```bash
# Set SELinux in permissive mode (effectively disabling it)
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```
set kubernetes repository 
```bash
# This overwrites any existing configuration in /etc/yum.repos.d/kubernetes.repo
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.33/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.33/rpm/repodata/repomd.xml.key
exclude=kubelet kubeadm kubectl cri-tools kubernetes-cni
EOF
```
Install kubeadm tools( kubelet, kubeadm and kubectl):

```bash
sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
sudo systemctl enable --now kubelet

```

7: Now Only on master node run 
``` bash
kubeadm init
```

all master node tools will be install & configuration done

8> login to cluster

To login to cluster require token & ca certificate & login id & passwd
Now goto root user & authenticate it
follow steps 
goto root home dir 
```bash
cd /root
mkdir .kube 
cd /etc/kubenetes
cp admin.conf /root/.kube/config
       or 
kubectl get nodes --kubeconfig=/etc/kubernetes/admin.conf
now check 
kubectl get nodes
```

it shows master node detail (ypu have succesfully login to kubernetes cluster )

9> connect worker node to master node

token genereted by master node copy it and paste on worker node 
Connect worker node to master node
kubectl get nodes
You have to take token from master node and paste to worker node with which you can connect 

Token can be generate with following command

kubeadm token generate 

kubeadm token list 
kubeadm token create

kubeadm token create –print-join-command

Copy token from kudeadm init output from master node (to create new token again you can reset kubeadm reset then again kubeadm init )
Paste that command on worker node 

4: install calico on matsre node Calico.yaml search follow documentation for  kubenetes on 50 less than node

curl https://raw.githubusercontent.com/projectcalico/calico/v3.29.1/manifests/calico.yaml -O

kubectl apply -f calico.yaml

kubectl get pods -n kube-system

kubectl get nodes 

After calico installation on master node





