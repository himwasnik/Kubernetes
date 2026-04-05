# Scaling - HPA & VPA

This folder contains configurations for horizontal and vertical pod autoscaling.

## Files
- `namespace.yml` - Namespace for scaling tests
- `deployment.yml` - Application deployment
- `service.yml` - Service configuration
- `HPC.yml` - Horizontal Pod Autoscaler configuration
- `VPC.yml` - Vertical Pod Autoscaler configuration
- `stress-pod.yml` - Load testing pod
- `note` - Additional notes

## HPA (Horizontal Pod Autoscaler)
Automatically scales the number of pods based on CPU/memory metrics.

## VPA (Vertical Pod Autoscaler)
Automatically adjusts resource requests and limits.

## Usage
Deploy scaling configuration:
```bash
kubectl apply -f namespace.yml
kubectl apply -f deployment.yml
kubectl apply -f service.yml
kubectl apply -f HPC.yml
kubectl apply -f VPC.yml
```

Monitor scaling:
```bash
kubectl get hpa -n scaling
kubectl get vpa -n scaling
```

## Load Testing
```bash
kubectl apply -f stress-pod.yml
```

## References
- [HPA Documentation](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)
- [VPA Documentation](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)
