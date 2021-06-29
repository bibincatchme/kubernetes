1. Create AmazonEKSAdminPolicy with below json
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "eks:*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "eks.amazonaws.com"
                }
            }
        }
    ]
}
```

2. Create a Role EKSAdminRole with the abobe Policy AmazonEKSAdminPolicy 
(Role -> Another AWS Account (Cross Account) enter the account ID)

3. Get the role details from AWS CLI
```
aws iam get-role --role-name EKSAdminRole
```

Principle - who will able to assume the role
Action AsuumeRole
All the users in the account will be assume the role, Still need to establish the trust between the user and role



4. Establish the trust between the user and role

create policy called AmazonEKSAssumePolicy
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::04244873:role/EKSAdminRole"
        }
    ]
}
```
Admin access from k8s Cluster and Amazon CLI access

5. Create an IAM User called manager and attach the AmazonEKSAssumePolicy

6. Configure the AWS configure profile 
```
aws configure --profile manager
```
7. Verify the assume the role with user manager
```
aws sts assume-role --role-arn arn:aws:iam::04244873:role/EKSAdminRole
 --role-session-name manager-session --profile manager
 ```
 
 
8. Update the configmap For the assume role
```
kubectl edit -n kube-system configmap/aws-auth
kubectl -n kube-system edit configmap aws-auth
```
```
apiVersion: v1
data:
  mapRoles:
    - rolearn: arn:aws:iam::0424487:role/EKSAdminRole
      username: EKSAdminRole
      groups:
      - system:masters
```      
9. Create additional profile for that role

```
vi ~/.aws/config

[default]
region = us-east-1
[profile developer]
region = us-east-1
[profile manager]
region = us-east-1
[profile eks-admin]
role_arn = arn:aws:iam::0424487:role/EKSAdminRole
source_profile = manager
```

10. Update the kubectl config with profile eks-admin 
```
aws eks --region us-east-1 update-kubeconfig --name eksdemo1 --profile eks-admin
```

11. verifty the configuration
```
kubectl config view --minify
```

12. 
```
kubectl auth can-i "*" "*"
Argument 1 : actions create list
Argument 2: resousrces: configmap, deployment.
```

Trust Relationship
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::111111111:root"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}

```
kubectl describe configmap -n kube-system aws-auth
kubectl -n kube-system get configmap aws-auth -o yaml
curl -o aws-auth-cm.yaml https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/aws-auth-cm.yaml



https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html
```


13. Update kubeconfig

```
aws eks update-kubeconfig \
    --region us-east-1 \
    --name eksdemo1 \
    --role-arn arn:aws:iam::04244873:user/developer
```	
