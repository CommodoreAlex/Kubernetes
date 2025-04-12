## Helm Usage Guide 

Helm is a powerful tool that helps you manage Kubernetes applications through packages called **charts**. It simplifies the process of deploying, updating, and managing applications on your Kubernetes cluster.

### Table of Contents

- [What is Helm?](#what-is-helm)
- [Installing Helm](#installing-helm)
- [Helm Commands Overview](#helm-commands-overview)
- [Deploying Applications with Helm](#deploying-applications-with-helm)
- [Managing Helm Releases](#managing-helm-releases)
- [Upgrading and Rolling Back](#upgrading-and-rolling-back)
- [Helm Chart Repositories](#helm-chart-repositories)
- [Using Values and Configurations](#using-values-and-configurations)

### What is Helm? 

Helm helps you define, install, and upgrade Kubernetes applications. It packages applications and services into **charts**, which are a collection of YAML files describing Kubernetes resources.

Charts are essentially **packages** in Helm. They contain all the necessary Kubernetes YAML files and templates required to deploy a specific application or service. A chart might include configurations for deployments, services, volumes, and other resources needed for the application to run in Kubernetes. You can think of it like a blueprint or template for deploying an application.

Releases, on the other hand, are **instances** of a chart that has been deployed to a Kubernetes cluster. When you install a chart using Helm, it creates a release with a unique name. A release allows you to manage and upgrade that specific instance of the application over time. Each release has its own version and configuration, and you can upgrade or roll it back independently.

In short:

- **Chart** = A package that defines how to deploy an application.
- **Release** = A deployed instance of a chart in your Kubernetes cluster.

### Installing Helm 🛠️

1. **Download Helm**: You can install Helm on your machine using the package manager for your system. For example:
    
    - For macOS: `brew install helm`
    - For Linux: `sudo snap install helm --classic`
2. **Verify Installation**: After installation, check that Helm is installed correctly:
```bash
helm version
```

### Helm Commands Overview

Familiarize yourself with these basic Helm commands:

- **Install a Chart**:
```bash
helm install <release-name> <chart-name>
```

- **List Installed Releases**:
```bash
helm list
```

- **Upgrade a Release**:
```bash
helm upgrade <release-name> <chart-name>
```

- **Uninstall a Release**:
```bash
helm uninstall <release-name>
```
### Deploying Applications with Helm

To deploy an application with Helm:

1. Find a chart (you can use public charts from the Helm Hub or create your own).
2. Install the chart using the `helm install` command:
```bash
helm install my-app bitnami/nginx
```

### Managing Helm Releases 

You can manage your Helm releases with commands like:

- **Viewing Release Status**:
```bash
helm status <release-name>
```

- **Viewing Release History**:
```bash
helm history <release-name>
```

### Upgrading and Rolling Back

Helm makes it easy to upgrade applications or roll them back:

- **Upgrade**:
```bash
helm upgrade <release-name> <chart-name>
```

- **Rollback to a previous version**:
```bash
helm rollback <release-name> <revision-number>
```

### Helm Chart Repositories 

Helm allows you to use public or private chart repositories to find and share charts. You can add repositories with:
```bash
helm repo add <repo-name> <repo-url>
helm repo update
```

### Using Values and Configurations 

Helm charts can be customized using values files or direct command-line overrides:

- **Using a values file**:
```bash
helm install my-app bitnami/nginx -f values.yaml
```


- **Overriding values via command line**:
```bash
helm install my-app bitnami/nginx --set replicaCount=3
```

---
