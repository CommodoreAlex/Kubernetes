# Scaling Applications

Scaling applications in Kubernetes allows you to adjust the number of replicas (instances) of your application to meet demand. This guide covers scaling manually and using auto-scaling.

---

## Step 1: Manual Scaling 🔧

Manual scaling lets you increase or decrease the number of pods for a deployment.

### Scale Up or Down
To scale a deployment, use the `kubectl scale` command:

```bash
kubectl scale deployment <deployment-name> --replicas=<number-of-replicas>
```

Example:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

Verify the scaling:

```bash
kubectl get pods
```

You should see 5 pods running.

---

## Step 2: Horizontal Pod Autoscaler (HPA) ⚖️

The Horizontal Pod Autoscaler automatically adjusts the number of replicas based on resource usage (e.g., CPU or memory).

### Enable Metrics Server
To use HPA, ensure the **Metrics Server** is running in your cluster. If not, install it:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

### Create an Autoscaler
Use the `kubectl autoscale` command:

```bash
kubectl autoscale deployment <deployment-name> --min=<min-replicas> --max=<max-replicas> --cpu-percent=<target-cpu-utilization>
```

Example:

```bash
kubectl autoscale deployment nginx-deployment --min=2 --max=10 --cpu-percent=50
```

- **`--min`**: Minimum number of replicas.
- **`--max`**: Maximum number of replicas.
- **`--cpu-percent`**: Target CPU utilization percentage.

Verify the HPA:

```bash
kubectl get hpa
```

---

## Step 3: Test Autoscaling ⚙️

Generate CPU load to test autoscaling using a simple containerized tool:

### Deploy a Stress Test Pod
Run the following command to deploy a pod that generates CPU load:

```bash
kubectl run stress-pod --image=busybox --restart=Never -- /bin/sh -c "while true; do :; done"
```

### Monitor Scaling
Keep an eye on your HPA:

```bash
kubectl get hpa -w
```

You should see the number of replicas increase as the CPU utilization rises.

---

## Step 4: Clean Up 🧹

When you're done, clean up your resources:

```bash
kubectl delete hpa <hpa-name>
kubectl scale deployment nginx-deployment --replicas=1
kubectl delete pod stress-pod
```

---

## Best Practices for Scaling 🧠

1. **Monitor Resource Utilization**: Ensure you have tools in place to monitor CPU, memory, and other metrics.
2. **Set Reasonable Limits**: Define minimum and maximum replica counts that match your application’s requirements.
3. **Plan for Load**: Understand traffic patterns and plan autoscaling rules accordingly.

---

With your application scaled, you can move on to [Networking & Storage](networking-storage.md) to learn how Kubernetes handles these critical components.
