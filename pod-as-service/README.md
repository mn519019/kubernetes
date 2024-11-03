```
k apply -f nginx-pod.yaml
k apply -f nginx-service.yaml 

or 

k expose pod/nginx-pod -n nginx-http --type=NodePort --port=80 --target-port=80


## Verification 1 (localhost)
k get svc -n nginx-http

NAME        TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
nginx-svc   NodePort   10.108.209.110   <none>        80:30713/TCP   17s

curl localhost:30713 

root@ip-172-31-20-7:~/rickyang# curl localhost:30713

This is a test Rick Yang Version 1.0.0


## Verification 2 (Node IP)
k get node -o wide | awk '{print $6}'
INTERNAL-IP
172.31.16.46
172.31.20.7
172.31.23.161

curl 172.31.16.46:30713; curl 172.31.20.7:30713; curl 172.31.23.161:30713;

This is a test Rick Yang Version 1.0.0
This is a test Rick Yang Version 1.0.0
This is a test Rick Yang Version 1.0.0
```