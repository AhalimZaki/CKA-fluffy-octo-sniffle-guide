# CKA-fluffy-octo-sniffle-guide
A CKA exam questions bank / tips and tricks for the fluffy octo sniffle engineers to ace CKA and actually goes beyond the exam and leak to your actual work.
## 🛠 Pre-requisites
<details><summary> Useful terminal Shortcuts </summary>
<p>
Be a Bash Wiz (not really T_T).

`Ctrl+A` beginning of a terminal line.

`Ctrl+E` end of a terminal line.

`Alt+f`

`Alt+b`

`Ctrl+A then #` hash the command so you can adjust it without running it accidentally.

`cat /etc/*-release` or `cat /etc/os-release` tells you info about the OS.
</p>
</details>

<details><summary> Useful VIM Shortcuts </summary>
<p>
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

</p>
</details>

## 📚 Courses & Books a starting point.

# Cluster Install

# Initialize the Cluster

# CNI

kubectl apply -f CNI

# DNS
to check the svc we can use curl
`kubectl exec web -n payroll -- curl web-service.default`

nslookup for a service
kubectl exec -it hr -- nslookup mysql.payroll > /root/CKA/nslookup.out

# Node Upgrade

# etcd backup 

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
    
