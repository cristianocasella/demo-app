# Demo Workload

This is a simple demo application that displays pod information using `nginxdemos/hello`.

## What's Inside

- **Deployment**: 2 replicas of nginx demo pod
- **Service**: ClusterIP service exposing port 80
- **Route**: OpenShift route with TLS enabled
- **Catalog Info**: Backstage catalog entity with Kubernetes integration

## Features

The demo application shows:
- Server name (pod name)
- Server address (pod IP)
- Server port
- Request URI
- Date/time
- Client address

Perfect for testing:
- Kubernetes plugin in RHDH
- Topology view
- Load balancing across pods
- RHACS integration (security scanning)

## Deploy

```bash
# Create namespace and deploy
oc apply -f namespace.yaml
oc apply -f deployment.yaml
oc apply -f service.yaml
oc apply -f route.yaml

# Verify deployment
oc get pods -n demo-workload
oc get route -n demo-workload
```

## View in RHDH

Once registered, you'll see:

- **Overview Tab**: Component information
- **Kubernetes Tab**: Pods, Deployments, Services, Routes
- **Topology Tab**: Visual application topology
- **Security Tab** (if RHACS configured): Vulnerabilities and policy violations

## Access the Application

Get the route URL:
```bash
oc get route hello-demo -n demo-workload -o jsonpath='{.spec.host}'
```

## Cleanup

```bash
oc delete -f .
```
