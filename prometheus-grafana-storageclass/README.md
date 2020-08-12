You should only create the PV (not the PVC) beforehand. prometheus-operator can automatically create a PVC based on an existing PV.

For example, my default-values.yaml:

prometheus:
  prometheusSpec:
    storageSpec:
      volumeClaimTemplate:
        spec:
          # Name of the PV you created beforehand
          volumeName: MY-PREEXISTING-PV
          accessModes: ["ReadWriteOnce"]
          # StorageClass should match your existing PV's storage class
          storageClassName: gp2
          resources:
            requests:
              # Size below should match your existing PV's size
              storage: 500
              

volumeClaimTemplate:
    description: PersistentVolumeClaim is a user's request for and claim
     to a persistent volume
