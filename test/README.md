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