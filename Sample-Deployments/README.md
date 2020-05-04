
Create the deployemnt
#kubectl create -f deployment-definition.yml

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


Rollout for new versions
Deployemnt strategy 
1.Recreate
2.Rolling Update (by deafult)



Create
kubectl create -f deployment-definition.yml

Get detils
kubectl get deployments

Uddate
kubectl apply -f deployment-definition.yml
kubectl set image deployment/< pod_name > nginx=nginx:1.9.1
kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1

Status
kubectl set image deployment/< pod_name >
kubectl rollout status deployment/myapp-deployment
kubectl rollout history deployment/myapp-deployment

Rollback
kubectl rollout undo deployment/myapp-deployment
