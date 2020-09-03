
 a backup of the etcd cluster and save it to /tmp/etcd-backup.db
 
 ```
ETCDCTL_API=3 etcdctl --endpoints https://[127.0.0.1]:2379 
--cacert /etc/kubernetes/pki/etcd/ca.crt 
--cert /etc/kubernetes/pki/etcd/healthcheck-client.crt 
--key=/etc/kubernetes/pki/etcd/healthcheck-client.key snapshot save /tmp/etcd-backup.db
```

Create a new pod called super-user-pod with image busybox:1.28. Allow the pod to be able to set system_time
The container should sleep for 4800 seconds

```
apiVersion: v1
kind: Pod
metadata:
  name: super-user-pod
spec:
  containers:
  - image: busybox:1.28
    name: super-user-pod
    command: ["sleep","4800"]
    securityContext:
      capabilities:
        add: ["SYS_TIME"]
```        


A pod definition file is created at /root/use-pv.yaml. Make use of this manifest file and mount the persistent volume called pv-1. Ensure the pod is running and the PV is bound.
mountPath: /data persistentVolumeClaim Name: my-pvc
```
master $ cat pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 10Mi
master $ cat use-pv.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: use-pv
  name: use-pv
spec:
  containers:
  - image: nginx
    name: use-pv
    resources: {}
    volumeMounts:
    - mountPath: /data
      name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: my-pvc
```
        
        

Create pod

```
kubectl run nginx-resolver --image=nginx --generator=run-pod/v1        
```

Create CSR for the user

apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  name: john-developer
spec:
  request: $(cat server.csr | base64 | tr -d '\n')
  usages:
  - digital signature
  - key encipherment
  - server auth


csr approve 
kubectl certificate approve john-developer





Role
kubectl create role test --resource=pods --verb=create   --namespace=development

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: development
  name: developer
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["create", "list", "get", "update", "delete"]
```

Role Binding


kubectl create rolebinding dev-role-binding --role=developer --user=john --namespace=development

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: dev-role-binding
  namespace: development
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: developer
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: john
  ```



#Create pod
kubectl run nginx-resolver --image=nginx --generator=run-pod/v1

#Create service 
kubectl expose pod nginx-resolver --name=nginx-resolver-service  --port=80 --target-port=80 --type=ClusterIP

#verify svc
kubectl describe svc nginx-resolver-service
kubectl get pod nginx-resolver -o wide

#nslookup 

#It doesn't work,I think the image is not good.
kubectl run --generator=run-pod/v1 test-nslookup --image=busybox:1.28 --rm -it -- nslookup nginx-resolver-service

#method1 :copy the result for the logs,without server
kubectl logs test-nslookup



Cluster Role

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: pvviewer-role
rules:
- apiGroups:
  - ""
  resources:
  - persistentvolumes
  verbs:
  - list
```
Cluster Role Bindings

```
master $ cat clusterrolebinding.yaml
apiVersion: rbac.authorization.k8s.io/v1
# This cluster role binding allows anyone in the "manager" group to read secrets in any namespace.
kind: ClusterRoleBinding
metadata:
  name: pvviewer
subjects:
- kind: ServiceAccount
  name: pvviewer # Name is case sensitive
  namespace: default
roleRef:
  kind: ClusterRole
  name: pvviewer-role
  apiGroup: rbac.authorization.k8s.io
```

Multi Container Pod with command n env
```
apiVersion: v1
kind: Pod
metadata:
  name: multi-pod
spec:
  containers:
  - image: nginx
    name: alpha
    env:
    - name: name
      value: alpha
  - image: busybox
    name: beta
    command:
     - "sleep"
     - "4800"
    env:
    - name: name
      value: beta
 ```



Run as User
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: non-root-pod
  name: non-root-pod
spec:
  securityContext:
   runAsUser: 1000
   fsGroup: 2000
  containers:
  - image: redis:alpine
    name: non-root-pod
```

