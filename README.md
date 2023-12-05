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



create keyring



install kubeadm kubectl and kubelet


systemctl status containerd

systemctl status kube-apiserver

cat /etc/kubernetes/manifests 
# Initialize the Cluster

# CNI

# DNS

# Node Upgrade


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
    
