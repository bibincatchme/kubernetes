



Add a Secret generator in kustomization.yaml from the following command. You will need to replace YOUR_PASSWORD with the password you want to use.
```
cat <<EOF >./kustomization.yaml
secretGenerator:
- name: mysql-pass
  literals:
  - password=YOUR_PASSWORD
EOF
```

Add wordpress-deployment.yaml and mysql-deployment.yaml to kustomization.yaml

```
cat <<EOF >>./kustomization.yaml
resources:
  - mysql-deployment.yaml
  - wordpress-deployment.yaml
EOF
```


### Apply and Verify
The kustomization.yaml contains all the resources for deploying a WordPress site and a MySQL database. You can apply the directory by
```
kubectl apply -k ./
```

Verify that the Service is running by running the following command:
```
kubectl get services wordpress
```


Run the following command to get the IP Address for the WordPress Service:
---
```
minikube service wordpress --url
```



### PersistentVolumeClaim use the default storageclass 
```
 kubectl get sc
NAME            PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
gp2 (default)   kubernetes.io/aws-ebs   Delete          WaitForFirstConsumer   false                  2d3h

```

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: wp-pv-claim
  labels:
    app: wordpress
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
```      
