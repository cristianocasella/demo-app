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

Once registered in the catalog, you'll see:

- **Overview Tab**: Component information
- **Kubernetes Tab**: Pods, Deployments, Services, Routes, HPA
- **Topology Tab**: Visual application topology
- **Security Tab**: RHACS vulnerabilities and policy violations

## Access the Application

Get the route URL:
```bash
oc get route hello-demo -n demo-workload -o jsonpath='{.spec.host}'
```

## Cleanup

```bash
oc delete -f .
```
