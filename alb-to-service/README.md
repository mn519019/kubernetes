```
k get svc -n nginx-http
NAME        TYPE           CLUSTER-IP       EXTERNAL-IP                   PORT(S)        AGE
nginx-alb   LoadBalancer   10.99.124.211    52.60.199.226,15.156.229.30   80:32228/TCP   12h
nginx-svc   NodePort       10.105.127.223   <none>                        80:31824/TCP   11h
```

# Why Not NodePort 32228?
The port 32228 is the NodePort assigned by Kubernetes for the nginx-alb service. However, when using an ALB, the traffic is routed through the ALB’s external IP on the standard HTTP port 80, which then forwards the traffic to the appropriate target group and port within your cluster.