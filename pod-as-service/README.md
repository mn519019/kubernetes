```
k apply -f nginx-pod.yaml
k apply -f nginx-service.yaml 

or 

k expose pod/nginx-pod -n nginx-http --type=NodePort --port=80 --target-port=80
```