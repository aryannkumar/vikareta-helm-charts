# Vikareta Platform Helm Charts

This repository contains the centralized Helm charts for the Vikareta B2B Marketplace Platform.

## Architecture

The Vikareta platform consists of 4 microservices:
- **Admin** - Administrative interface (Port 3002)
- **Backend** - API server with health checks (Port 5001)
- **Dashboard** - Analytics dashboard (Port 3001)
- **Web** - Public web interface (Port 3000)

## Quick Start

### Deploy to Development
```bash
helm install vikareta-dev ./vikareta-platform -f ./vikareta-platform/values-dev.yaml
```

### Deploy to Production
```bash
helm install vikareta-prod ./vikareta-platform -f ./vikareta-platform/values-prod.yaml
```

### Upgrade Deployment
```bash
helm upgrade vikareta-prod ./vikareta-platform -f ./vikareta-platform/values-prod.yaml
```

## Configuration

### Environment-Specific Values

- `values.yaml` - Default configuration
- `values-dev.yaml` - Development environment overrides
- `values-prod.yaml` - Production environment overrides

### Service Configuration

Each service can be configured individually:

```yaml
services:
  admin:
    enabled: true
    replicaCount: 2
    image:
      repository: ghcr.io/aryannkumar/vikareta-admin
      tag: main
    port: 3002
    resources:
      limits:
        cpu: 500m
        memory: 512Mi
```

### Disabling Services

To disable a service, set `enabled: false`:

```yaml
services:
  dashboard:
    enabled: false
```

## ArgoCD Integration

This Helm chart is designed to work with ArgoCD Image Updater for automatic deployments:

### Image Update Annotations
```yaml
argocd-image-updater.argoproj.io/image-list: |
  admin=ghcr.io/aryannkumar/vikareta-admin,
  backend=ghcr.io/aryannkumar/vikareta-backend,
  dashboard=ghcr.io/aryannkumar/vikareta-dashboard,
  web=ghcr.io/aryannkumar/vikareta-web
```

## Development

### Testing Locally
```bash
# Validate the chart
helm lint ./vikareta-platform

# Dry run
helm install vikareta-test ./vikareta-platform --dry-run --debug

# Template output
helm template vikareta-test ./vikareta-platform
```

### Chart Structure
```
vikareta-platform/
├── Chart.yaml              # Chart metadata
├── values.yaml             # Default values
├── values-dev.yaml         # Development overrides
├── values-prod.yaml        # Production overrides
└── templates/
    ├── deployment.yaml     # Deployment templates for all services
    ├── service.yaml        # Service templates for all services
    └── _helpers.tpl        # Template helpers
```

## Benefits of Centralized Helm Charts

1. **Consistency** - All services use the same deployment patterns
2. **Maintainability** - Single place to update deployment logic
3. **Environment Management** - Easy environment-specific configurations
4. **Simplified CI/CD** - One chart to rule them all
5. **Better Resource Management** - Centralized resource allocation
6. **Easier Scaling** - Scale all services or individual services easily