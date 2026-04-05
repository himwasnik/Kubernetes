# Helm Charts

This folder contains Helm chart configurations for Kubernetes deployments.

## Files
- `REDME.md` - Helm documentation
- `apache-helm/` - Apache web server Helm chart

## Apache Helm Chart
Contains templates for deploying Apache with Kubernetes resources.

### Chart Structure
```
apache-helm/
├── Chart.yaml              # Chart metadata
├── values.yaml             # Default configuration values
└── templates/
    ├── _helpers.tpl        # Template helpers
    ├── deployment.yaml     # Deployment manifest
    ├── hpa.yaml           # Horizontal Pod Autoscaler
    ├── ingress.yaml       # Ingress configuration
    ├── NOTES.txt          # Post-install notes
    ├── service.yaml       # Service manifest
    ├── serviceaccount.yaml # Service account
    └── tests/
        └── test-connection.yaml
```

## Usage
Deploy the chart:
```bash
helm install my-apache ./apache-helm
```

Upgrade:
```bash
helm upgrade my-apache ./apache-helm
```

Remove:
```bash
helm uninstall my-apache
```

## References
- [Helm Documentation](https://helm.sh/docs/)
