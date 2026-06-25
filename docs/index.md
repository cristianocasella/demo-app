# Demo Workload

A simple demo application using nginx from a Quay registry, configured to run on OpenShift with restricted SCC (Security Context Constraints).

## What's Inside

- **Deployment**: 2 replicas of nginx pod from a private Quay registry
- **Service**: ClusterIP service exposing port 80 (targeting pod port 8080)
- **Route**: OpenShift route with TLS enabled
- **Catalog Info**: Backstage catalog entity with Kubernetes and RHACS integration
- **Image Pull Secret**: For accessing private Quay registry

## Features

The demo application shows:
- Hostname (pod name)
- Image information
- Simple HTML page

Configured for OpenShift:
- Runs nginx on port 8080 (non-privileged)
- Uses emptyDir volumes for writable cache directories (/tmp/nginx/cache, /tmp/nginx/run)
- Custom nginx.conf to avoid permission issues with random UIDs

Perfect for testing:
- Kubernetes plugin in RHDH
- Topology view
- Load balancing across pods
- RHACS integration (security scanning)

## Catalog Integration

This component is registered in Red Hat Developer Hub through the `catalog-info.yaml` file. The following integrations are enabled via annotations:

- **Kubernetes**: Monitors pods, deployments, and services in the cluster
- **RHACS**: Scans container images for vulnerabilities and policy compliance
- **Quay**: Displays container registry information and security scan results
- **TechDocs**: Generates documentation from the `docs/` folder
- **GitHub**: Tracks repository metrics and activity
- **OpenSSF Scorecard**: Evaluates repository security practices

These integrations provide a unified developer portal experience, bringing together information from multiple tools and platforms.

## Deploy

```bash
# Create namespace
oc apply -f namespace.yaml

# Create Quay image pull secret (replace with your credentials)
oc create secret docker-registry quay-pull-secret \
  --docker-server=<your-quay-registry-url> \
  --docker-username='<your-robot-account>' \
  --docker-password='<your-robot-token>' \
  -n demo-workload

# Link pull secret to default service account
oc patch serviceaccount default -n demo-workload -p '{"imagePullSecrets":[{"name":"quay-pull-secret"}]}'

# Deploy application
oc apply -f deployment.yaml
oc apply -f service.yaml
oc apply -f route.yaml

# Verify deployment
oc get pods -n demo-workload
oc get route -n demo-workload
```

## View in RHDH

Once registered in the catalog, you'll see multiple tabs providing comprehensive insights:

### Overview Tab
Component metadata, ownership, lifecycle status, and system relationships.

### Documentation (TechDocs)
Auto-generated documentation from this repository's `docs/` folder, built with MkDocs.

### Kubernetes Tab
Real-time Kubernetes resources:
- **Pods**: Running instances with logs and terminal access
- **Deployments**: Replica sets and rollout status
- **Services**: Network endpoints and port mappings
- **Routes**: OpenShift routes with external URLs
- **Topology**: Visual graph of application components and relationships

### Security Tab
Security analysis powered by Red Hat Advanced Cluster Security (RHACS):
- **CVE Scanning**: Container image vulnerabilities with severity ratings
- **Policy Violations**: Security policy compliance status
- **Risk Score**: Overall security posture assessment

### Scorecard Tab
Health and quality metrics:
- **GitHub Metrics**: Open pull requests, repository activity
- **OpenSSF Scorecard**: Security best practices score from [OpenSSF Scorecard](https://securityscorecards.dev/)
- **File Checks**: Presence of important files (LICENSE, README, SECURITY.md, etc.)

### Image Registry
Container image information from Quay registry:
- **Tags**: Available image versions
- **Security Scans**: Image vulnerability reports
- **Build History**: Image build timestamps and metadata

## Access the Application

Get the route URL:
```bash
oc get route hello-demo -n demo-workload -o jsonpath='{.spec.host}'
```

## Cleanup

```bash
oc delete -f .
```
