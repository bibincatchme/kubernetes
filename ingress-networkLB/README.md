We can verify that by
kubectl get svc -n nginx-ingress
Using above you will get a Public DNS address of Loadbalancer. Let’s Create an Ingress Resource in any namespace.



https://aws.amazon.com/blogs/opensource/network-load-balancer-nginx-ingress-controller-eks/



Step 3: Adding Nginx Ingress Controller
Ingress in Kubernetes cluster is an object that allows Kubernetes services to be accessible from outside the cluster. There can be host-based as well as path-based routing rule that defines which inbound connection will reach which resource. Thus, all the routing rules can be consolidated within one resource in a cluster.
An Ingress Controller in Kubernetes cluster can be deployed as a Deployment Object or DeamonSet. Here we will be deploying it as Deployment and exposing it by service of type “LoadBalancer”.
An Ingress controller does not typically eliminate the need for an external load balancer, it simply adds an additional layer of routing and control behind the load balancer.



Create the Ingress controller deployment.
kubectl create -f https://github.com/A-kanksh-a/MONGO_AWS/blob/master/Deployment_files/IngressController.yaml
The above will create an Ingress Controller Deployment in “nginx-ingress” namespace. We can verify that by
kubectl get deployment -n inginx-ingress



Create an NLB service for the above deployment which will create a Network Load Balancer in AWS.
kubectl create -f https://github.com/A-kanksh-a/MONGO_AWS/blob/master/Deployment_files/IngressNLBService.yaml
We can verify that by
kubectl get svc -n nginx-ingress
Using above you will get a Public DNS address of Loadbalancer. Let’s Create an Ingress Resource in any namespace.
At this stage, you should have a domain name of your own. Here, I have a domain name “akankshakumari.xyz” from namecheap.com
Create a self-signed certificate to use SSL.
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout
tls.key -out tls.crt -subj “/CN=akankshakumari.xyz/O=akankshakumari.xyz”
Create a secret resource with the self-signed certificate:
kubectl create secret tls tls-backend — key tls.key — cert tls.crt
Create an Ingress Resource
kubectl create -f https://github.com/A-kanksh-a/MONGO_AWS/blob/master/Deployment_files/Ingress.yaml
Create the required resources behind Ingress.
kubectl create -f https://github.com/A-kanksh-a/MONGO_AWS/blob/master/Deployment_files/mongo-service.yaml
kubectl create -f https://github.com/A-kanksh-a/MONGO_AWS/blob/master/Deployment_files/mongo-deployment.yaml
kubectl create -f https://github.com/A-kanksh-a/MONGO_AWS/blob/master/Deployment_files/timesheet-service.yaml
kubectl create -f https://github.com/A-kanksh-a/MONGO_AWS/blob/master/Deployment_files/timesheet-deployment.yaml
