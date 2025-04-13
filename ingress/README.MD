### What is **Ingress** in Kubernetes?

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

**Resource**: [kind – Ingress](https://kind.sigs.k8s.io/docs/user/ingress/)

You should `have services running` because ingress controls and exposes services by defining some rules on them.

First you need to setup ingress by running applying the ingress-nginx manifest file (can be found in resource) using:

```bash
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml
```

This creates a namespace `ingress-nginx` under which some resources (pods, services, deployments) are running.

Now, create an ingress manifest file defining the configs and rules and apply the file.

To accept external traffic, you might need to expose `ingress-nginx-controller` service from the ingress-nginx ns by forwarding port as:

```bash
kubectl port-forward service/ingress-nginx-controller -n ingress-nginx 80:80 --address=0.0.0.0
```

Suppose the spec in ingress manifest file looks like this:

```yaml
spec:
  rules:
  - http:
      paths:
      - pathType: Prefix
        path: /nginx
        backend:
          service:
            name: nginx-service
            port:
              number: 80
      - pathType: Prefix
        path: /zenith
        backend:
          service:
            name: zenith-service
            port:
              number: 3000
```

After port-forwarding the ingress-nginx-controller, when you try to access **[ip]/nginx** or **[ip]/zenith** (where ip is the IPA of kube-cluster running host) you might not get expected result. 

> Why?
> 

Because often your backend service **expects requests to start from `/`**, but your Ingress might be routing with a path like `/app`. Without rewriting, your backend would receive `/app`, which might return a 404 or not behave as expected.

To replace `/nginx` or `/zenith` path before routing to corresponding backend service, we need to change the behavior of nginx-ingress controller using `annotations` .

Learn more about annotations here:

[Annotations](ANNOTATIONS.md)