# Kubernetes Architecture

Understanding Kubernetes architecture is key to effectively managing and deploying containerized applications. This guide will walk you through the core components and their roles in a Kubernetes cluster.

## Overview
Kubernetes operates on a **cluster architecture**. A cluster is made up of:

1. **Control Plane**: Manages the cluster and coordinates its components.
2. **Worker Nodes**: Run the applications and workloads.

### High-Level Diagram
```
+-------------------------------+
|         Control Plane         |
| +---------------------------+ |
| |      API Server           | |
| |    Scheduler              | |
| |  Controller Manager       | |
| | etcd (Key-Value Store)    | |
| +---------------------------+ |
+-------------------------------+
           |         ^
           |         |
+-------------------+-----------------+
|                   |                 |
|       Worker Node 1                |
| +---------------------------+      |
| |     kubelet               |      |
| |     kube-proxy            |      |
| |     Pod(s)                |      |
| +---------------------------+      |
|                                 ...|
+-------------------------------------+
```

## Core Components

### Control Plane Components

1. **API Server** 🖥️
   - Acts as the front end of the cluster.
   - Exposes the Kubernetes API, which is used by `kubectl` and other tools to interact with the cluster.

2. **etcd** 📒
   - A distributed, reliable key-value store that holds the entire state of the cluster.
   - Highly available and consistent.

3. **Scheduler** 🗓️
   - Assigns work (pods) to specific nodes in the cluster based on resource requirements and policies.

4. **Controller Manager** 🔄
   - Ensures the desired state of the cluster by managing controllers, such as:
     - Node Controller: Monitors node health.
     - Replication Controller: Ensures the correct number of pods are running.

### Worker Node Components

1. **kubelet** 🛠️
   - An agent that runs on each worker node.
   - Ensures containers in pods are running as expected.

2. **kube-proxy** 🌐
   - Manages network rules on nodes, allowing communication between pods and services.

3. **Pods** 📦
   - The smallest deployable unit in Kubernetes.
   - Encapsulates one or more containers, storage, and networking configurations.

## Cluster Workflow

1. **User Interaction**
   - Users interact with the API Server using tools like `kubectl` or Kubernetes dashboards.

2. **API Server Processing**
   - The API Server validates requests and updates the cluster’s state in etcd.

3. **Scheduling and Control**
   - The Scheduler assigns pods to nodes.
   - The Controller Manager ensures the cluster’s desired state.

4. **Worker Node Execution**
   - The kubelet on the worker node pulls container images and runs them in pods.
   - kube-proxy manages networking to connect services and pods.

## Key Concepts

- **Desired State**: Kubernetes ensures that the cluster’s actual state matches the desired state defined in configuration files.
- **Self-Healing**: Automatically replaces failed pods and nodes.
- **Declarative Configuration**: Users define their desired state through YAML or JSON manifests.

---

Proceed to the [Basic kubectl Commands](basic-kubectl-commands.md) section to learn how to interact with your cluster.
