# Networking & Storage

Kubernetes simplifies networking and storage for containerized applications, providing scalable and consistent solutions. This guide covers the basics of Kubernetes networking and storage concepts to help you manage communication and data persistence effectively.

---

## Networking in Kubernetes 🌐

Kubernetes networking ensures seamless communication between pods, services, and external resources.

### Key Concepts

1. **Pod-to-Pod Communication**:
   - Kubernetes uses a flat network model, enabling every pod to communicate with any other pod in the cluster.
   - Pods are assigned unique IP addresses.

2. **Service-to-Pod Communication**:
   - Services abstract pod communication and ensure reliability.
   - Kubernetes services include:
     - **ClusterIP**: Default service type, accessible only within the cluster.
     - **NodePort**: Exposes the service on a static port on each node.
     - **LoadBalancer**: Integrates with cloud provider load balancers for external access.

### Example: Exposing a Service

Create a service for an existing deployment:

```bash
kubectl expose deployment <deployment-name> --type=NodePort --port=80
```

List services to verify:

```bash
kubectl get services
```

### Accessing Services

- For **ClusterIP** services, use other pods or services within the cluster.
- For **NodePort** or **LoadBalancer** services, use the node's IP and the exposed port:

```bash
http://<node-ip>:<node-port>
```

---

## Storage in Kubernetes 📦

Storage in Kubernetes is essential for persisting data beyond the lifecycle of individual pods.

### Key Concepts

1. **Volumes**:
   - Kubernetes volumes provide storage shared by all containers in a pod.
   - Volumes are ephemeral by default and tied to the pod's lifecycle.

2. **Persistent Volumes (PV)**:
   - Persistent storage independent of pod lifecycles.
   - Managed by cluster administrators.

3. **Persistent Volume Claims (PVC)**:
   - Storage requests by users.
   - Automatically bind to available PVs.

4. **Storage Classes**:
   - Define storage provisioners and parameters.
   - Allow dynamic volume provisioning.

### Example: Creating a Persistent Volume and PVC

#### Persistent Volume (PV)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

#### Persistent Volume Claim (PVC)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: example-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

Apply the PV and PVC:

```bash
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
```

Verify the PVC status:

```bash
kubectl get pvc
```

### Using PVCs in Pods

Attach the PVC to a pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-pvc
spec:
  containers:
  - name: app-container
    image: nginx
    volumeMounts:
    - mountPath: "/usr/share/nginx/html"
      name: storage-volume
  volumes:
  - name: storage-volume
    persistentVolumeClaim:
      claimName: example-pvc
```

Apply the pod configuration:

```bash
kubectl apply -f pod-with-pvc.yaml
```

---

## Best Practices 🧠

- **Plan Storage Needs**: Use dynamic provisioning with storage classes for scalable storage solutions.
- **Secure Networking**: Implement network policies to control traffic between pods and services.
- **Monitor Traffic**: Use tools like `kubectl logs` and cloud dashboards to track networking issues.

---

Explore [Troubleshooting](troubleshooting.md) to debug networking and storage issues effectively.
