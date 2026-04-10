# gpu-blueprint-0408-1

## Overview
This repository contains a **Kubernetes Helm Chart** for deploying a **Sovereign AI Architecture** on Google Kubernetes Engine (GKE).
It demonstrates a multi-tenant setup where GPU resources are logically partitioned using **MIG (Multi-Instance GPU)** emulation, allowing multiple "Cognitive Agents" to share the same physical GPU safely.

## Architecture
The system follows a **Brain-and-Agents** pattern:
1.  **vLLM "Brain"**: A high-performance inference server (mocked in this version) responsible for heavy lifting.
2.  **Cognitive Agents**: Multiple lightweight pods that act as "workers" or "specialists", querying the Brain.

## Prerequisites
-   **Google Cloud Project**: With billing enabled.
-   **GKE Cluster**: `gpu-gke-1` (configured in `cloudbuild.yaml`).
-   **Cloud Build API**: Enabled for CI/CD.
-   **Helm**: Installed locally.

## Installation

### 1. Deploy to Development
```bash
helm upgrade --install gpu-blueprint . --namespace dev --create-namespace -f values-dev.yaml
```

### 2. Deploy to Staging
```bash
helm upgrade --install gpu-blueprint . --namespace stage --create-namespace -f values-stage.yaml
```

### 3. Deploy to Production
```bash
helm upgrade --install gpu-blueprint . --namespace prod --create-namespace -f values-prod.yaml
```

## CI/CD Pipeline
Changes to the `main` branch are automatically deployed to the `prod` namespace via Cloud Build.
Changes to the `dev` branch are automatically deployed to the `dev` namespace.

## Helm Chart Structure
-   `values.yaml`: Default configuration.
-   `values-dev.yaml`, `values-stage.yaml`, `values-prod.yaml`: Environment-specific overrides.
-   `templates/`: Kubernetes manifests.
    -   `vllm-mock-deploy.yaml`: The inference server (currently a busybox placeholder).
    -   `agent-deploy.yaml`: The "Cognitive Agents" (5 replicas in default).
    -   `vllm-service.yaml`: Exposes the inference server.
