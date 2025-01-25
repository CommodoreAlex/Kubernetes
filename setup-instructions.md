# Setup Instructions

Follow these instructions to prepare your environment and get started with Kubernetes.

## Prerequisites
Before diving into the setup, ensure you have the following:

1. **Basic command-line knowledge**: Familiarity with using the terminal is essential.
2. **Container runtime installed**: Docker or containerd is commonly used.
3. **Supported operating system**: Linux, macOS, or Windows (with WSL2 recommended).

## Install Required Tools
Here are the essential tools you need to install:

### 1. Install kubectl
`kubectl` is the command-line tool used to interact with Kubernetes clusters.

- **On Linux**:
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
  ```

- **On macOS**:
```bash
brew install kubectl
```

- **On Windows**:
  Use `choco` or `scoop`:
```powershell
choco install kubernetes-cli
```

### 2. Install Minikube (Optional for Local Clusters)
Minikube is a tool that allows you to run Kubernetes locally on your machine.

- **On Linux**:
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
  ```

- **On macOS**:
```bash
brew install minikube
```

- **On Windows**:
```powershell
choco install minikube
```

### 3. Install Helm (Optional for Managing Charts)
Helm is a package manager for Kubernetes, making it easier to deploy complex applications.

- **On Linux/macOS**:
```bash
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
  ```

- **On Windows**:
```powershell
choco install kubernetes-helm
```

## Start a Local Kubernetes Cluster (Using Minikube)

1. Start Minikube:
```bash
minikube start
```

If you try to run as the root user you will need to use `--force`:
```bash
┌──(root㉿kali)-[/home/kali]
└─# minikube start
😄  minikube v1.35.0 on Debian kali-rolling
✨  Automatically selected the docker driver. Other choices: none, ssh
🛑  The "docker" driver should not be used with root privileges. If you wish to continue as root, use --force.
💡  If you are running minikube within a VM, consider using --driver=none:
📘    https://minikube.sigs.k8s.io/docs/reference/drivers/none/

❌  Exiting due to DRV_AS_ROOT: The "docker" driver should not be used with root privileges.
```

2. Verify the cluster is running:
```bash
kubectl cluster-info
```

3. Check available nodes:
```bash
kubectl get nodes
```

## Verify Your Setup
Run the following commands to ensure your Kubernetes setup is working correctly:

1. Create a test deployment:
```bash
kubectl create deployment hello-world --image=nginx
```

2. Expose the deployment:
```bash
kubectl expose deployment hello-world --type=LoadBalancer --port=80
```

3. Get the service URL (Minikube only):
```bash
minikube service hello-world --url
```

4. Visit the URL in your browser to confirm it’s working.

---


Once your setup is complete, proceed to the [Kubernetes Architecture](kubernetes-architecture.md) section to understand the core components of a Kubernetes cluster.
