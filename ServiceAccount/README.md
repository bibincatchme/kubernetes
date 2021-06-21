## IAM Roles for Service Accounts & Pods: (IRSA EKS | IAM OIDC Provider | AWS EKS | Kubernetes)


### What is a Service Account? Using RBAC with Service Accounts in Kubernetes
#### In Kubernetes, service accounts are used to provide an identity for pods. Pods that want to interact with the Kubernetes API server will authenticate with a particular service account. By default, applications will authenticate as the default service account in the namespace they are running in. 
#### This means, for example, that an application running in the test namespace will use the default service account of the test namespace.

which can be used to:
1. Authenticate Pods to the Kubernetes API server, allowing the Pods to read and manipulate Kubernetes API objects (for example, a CI/CD pipeline that deploys applications to your cluster).
2. Authenticate Pods to Google Cloud resources through Workload Identity, allowing Pods to act as a Google service account. This allows you to give fine-grained identity and authorization to Pods when they need access to Google Cloud APIs.

##### In order for my application to query the Kubernetes API, it has to be authenticated. For that we use a service account. To create the service account run the command kubectl create
##### It also creates a token automatically the service account token is what must be used by the external application while authenticating to the Kubernetes API.

#### The token however is stored as a secret object.

#### k8s automatically mount the default  serviceaccount with each pods Each namespace have its own serviceaccount


```
kubectl logs -l app=nginx -c aws-cli						
kubectl logs -f nginx-deployment-57947467f8-vw2m4 -c aws-cli
kubectl get serviceaccount
```
