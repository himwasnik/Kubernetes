# Custom Resources

This folder contains Custom Resource Definitions (CRD) and custom resources.

## Files
- `Devops-crd.yml` - DevOps Custom Resource Definition
- `devops.yml` - Custom resource instance

## Usage
Apply the CRD first, then the custom resource:
```bash
kubectl apply -f Devops-crd.yml
kubectl apply -f devops.yml
```

## References
- [Kubernetes CRD Documentation](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/)
