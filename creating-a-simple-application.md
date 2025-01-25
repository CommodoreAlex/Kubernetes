# Creating a Simple Application

This guide will walk you through deploying a simple application in Kubernetes. We'll use `kubectl` to create a deployment, expose it as a service, and verify everything is running smoothly.

---

## Step 1: Create a Deployment 📦

A Deployment manages a set of identical pods. For this example, we'll deploy an Nginx web server.

Run the following command:

```bash
kubectl create deployment nginx-deployment --image=nginx
```

- **`nginx-deployment`**: The name of the deployment.
- **`nginx`**: The Docker image to deploy.

You can verify the deployment:

```bash
kubectl get deployments
kubectl get pods
```

---

## Step 2: Expose the Deployment 🌐

Expose the deployment as a service to make it accessible:

```bash
kubectl expose deployment nginx-deployment --type=NodePort --port=80
```

- **`--type=NodePort`**: Exposes the service on a port accessible from outside the cluster.
- **`--port=80`**: The port the application listens on.

List the services to confirm:

```bash
kubectl get services
```

---

## Step 3: Access the Application 🌍

Retrieve the NodePort assigned to the service:

```bash
kubectl describe service nginx-deployment
```

Look for a line like:

```
NodePort: <random-port>  (e.g., 30007)
```

Access the application using your node's IP address and the NodePort:

```
http://<node-ip>:<node-port>
```

To find your node's IP address:

```bash
kubectl get nodes -o wide
```

---

## Step 4: Clean Up 🧹

When you're done, delete the resources to free up cluster resources:

```bash
kubectl delete service nginx-deployment
kubectl delete deployment nginx-deployment
```

---

## YAML Configuration (Optional)

Instead of using commands, you can define the Deployment and Service in YAML files.

### Deployment YAML
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

### Service YAML
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30007
```

Apply the YAML files:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

With your application up and running, you can explore [Scaling Applications](scaling-applications.md) to handle varying workloads
