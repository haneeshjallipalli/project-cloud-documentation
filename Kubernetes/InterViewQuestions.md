## **🔥 Top 20 Most Asked Kubernetes Interview Questions & Answers (Advanced Level)**  

These questions cover essential Kubernetes topics like **Namespaces, RBAC, StatefulSets, Custom Resources, and more**, often asked in DevOps & SRE interviews.

---

### **1️⃣ What is a Kubernetes Namespace? Why is it used?**  
**📌 Answer:**  
A **Namespace** in Kubernetes is a virtual cluster inside a physical cluster. It isolates workloads, allowing multiple teams to share the same cluster while keeping resources separate.

🔹 **Use Cases:**  
- Multi-tenant clusters  
- Resource isolation  
- RBAC policies for different teams  

📌 **Example: Creating a Namespace**  
```sh
kubectl create namespace dev
kubectl get namespaces
```
📌 **YAML Definition:**  
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

---

### **2️⃣ What is Kubernetes RBAC? How does it work?**  
**📌 Answer:**  
**Role-Based Access Control (RBAC)** manages user permissions in Kubernetes. It defines **who can do what on which resources**.  

📌 **Key RBAC Components:**  
- **Role**: Grants permissions within a namespace.  
- **ClusterRole**: Grants cluster-wide permissions.  
- **RoleBinding**: Binds a Role to a user.  
- **ClusterRoleBinding**: Binds a ClusterRole to a user across namespaces.  

📌 **Example: Creating a Role & RoleBinding**  
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: developer-role
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list"]
```
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: dev-binding
  namespace: dev
subjects:
  - kind: User
    name: john
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer-role
  apiGroup: rbac.authorization.k8s.io
```
📌 **Command to Apply RBAC Rules:**  
```sh
kubectl create -f role.yaml
kubectl create -f rolebinding.yaml
```

---

### **3️⃣ What is a StatefulSet? How is it different from a Deployment?**  
**📌 Answer:**  
A **StatefulSet** is used to manage stateful applications like databases (**MySQL, Redis, Elasticsearch**). Unlike Deployments, StatefulSets **maintain Pod identity across restarts**.

📌 **Key Differences:**  
| Feature         | Deployment | StatefulSet |
|----------------|------------|------------|
| Pod Names      | Random names | **Stable, ordered names** (`pod-0`, `pod-1`) |
| Volume         | Shared storage | **Persistent per Pod** |
| Scaling        | Any Pod can be replaced | **Pods are replaced in order (graceful shutdown & startup)** |

📌 **Example: StatefulSet for MySQL**  
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql
  replicas: 2
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:5.7
          ports:
            - containerPort: 3306
```
📌 **Command to Apply:**  
```sh
kubectl apply -f statefulset.yaml
kubectl get statefulsets
```

---

### **4️⃣ What is a Custom Resource Definition (CRD)?**  
**📌 Answer:**  
A **Custom Resource Definition (CRD)** allows users to define their own Kubernetes objects.  

📌 **Example: Creating a CRD for a "Database" Object**  
```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: databases.example.com
spec:
  group: example.com
  names:
    kind: Database
    plural: databases
  scope: Namespaced
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                engine:
                  type: string
```
📌 **Command to Create CRD:**  
```sh
kubectl apply -f crd.yaml
kubectl get crds
```

---

### **5️⃣ What is a DaemonSet? How is it different from a Deployment?**  
**📌 Answer:**  
A **DaemonSet** ensures that a copy of a Pod runs on every node in a cluster. It is used for **monitoring, logging, and networking**.

📌 **Key Differences:**  
| Feature     | Deployment | DaemonSet |
|------------|------------|------------|
| Pod Count  | Dynamic | **One Pod per Node** |
| Use Case   | Web apps, APIs | **Logging, monitoring, networking (Fluentd, Prometheus, Calico, etc.)** |

📌 **Example: Running Fluentd on Every Node**  
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
spec:
  selector:
    matchLabels:
      app: fluentd
  template:
    metadata:
      labels:
        app: fluentd
    spec:
      containers:
        - name: fluentd
          image: fluent/fluentd
```
📌 **Command to Apply:**  
```sh
kubectl apply -f daemonset.yaml
kubectl get daemonsets
```

---

### **6️⃣ What is a Kubernetes Operator?**  
**📌 Answer:**  
An **Operator** automates the deployment and lifecycle management of applications using CRDs.

📌 **Example Use Cases:**  
- **Database Management** (e.g., MySQL Operator)  
- **Auto-scaling complex apps**  
- **Backup & recovery of applications**  

📌 **Example: Operator Managing a PostgreSQL Database**  
```yaml
apiVersion: postgres.example.com/v1
kind: PostgresCluster
metadata:
  name: my-postgres
spec:
  replicas: 3
```
📌 **Command to Create Operator:**  
```sh
kubectl apply -f operator.yaml
```

---

### **7️⃣ What is the difference between ConfigMap and Secret?**  
| Feature       | ConfigMap | Secret |
|--------------|----------|--------|
| Stores       | **Non-sensitive data** (config files, environment variables) | **Sensitive data** (passwords, API keys) |
| Data Format  | Plaintext | **Base64 encoded** |
| Security     | Less secure | More secure |

📌 **Example: Secret for Database Password**  
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  password: bXlwYXNzd29yZA==  # Base64 encoded "mypassword"
```
📌 **Command to Apply & Retrieve Secret:**  
```sh
kubectl apply -f secret.yaml
kubectl get secrets
kubectl describe secret db-secret
```

---
## **🔥 Advanced Kubernetes Interview Questions & Answers (Part 2)**  

These are the **most frequently asked Kubernetes interview questions** covering **networking, scaling, storage, troubleshooting, and deployments.**  

---

### **8️⃣ What is the difference between ClusterIP, NodePort, and LoadBalancer in Kubernetes Services?**  
📌 **Answer:** Kubernetes Services expose Pods to enable network communication. There are **three main service types**:

| **Service Type** | **Visibility** | **Usage** |
|-----------------|---------------|-----------|
| **ClusterIP** (default) | **Only within the cluster** | Internal communication between microservices |
| **NodePort** | **Accessible via `<NodeIP>:<Port>`** | Used for development/testing or exposing apps without a LoadBalancer |
| **LoadBalancer** | **Externally accessible (Cloud-based)** | Used in cloud environments (AWS, GCP, Azure) to expose services |

📌 **Example: Service with LoadBalancer**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```
📌 **Command to Apply:**
```sh
kubectl apply -f service.yaml
kubectl get services
```

---

### **9️⃣ How does Kubernetes Horizontal Pod Autoscaler (HPA) work?**  
📌 **Answer:**  
**Horizontal Pod Autoscaler (HPA)** scales Pods **based on CPU or memory usage**.

📌 **Steps in HPA Scaling:**
1️⃣ **Monitors CPU/memory usage** using Metrics API  
2️⃣ **Adjusts replica count dynamically**  
3️⃣ **Ensures efficient resource utilization**  

📌 **Example: HPA for CPU Scaling**
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```
📌 **Command to Apply & Check HPA:**
```sh
kubectl apply -f hpa.yaml
kubectl get hpa
```

---

### **🔟 What are Kubernetes Ingress and Ingress Controllers?**  
📌 **Answer:**  
Kubernetes **Ingress** manages external access to Services **via HTTP/HTTPS**. It allows **domain-based routing**.

📌 **Key Components:**
- **Ingress Resource** (Defines routing rules)
- **Ingress Controller** (Handles traffic, e.g., Nginx, Traefik)

📌 **Example: Ingress for Routing Traffic**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
    - host: myapp.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-service
                port:
                  number: 80
```
📌 **Command to Apply:**
```sh
kubectl apply -f ingress.yaml
```

---

### **1️⃣1️⃣ How do you debug Kubernetes Pods?**  
📌 **Answer:** Debugging involves checking logs, inspecting events, and running commands inside the container.

📌 **Common Debugging Commands:**
```sh
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/sh
```
📌 **Example: Checking Events for Failures**
```sh
kubectl describe pod my-app
```
```
Events:
  Warning  Failed    2m10s  kubelet  ImagePullBackOff: Failed to pull image
```
✅ **Solution:** Check if the image exists or has correct permissions.

---

### **1️⃣2️⃣ What is a Persistent Volume (PV) and Persistent Volume Claim (PVC)?**  
📌 **Answer:**  
- **Persistent Volume (PV)**: A storage unit (NFS, AWS EBS, etc.) managed by Kubernetes.  
- **Persistent Volume Claim (PVC)**: A request by a Pod to use storage.  

📌 **Example: Creating a PV and PVC**
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```
📌 **Command to Apply:**
```sh
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
```

---

### **1️⃣3️⃣ How do you perform a rolling update and rollback in Kubernetes?**  
📌 **Answer:** Rolling updates allow zero-downtime deployments.

📌 **Rolling Update:**
```sh
kubectl set image deployment/my-app my-container=nginx:latest
kubectl rollout status deployment/my-app
```

📌 **Rollback:**
```sh
kubectl rollout undo deployment/my-app
```

---

### **1️⃣4️⃣ What are Init Containers in Kubernetes?**  
📌 **Answer:**  
Init Containers **run before the main application container starts**.  
Use cases:
- **Wait for dependencies** (e.g., database to be ready)
- **Pre-initialization tasks** (e.g., downloading files)

📌 **Example:**
```yaml
spec:
  initContainers:
    - name: init-db
      image: busybox
      command: ["sh", "-c", "until nc -z db-service 5432; do sleep 2; done"]
  containers:
    - name: app
      image: my-app
```

---

### **1️⃣5️⃣ What is the difference between a Job and a CronJob in Kubernetes?**  
| Feature   | **Job** | **CronJob** |
|-----------|--------|------------|
| **Purpose** | Runs a task once | Runs tasks on a schedule |
| **Examples** | Data backup | Scheduled database cleanup |

📌 **Example: CronJob for Daily Cleanup**
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: cleanup-job
spec:
  schedule: "0 2 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: cleanup
              image: busybox
              command: ["sh", "-c", "rm -rf /tmp/*"]
          restartPolicy: OnFailure
```
📌 **Command to Apply:**
```sh
kubectl apply -f cronjob.yaml
kubectl get cronjobs
```

---

### **1️⃣6️⃣ How does Kubernetes handle networking between Pods?**  
📌 **Answer:**  
- **Every Pod gets a unique IP.**  
- **Pods communicate using ClusterIP Services.**  
- **CNI (Container Network Interface) plugins** like Flannel, Calico manage networking.  

📌 **Check Pod IPs:**
```sh
kubectl get pods -o wide
```

---

### **1️⃣7️⃣ What is a Kubernetes Admission Controller?**  
📌 **Answer:**  
Admission Controllers **intercept API requests before they are executed.**  
- **ValidatingAdmissionWebhook** → Rejects invalid configurations.  
- **MutatingAdmissionWebhook** → Modifies requests dynamically.  

📌 **Example: Enforcing Image Pull Policy**
```yaml
apiVersion: admissionregistration.k8s.io/v1
kind: ValidatingWebhookConfiguration
metadata:
  name: image-checker
```

---

### **1️⃣8️⃣ How does Pod Affinity & Anti-Affinity work?**  
📌 **Answer:**  
- **Pod Affinity** → Schedule related Pods on the same node.  
- **Pod Anti-Affinity** → Prevent Pods from running on the same node.  

📌 **Example: Enforcing Anti-Affinity**
```yaml
podAntiAffinity:
  requiredDuringSchedulingIgnoredDuringExecution:
    - labelSelector:
        matchExpressions:
          - key: app
            operator: In
            values:
              - my-app
```

---

