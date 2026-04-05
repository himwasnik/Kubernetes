# Resource Quotas and Limits

This folder contains Kubernetes resource quotas and limits configuration.

## Files
- `namespace.yml` - Namespace with resource quotas
- `deployment.yml` - Deployment with resource limits
- `persistancevolume.yml` - PersistentVolume definition
- `persistancevolumeclaim.yml` - PersistentVolumeClaim

## Usage
Apply resource quota configuration:
```bash
kubectl apply -f namespace.yml
kubectl apply -f persistancevolume.yml
kubectl apply -f persistancevolumeclaim.yml
kubectl apply -f deployment.yml
```

View quotas:
```bash
kubectl describe resourcequota -n resource-quotas
```

## References
- [Resource Quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/)
- [Limits and Requests](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)
