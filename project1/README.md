# Project 1 Deployment

This folder contains Kubernetes manifests for Project 1 application.

## Files
- `namespace.yml` - Project namespace
- `deployment.yml` - Application deployment
- `service.yml` - Kubernetes Service
- `ingress.yml` - Ingress configuration for external access

## Usage
Deploy application:
```bash
kubectl apply -f namespace.yml
kubectl apply -f deployment.yml
kubectl apply -f service.yml
kubectl apply -f ingress.yml
```

## References
- [Kubernetes Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
