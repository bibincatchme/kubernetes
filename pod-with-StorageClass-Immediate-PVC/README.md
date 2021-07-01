##Volume Binding Mode
```
The volumeBindingMode field controls when volume binding and dynamic provisioning should occur. When unset, "Immediate" mode is used by default.

The Immediate mode indicates that volume binding and dynamic provisioning occurs once the PersistentVolumeClaim is created. For storage backends that are topology-constrained and not globally accessible from all Nodes in the cluster, PersistentVolumes will be bound or provisioned without knowledge of the Pod's scheduling requirements. This may result in unschedulable Pods.

A cluster administrator can address this issue by specifying the WaitForFirstConsumer mode which will delay the binding and provisioning of a PersistentVolume until a Pod using the PersistentVolumeClaim is created. PersistentVolumes will be selected or provisioned conforming to the topology that is specified by the Pod's scheduling constraints. These include, but are not limited to, resource requirements, node selectors, pod affinity and anti-affinity, and taints and tolerations.

The following plugins support WaitForFirstConsumer with dynamic provisioning:

AWSElasticBlockStore
GCEPersistentDisk
AzureDisk
The following plugins support WaitForFirstConsumer with pre-created PersistentVolume binding:

All of the above
Local

``
