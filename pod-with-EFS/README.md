 kubectl exec -it efs-app -- bash -c "cat /efs-data/out.txt"
 
 
 
 ##### Mount the EFS on a machine and create the folder efsbibin-test
 
 ```
 apiVersion: v1
kind: PersistentVolume
metadata:
  name: efs-pv
spec:
  capacity:
    storage: 5Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  storageClassName: efs-csi
  csi:
    driver: efs.csi.aws.com
    volumeHandle: fs-0a6606be
    volumeAttributes:
      path: "/efsbibin-test"

 ```
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: efs-test
spec:
  storageClassName: efs
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Delete
  mountOptions:
    - tls
  csi:
    driver: efs.csi.aws.com
    volumeHandle: fs-xxx
    volumeAttributes:
      path: "/efs-test"
```
