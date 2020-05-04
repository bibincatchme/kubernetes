kubectl create -f service-nodeport.yml  

kubectl get services

 kubectl create -f deployemnts.yml
 
kubectl get pods
kubectl get all



Kubernetes service is an object just like pods
Use case is to listen to a port on the node and forward request on the node port to a port on the pod.
clinet end -port >> node -port >> pod -port 

Running the web appplication this type of service is known as a node port service
because the service listen to port on the node and forward request to the pods.

Service Type:
1.NodePort
2.ClusterIP
3.LoadBalancer

NodePort:
Port on the pod > TargetPort 80
Port on the Service > Port 80
Port on the Node > NodePort 30000 to 32767



kubectl create -f
kubectl get services





apiVersion: v1
kind: Service
metadata:
  name: myapp-service

spec:
  type: NodePort
  ports:
   - targetPort: 80
     port: 80
     nodePort: 30008
  selector:
      teir: frontend
