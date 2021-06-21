## IAM Roles for Service Accounts & Pods: (IRSA EKS | IAM OIDC Provider | AWS EKS | Kubernetes)

###What is a Service Account?
####In Kubernetes, service accounts are used to provide an identity for pods. Pods that want to interact with the API server will authenticate with a particular service account. By default, applications will authenticate as the default service account in the namespace they are running in. 


```
kubectl logs -l app=nginx -c aws-cli						
kubectl logs -f nginx-deployment-57947467f8-vw2m4 -c aws-cli
kubectl get serviceaccount
```
