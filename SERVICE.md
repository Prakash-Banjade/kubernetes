## 🚀 What is a **Service** in Kubernetes?

A **Service** in Kubernetes is a **stable networking abstraction** that allows you to expose one or more Pods to other services, pods, or even external users.

Why? Because **Pods are temporary** — they can die, restart, or be replaced — and each time, they might get a new IP. So we need something that:

- Keeps a consistent way to **access a group of Pods**
- **Load balances** across those Pods
- Optionally **exposes them outside the cluster**

That's where **Services** come in.

---

### 🛠️ Types of Services

| Type                  | Description                                                        |
| --------------------- | ------------------------------------------------------------------ |
| `ClusterIP` (default) | Exposes the service **internally** within the cluster              |
| `NodePort`            | Exposes the service on a **static port** on each node’s IP         |
| `LoadBalancer`        | Works with cloud providers to create an **external load balancer** |
| `ExternalName`        | Maps a service to a **DNS name** outside the cluster               |

---

### 🎯 Example: Exposing a Deployment with a Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app          # Matches pods with this label
  ports:
    - protocol: TCP
      port: 80           # Port on the service
      targetPort: 8080   # Port on the pod/container
  type: ClusterIP        # Only accessible inside the cluster
```

This service will load-balance traffic across all Pods with label `app: my-app`, targeting port `8080` inside the pods but exposing it as port `80` to clients in the cluster.