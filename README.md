# GitOps Kubernetes Configuration for AWS Load Balancer Controller

This repository contains the GitOps-based Kubernetes configuration for AWS Load Balancer Controller, following Flux GitOps patterns and best practices.

## Repository Structure

```
├── base                    # Base Kustomize configurations
│   ├── aws-load-balancer-controller     # Base configuration for AWS Load Balancer Controller
│   │   ├── crds           # Custom Resource Definitions
│   │   ├── deployment     # Core Kubernetes resources (deployment, service, etc.)
│   │   └── kustomization.yaml
│   └── kustomization.yaml
├── charts                  # Helm charts
│   └── aws-load-balancer-controller    # AWS Load Balancer Controller Helm chart
├── overlays                # Environment-specific overlays
│   ├── dev                 # Development environment
│   │   ├── aws-load-balancer-controller
│   │   │   ├── kustomization.yaml
│   │   │   └── values.yaml  # Dev-specific values
│   │   └── kustomization.yaml
│   ├── staging             # Staging environment
│   │   ├── aws-load-balancer-controller
│   │   │   ├── kustomization.yaml
│   │   │   └── values.yaml  # Staging-specific values
│   │   └── kustomization.yaml
│   └── prod                # Production environment
│       ├── aws-load-balancer-controller
│       │   ├── kustomization.yaml
│       │   └── values.yaml  # Production-specific values
│       └── kustomization.yaml
└── .github
    └── workflows           # GitHub Actions workflows for validation
        └── validate.yaml   # Workflow to validate Kubernetes manifests
```

## Migration Process

This repository was created by migrating Kubernetes configurations from the [kubernetes-sigs/aws-load-balancer-controller](https://github.com/kubernetes-sigs/aws-load-balancer-controller) repository. The migration process involved:

1. Forking the original repository
2. Extracting Kubernetes manifests
3. Converting them to Kustomize-compatible format
4. Organizing them into a GitOps-friendly structure
5. Setting up CI/CD validation via GitHub Actions

## Usage

### Prerequisites

- Kubernetes cluster
- Flux CD installed on the cluster
- AWS credentials configured

### Applying Configurations

For Flux CD:

```yaml
# Example Flux Kustomization for dev environment
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: aws-load-balancer-controller
  namespace: flux-system
spec:
  interval: 10m
  path: ./overlays/dev
  prune: true
  sourceRef:
    kind: GitRepository
    name: gitops-k8s-config
  validation: client
```

## Environment-Specific Configurations

- **Dev**: Lower resource requirements, debug logging
- **Staging**: Mid-level resources, more replicas for testing
- **Production**: High availability setup, optimized resource limits

## Contributing

1. Create a branch from `main`
2. Make your changes
3. Create a pull request
4. CI will validate your Kubernetes manifests
5. Request reviews from maintainers

## Validation

All Kubernetes manifests are validated on every pull request using GitHub Actions. The validation includes:

- YAML syntax validation
- Kubernetes schema validation
- Helm chart testing
- Kustomize build verification