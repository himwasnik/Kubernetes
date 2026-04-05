# RBAC (Role-Based Access Control)

This folder contains RBAC configuration for Kubernetes access control.

## Files
- `namespace.yml` - RBAC namespace
- `service-account.yml` - Service accounts
- `role.yml` - Role definitions
- `role-binding.yml` - Role bindings
- `depoyment.yml` - Example deployment using RBAC
- `note.txt` - Additional notes

## Usage
Apply RBAC configuration:
```bash
kubectl apply -f namespace.yml
kubectl apply -f service-account.yml
kubectl apply -f role.yml
kubectl apply -f role-binding.yml
kubectl apply -f depoyment.yml
```

## References
- [RBAC Documentation](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
