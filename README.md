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

`sudo ap-get update`

`sudo apt-get install -y apt-transport-https ca-certificates curl gpg`

`curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg`

`echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list`

`sudo apt-get update`
`sudo apt-get install -y kubelet kubeadm kubectl`
`sudo apt-mark hold kubelet kubeadm kubectl`

`install kubeadm kubectl and kubelet`


ip addr | grep eth0

kubeadm init --apiserver-advertise-address=192.15.120.12 --apiserver-cert-extra-sans=controlplane --pod-network-cidr=10.244.0.0/16

If you lost the token you can create a new one by `kubeadm token create --print-join-command` [reference](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-join/#token-based-discovery-with-ca-pinning)

1- how to  know pod network cidr k get nodes -o yaml | grep -i podcidr
2- kubeadm  token
curl -L0 fflannaelurl > flannel.yaml
look for container and put --face arg

/net flannel to change the cidr to match the cidr

systemctl status containerd

systemctl status kube-apiserver

cat /etc/kubernetes/manifests 
# Initialize the Cluster

# CNI

# DNS
to check the svc we can use curl
`kubectl exec web -n payroll -- curl web-service.default`

nslookup for a service
kubectl exec -it hr -- nslookup mysql.payroll > /root/CKA/nslookup.out

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
    
