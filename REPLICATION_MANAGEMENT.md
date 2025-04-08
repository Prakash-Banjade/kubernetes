Kubernetes gives us **multiple controllers** to manage Pod replication, each designed for different use-cases.

---

## Pod Replication Controllers in Kubernetes

| Controller | Purpose | When to Use  |
| --- | --- | --- |
| **Deployment** | Manages stateless pods; handles rolling updates, rollbacks. | Most common for web apps, APIs, etc. |
| **ReplicaSet** | Ensures a specific number of pod replicas are running. | Rarely used directly (used internally by Deployments). |
| **DaemonSet** | Runs **one pod per node** (or more if needed) across the cluster. | Logging agents, monitoring, networking tools. |
| **StatefulSet** | Manages **stateful applications** with stable identities & storage. | Databases, queues, anything that needs persistence. |

---

## 1. **Deployment**

- Wraps and manages **ReplicaSets**.
- Supports **rolling updates**, **rollbacks**, **scaling**.
- **Stateless** workloads.

```bash
kubectl apply -f deployment.yaml
```

---

## 2. **ReplicaSet**

- Maintains **a set number of identical Pods**.
- Doesn’t support updates/rollbacks natively.
- Mostly used **indirectly** via Deployments.

```yaml
kind: ReplicaSet
```

> You almost never need to define a ReplicaSet manually — Deployments handle this for you.
> 

---

## 3. **DaemonSet**

- Ensures **one Pod runs on each (or selected) Node**.
- Perfect for cluster-wide services like:
    - log collectors (Fluentd, Filebeat),
    - monitoring agents (Prometheus Node Exporter),
    - network plugins (CNI).

```yaml
kind: DaemonSet
```

---

## 4. **StatefulSet**

- For **stateful apps** needing:
    - stable network identity (`pod-0`, `pod-1`, ...),
    - stable storage (e.g., persistent volume per pod),
    - **ordered deployment and scaling**.

Example: Running databases like PostgreSQL, MongoDB, Kafka, etc.

```yaml
kind: StatefulSet
```

---

### Summary:

- **Use Deployment**: for 99% of your app workloads.
- **Use DaemonSet**: for 1 pod per node stuff (e.g., monitoring).
- **Use StatefulSet**: for apps needing sticky identity & storage.
- **Use ReplicaSet**: only if you're doing low-level orchestration.