## How to Add IAM User and IAM Role to AWS EKS Cluster?

Create developer user with below policy

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "eks:DescribeNodegroup",
                "eks:ListNodegroups",
                "eks:DescribeCluster",
                "eks:ListClusters",
                "eks:AccessKubernetesApi",
                "ssm:GetParameter",
                "eks:ListUpdates",
                "eks:ListFargateProfiles"
            ],
            "Resource": "*"
        }
    ]
}
```

1. Run the rbac.yaml 
2. Create aws developer porfile 
```
aws configure --profile developer
```
3. Get the list of configMaps
```
kubectl get -n kube-system configmap
```
4. Edit the aws-auth configmap
```
kubectl edit -n kube-system configmap/aws-auth
```
```
  mapUsers: |
      - userarn: arn:aws:iam::042448734048:user/developer
        username: developer
        groups:
         - reader

```
5. Update the kubeconfig
 aws eks --region us-east-1 update-kubeconfig --name <CLUSTER-NAME> --profile developer
  
6. To view the config
   kubectl config view --minify
  
7. To check the Developer Permission
kubectl auth can-i get pods
kubectl auth can-i create pods
kubectl get svc
kubectl run nginx --image=nginx
