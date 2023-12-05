# CKA-fluffy-octo-sniffle-guide
A CKA exam questions bank / tips and tricks for the fluffy octo sniffle engineers to ace CKA and actually goes beyond the exam and leak to your actual work.

# Shortcuts
Be a Bash Wiz (not really T_T).

`Ctrl+A` beginning of a terminal line.

`Ctrl+E` end of a terminal line.

`Alt+f`

`Alt+b`

`Ctrl+A then #` hash the command so you can adjust it without running it accidentally.

`cat /etc/*-release` or `cat /etc/os-release` tells you info about the OS.



# Vim Shortcuts
No you wont use vim for writing production level code.

However, you need to know some shortcuts as a yaml warrior.

## command mode

`:q!`

`:wq!`

`:%d`

## normal mode

`dd`

`/`

`n-N`

`w-b`

`Ctrl+d`

`Ctrl+u`

## visual mode
this mode is similar to dragging your cursor to highlight, and you can delete, copy, paste with it.

# Cluster Install

make sure that port forwarding is [enabled](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#forwarding-ipv4-and-letting-iptables-see-bridged-traffic)



make sure cgroups are configured for systemd
- First you need to open this file with vim `/etc/containerd/config.toml.` then use `%d` to remove everything then put the following lines, to enable cgroups
`[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  ...
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true`
then restart the container d `sudo systemctl restart containerd`

create keyring

`sudo mkdir -m 7 /etc/apt/keyrings`

install kubeadm kubectl and kubelet

ip addr | grep eth0

1- how to  know pod network cidr k get nodes -o yaml | grep -i podcidr
2- kubeadm  token

/net flannel to change the cidr to match the cidr

systemctl status containerd

systemctl status kube-apiserver

cat /etc/kubernetes/manifests 
# Initialize the Cluster

# CNI

# DNS

# Node Upgrade

## prepare the node for upgrade
k cordon $NODENAME
k drain $NODENAME --ignore-daemonset
k describe $NODEName (NodeNotSchedulable)
k describe node node01| grep -i schedule
k uncordon $NODENAME

## 
kubeadm upgrade plan | grep -i remote # to capture the remote latest vers

apt update
apt-cache madison kubeadm
apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm='1.27.0-00' && \
apt-mark hold kubeadm

kubeadm version

kubeadm upgrade plan

sudo kubeadm upgrade apply v1.27.0

if you get nodes you will still see the 1.26 version because  it capture the kubelet version which we didn't upgrade 

then do 
`kubectl drain <node-to-drain> --ignore-daemonsets`

apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet='1.27.x-*' kubectl='1.27.x-*' && \
apt-mark hold kubelet kubectl

sudo systemctl daemon-reload
sudo systemctl restart kubelet

kubectl uncordon <node-to-uncordon>

k drain node01 to evacted
then ssh to node01
then do 
sudo kubeadm upgrade node
apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet='1.27.x-*' kubectl='1.27.x-*' && \
apt-mark hold kubelet kubectl
then 

sudo systemctl daemon-reload
sudo systemctl restart kubelet
# etcd backup 
  ## Stacked
  certificates
  validity
  
  ## External


# Imperative Commands 
  ## Doc Links

# Exam Questions

  ## RBAC
   - Create role
   - rolebinding
   - User
   - Can create and list deployment
   - clusterrole --> create deployment
   - role --> list deployment 

  ## 
    
