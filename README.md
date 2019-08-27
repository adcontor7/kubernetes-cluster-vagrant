# Kubernetes Cluster with Ansible and Vagrant
Multi node Kubernetes cluster for development purposes

## Prerequisites
 - Vagrant should be installed on your machine. Installation binaries can be found here.
 - Oracle VirtualBox can be used as a Vagrant provider or make use of similar providers as described in Vagrant’s official documentation.
 - Ansible should be installed in your machine. Refer to the Ansible installation guide for platform specific installation.


```
## Up Vagrant VirtualMachines
$ vagrant up

## Accessing master
$ vagrant ssh k8s-master

vagrant@k8s-master:~$ kubectl get nodes
NAME         STATUS   ROLES    AGE     VERSION
k8s-master   Ready    master   18m     v1.13.3
k8s-node-1       Ready    <none>   12m     v1.13.3
k8s-node-2       Ready    <none>   6m22s   v1.13.3

## Accessing nodes
$ vagrant ssh node-1
$ vagrant ssh node-2
``` 

## DEPLOY DEVOPS TOOLS

```
$ ansible-playbook devops-deployments/playbook.yml -i inventory
```

## Deploy Kubernetes dashboard

https://github.com/kubernetes/dashboard
