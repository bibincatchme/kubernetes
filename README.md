Commands

```
Create simple application
kubectl run nginx  --replicas=2 --labels='app=nginx' --image=nginx --port=80

To verify that your pods are running and have their own internal IP addresses,
kubectl get pods -l 'app=nginx' -o wide | awk {'print $1" " $3 " " $6'} | column -t


node’s public IP address.
kubectl get nodes -o wide |  awk {'print $1" " $2 " " $7'} | column -t

loadbalacner details
kubectl get service/nginx-service |  awk {'print $1" " $2 " " $4 " " $5'} | column -t

curl -silent *****.eu-west-1.elb.amazonaws.com:80 | grep title

spin up a new pod, for example with Debian, and install curl:
kubectl run --rm -ti debug --image=debian --restart=Never bash

root@debug:/# apt update && apt -y install curl

---

kubectl exec redis cat /redis-master/redis.conf

kubectl exec -it <pod> -- /bin/sh

Change default namespace to monitoring
kubectl config set-context --current --namespace=monitoring
```


Test the scale out of the EKS worker nodes

1.    To see the current number of worker nodes, run the following command:

kubectl get nodes
2.    To increase the number of worker nodes, run the following commands:

kubectl create deployment autoscaler-demo --image=nginx
kubectl scale deployment autoscaler-demo --replicas=50
Note: This command creates a deployment named autoscaler-demo using an NGINX image directly on the Kubernetes cluster, and then launches 50 pods.

3.    To check the status of your deployment and see the number of pods increasing, run the following command:

kubectl get deployment autoscaler-demo --watch
4.    When the number of available pods equals 50, check the number of worker nodes by running the following command:

kubectl get nodes
