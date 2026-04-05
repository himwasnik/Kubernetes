# Argo-cd

This folder contains Argo CD configuration files for GitOps-based continuous deployment.

## Files
- `argo-application.yaml` - Application manifest for Argo CD
- `argo-cd.yml` - Argo CD server configuration

## Usage
Install Argo CD and apply the configurations:
```bash
kubectl apply -f argo-cd.yml
kubectl apply -f argo-application.yaml
```

## References
- [Argo CD Documentation](https://argo-cd.readthedocs.io/)
