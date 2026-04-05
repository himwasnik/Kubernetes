# Kind Cluster Manifests

This folder contains Kubernetes manifests for testing with Kind (Kubernetes in Docker).

## Files
- `cron-job.yml` - CronJob manifest for scheduled tasks
- `deployment.yml` - Deployment manifest
- `jobs.yml` - Job manifest
- `namespace.yml` - Namespace configuration
- `pod.yml` - Pod manifest

## Usage
Apply manifests:
```bash
kubectl apply -f namespace.yml
kubectl apply -f deployment.yml
kubectl apply -f jobs.yml
kubectl apply -f cron-job.yml
kubectl apply -f pod.yml
```

Or apply all:
```bash
kubectl apply -f .
```

## References
- [Kind Documentation](https://kind.sigs.k8s.io/)
