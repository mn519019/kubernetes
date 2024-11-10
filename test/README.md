# pod_in_control_plane.yaml
- Deploy a pod call nginxpod with image nginx in controlplane, make sure pod is not scheduled in worker node

```
k run nginxpod --image=nginx --dry-run=client -o yaml > pod_in_control_plane.yaml 

# Check the controlplnae add nodeName in "spec.nodeName[]" to assign the pod for a specific node 

```

