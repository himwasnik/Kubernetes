# Monitoring Stack

This folder contains monitoring and logging stack configurations: Prometheus, Grafana, and Loki.

## Files
- `namespace.yml` - Monitoring namespace
- `prometheus.yml` - Prometheus monitoring with ConfigMap, Deployment, Service, ServiceAccount and RBAC
- `grafana.yml` - Grafana visualization with datasources
- `loki.yml` - Loki log aggregation with ConfigMap and Deployment

## Architecture
- **Prometheus**: Metrics collection and storage
- **Grafana**: Visualization and dashboarding
- **Loki**: Log aggregation and querying

## Usage
Deploy monitoring stack:
```bash
kubectl apply -f namespace.yml
kubectl apply -f prometheus.yml
kubectl apply -f grafana.yml
kubectl apply -f loki.yml
```

Or deploy all at once:
```bash
kubectl apply -f .
```

## Access Services

**Prometheus:**
```bash
kubectl port-forward -n monitoring svc/prometheus 9090:9090
# Visit http://localhost:9090
```

**Grafana:**
```bash
kubectl port-forward -n monitoring svc/grafana 3000:3000
# Visit http://localhost:3000
# Default credentials: admin / admin123
```

**Loki:**
```bash
kubectl port-forward -n monitoring svc/loki 3100:3100
# Loki API available at http://localhost:3100
```

## Configuration
- Prometheus scrapes Kubernetes API and node metrics
- Grafana auto-discovers Prometheus as datasource
- Loki stores logs with BoltDB shipper
- All use emptyDir volumes (data lost on pod restart - use PersistentVolumes for production)

## References
- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)
- [Loki Documentation](https://grafana.com/docs/loki/latest/)
