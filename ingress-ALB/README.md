https://github.com/kubernetes-sigs/aws-alb-ingress-controller/issues/886



Attach the IAM policy to the EKS worker nodes
Attach the IAM policy to the EKS worker nodes:

Go back to the IAM Console.
Choose the section Roles and search for the NodeInstanceRole of our EKS worker node. Example: eksctl-attractive-gopher-NodeInstanceRole-xxxxxx.
Attach policy ingressController-iam-policy.





https://docs.aws.amazon.com/eks/latest/userguide/alb-ingress.html
https://aws.amazon.com/premiumsupport/knowledge-center/eks-alb-ingress-controller-setup/
https://rtfm.co.ua/en/aws-elastic-kubernetes-service-running-alb-ingress-controller/
