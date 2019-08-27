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
$ vagrant ssh k8s-master

vagrant@k8s-master:~$ kubectl describe deployments nexus --namespace=devops-deployments
Name:                   nexus
Namespace:              devops-deployments
CreationTimestamp:      Tue, 27 Aug 2019 20:39:42 +0000
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
                        kubectl.kubernetes.io/last-applied-configuration:
                          {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"nexus","namespace":"devops-deployments"},"spec":{"replica...
Selector:               run=nexus-server
Replicas:               1 desired | 1 updated | 1 total | 0 available | 1 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  run=nexus-server
  Containers:
   nexus:
    Image:      sonatype/nexus3:latest
    Port:       8081/TCP
    Host Port:  0/TCP
    Limits:
      cpu:     500m
      memory:  500Mi
    Requests:
      cpu:        256m
      memory:     256Mi
    Environment:  <none>
    Mounts:
      /nexus-data from nexus-data (rw)
  Volumes:
   nexus-data:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     
    SizeLimit:  <unset>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      False   MinimumReplicasUnavailable
OldReplicaSets:  <none>
NewReplicaSet:   nexus-5d89685746 (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  18m   deployment-controller  Scaled up replica set nexus-5d89685746 to 1



vagrant@k8s-master:~$ kubectl describe services nexus-service --namespace=devops-deployments
Name:                     nexus-service
Namespace:                devops-deployments
Labels:                   <none>
Annotations:              <none>
Selector:                 run=nexus-server
Type:                     NodePort
IP:                       10.105.0.143
Port:                     <unset>  8081/TCP
TargetPort:               8081/TCP
NodePort:                 <unset>  30000/TCP
Endpoints:                
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>

vagrant@k8s-master:~$ kubectl get pods --selector="run=nexus-server" --output=wide --namespace=devops-deployments
NAME                     READY   STATUS             RESTARTS   AGE   IP               NODE         NOMINATED NODE   READINESS GATES
nexus-5d89685746-49c69   1/1     Running            1          19m   192.168.140.66   k8s-node-2   <none>           <none>


```

## Deploy Kubernetes dashboard

https://github.com/kubernetes/dashboard
