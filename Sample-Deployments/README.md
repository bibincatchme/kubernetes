Only the chnages in apiVersion and Kind with replicaSet
  
apiVersion: apps/v1
kind: Deployment


Under the Spec it have
replicas:
template:
selector

Create the deployemnt
#kubectl create -f deployment-definition.yml
kubectl create -f deployment-definition.yml --record (new flag record, for see deployment change cause)


Get all deployments
#kubectl get deployments

Get the replicaset detail
#kubectl get replicaset

Get all the pods details
#kubectl get pods

List everything
#kubectl get all

Describe
#kubectl describe deployment

Delete
kubectl delete deployment myapp-deployment


Rollout for new versions, When a rollout is created a new revission is created.
Deployemnt strategy 
1.Recreate
2.Rolling Update (by deafult)



Create
kubectl create -f deployment-definition.yml

Get detils
kubectl get deployments

Update
kubectl apply -f deployment-definition.yml
kubectl set image deployment/< pod_name > nginx=nginx:1.9.1
kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1
kubectl set image deployment/replica-first nginx-containers1=nginx:1.12

Status
kubectl rollout status deployment/myapp-deployment
kubectl rollout history deployment/myapp-deployment

Rollback
kubectl rollout undo deployment/myapp-deployment
