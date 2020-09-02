
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
