https://github.com/kubernetes-sigs/aws-alb-ingress-controller/issues/886



Attach the IAM policy to the EKS worker nodes
Attach the IAM policy to the EKS worker nodes:

Go back to the IAM Console.
Choose the section Roles and search for the NodeInstanceRole of our EKS worker node. Example: eksctl-attractive-gopher-NodeInstanceRole-xxxxxx.
Attach policy ingressController-iam-policy.

Policy 
https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/master/docs/examples/iam-policy.json



Support links

https://docs.aws.amazon.com/eks/latest/userguide/alb-ingress.html
https://aws.amazon.com/premiumsupport/knowledge-center/eks-alb-ingress-controller-setup/
https://rtfm.co.ua/en/aws-elastic-kubernetes-service-running-alb-ingress-controller/




```
The trick was not relying on merging between the Ingress objects. Yes, it can handle a certain degree of merging, but there's not really a one-to-one relationship between Services as TargetGroups and Ingress as ALB. So you have to be very cautious and aware of what's in each Ingress object.

Once I combined all of my ingress into a single object definition, I was able to get it working exactly as I wanted with the following YAML:

apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: svc-ingress
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/group.name: services
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/security-groups: sg-01234567898765432
    alb.ingress.kubernetes.io/ip-address-type: ipv4
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS": 443}]'
    alb.ingress.kubernetes.io/actions.ssl-redirect: '{"Type": "redirect", "RedirectConfig": { "Protocol": "HTTPS", "Port": "443", "StatusCode": "HTTP_301"}}'
    alb.ingress.kubernetes.io/actions.response-503: >
      {"type":"fixed-response","fixedResponseConfig":{"contentType":"text/plain","statusCode":"503","messageBody":"Unknown Host"}}
    alb.ingress.kubernetes.io/actions.svc-a-host: >
      {"type":"forward","forwardConfig":{"targetGroups":[{"serviceName":"svc-a-service","servicePort":80,"weight":100}]}}
    alb.ingress.kubernetes.io/conditions.svc-a-host: >
      [{"field":"host-header","hostHeaderConfig":{"values":["svc-a.example.com"]}}]
    alb.ingress.kubernetes.io/actions.svc-b-host: >
      {"type":"forward","forwardConfig":{"targetGroups":[{"serviceName":"svc-b-service","servicePort":80,"weight":100}]}}
    alb.ingress.kubernetes.io/conditions.svc-b-host: >
      [{"field":"host-header","hostHeaderConfig":{"values":["svc-b.example.com"]}}]
    alb.ingress.kubernetes.io/target-type: instance
    alb.ingress.kubernetes.io/load-balancer-attributes: routing.http2.enabled=true,idle_timeout.timeout_seconds=600
    alb.ingress.kubernetes.io/tags: Environment=Test
    alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:us-east-2:555555555555:certificate/33333333-2222-4444-AAAA-EEEEEEEEEEEE,arn:aws:acm:us-east-2:555555555555:certificate/44444444-3333-5555-BBBB-FFFFFFFFFFFF
    alb.ingress.kubernetes.io/ssl-policy: ELBSecurityPolicy-2016-08
spec:
  backend:
    serviceName: response-503
    servicePort: use-annotation
  rules:
    - http:
        paths:
          - backend:
              serviceName: ssl-redirect
              servicePort: use-annotation
          - backend:
              serviceName: svc-a-host
              servicePort: use-annotation
          - backend:
              serviceName: svc-b-host
              servicePort: use-annotation
Default Action:

Set by specifying the serviceName and servicePort directly under spec:

spec:
  backend:
    serviceName: response-503
    servicePort: use-annotation
Routing:

Because I'm using subdomains and paths won't work for me, I simply omitted the path and instead relied on hostname as a condition.

metadata:
  alb.ingress.kubernetes.io/actions.svc-a-host: >
      {"type":"forward","forwardConfig":{"targetGroups":[{"serviceName":"svc-a-service","servicePort":80,"weight":100}]}}
  alb.ingress.kubernetes.io/conditions.svc-a-host: >
      [{"field":"host-header","hostHeaderConfig":{"values":["svc-a.example.com"]}}]
End Result:

The ALB rules were configured precisely how I wanted them:

default action is a 503 fixed response
all http traffic is redirected to https
traffic is directed to TargetGroups based on the host header
```
