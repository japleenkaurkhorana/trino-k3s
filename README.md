# Trino with OPA and MinIO Setup

This repository contains Kubernetes manifests to set up a Trino cluster with OPA for authorization and MinIO for storage.

## Components

- **Trino**: SQL query engine
- **OPA**: Open Policy Agent for authorization policies
- **MinIO**: S3-compatible object storage

## Prerequisites

- Kubernetes cluster (Minikube, K3s, or Docker Desktop with Kubernetes)
- kubectl installed and configured
- WSL Ubuntu with VS Code

## Deployment Steps

1. **Create the namespace**:

```bash
kubectl apply -f k8s/namespace.yaml
```

2. **Deploy MinIO**:

```bash
kubectl apply -f k8s/minio-deployment.yaml
kubectl apply -f k8s/minio-service.yaml
```

3. **Deploy OPA**:

```bash
kubectl apply -f k8s/opa-policy.yaml
kubectl apply -f k8s/opa-deployment.yaml
kubectl apply -f k8s/opa-service.yaml
```

4. **Deploy Trino**:

```bash
kubectl apply -f k8s/trino-configmap-updated.yaml
kubectl apply -f k8s/trino-deployment.yaml
kubectl apply -f k8s/trino-service.yaml
```

5. **Initialize MinIO bucket**:

```bash
kubectl apply -f k8s/minio-init-job.yaml
```

## Accessing Trino

To access the Trino web UI, forward the port to your local machine:

```bash
kubectl port-forward -n trino-simplified svc/trino 8080:8080
```

Then visit http://localhost:8080 in your browser.

Use the following credentials to log in:
- Username: admin
- Password: password123

Or:
- Username: analyst
- Password: readme

## Accessing MinIO Console

To access the MinIO console:

```bash
kubectl port-forward -n trino-simplified svc/minio 9001:9001
```

Then visit http://localhost:9001 in your browser.

Use the following credentials:
- Username: x00jlqcW2EMuzsk9FVs4
- Password: LdtcxOrNitCcc0j2zuqCPk5Ncb6bGd4ZLlad1mjt

## Troubleshooting

If you encounter issues:

1. Check pod status:
```bash
kubectl get pods -n trino-simplified
```

2. Check logs:
```bash
kubectl logs -n trino-simplified deployment/trino
kubectl logs -n trino-simplified deployment/opa
kubectl logs -n trino-simplified deployment/minio
```

3. Check configmap:
```bash
kubectl describe configmap -n trino-simplified trino-config
```