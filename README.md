# MLOps Model CI/CD Pipeline with Flux & Zot

This repository provides an enterprise MLOps CI/CD pipeline using **Taskfile** (ecosystem-agnostic task runner), **Zot** (lightweight OCI-compliant registry), and **Flux** (Kubernetes GitOps controller).

## Architecture

```
[ Model Development ] 
       │
       ▼ (task build / test)
[ Local / CI Build ] 
       │
       ▼ (task push / ORAS)
[ Zot OCI Registry ] <── (OCIRepository sync) ── [ Flux CD ] ──► [ Kubernetes Cluster ]
```

## Quick Start with Taskfile

Always use `task` as the project command vocabulary:

- **List available tasks**: `task`
- **Start local Zot registry**: `task registry:up`
- **Build model artifact**: `task build`
- **Push model artifact to Zot**: `task push`
- **Run full CI locally**: `task ci`

## CI/CD Pipeline Workflows

GitHub Actions workflow (`.github/workflows/ci-cd.yaml`) automates:
1. **Validation & Testing**: Lints YAML manifests and runs model validation tests.
2. **Packaging**: Bundles model assets into versioned `.tar.gz` OCI layers.
3. **Registry Push**: Publishes artifacts to **Zot** via ORAS.
4. **GitOps Automation**: Automatically updates the model tag in `gitops/model-oci.yaml`, triggering **Flux** reconciliation in the cluster.
