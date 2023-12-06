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
first you need to check if the container.d installed by using 
`systemctl status containerd`
then if you check kube-apiserver you will see nothing
`systemctl status kube-apiserver`

make sure that port forwarding is [enabled](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#forwarding-ipv4-and-letting-iptables-see-bridged-traffic)



make sure cgroups are configured for systemd
- First you need to open this file with vim `/etc/containerd/config.toml.` then use `%d` to remove everything then put the following lines, to enable cgroups
`[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  ...
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true`
then restart the container d `sudo systemctl restart containerd`

As per docs we should continue with installing the cluster Kubeadm / Kubectl / Kubelet From this [Link](https://v1-27.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- create keyring

`sudo mkdir -m 7 /etc/apt/keyrings`
- Update pakacages

`sudo apt-get update`

`sudo apt-get install -y apt-transport-https ca-certificates curl gpg`

`curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.27/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg`

`echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.27/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list`

`sudo apt-get update`
- Install the correct version

`sudo apt-get install -y kubeadm=1.27.0-2.1 kubectl=1.27.0-2.1 kubelet=1.27.0-2.1`

then we have to hold all 3
`sudo apt-mark hold kubelet kubeadm kubectl`

Then to join nodes we have to booststrap our cluster
we will start by allocating the Masternode ip address

`ip addr | grep -i eth0`
then by using kubeadm init we will pass the needed vars

`kubeadm init --apiserver-advertise-address=192.21.161.255 --apiserver-cert-extra-sans=controlplane --pod-network-cidr=10.244.0.0/16`

 **Note If you lost the token you can create a new one by `kubeadm token create --print-join-command` [reference](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-join/#token-based-discovery-with-ca-pinning)**

1- how to  know pod network cidr k get nodes -o yaml | grep -i podcidr
2- kubeadm  token

curl -L0 fflannaelurl > flannel.yaml
look for container and put --face arg

/net flannel to change the cidr to match the cidr

cat /etc/kubernetes/manifests 
# Initialize the Cluster

# CNI

kubectl apply -f CNI

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
  - Get the version of etcd
    k describe pod <etcdpo> -n kube-system
  - backup the etcd
  `ETCDCTL_API=3 etcdctl --endpoints --certfile --key= --trusted-ca snapshot save /opt/snapshot=pre-boot.db`

  restore from backup
`  ETCDCTL_API=3 etcdctl --data-dir /var/lib/etcd-from-backup snapshot restore /opt/snapshot-pre-boot.db`

  and then we need also to change the hostpath in the manifest file which is located in `/etc/kubernetes/manifests/etcd.yaml`
```hostPath:
    path: /var/lib/etcd-from-backup
    type: DirectoryOrCreate
    name: etcd-data
    also you need to change the volume mounts
    volumeMounts:
      mountPath: /var/lib/etcd-from-backup
      name: etcd-data
```
  - validity
    if you want to check validity run the following command
    `openssl x509  -noout -text -in /etc/kubernetes/pki/etcd/server.crt | grep -iA  3 validity`
  **Note** : for the grep -i `A` is after and 3 for 3 lines and if you want before you should use `B` so it get lines before

  ## External
  to check clusters allocated with a node you can use `kubectl config view`
  to switch context you can use `k config use-context $CLUSTERCONFIGNAME`
  you can check the kube-apiserver to check what is the etcd running with it from it's paramters 

  To make a external backup of etcd you should ssh to the controlplane then backup it's etcd using endpoint of advertisment url
  ```ETCDCTL_API=3 etcdctl --endpoints 192.16.173.12:2379 \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  snapshot save /opt/cluster1.db
```
Then you logout and do a `scp` where did you saved the backup to your currenct `scp cluster1-controlplane:/opt/cluster1.db /opt/cluster1.db`

To do an external server restore you should copy the external back to the external server then ssh into the etcd server `scp /opt/cluster2.db etcd-server:/root`

after that you can restore the snapshoot you did copied from the exteral server
`ETCDCTL_API=3 etcdctl --data-dir /var/lib/etcd-data-new snapshot restore /root/cluster2.db`
or by passing the certs
```
ETCDCTL_API=3 etcdctl --endpoints 192.16.173.3:2379 \
  --cert=/etc/etcd/pki/etcd.pem  \
  --key=/etc/etcd/pki/etcd-key.pem \
  --cacert=/etc/etcd/pki/ca.pem \
  --data-dir /var/lib/etcd-data-new snapshot restore /root/cluster2.db
```

Then you need to change the ownership for the new created dir by using the following command `chown -R etcd:etcd /var/lib/etcd-data-new`
then you should restart the daemon-reload by using `systemctl daemon-reload`
then `systemctl restart etcd.service`

# Storage

## Volume types
  - hostPath
```    
  volumeMounts:
    - mountPath: /var/local/aaa
      name: mydir
    - mountPath: /var/local/aaa/1.txt
      name: myfile
  volumes:
    - name: mydir
      hostPath:
        # Ensure the file directory is created.
        path: /var/local/aaa
        type: DirectoryOrCreate
    - name: myfile
      hostPath:
        path: /var/local/aaa/1.txt
        type: FileOrCreate
  ```

  - configMap
```
  volumeMounts:
    - name: config-vol
      mountPath: /etc/config
  volumes:
    - name: config-vol
      configMap:
        name: log-config
        items:
          - key: log_level
            path: log_level
```
 - PVC

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
    
