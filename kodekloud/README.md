# Section3: Scheduling
- manual scheduling (Done)
- labels and selectors (Done)
- taints and tolerations (Done)
- Node Affinity (Done)
- Resource Requirements and Limtis (Done)
- Daemonsets (Done)
- Static Pods (Done)

# Section4: Logging & Monitoring
- Monitoring (Done)
- Monitoring Application Logs (Done)

# Section5: Application Lifecycle Management
- Rolling Updates &  Rollbacks (Done) 
- Commands & Arguments (Done)
- Environment Variables (Done)
- Secrets (Done)
- Multi Container Pods (Done)
- Init Containers (Done)

# Section 6: Cluster Maintenance
- OS Upgrade (Done)
```
k cordon node01
k drain --ignore-ademonsets node01
k uncordon node01
```
- Cluster Upgrade (Done)
https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

```
# master node to be updated first 
# then the worker node to be updated one by one 

To see available version update "vim /etc/apt/sources.list.d/kubernetes.list"

# For controlplane, you should run 
-> kubeadm upgrade apply v1.31.0

# For other nodes, you should run 
-> kubeadm upgrade node 

```
- Backup and Restore 1 (Done)
```
etcdctl --data-dir /var/lib/etcd-backup snapshot restore /opt/snapshot-pre-boot.db 
```
- Backup and Restore 2 (Review is required)

# Section 7: Security 
- View Certificate (Done)
- Certificates API (Done)
- KubeConfig (Done)
```
export KUBECONFIG=<PATH>
Otherwise
k config --kubeconfig=<PATH> use-config research
```
- RBAC (Review is required)

```
# Roles and rolebindings are namespaced
# The kubernets api server may authorize a request using one of several authorization modes: ABAC, RBAC, Node, and so on. 
k get pod --as=dev-user
k auth can-i create deployment -n blue --as=dev-user
```

- Cluster Roles and Role Binding (Done)
```
# cluster scoped, so not namespaced

```
- Service Accounts (Done, a review is required)
- Image Security (Done)
- Security Contexts (Done)
- Network Policy  (Done)
-> It is a firewall for kubernetes

# Section 8: Storage
- pv and pvc (A review is required)
- storage class (A review is required)

# Section 9: Networking
- Explore Kubernetes Environment (Done)
- Explore CNI (Done)
- Deploy Network Solution (Done)
- Networking Weave  (Done)
- Service Networking  (Done)
- Explore DNS (Done)
- Ingress 1 (Done)
- Ingress 2 (Done)



- Must to be studied
Create roles, service, and rolebindings

Unschedule a node and reschedule the pods

Backup and restore ETCD

Upgrade a cluster to a specific version

NETWORK POLICY

Scale deployments

NodeSchedulers

Pod logs

Pod resource usage

Persistent Volumes

Assigning PVC to pods

Sidecar containers

Ingress

Exposing deployments/ pods

Troubleshooting nodes, pods, or network (practice all)

https://www.reddit.com/r/kubernetes/comments/s6l7xs/just_passed_the_cka_here_are_some_tips_and_tricks/