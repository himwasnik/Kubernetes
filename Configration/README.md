# Configuration

This folder contains Kubernetes configuration and setup files.

## Files
- `dashboard-admin-user.yml` - Kubernetes Dashboard admin user and RBAC bindings
- `kube-config.yml` - Kind cluster configuration with control-plane and worker nodes

## Usage
Create a Kind cluster:
```bash
kind create cluster --config kube-config.yml --name cluster
```

Apply dashboard admin configuration:
```bash
kubectl apply -f dashboard-admin-user.yml
```
