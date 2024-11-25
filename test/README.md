# pod_in_control_plane.yaml
- Deploy a pod call nginxpod with image nginx in controlplane, make sure pod is not scheduled in worker node

```
k run nginxpod --image=nginx --dry-run=client -o yaml > pod_in_control_plane.yaml 

# Check the controlplnae add nodeName in "spec.nodeName[]" to assign the pod for a specific node 

```

# pod_as_a_service_80
- Expose a exsiting pod called nginxpod as a service, service name should be nginxsvc 

```
k expose pod nginxpod --port=80

## Expected output
service/nginxsvc exposed

k get svc

## Expected output 
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP   55d
nginxsvc     ClusterIP   10.105.133.221   <none>        80/TCP    99s

curl 10.105.133.221

## Expected output 
!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
...
...
<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
# pod_as_a_service_30200

- Expose a existing pod called nginxpod, service name as nginxnodeportsvc, service should access through nodeport 

```
k expose pod nginxpod --type=NodePort --port=80 --name=nginxnodeportsvc

# Update nodePort to 30200
get svc | grep -i nodeport

## Expected output
## This means that traffic coming to port 30200 on any Node in the cluster will be forwarded to port 80 on the service. The protocol used is TCP.

nginxnodeportsvc   NodePort    10.102.10.28     <none>        80:30200/TCP 

curl -s 172.31.16.46:30200 | grep -i welcome

## Expected output 
<title>Welcome to nginx!</title>
<h1>Welcome to nginx!</h1>

```
# manual_scale_up
- You can find an existing deployment frontend in production namespace, scale down the replicas to 2 and change the image to ngnix:1.25

```
kubectl scale deploy frontend --replicas=2 -n production
```

# auto_sclae_deployment
- Auto scale the existing deployment frontend in production namespace at 80% of pod CPU usage, and set minimum replicas=3 and Maximum replicase=5 

```
k autoscale deployment/frontend -n production --min=3 --max=5 --cpu-percent=80
k get hpa -n production

## Expected output
AME       REFERENCE             TARGETS         MINPODS   MAXPODS   REPLICAS   AGE
frontend   Deployment/frontend   <unknown>/80%   2         4         2          69s
```

# expose_deployment_nodeport
- Expose existing deployment in production namespace named as frontend through nodeport and nodeport service name should be frontendsvc

```
k expose -n production svc frontendsvc --name=frontendsvc  --port=80 --type=NodePort 

or 

k apply -f expose_deployment_nodeport.yaml 
```

# persistent_volume
- You can find a pod named task-pv-pod in the default namespace, please check the status of the pod and troubleshoot, you can recreate the pod if you want

# taint_and_toleration

- Deploy a pod with the following specifications:
- pod name: web-pod
- image: httpd
- Node: Node01
- Note: Do not modify any settings on amster and worker nodes 

```
 k get node ip-172-31-23-161 -o jsonpath='{.spec.taints}'
 {"effect":"NoSchedule","key":"node.kubernetes.io/disk-pressure","timeAdded":"2024-11-10T18:05:22Z"}]
 
 Then, add a tolerance into a pod 

```

# pv_pvc_deploy
- Create a new PV named web-pv. It should have a capacity of 2Gi, accessMode ReadWriteOnce, hostpath /vol/data and no storageClassName defined.
- Next Create a new PVC in namespace production named web-pvc. It should request 2Gi storage, accessMode ReadWriteOnce and should not define a storageClassName. The PVC should bound to the PV correctly. 
- Finally create a new deployment web-deploy in namespace production which mounts that volume at /tmp/web-data. The pods of that deployment should e of image nginx:1.14.2

```
k apply -f pv_pvc_deployment.yaml
k exec -it pod/web-deploy-74d4979444-v4bkx -n production -- /bin/bash
df -h

## Expected output 
Filesystem      Size  Used Avail Use% Mounted on
overlay         7.6G  6.2G  1.5G  82% /
tmpfs            64M     0   64M   0% /dev
/dev/root       7.6G  6.2G  1.5G  82% /tmp/web-data           <-------- Mounted 
shm              64M     0   64M   0% /dev/shmtmpfs           850M   12K  850M   1% /run/secrets/kubernetes.io/serviceaccount
tmpfs           475M     0  475M   0% /proc/acpitmpfs           475M     0  475M   0% /proc/scsi
tmpfs           475M     0  475M   0% /sys/firmware
```

# deploy_busy_box
- Create a Kubernetes Pod named "my-busybox" with the busybox:1.31.1 image.
- The pod should run a sleep command for 4800 seconds. 
- Verify that the Pod is running in Node01


```
# To make node unschedullable, we can use the command given below
kubectl cordon $NodeName

# To make node schedulable again, we can use the command given below
kubectl uncordon $NodeNAme
```

# network_policy1
- You have a kubernetes cluster that runs a three-tier web application:
- a front-end tier (port 80)
- an application tier (port 8080)
- a backend tier (port 3306)
- The security team has mandated that the backend tier should only be accessible from the application tier. 

```
fron-end -> application -> backend

```
# side_car_container
- You can find a pod named multi-pod is running in the cluster and that is logging to a volume.
- You need to insert a sidecar container into the pod that will also read the logs from the volume using this command
- "tail -f /var/busybox/log/*.log"
- image: busybox:1.28 name: sidecar volumepath: /var/busybox/log

-> This type of question will require get the yaml file and modify the file to deploy it 

```
k get pod
NAME        READY   STATUS    RESTARTS   AGE
multi-pod   2/2     Running   0          75s

k logs pod/multi-pod -c sidecar
```

# cronjob
- Create a cronjob for running every 2 minutes with busybox image. 
- The job name should be my-job and it should print the current date and time to the console.
- After running the job save any one of the pod logsto below path /root/logs.txt

# find_the_schedulable_nodes_in_the_cluster
- Find the schedulable nodes in the cluster and save the name and count in to the below file
- File path: /root/nodes.txt

```
 k get nodes -o jsonpath='{.items[*].spec.taints}'

# Expected output
[{"effect":"NoSchedule","key":"node-role.kubernetes.io/control-plane"}] [{"effect":"NoSchedule","key":"node.kubernetes.io/disk-pressure","timeAdded":"2024-11-10T18:05:22Z"}]

-> This implies controlplane is not schedulable 

cat /root/nodes.txt

# Expected output
ip-172-31-16-46
ip-172-31-23-161
```

# kubelet troubleshooting
- Check whether kubelet is running on the node
- Check the config file

# Joining a node

- Run the below command on a node 
- Make sure whether kubelet runs on the node

```
kubeadm token create --print-join-command

# Expected output
kubeadm join 172.31.20.7:6443 --token isiv82.97gsyns4tz2l8bho --discovery-token-ca-cert-hash sha256:3cfc59d8de87f071aa035e0a43831c4fcb0b
e8414a9ac6f3dfa49febdfc8f09d

ssh node01 
-> Then run the command
```

# roles_and_role_binding
- Create a new serviceaccount gitops in namespace project-1
- Create a role and rolebinding, both named gitops-role and gitops-rolebinding as well. 
- These should allow the new SA to only create Secrets and ConfigMaps in that Namespace.

-> Rolebindg will bind role and serviceaccount together so the serviceaccount can act based on the role

```
k apply -f role_rolebinding.yaml

# verification
k -n project-1 auth can-i create pod --as system:serviceaccount:project-1:gitops
no

k -n project-1 auth can-i create secret --as system:serviceaccount:project-1:gitops
yes

k -n project-1 auth can-i create configmaps --as system:serviceaccount:project-1:gitops
yes
```

# ingress

# kubernetes node upgrade