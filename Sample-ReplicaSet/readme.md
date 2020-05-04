Important point 
can't use the replicaSet selector >> matcheLables >> for an another definition file lables, it will conflict 

Main Terms:
spec:
  replicas: 6

  template:
    metadata:
      name: myfist-app
      labels:
        teir: frontend

    spec:
      containers:
        - name: nginx-containers1
          image: nginx

  selector:
    matchLabels:
      teir: frontend


create replicaset pods
kubectl create -f replicatset-definition.yml

Get the replicaset details"
kubectl get replicaset

Get details of your replicaset:
kubectl describe replicaset

Get the pods details:
kubectl get pods

Delete one pod from replicaset for testing it:
kubectl delete pod replica-first-br97t
 
 More information about pods :
 kubectl get pods -o wide
 
 Increase the replicaset and reploy :
 kubectl replace -f replicaset-definition.yml
  
 Increase the scale of replica manually via command line :
 kubectl scale --replicas=3 -f replicaset-definition.yml
  
 Delete the replica set
 kubectl delete replicaset replica-first
   
   
 kubectl get all
