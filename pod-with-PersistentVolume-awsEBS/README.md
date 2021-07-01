#### NOTE: the EBS volume integration with Kubernetes PV will only work on one node at a time. This means that two nodes cannot mount the same EBS volume at once. Thus, when making deployments using PVs that are backed by EBS, be sure to properly allocate the pods being located on the instance that has the volume attached to it.


```
This defines the volume type being used, in this case the awsElasticBlockStore plug-in.
  awsElasticBlockStore: 
    fsType: "ext4" 
    volumeID: "vol-f37a03aa"
```    



```
Walkthrough
Create the AWS Elastic Block Store (EBS) volume in the same region as your cluster. If you have the aws cli installed and configured, this command will create one for you:
#### aws ec2 create-volume --availability-zone=eu-west-1a --size=10 --volume-type=gp2

With this new volume, attach it onto the master node in your cluster. If you have the aws cli installed and configured, this command will perform this for you:
### aws ec2 attach-volume --device /dev/xvdf --instance-id <MASTER NODE ID> --volume-id <YOUR VOLUME ID>
In the master node, check to see if your device is attached to your instance by running lsblk. If the last step worked, you should see your volume at the bottom of the list. In this case, the volume I made earlier is called nvme1n1.
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0         7:0    0 17.9M  1 loop /snap/amazon-ssm-agent/1068
loop1         7:1    0 89.3M  1 loop /snap/core/6673
nvme0n1     259:0    0   25G  0 disk
└─nvme0n1p1 259:1    0   25G  0 part /
nvme1n1     259:2    0   10G  0 disk
With the name of the volume, create the filesystem on the volume. This only needs to be done once on the volume.

### sudo mkfs -t xfs /dev/<NAME OF VOLUME FROM PREV STEP>

```
