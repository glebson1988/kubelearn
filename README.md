# kubelearn

Learning repository for Kubernetes. All materials and manifests here were created for educational purposes, and the local cluster used was minikube.

## Topics covered
- Gateway API: `GatewayClass`, `Gateway`, `HTTPRoute` for routing traffic to web and api.
- Services: `ClusterIP` for `api` and `web`, plus a dedicated `crawler` service in the `crawler` namespace.
- Deployments: `synergychat-web`, `synergychat-api`, `synergychat-crawler` (multiple containers in one Pod).
- ConfigMaps: environment configuration for `web`, `api`, `crawler`, and the `testcpu/testram` test workloads.
- Volumes and storage: `PersistentVolumeClaim` for API and `emptyDir` cache for crawler.
- HPA: CPU-based autoscaling for `web` and `testcpu`.
- Resource limits: CPU limit for `testcpu` and memory limit for `testram`.
- In-cluster communication: API uses the `crawler` service DNS name.

## Files
- Gateway API: `app-gatewayclass.yaml`, `app-gateway.yaml`, `api-httproute.yaml`, `web-httproute.yaml`
- Web: `web-deployment.yaml`, `web-service.yaml`, `web-configmap.yaml`, `web-hpa.yaml`
- API: `api-deployment.yaml`, `api-service.yaml`, `api-configmap.yaml`, `api-pvc.yaml`
- Crawler: `crawler-deployment.yaml`, `crawler-service.yaml`, `crawler-configmap.yaml`
- Resource tests: `testcpu-deployment.yaml`, `testcpu-hpa.yaml`, `testram-deployment.yaml`, `testram-configmap.yaml`

## How to run (minikube)
Start the local cluster and ensure the context points to minikube:
```bash
minikube start
kubectl config use-context minikube
```

Apply the base resources:
```bash
kubectl apply -f app-gatewayclass.yaml
kubectl apply -f app-gateway.yaml
kubectl apply -f api-configmap.yaml -f api-pvc.yaml -f api-deployment.yaml -f api-service.yaml
kubectl apply -f web-configmap.yaml -f web-deployment.yaml -f web-service.yaml
kubectl apply -f crawler-configmap.yaml -f crawler-deployment.yaml -f crawler-service.yaml
```

Apply Gateway HTTPRoutes:
```bash
kubectl apply -f api-httproute.yaml
kubectl apply -f web-httproute.yaml
```

Optional tests and autoscaling:
```bash
kubectl apply -f testcpu-deployment.yaml -f testcpu-hpa.yaml
kubectl apply -f testram-configmap.yaml -f testram-deployment.yaml
kubectl apply -f web-hpa.yaml
```
