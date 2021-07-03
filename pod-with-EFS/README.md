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
  storageClassName: efs-sc
  csi:
    driver: efs.csi.aws.com
    volumeHandle: identifier for our file system::identifier for our 
access point (i.e. fs-123b45fa::fsap-12345678910ab12cd34)
```


```
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: efs-csi
provisioner: efs.csi.aws.com

---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: efs-claim
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: efs-csi
  resources:
    requests:
      storage: 5Gi

```




```
Create an Amazon EFS file system
In this section, you create a security group, Amazon EFS file system, two mount targets, and an EFS Access Point. The access point allows the Amazon EKS cluster to use the EFS file system.

Capture your VPC ID
First, you must find the VPC ID that was created in the previous step using the following command:

aws ec2 describe-vpcs
Create a security group for your Amazon EFS mount target
Next, create a security group for the Amazon EFS mount target, which requires your VPC ID.

aws ec2 create-security-group \
--region us-east-1 \
--group-name efs-mount-sg \
--description "Amazon EFS for EKS, SG for mount target" \
--vpc-id identifier for our VPC (i.e. vpc-1234567ab12a345cd)  
Add rules to the security group to authorize inbound/outbound access
Authorize inbound access to the security group for the Amazon EFS mount target (efs-mount-sg) to allow inbound traffic to the NFS port (2049) from the VPC CIDR block using the following command:

aws ec2 authorize-security-group-ingress \
--group-id identifier for the security group created for our Amazon 
EFS mount targets (i.e. sg-1234567ab12a345cd) \
--region us-east-1 \
--protocol tcp \
--port 2049 \
--cidr 192.168.0.0/16
Create an Amazon EFS file system
Next, create an encrypted Amazon EFS file system using the following command and record the file system ID:

aws efs create-file-system \
--creation-token creation-token \
--performance-mode generalPurpose \
--throughput-mode bursting \
--region us-east-1 \
--tags Key=Name,Value=MyEFSFileSystem \
--encrypted
Capture your VPC subnet IDs
To create mount targets, you must have subnet IDs for my node group. Use the following command to find and record the subnets IDs:

aws ec2 describe-instances --filters Name=vpc-id,Values= identifier
for our VPC (i.e. vpc-1234567ab12a345cd) --query 
'Reservations[*].Instances[].SubnetId'

Create two Amazon EFS mount targets

Now that you have the security group, file system ID, and subnets, you can create mount targets in each of the Availability Zones using the following command:

aws efs create-mount-target \
--file-system-id identifier for our file system (i.e. fs-123b45fa) \
--subnet-id identifier for our node group subnets (i.e. subnet-
1234567ab12a345cd) \
--security-group identifier for the security group created for our 
Amazon EFS mount targets (i.e. sg-1234567ab12a345cd) \
--region us-east-1
Be sure to create a mount target in each of the two Availability Zones!

Create an Amazon EFS access point
Now that you have your file system, let’s create an Amazon EFS Access Point. Amazon EFS access points are application-specific entry points into an EFS file system that make it easier to manage application access to shared datasets or, in our case, configuration. Regardless of how a container is built, access points can enforce a user identity, including the user’s POSIX groups, for all file system requests that are made through them. For our purposes, let’s create a Jenkins-specific EFS access point and choose to enforce user ID and a group ID of 1000 using the following command:

aws efs create-access-point --file-system-id identifier for our file 
system (i.e. fs-123b45fa) \
--posix-user Uid=1000,Gid=1000 \
--root-directory 
"Path=/jenkins,CreationInfo={OwnerUid=1000,OwnerGid=1000,Permissions=777}"
Record the access point ID (that is, fsap-0123456abc987634a) for future reference.
```
