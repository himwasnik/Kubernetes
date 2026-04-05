# Storage

This folder contains Kubernetes storage configuration including PersistentVolumes and PersistentVolumeClaims.

## Files
- `namespace.yml` - Storage namespace
- `persistancevolume.yml` - PersistentVolume definition
- `persistancevolumeclaim.yml` - PersistentVolumeClaim
- `deployment_storage.yml` - Deployment using persistent storage
- `service.yml` - Service configuration

## Usage
Deploy storage configuration:
```bash
kubectl apply -f namespace.yml
kubectl apply -f persistancevolume.yml
kubectl apply -f persistancevolumeclaim.yml
kubectl apply -f deployment_storage.yml
kubectl apply -f service.yml
```

View storage resources:
```bash
kubectl get pv
kubectl get pvc -n storage
```

## References
- [PersistentVolumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
- [Storage Classes](https://kubernetes.io/docs/concepts/storage/storage-classes/)
