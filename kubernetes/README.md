# kubernetes

## prerequisites

Pre-installations
- Oracle Virtual Box
- Vagrant

Oracle Virtual box comes by default with 192.168.56.1 series as the network and hence these boxes use the same over that network.

- Master IP: 192.168.56.51
- Worker-1 IP: 192.168.56.52
- Worker-2 IP: 192.168.56.53

## Installations

clone the repository and perform these actions
```
git clone git@github.com:samperay/localk8cluster.git
cd localk8cluster
vagrant up
```

Once you have your installations setup, check your /etc/hosts and you are able to communicate between the virtual boxes.

list all the VM's of the virtual box

```
vagrant status
vagrant ssh kubemaster
vagrant ssh kubenode01
vagrant ssh kubenode02
```

## Kubernetes Cluster Setup

### Master Node(kubemaster)

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt-get install kubeadm kubelet kubectl
kubeadm version
```

If there is any error in stating the kubelet service due to Cgroup setiings, perform the below

vim /etc/docker/daemon.json
```
{
    "exec-opts": ["native.cgroupdriver=systemd"]
}
```

```
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo systemctl restart kubelet
```

```
kubeadm init --apiserver-advertise-address=192.168.56.51 --pod-network-cidr=192.168.0.0/16

sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

kubectl get pods --all-namespaces
```

You would be promted a command to join the worker nodes to the master node(control plane), make a note of it so that we could use that to bootstrap the worker nodes.

Example:

kubeadm join 192.168.56.51:6443 --token 7kk6uh.n6ojcbz90f6rqgx5 \
    --discovery-token-ca-cert-hash sha256:1ab498d5becd038bf9bda579c51a1227908c0ff65cf9c218fe0f8bb7b934f279

### Worker Node(kubenode01/kubenode02)

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt-get install kubeadm kubelet kubectl
kubeadm version
```

If there is any error in stating the kubelet service due to Cgroup setiings, perform the below

vim /etc/docker/daemon.json
```
{
    "exec-opts": ["native.cgroupdriver=systemd"]
}
```

```
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo systemctl restart kubelet
```

Now, join the worker nodes to master node using below command that you got above.

kubeadm join 192.168.56.51:6443 --token 7kk6uh.n6ojcbz90f6rqgx5 \
    --discovery-token-ca-cert-hash sha256:1ab498d5becd038bf9bda579c51a1227908c0ff65cf9c218fe0f8bb7b934f279

### control-plane or the kubemaster

validate your kubernetes cluster along with the nodes.

```
kubemaster $ kubectl  get nodes
NAME         STATUS   ROLES                  AGE   VERSION
kubemaster   Ready    control-plane,master   52m   v1.23.3
kubenode01   Ready    <none>                 25m   v1.23.3
kubenode02   Ready    <none>                 23m   v1.23.3
kubemaster $
```
