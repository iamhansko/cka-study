# Storage

## PV
Persistent Volume
- Access Mode
    - ReadOnlyMany
    - ReadWriteOnce
    - ReadWriteMany
- [persistentVolumeReclaimPolicy](https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/persistent-volume-v1/#PersistentVolumeSpec)
    - Retain = not deleted but unavailable
    - Delete = deleted
    - ~~Recycle~~


## PVC
Persistent Volume Claim

## PVC ❤️ PV
- Sufficient Capacity
- Access Mode
- Volume Mode
- Storage Class
- Selector

## Pod/Deployment/ReplicaSet ❤️ PVC
- [Reference](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes)

## Dynamic Provisioning
- StorageClass + provisioner

## Projected Volumes
- [Reference](https://kubernetes.io/docs/concepts/storage/projected-volumes/)