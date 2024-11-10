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
