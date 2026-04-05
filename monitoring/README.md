# Production Monitoring Stack

Complete Kubernetes monitoring solution with Prometheus, Grafana, and Loki. This setup includes persistent storage, RBAC, network policies, and resource quotas.

## Architecture

- **Prometheus** (v2.45.2): Metrics collection and time-series database
- **Grafana** (v10.2.2): Visualization and dashboarding platform
- **Loki** (v2.9.3): Log aggregation and querying system
- **Promtail** (v2.9.3): Log collector agent for shipping logs to Loki
- **StatefulSet**: Ensures data persistence across pod restarts
- **DaemonSet**: Promtail runs on every node for log collection

## Features

✅ PersistentVolumes with 30-day retention  
✅ RBAC and ServiceAccounts configured  
✅ ResourceQuotas and LimitRanges for namespace isolation  
✅ NetworkPolicies for security  
✅ Health checks (liveness & readiness probes)  
✅ Resource requests and limits  
✅ Production-grade logging and monitoring  

## Files

- `namespace.yml` - Namespace with ResourceQuota, LimitRange, and NetworkPolicy
- `prometheus.yml` - Prometheus with persistent storage and comprehensive scraping
- `grafana.yml` - Grafana with datasources and admin credentials
- `loki.yml` - Loki StatefulSet with boltdb-shipper storage
- `promtail.yml` - Promtail DaemonSet for log collection from all pod nodes

## Prerequisites

```bash
# Kubernetes 1.19+
# Kind cluster with 4+ cores and 4GB+ RAM
# Docker/container runtime
```

## Installation

**Deploy entire monitoring stack:**
```bash
kubectl apply -f namespace.yml
kubectl apply -f prometheus.yml
kubectl apply -f grafana.yml
kubectl apply -f loki.yml
kubectl apply -f promtail.yml
```

**Or deploy all at once:**
```bash
kubectl apply -f .
```

## Verify Deployment

```bash
# Check namespace
kubectl get ns monitoring

# Check all resources
kubectl get all -n monitoring

# Check PersistentVolumes
kubectl get pvc -n monitoring

# Check pod status
kubectl get pods -n monitoring -w
```

## Access Services

### Port-Forward (Background Mode)

```bash
# Start all port-forwards in background
kubectl port-forward -n monitoring svc/prometheus 9090:9090 &
kubectl port-forward -n monitoring svc/grafana 3000:3000 &
kubectl port-forward -n monitoring svc/loki 3100:3100 &

# List background jobs
jobs -l

# Stop all port-forwards
pkill -f "kubectl port-forward"
```

### Service URLs

| Service | URL | Purpose |
|---------|-----|---------|
| Prometheus | http://localhost:9090 | Metrics UI & API |
| Grafana | http://localhost:3000 | Dashboards & Visualization |
| Loki | http://localhost:3100 | Logs API || Promtail | (no direct access) | Log collector (DaemonSet) |
## Credentials

**Grafana Admin:**
- **Username**: `admin`
- **Password**: `ChangeMeSecurePassword123`

**Retrieve from Secret:**
```bash
kubectl get secret grafana-admin -n monitoring -o jsonpath='{.data.admin-user}' | base64 -d
kubectl get secret grafana-admin -n monitoring -o jsonpath='{.data.admin-password}' | base64 -d
```

**Change Password:**
```bash
# Update secret with new password
NEW_PASS="YourNewPassword123"
kubectl patch secret grafana-admin -n monitoring \
  -p "{\"data\":{\"admin-password\":\"$(echo -n $NEW_PASS | base64)\"}}"
```

## Storage

### PersistentVolumes

| Component | Storage | Retention | Status |
|-----------|---------|-----------|--------|
| Prometheus | 20Gi | 30 days (TSDB) | Bound |
| Grafana | 5Gi | Dashboards & settings | Bound |
| Loki | 10Gi | 30 days (logs) | Bound |

**Check storage usage:**
```bash
kubectl exec -n monitoring prometheus-<pod-id> -- df -h /prometheus
kubectl exec -n monitoring loki-0 -- du -sh /loki
```

## Resource Quotas & Limits

**Namespace Quotas:**
- CPU: 2 cores (requests), 4 cores (limits)
- Memory: 2Gi (requests), 4Gi (limits)
- PVCs: up to 5
- Pods: up to 20

**Per-Container Defaults:**
- CPU request: 100m, limit: 500m
- Memory request: 128Mi, limit: 512Mi

**View quotas:**
```bash
kubectl describe resourcequota monitoring-quota -n monitoring
kubectl describe limitrange monitoring-limits -n monitoring
```

## Network Security

**NetworkPolicies enforced:**
- Ingress only from monitoring namespace and default namespace
- Grafana accessible only from prometheus and monitoring pods

**Verify:**
```bash
kubectl get networkpolicies -n monitoring
```

## Grafana Setup

### Connect Datasources

1. **Access Grafana**: http://localhost:3000
2. Login: `admin` / `ChangeMeSecurePassword123`
3. Datasources auto-provisioned:
   - Prometheus (http://prometheus:9090)
   - Loki (http://loki:3100)

### Import Dashboards

**Popular Dashboards:**
- Node Exporter: ID 1860
- Kubernetes Cluster: ID 7249
- Prometheus Stats: ID 3662

**Import step:**
```
Dashboard → New → Import → Enter Dashboard ID
```

## Prometheus Configuration

### Scrape Targets

- `prometheus` - Self-scraping (9090)
- `kubernetes-apiservers` - Kubernetes API servers (443)
- `kubernetes-nodes` - Node metrics (10250)
- `kubernetes-pods` - Pod metrics (auto-discovered)

### Custom Metrics

Add scrape config to ConfigMap and restart:
```bash
kubectl rollout restart deployment/prometheus -n monitoring
```

## Loki Configuration

### Log Retention

Default: 720 hours (30 days)  
Modify `retention_period` in `loki.yml` limits_config

### Query Logs

```bash
curl "http://localhost:3100/loki/api/v1/query_range?query={app=\"prometheus\"}&start=1000&end=2000"
```

## Promtail Configuration

### Overview

Promtail runs as a DaemonSet on every node and collects logs from:
- `/var/log/pods/` - Kubernetes pod logs
- All containers in all namespaces
- Applies relabel configs to extract pod, namespace, container metadata

### Log Labels

Promtail automatically adds labels:
- `namespace` - Pod namespace
- `pod` - Pod name
- `container` - Container name
- `node` - Node name
- `job` - namespace/pod_name combination
- `filename` - Log file path

### Check Promtail Status

```bash
# View Promtail pods
kubectl get pods -n monitoring -l app=promtail

# Check Promtail logs for errors
kubectl logs -n monitoring -l app=promtail --tail=50

# Verify Promtail is scraping pod logs
kubectl logs -n monitoring -l app=promtail | grep "tail routine: started"

# Check Loki for available labels
curl -s 'http://localhost:3100/loki/api/v1/labels'
```

### Query Logs in Grafana

1. Open Grafana: http://localhost:3000
2. Go to Explore → Select Loki datasource
3. Use Label browser to select:
   - `namespace` = "monitoring"
   - `pod` = "loki-0" (or any pod name)
4. Click "Run query" to see logs

### Troubleshooting Promtail

**Promtail pods not ready:**
```bash
kubectl describe pod <promtail-pod-name> -n monitoring
kubectl logs <promtail-pod-name> -n monitoring
```

**Logs not appearing in Loki:**
```bash
# Check if Promtail can reach Loki
kubectl exec -n monitoring <promtail-pod> -- curl -s http://loki.monitoring.svc.cluster.local:3100/ready

# Verify log paths are correct
kubectl exec -n monitoring <promtail-pod> -- ls -la /var/log/pods/
```

**DNS resolution errors:**
Promtail uses FQDN: `loki.monitoring.svc.cluster.local:3100`  
If logs show DNS errors, restart Promtail DaemonSet:
```bash
kubectl rollout restart daemonset/promtail -n monitoring
```

## Troubleshooting

### Check Pod Status

```bash
kubectl describe pod loki-0 -n monitoring
kubectl logs loki-0 -n monitoring --tail=50
```

### Database Connectivity

```bash
kubectl exec -n monitoring prometheus-<pod-id> -- curl -s grafana:3000/api/health
```

### Storage Issues

```bash
# Check PVC binding
kubectl get pvc -n monitoring

# Check disk usage
kubectl exec -n monitoring -it loki-0 -- du -sh /loki
```

## Production Best Practices

1. **Change default Grafana password** immediately
2. **Enable SSL/TLS** for external access
3. **Configure persistent backups** for critical data
4. **Set up alerting rules** in Prometheus
5. **Enable authentication** for all components
6. **Monitor the monitoring stack** itself
7. **Scale based on metrics volume** - adjust PVC sizes
8. **Enable audit logging** for compliance
9. **High Availability**: Deploy multiple replicas for HA
10. **Regular backups**: Schedule daily snapshots

## References

- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)
- [Loki Documentation](https://grafana.com/docs/loki/latest/)
- [Kubernetes Monitoring](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-metrics-pipeline/)
