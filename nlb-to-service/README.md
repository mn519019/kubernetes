## Requirements
- Create a NLB 
- Create a target group with required instances 
- Target group to use port 80 and forward a traffic to service port, in this case 80->31824

```
nginx-svc   NodePort       10.105.127.223   <none>                        80:31824/TCP   7m9s

```

## Kubernetes Implementation
- Create a pod with a dev label
- Create a nginx-srv to expose the port from 80 (Type: NodePort)
- Create a LB service with External IP (Type: LoadBalancer)

```
k get svc -n nginx-http
NAME        TYPE           CLUSTER-IP       EXTERNAL-IP                   PORT(S)        AGE
nginx-nlb   LoadBalancer   10.103.202.27    35.182.69.20                  80:32162/TCP   5m15s
nginx-svc   NodePort       10.105.127.223   <none>                        80:31824/TCP   7m9s

curl 35.182.69.20 

## Expected output 
dev version 1.0.0
```

## Limitation
- It is not suitable for a production usage
- Each service requires a Loadbalancer, so it can get really expensive
- If you have many services, it requires a lot of repetitive tasks