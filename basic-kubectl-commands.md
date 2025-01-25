# Basic kubectl Commands

`kubectl` is the command-line tool used to interact with your Kubernetes cluster. This guide introduces the most commonly used commands to help you manage and troubleshoot your cluster effectively.

## Getting Started with kubectl

Before running any commands, ensure you have `kubectl` installed and configured to connect to your Kubernetes cluster. You can verify your setup with:

```bash
kubectl version --client
kubectl cluster-info
```

---

## Common kubectl Commands

### 1. Cluster Information 🌐
Get details about the cluster and its components:

```bash
kubectl cluster-info
kubectl get nodes
```

- `kubectl cluster-info`: Displays cluster endpoints.
- `kubectl get nodes`: Lists all nodes in the cluster.

---

### 2. Managing Pods 📦
#### List Pods
```bash
kubectl get pods
```

#### Get Detailed Pod Information
```bash
kubectl describe pod <pod-name>
```

#### Delete a Pod
```bash
kubectl delete pod <pod-name>
```

---

### 3. Managing Deployments 📋
#### List Deployments
```bash
kubectl get deployments
```

#### Create a Deployment
```bash
kubectl create deployment <deployment-name> --image=<image-name>
```

Example:
```bash
kubectl create deployment nginx-deployment --image=nginx
```

#### Scale a Deployment
```bash
kubectl scale deployment <deployment-name> --replicas=<number>
```

#### Update a Deployment
```bash
kubectl set image deployment/<deployment-name> <container-name>=<new-image>
```

#### Delete a Deployment
```bash
kubectl delete deployment <deployment-name>
```

---

### 4. Working with Services 🌐
#### List Services
```bash
kubectl get services
```

#### Expose a Deployment as a Service
```bash
kubectl expose deployment <deployment-name> --type=LoadBalancer --port=<port>
```

#### Delete a Service
```bash
kubectl delete service <service-name>
```

---

### 5. Logs and Troubleshooting 🛠️
#### View Pod Logs
```bash
kubectl logs <pod-name>
```

#### View Logs of a Specific Container in a Pod
```bash
kubectl logs <pod-name> -c <container-name>
```

#### Debugging with a Shell
Access a shell in a running container:
```bash
kubectl exec -it <pod-name> -- /bin/bash
```

---

### 6. Configurations and Resources ⚙️
#### Apply a Configuration File
```bash
kubectl apply -f <file-name>.yaml
```

#### Delete Resources from a File
```bash
kubectl delete -f <file-name>.yaml
```

#### Get Resource Information
```bash
kubectl get <resource-type>
```

Example:
```bash
kubectl get pods
kubectl get services
kubectl get deployments
```

---

## Tips for Success 🧠
- Use `kubectl help` to explore additional command options.
- Add the `-o wide` flag to get more detailed output for many commands.
- Use YAML manifests for declarative configuration, as it makes managing resources more efficient.

---

Now that you know the basic `kubectl` commands, you can proceed to [Creating a Simple Application](creating-a-simple-application.md) to start deploying workloads in your Kubernetes cluster.
