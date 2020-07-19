1. Create namespace
kubectl create namespace wordpress

///
kubectl get storageclass
NAME      PROVISIONER           AGE
aws-efs   example.com/aws-efs   45m
[root@ip-172-31-63-86 bibin]# kubectl describe storageclass
Name:                  aws-efs
IsDefaultClass:        No
Annotations:           <none>
Provisioner:           example.com/aws-efs
Parameters:            <none>
AllowVolumeExpansion:  <unset>
MountOptions:          <none>
ReclaimPolicy:         Delete
VolumeBindingMode:     Immediate
Events:                <none>
///

///
kubectl get pvc -n wordpress
NAME            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
efs-mysql       Bound    pvc-e4060d3c-0a4b-4f88-ac2b-d89e16ab0640   10Gi       RWX            aws-efs        48m
efs-wordpress   Bound    pvc-7e297f10-a94e-4ce5-af35-cce51e0d7ce7   10Gi       RWX            aws-efs        48m
[root@ip-172-31-63-86 bibin]# kubectl describe pvc -n wordpress
Name:          efs-mysql
Namespace:     wordpress
StorageClass:  aws-efs
Status:        Bound
Volume:        pvc-e4060d3c-0a4b-4f88-ac2b-d89e16ab0640
Labels:        <none>
Annotations:   pv.kubernetes.io/bind-completed: yes
               pv.kubernetes.io/bound-by-controller: yes
               volume.beta.kubernetes.io/storage-class: aws-efs
               volume.beta.kubernetes.io/storage-provisioner: example.com/aws-efs
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      10Gi
Access Modes:  RWX
VolumeMode:    Filesystem
Mounted By:    wordpress-mysql-74c6577759-58v86
Events:
  Type    Reason                 Age   From                                                                                       Message
  ----    ------                 ----  ----                                                                                       -------
  Normal  ExternalProvisioning   48m   persistentvolume-controller                                                                waiting for a volume to be created, either by external provisioner "example.com/aws-efs" or manually created by system administrator
  Normal  Provisioning           48m   example.com/aws-efs_efs-provisioner-748c8ff5c8-wgcgz_47a8c99e-e336-465e-a22f-5fc329f2ee26  External provisioner is provisioning volume for claim "wordpress/efs-mysql"
  Normal  ProvisioningSucceeded  48m   example.com/aws-efs_efs-provisioner-748c8ff5c8-wgcgz_47a8c99e-e336-465e-a22f-5fc329f2ee26  Successfully provisioned volume pvc-e4060d3c-0a4b-4f88-ac2b-d89e16ab0640


Name:          efs-wordpress
Namespace:     wordpress
StorageClass:  aws-efs
Status:        Bound
Volume:        pvc-7e297f10-a94e-4ce5-af35-cce51e0d7ce7
Labels:        <none>
Annotations:   pv.kubernetes.io/bind-completed: yes
               pv.kubernetes.io/bound-by-controller: yes
               volume.beta.kubernetes.io/storage-class: aws-efs
               volume.beta.kubernetes.io/storage-provisioner: example.com/aws-efs
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      10Gi
Access Modes:  RWX
VolumeMode:    Filesystem
Mounted By:    wordpress-7969dbd88f-bssp8
Events:
  Type    Reason                 Age   From                                                                                       Message
  ----    ------                 ----  ----                                                                                       -------
  Normal  ExternalProvisioning   48m   persistentvolume-controller                                                                waiting for a volume to be created, either by external provisioner "example.com/aws-efs" or manually created by system administrator
  Normal  Provisioning           48m   example.com/aws-efs_efs-provisioner-748c8ff5c8-wgcgz_47a8c99e-e336-465e-a22f-5fc329f2ee26  External provisioner is provisioning volume for claim "wordpress/efs-wordpress"
  Normal  ProvisioningSucceeded  48m   example.com/aws-efs_efs-provisioner-748c8ff5c8-wgcgz_47a8c99e-e336-465e-a22f-5fc329f2ee26  Successfully provisioned volume pvc-7e297f10-a94e-4ce5-af35-cce51e0d7ce7
///


///
kubectl get pv -n wordpress
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                               STORAGECLASS   REASON   AGE
pvc-799112a2-3fc7-4247-9d81-77286fa9656c   10Gi       RWX            Delete           Bound    efs-wordpress/efs-wordpress-claim   aws-efs                 49m
pvc-7e297f10-a94e-4ce5-af35-cce51e0d7ce7   10Gi       RWX            Delete           Bound    wordpress/efs-wordpress             aws-efs                 49m
pvc-e4060d3c-0a4b-4f88-ac2b-d89e16ab0640   10Gi       RWX            Delete           Bound    wordpress/efs-mysql                 aws-efs                 49m
[root@ip-172-31-63-86 bibin]# kubectl describe pv -n wordpress
Name:            pvc-799112a2-3fc7-4247-9d81-77286fa9656c
Labels:          <none>
Annotations:     pv.beta.kubernetes.io/gid: 2002
                 pv.kubernetes.io/provisioned-by: example.com/aws-efs
Finalizers:      [kubernetes.io/pv-protection]
StorageClass:    aws-efs
Status:          Bound
Claim:           efs-wordpress/efs-wordpress-claim
Reclaim Policy:  Delete
Access Modes:    RWX
VolumeMode:      Filesystem
Capacity:        10Gi
Node Affinity:   <none>
Message:
Source:
    Type:      NFS (an NFS mount that lasts the lifetime of a pod)
    Server:    fs-a9407e2a.efs.us-east-1.amazonaws.com
    Path:      /efs-wordpress-claim-pvc-799112a2-3fc7-4247-9d81-77286fa9656c
    ReadOnly:  false
Events:        <none>


Name:            pvc-7e297f10-a94e-4ce5-af35-cce51e0d7ce7
Labels:          <none>
Annotations:     pv.beta.kubernetes.io/gid: 2000
                 pv.kubernetes.io/provisioned-by: example.com/aws-efs
Finalizers:      [kubernetes.io/pv-protection]
StorageClass:    aws-efs
Status:          Bound
Claim:           wordpress/efs-wordpress
Reclaim Policy:  Delete
Access Modes:    RWX
VolumeMode:      Filesystem
Capacity:        10Gi
Node Affinity:   <none>
Message:
Source:
    Type:      NFS (an NFS mount that lasts the lifetime of a pod)
    Server:    fs-a9407e2a.efs.us-east-1.amazonaws.com
    Path:      /efs-wordpress-pvc-7e297f10-a94e-4ce5-af35-cce51e0d7ce7
    ReadOnly:  false
Events:        <none>


Name:            pvc-e4060d3c-0a4b-4f88-ac2b-d89e16ab0640
Labels:          <none>
Annotations:     pv.beta.kubernetes.io/gid: 2001
                 pv.kubernetes.io/provisioned-by: example.com/aws-efs
Finalizers:      [kubernetes.io/pv-protection]
StorageClass:    aws-efs
Status:          Bound
Claim:           wordpress/efs-mysql
Reclaim Policy:  Delete
Access Modes:    RWX
VolumeMode:      Filesystem
Capacity:        10Gi
Node Affinity:   <none>
Message:
Source:
    Type:      NFS (an NFS mount that lasts the lifetime of a pod)
    Server:    fs-a9407e2a.efs.us-east-1.amazonaws.com
    Path:      /efs-mysql-pvc-e4060d3c-0a4b-4f88-ac2b-d89e16ab0640
    ReadOnly:  false
Events:        <none>
///


///
kubectl get all -n wordpress
NAME                                   READY   STATUS    RESTARTS   AGE
pod/efs-provisioner-748c8ff5c8-wgcgz   1/1     Running   0          52m
pod/wordpress-7969dbd88f-bssp8         1/1     Running   0          44m
pod/wordpress-mysql-74c6577759-58v86   1/1     Running   0          44m

NAME                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/wordpress         NodePort    10.100.224.35   <none>        80:32000/TCP   44m
service/wordpress-mysql   ClusterIP   None            <none>        3306/TCP       44m

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/efs-provisioner   1/1     1            1           52m
deployment.apps/wordpress         1/1     1            1           44m
deployment.apps/wordpress-mysql   1/1     1            1           44m

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/efs-provisioner-748c8ff5c8   1         1         1       52m
replicaset.apps/wordpress-7969dbd88f         1         1         1       44m
replicaset.apps/wordpress-mysql-74c6577759   1         1         1       44m
[root@ip-172-31-63-86 bibin]#
///
