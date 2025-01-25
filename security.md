# Security

Securing your Kubernetes cluster is critical to protecting your workloads and sensitive data from unauthorized access and potential attacks. This guide provides an overview of key security practices and configurations.

---

## Core Security Concepts 🔒

1. **Authentication**: Verifying the identity of users and service accounts.
2. **Authorization**: Defining what authenticated users can do within the cluster.
3. **Network Security**: Restricting communication between pods, services, and external entities.
4. **Workload Security**: Ensuring pods and containers run with minimal privileges.
5. **Audit Logging**: Keeping track of activities within the cluster.

---

## Kubernetes Security Best Practices 🛡️

### 1. Enable Role-Based Access Control (RBAC)
RBAC restricts user actions based on defined roles.

- List current roles and role bindings:

  ```bash
  kubectl get roles,rolebindings
  ```

- Create a role with specific permissions:

  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    namespace: default
    name: pod-reader
  rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "watch", "list"]
  ```

- Bind the role to a user or group:

  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: read-pods
    namespace: default
  subjects:
  - kind: User
    name: jane-doe
    apiGroup: ""
  roleRef:
    kind: Role
    name: pod-reader
    apiGroup: ""
  ```

Apply the configurations with `kubectl apply`.

### 2. Secure ETCD Communication
ETCD is the database backing Kubernetes. Always:

- Enable TLS encryption for ETCD.
- Limit access to the ETCD server to only Kubernetes components.

### 3. Use Network Policies
Restrict pod-to-pod communication using Network Policies.

Example policy to allow traffic only from a specific app:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-app-traffic
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: my-app
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
```

Apply it:

```bash
kubectl apply -f network-policy.yaml
```

### 4. Use Pod Security Standards (PSS)
Define security contexts for pods to enforce policies like:

- Running as a non-root user.
- Restricting host access.

Example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-pod
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  containers:
  - name: app
    image: nginx
    securityContext:
      allowPrivilegeEscalation: false
```

### 5. Enable Audit Logs
Audit logs track access and activities in the cluster.

- Enable audit logging by adding these flags to the API server:

  ```bash
  --audit-log-path=/var/log/kubernetes/audit.log \
  --audit-log-maxage=30 \
  --audit-log-maxbackup=10 \
  --audit-log-maxsize=100
  ```

Analyze audit logs regularly to identify suspicious activity.

### 6. Regularly Scan Images for Vulnerabilities
Use tools like `Trivy` or `Aqua Security` to scan container images for vulnerabilities:

```bash
trivy image my-container-image
```

### 7. Use Secrets for Sensitive Data
Avoid hardcoding sensitive data in your application code.

- Create a secret:

  ```bash
  kubectl create secret generic my-secret --from-literal=username=admin --from-literal=password=secret
  ```

- Use the secret in your pod:

  ```yaml
  env:
  - name: DB_USERNAME
    valueFrom:
      secretKeyRef:
        name: my-secret
        key: username
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: my-secret
        key: password
  ```

---

## Tools for Enhancing Security 🛠️

- **Kube-Bench**: Checks cluster configuration against security benchmarks.
- **Kube-Hunter**: Identifies security vulnerabilities in Kubernetes clusters.
- **OPA/Gatekeeper**: Enforce custom policies on cluster resources.
- **Falco**: Monitors runtime activity for anomalous behavior.

---

Revisit [Networking & Storage](networking-storage.md) to ensure your cluster is both secure and performant.
