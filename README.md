# K8s-API

A Kubernetes Gateway API demonstration project showcasing traffic management patterns using Envoy Gateway.

## Overview

This project demonstrates various traffic routing and management capabilities of the Kubernetes Gateway API, including basic routing, canary deployments, A/B testing, and URL rewriting.

## Prerequisites

- Kubernetes cluster
- kubectl configured
- Helm 3.x

## Quick Start

1. Install Gateway API CRDs and Envoy Gateway:
```bash
helm install eg oci://docker.io/envoyproxy/gateway-helm --version v1.6.1 -n envoy-gateway-system --create-namespace
```

2. Check the installation status:
```bash
kubectl wait --timeout=5m -n envoy-gateway-system deployment/envoy-gateway --for=condition=Available
```

3. Deploy the basic setup:
```bash
kubectl apply -f deploy-folder/
```

## Project Structure

```
K8s-API/
├── deploy-folder/          # Basic Gateway API setup
│   ├── gateway-class.yaml  # GatewayClass definition
│   ├── gateway.yaml        # Gateway with HTTP listener
│   ├── httproute.yaml      # Basic routing configuration
│   ├── deploy.yaml         # Backend deployment
│   ├── svc.yaml            # Backend service
│   └── svc-account.yaml    # ServiceAccount
├── weighted/               # Canary deployment (80/20 split)
│   └── httproute_weighted.yaml
├── traffic-split/          # A/B testing (50/50 split)
│   ├── httproute-traffic-split.yaml
│   └── backend-2/          # Second backend for testing
│       ├── deploy.yaml
│       ├── svc.yaml
│       └── svc-account.yaml
└── url-rewrite/            # Path rewriting examples
    └── rewrite-httproute.yaml
```

## Usage Examples

### Basic Routing
The `deploy-folder/` contains the foundational setup with a Gateway and HTTPRoute for standard traffic routing.

### Canary Deployment (80/20)
Deploy weighted traffic distribution for gradual rollouts:
```bash
kubectl apply -f weighted/httproute_weighted.yaml
```

### A/B Testing (50/50)
Split traffic evenly between two backend versions:
```bash
kubectl apply -f traffic-split/
```

### URL Rewriting
Apply path rewriting rules:
```bash
kubectl apply -f url-rewrite/rewrite-httproute.yaml
```

## License

MIT
