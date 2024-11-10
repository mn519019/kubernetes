# pod_in_control_plane.yaml
- Deploy a pod call nginxpod with image nginx in controlplane, make sure pod is not scheduled in worker node

```
k run nginxpod --image=nginx --dry-run=client -o yaml > pod_in_control_plane.yaml 

# Check the controlplnae add nodeName in "spec.nodeName[]" to assign the pod for a specific node 

```

# pod_as_a_service
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

