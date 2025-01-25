# Troubleshooting

Kubernetes troubleshooting is essential for identifying and resolving issues that arise in your cluster. This guide outlines common problems and the tools/commands needed to debug them effectively.

---

## Step 1: Check Pod Status 🛠️

Start by checking the status of your pods:

```bash
kubectl get pods
```

Look for pods that are in `Pending`, `CrashLoopBackOff`, or `Error` states.

### Describe a Pod
To get more information about a specific pod:

```bash
kubectl describe pod <pod-name>
```

This will show detailed information, including events and error messages.

### View Pod Logs
Check the logs for application errors:

```bash
kubectl logs <pod-name>
```

For pods with multiple containers:

```bash
kubectl logs <pod-name> -c <container-name>
```

---

## Step 2: Inspect Deployments and Services 🔍

### Deployment Status
Check if deployments are running correctly:

```bash
kubectl get deployments
```

For detailed information:

```bash
kubectl describe deployment <deployment-name>
```

### Service Status
Ensure services are properly configured:

```bash
kubectl get services
```

Verify service connectivity:

```bash
kubectl exec -it <pod-name> -- curl http://<service-name>:<port>
```

---

## Step 3: Troubleshoot Networking Issues 🌐

### Check Network Policies
List network policies in your namespace:

```bash
kubectl get networkpolicies
```

Ensure policies allow traffic between relevant pods and services.

### Test DNS Resolution
Verify that DNS is working correctly:

```bash
kubectl exec -it <pod-name> -- nslookup <service-name>
```

### Ping Pods
Check connectivity between pods:

```bash
kubectl exec -it <pod-name> -- ping <target-pod-ip>
```

---

## Step 4: Debug Persistent Volume Issues 📦

### Inspect PVC and PV
Check the status of Persistent Volume Claims (PVCs):

```bash
kubectl get pvc
```

Check Persistent Volumes (PVs):

```bash
kubectl get pv
```

For detailed information:

```bash
kubectl describe pvc <pvc-name>
kubectl describe pv <pv-name>
```

### Check Pod Volume Mounts
Ensure the volume is correctly mounted in the pod:

```bash
kubectl describe pod <pod-name>
```

---

## Step 5: Debug Resource Constraints 🚦

### Check Resource Requests and Limits
Inspect pod resource settings to see if they exceed cluster capacity:

```bash
kubectl describe pod <pod-name>
```

Look for `Resource Limits` and `Resource Requests` under the `Containers` section.

### Monitor Node Capacity
Check if nodes have sufficient resources:

```bash
kubectl describe nodes
```

### View Metrics
Use the `metrics-server` to monitor cluster resource usage:

```bash
kubectl top nodes
kubectl top pods
```

---

## Step 6: Common Errors and Solutions ❗

1. **Pod Stuck in Pending**:
   - Cause: Insufficient resources or unscheduled nodes.
   - Solution: Check pod events and node capacity.

2. **CrashLoopBackOff**:
   - Cause: Application startup failure or configuration errors.
   - Solution: Check pod logs for error messages.

3. **Service Unreachable**:
   - Cause: Incorrect service configuration or network policies.
   - Solution: Verify service, endpoints, and pod connectivity.

4. **Volume Not Mounted**:
   - Cause: PVC not bound to PV or volume mount issue.
   - Solution: Check PVC/PV binding and pod volume configuration.

---

## Best Practices 🧠

- Use `kubectl` debugging tools regularly to stay familiar.
- Monitor cluster health using dashboards or external monitoring tools.
- Automate troubleshooting with scripts or CI/CD pipelines.
- Document recurring issues and their solutions for future reference.

---

You’re now equipped to troubleshoot Kubernetes issues. Continue improving your cluster by revisiting [Security](security.md) to harden your setup.
