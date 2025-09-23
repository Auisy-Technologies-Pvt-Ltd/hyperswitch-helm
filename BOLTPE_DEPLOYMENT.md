# BoltPE Hyperswitch Helm Deployment Guide

## Summary of Changes

All Docker images have been updated from the original Juspay repositories to BoltPE Docker Hub repositories:

### Updated Image Mappings

| Service | BoltPE Image |
|---------|-------------|
| Router | `docker.io/boltpe/hyperswitch-router:v1.116.0` |
| Consumer | `docker.io/boltpe/hyperswitch-consumer:v1.116.0` |
| Producer | `docker.io/boltpe/hyperswitch-producer:v1.116.0` |
| Drainer | `docker.io/boltpe/hyperswitch-drainer:v1.116.0` |
| Control Center | `docker.io/boltpe/hyperswitch-control-center:v1.37.3` |
| Card Vault | `docker.io/boltpe/hyperswitch-card-vault:v0.6.5-dev` |
| Encryption Service | `docker.io/boltpe/hyperswitch-encryption-service:v0.1.8` |
| Web | `docker.io/boltpe/hyperswitch-web:v1.0.10` |
| KYC | `boltpe/kyc` (Available on Docker Hub, ready for integration) |

## Generated Artifacts

The following Helm packages have been generated and are located in the `packages/` directory:

- hyperswitch-app-0.2.11.tgz
- hyperswitch-card-vault-0.1.2.tgz
- hyperswitch-istio-0.1.2.tgz
- hyperswitch-keymanager-0.1.2.tgz
- hyperswitch-monitoring-0.1.2.tgz
- hyperswitch-stack-0.2.12.tgz
- hyperswitch-ucs-0.1.1.tgz
- hyperswitch-web-0.2.10.tgz
- index.yaml (Helm repository index)

## Deployment Instructions

### 1. Using the Helm Repository

To use these charts from your repository:

```bash
# Add the repository (after hosting on GitHub Pages or similar)
helm repo add boltpe https://raw.githubusercontent.com/boltpe/hyperswitch-helm/main/packages

# Update repository cache
helm repo update

# Install a chart
helm install my-hyperswitch boltpe/hyperswitch-stack
```

### 2. Direct Installation from Local Packages

```bash
# Install from local package
helm install my-hyperswitch packages/hyperswitch-stack-0.2.12.tgz

# Or install with custom values
helm install my-hyperswitch packages/hyperswitch-stack-0.2.12.tgz -f my-values.yaml
```

### 3. Customizing Values

You can override default values by creating a custom values file:

```yaml
# my-values.yaml
hyperswitch-app:
  services:
    router:
      image: docker.io/boltpe/hyperswitch-router:v1.116.0
      replicas: 3
```

Then install with:
```bash
helm install my-hyperswitch packages/hyperswitch-app-0.2.11.tgz -f my-values.yaml
```

## Uploading to Docker Hub

The Helm repository itself doesn't get uploaded to Docker Hub. Docker Hub hosts container images, not Helm charts.

The container images referenced in this Helm chart are already available on Docker Hub:
- boltpe/kyc
- boltpe/hyperswitch-web
- boltpe/hyperswitch-encryption-service
- boltpe/hyperswitch-card-vault
- boltpe/hyperswitch-control-center
- boltpe/hyperswitch-drainer
- boltpe/hyperswitch-producer
- boltpe/hyperswitch-consumer
- boltpe/hyperswitch-router

## Hosting the Helm Repository

To make your Helm charts available for others, you can:

### Option 1: GitHub Pages
1. Push this repository to GitHub
2. Enable GitHub Pages in repository settings
3. Set source to main branch, /packages folder
4. Access charts via: https://[username].github.io/hyperswitch-helm/

### Option 2: GitHub Raw URLs (Current Setup)
The index.yaml is already configured to use raw GitHub URLs:
```
https://raw.githubusercontent.com/boltpe/hyperswitch-helm/main/packages/
```

Simply push to GitHub and charts will be accessible.

### Option 3: ChartMuseum
Deploy ChartMuseum to host your private Helm repository.

### Option 4: OCI Registry
Modern Helm supports OCI registries. You can push charts to Docker Hub as OCI artifacts:
```bash
helm push packages/hyperswitch-stack-0.2.12.tgz oci://registry-1.docker.io/boltpe
```

## Verification

To verify the images are correctly configured:

```bash
# Check the images in a chart
helm template packages/hyperswitch-app-0.2.11.tgz | grep image:

# Dry run installation
helm install my-hyperswitch packages/hyperswitch-stack-0.2.12.tgz --dry-run
```

## Next Steps

1. Commit and push these changes to your GitHub repository
2. The Helm packages in the `packages/` directory are ready to use
3. When deploying to Kubernetes, the pods will pull images from your BoltPE Docker Hub repositories
4. Consider implementing the KYC service as a new Helm chart if needed

## Support

For issues or questions about the Hyperswitch platform, refer to:
- Original Repository: https://github.com/juspay/hyperswitch
- Your Fork: https://github.com/boltpe/hyperswitch-helm