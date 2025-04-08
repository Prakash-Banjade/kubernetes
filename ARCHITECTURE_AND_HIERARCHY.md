## 🗂️ Kubernetes Hierarchy Overview

```
Cluster
├── Nodes
│   ├── Control Plane Node(s)
│   │   ├── kube-apiserver
│   │   ├── kube-controller-manager
│   │   ├── kube-scheduler
│   │   └── etcd
│   └── Worker Node(s)
│       ├── kubelet
│       ├── kube-proxy
│       └── Container Runtime (Docker, containerd, etc.)
│
├── Namespaces
│   ├── default
│   ├── kube-system
│   ├── kube-public
│   └── custom-namespaces
│       ├── Deployments
│       │   ├── ReplicaSets
│       │   │   ├── Pods
│       │   │   │   └── Containers
│       │   └── ConfigMaps / Secrets
│       ├── Services
│       └── ResourceQuotas / Limits
```

---

## Detailed Breakdown

### 1. **Cluster**

- The **whole system** that runs your Kubernetes environment.
- Consists of multiple **Nodes** (machines—either VMs or physical).

---

### 2. **Nodes**

### a. **Control Plane Node**

Manages the entire cluster. Runs components:

- **kube-apiserver** – front door for all API calls.
- **etcd** – distributed key-value store for all cluster data.
- **kube-scheduler** – decides which node should run a new pod.
- **kube-controller-manager** – watches and maintains desired state (e.g., ensures pods match deployment specs).

### b. **Worker Node**

Runs the actual workloads (Pods):

- **kubelet** – talks to the control plane, runs containers via container runtime.
- **kube-proxy** – manages networking and load balancing.
- **Container runtime** – like Docker or containerd to run containers.

---

### 3. **Namespaces**

- Logical partitions to group resources.
- Enables multi-tenancy and separation (like `dev`, `prod`, etc.).

---

### 4. **Deployments**

- Manage **ReplicaSets**, which ensure a certain number of **Pods** are always running.
- Good for self-healing, scaling, and rolling updates.

---

### 5. **Pods**

- Smallest unit deployed by Kubernetes.
- Usually contains **one container**, but can have more (e.g., sidecar pattern).
- Shares networking and storage between containers in the same pod.

---

### 6. **Containers**

- The actual application runtime.
- Each container runs an image (like `nginx`, `node`, etc.).
- Managed by container runtimes (e.g., containerd, Docker).

---

### 7. **Services**

- Expose Pods to other Pods or external clients.
- Types: `ClusterIP`, `NodePort`, `LoadBalancer`, `ExternalName`.

---

### 8. **Other Objects Inside Namespace**

- **ConfigMaps** – for non-secret config values.
- **Secrets** – for sensitive data like passwords.
- **ResourceQuotas** – limit CPU, memory usage per namespace.
- **Ingress** – expose HTTP/HTTPS routes to services.
- **Volumes** – persistent or ephemeral storage.

---

## Visual Tree Format (Compact)

```
Cluster
├── Control Plane
│   ├── kube-apiserver
│   ├── etcd
│   ├── kube-scheduler
│   └── controller-manager
├── Worker Nodes
│   ├── kubelet
│   ├── kube-proxy
│   └── container runtime
└── Namespaces
    └── Deployments
        └── ReplicaSets
            └── Pods
                └── Containers
```