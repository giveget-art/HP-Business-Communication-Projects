# Kubernetes Deployment Guide

## Overview
Complete Kubernetes manifests for HP Communication Android application using `apps/v1` Deployment API.

## Prerequisites
- Kubernetes 1.20+
- kubectl configured
- Docker images in registry

## Deployment Details

### App Module
- **API Version**: apps/v1
- **Kind**: Deployment
- **Replicas**: 3
- **CPU**: 250m requests / 1000m limits
- **Memory**: 512Mi requests / 2Gi limits
- **Port**: 8080 (HTTP)

### UI Module
- **API Version**: apps/v1
- **Kind**: Deployment
- **Replicas**: 2
- **CPU**: 100m requests / 500m limits
- **Memory**: 256Mi requests / 1Gi limits
- **Port**: 3000 (HTTP)

## Quick Start

### 1. Create Namespace
```bash
kubectl apply -f kubernetes/namespace.yaml
```

### 2. Create Config & Secrets
```bash
kubectl apply -f kubernetes/configmap.yaml
kubectl apply -f kubernetes/secrets.yaml
```

### 3. Create RBAC
```bash
kubectl apply -f kubernetes/rbac.yaml
```

### 4. Deploy Services
```bash
kubectl apply -f kubernetes/services.yaml
```

### 5. Deploy Applications
```bash
kubectl apply -f kubernetes/app-deployment.yaml
kubectl apply -f kubernetes/ui-deployment.yaml
```

### Deploy All
```bash
kubectl apply -f kubernetes/
```

## Verification

```bash
# Check deployments
kubectl get deployments -n hp-communication

# Check pods
kubectl get pods -n hp-communication

# Check services
kubectl get svc -n hp-communication

# View logs
kubectl logs -n hp-communication -l app=hp-communication-app
```

## Port Forwarding

```bash
# App
kubectl port-forward -n hp-communication svc/hp-communication-app-service 8080:80

# UI
kubectl port-forward -n hp-communication svc/hp-communication-ui-service 3000:80
```

## Scaling

```bash
kubectl scale deployment hp-communication-app --replicas=5 -n hp-communication
kubectl scale deployment hp-communication-ui --replicas=3 -n hp-communication
```

## Updates

```bash
kubectl set image deployment/hp-communication-app \
  app=registry.example.com/hp-communication/app:v1.0.1 \
  -n hp-communication
```

## Cleanup

```bash
kubectl delete namespace hp-communication
```

## File Structure

- `namespace.yaml` - Kubernetes namespace
- `app-deployment.yaml` - App module deployment
- `ui-deployment.yaml` - UI module deployment
- `services.yaml` - ClusterIP services
- `configmap.yaml` - Configuration
- `secrets.yaml` - Secrets (update credentials)
- `rbac.yaml` - Service accounts & roles
- `README.md` - This file
