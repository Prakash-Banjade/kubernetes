# Kubernetes

### Tools to create Kubernetes clusters:

| **Tool** | **Runs on** | **Use Case** | **Complexity** | **Real-World Use** |
| --- | --- | --- | --- | --- |
| **Kind** | Docker | Testing/CI | Low | Dev |
| **Minikube** | VM/Docker | Local Dev/Learning | Low | Dev |
| **Kubeadm** | Any servers | Real Cluster Setup | Medium-High | Production |

We will be using Kind. To install Kind in windows, you can use `Chocolatey` package manager and run the command: 

```bash
choco install kind
```

### Creating Kubernetes Cluster

To create Kubernetes cluster, you need a configuration file which is basically a `.yaml` file. The file usually looks like this:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes: # we are requiring one control-plane and 3 worker nodes
  - role: control-plane
    image: kindest/node:v1.31.2
  - role: worker
    image: kindest/node:v1.31.2
  - role: worker
    image: kindest/node:v1.31.2
  - role: worker
    image: kindest/node:v1.31.2  
    extraPortMappings: # expose ports 80 and 443
      - containerPort: 80 # http port
        hostPort: 80
        protocol: TCP
      - containerPort: 443 # https port
        hostPort: 443
        protocol: TCP
```

Now create cluster using following `kind` command:

```powershell
kind create cluster --name=my-kube-cluster --config=config.yml
```

Now set `kubectl` context to “kind-my-kube-cluster” as:

```powershell
kubectl cluster-info --context kind-mu-kube-cluster
```

## Some `kubectl` commands:

### Nodes

```powershell
kubectl get nodes # list all nodes i.e. control-plane, worker nodese
kubectl describe <node-name> # get description about the node
```

Namespace

A **Namespace** is a logical partition inside a Kubernetes cluster that groups resources like Pods, Services, Deployments, etc.

```powershell
kubectl get ns # list all namespaces
kubectl create ns nginx # create `nginx` namespace, can be any
kubectl delete ns nginx # delete ns nginx
```

### Pods

A **Pod** is the **smallest and simplest unit** in the Kubernetes object model that you can deploy. Think of it as a **wrapper around one or more containers**, plus some shared resources like networking and storage.

```powershell
kubectl get pods # list pods, from `default` ns
kubectl get pods -n kube-system # from `kube-system` ns
kubectl get pods -n <ns-name> -o wide # get more details of pods, eg: worker(node) they are running on
kubectl run nginx --image=nginx # create pod named `nginx`, in default ns
kubectl run nginx --image=nginx -n nginx # create pod `nginx` in nginx namespace
kubectl delete pod nginx # delete pod nginx

kubectl exec -it pod/nginx-pod -n nginx -- bash # entering in `nginx-pod` pod running in ns `nginx`
kubectl describe pod/<pod-name> -n <ns-name> # get details & history of pod
kubectl logs pods/<pod-name> -n <ns-name> # get logs inside of pod
```

**Tip**: You usually don’t create Pods directly in production. Instead, you use **Deployments**, **ReplicaSets**, or **Jobs** that manage Pods for scalability and resiliency.

### Deployments

A Deployment manages a set of Pods to run an application workload, usually one that doesn't maintain state.

A *Deployment* provides declarative updates for [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) and [ReplicaSets](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/).

```powershell
kubectl get deployments # get all deployments
kubectl get deployments -n <ns-name> # get all deployments of specific ns
kubectl delete deployment <dep-name> # delete deployment

kubectl scale deployemnt/<dep-name> -n <ns-name> --replicas=5 # scale deployment to 5 replicas
kubectl set image deployment/<dep-name> -n <ns-name> <image-name>=<new-image-verion> # updating pods/container, deployment performs rolling updates
		eg: kubectl set image deployment/nginx-deployment -n nginx nginx=nginx:1.27.3

```

# `Manifest files` instead of `kubectl run` commands manually

A **manifest file** in Kubernetes is a YAML (or JSON) configuration file that **defines the desired state** of a Kubernetes resource (like a Pod, Deployment, Service, etc.). Instead of running `kubectl run` commands manually, you **declare your infrastructure as code** using manifest files.

### Creating namespace manifest file

```yaml
kind: Namespace
apiVersion: v1
metadata:
  name: nginx
```

### Creating pods manifest file

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx
  namespace: nginx
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports: 
        - containerPort: 80
```

### Applying manifest file

```powershell
kubectl appply -f file-name.yml
```

Deleting resources defined by a manifest file

```powershell
kubectl delete -f file-name.yml
```