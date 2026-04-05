# MySQL Database

This folder contains MySQL database deployment configuration for Kubernetes.

## Files
- `config.yml` - MySQL configuration ConfigMap
- `secret.yml` - MySQL secrets (credentials)
- `service.yml` - Kubernetes Service for MySQL
- `statefullset.yml` - StatefulSet for MySQL deployment

## Usage
Deploy MySQL:
```bash
kubectl apply -f secret.yml
kubectl apply -f config.yml
kubectl apply -f statefullset.yml
kubectl apply -f service.yml
```

Connect to MySQL:
```bash
kubectl port-forward -l app=mysql 3306:3306
mysql -h 127.0.0.1 -u root -p
```

## Notes
- StatefulSet ensures data persistence
- Secrets store sensitive credentials
- ConfigMap holds configuration files

## References
- [MySQL on Kubernetes](https://kubernetes.io/docs/tasks/run-application/run-replicated-stateful-application/)
