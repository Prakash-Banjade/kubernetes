## Annotations

Annotations in Ingress are **key-value pairs** that allow you to configure **additional features or behaviors** specific to the **Ingress controller** you’re using (like NGINX, Traefik, etc.).

They’re placed under the `metadata` section of the Ingress YAML, and they **extend functionality** that isn’t directly covered by the core Kubernetes Ingress API.

---

### Why use annotations?

Because Ingress itself defines only basic routing rules (host/path → service), but controllers like NGINX support many **advanced features**, such as:

- Rewriting URLs
- Setting timeouts
- Enabling rate limiting
- Enforcing HTTPS
- Setting custom headers

These extras are implemented via annotations.

---

### Example

```yaml
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
```

### Explanation:

- `nginx.ingress.kubernetes.io/rewrite-target: /`
    
    → Rewrites the incoming path to `/` before forwarding to the backend.
    
- `nginx.ingress.kubernetes.io/ssl-redirect: "true"`
    
    → Forces HTTP to HTTPS redirection.
    

---

### Common NGINX Ingress Annotations

| Annotation | Description |
| --- | --- |
| `rewrite-target` | Rewrite incoming URL path |
| `ssl-redirect` | Force HTTPS |
| `use-regex` | Enable regex in paths |
| `proxy-body-size` | Limit request body size |
| `whitelist-source-range` | IP whitelisting |
| `connection-proxy-header` | Set headers like `X-Forwarded-For` |

---

### Important

- **Annotations are controller-specific**.
    
    For example, `nginx.ingress.kubernetes.io/*` annotations only work if you're using the **NGINX Ingress Controller**.
    
- If you're using **Traefik** or **HAProxy**, the annotations will be different.

- Path type `Prefix` is behaves differently on how kubernetes spec says and how nginx handles prefix path. Learn more in [Path type Prefix](PATH_TYPE_PREFIX.md).