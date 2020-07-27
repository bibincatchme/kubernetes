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
