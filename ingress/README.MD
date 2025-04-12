# What is **Ingress** in Kubernetes?

**Ingress** is an API object that **manages external access to services** in a Kubernetes cluster — typically over **HTTP and HTTPS**.

But it's more than just exposure — it lets you:

- Route traffic based on **URL paths or hostnames**
- Terminate **SSL/TLS (HTTPS)**
- Set **rules, rewrites, redirects**
- Use a single IP or domain for multiple services (via path-based routing)

---

### Without Ingress

You'd have to expose each service using:

- `NodePort`: fixed ports on each node
- `LoadBalancer`: expensive cloud load balancers per service

---

### With Ingress

You define routing rules like:

```yaml
example.com/api      → my-api-service
example.com/web      → frontend-service
```

Now **one external IP or domain** can serve multiple apps based on the path or subdomain.

---

### What You Need

1. **Ingress Resource** — defines the routing rules (YAML file).
2. **Ingress Controller** — runs inside the cluster and implements the rules (like NGINX Ingress Controller, Traefik, etc).

Without the controller, the Ingress resource does nothing.

## Getting started with Ingress-Nginx using Kind:

Resource: [kind – Ingress](https://kind.sigs.k8s.io/docs/user/ingress/)

First you need to setup ingress by running applying the ingress-nginx manifest file (can be found in resource) using:

```bash
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml
```