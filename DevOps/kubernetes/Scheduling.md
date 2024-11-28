### **Manual Scheduling**

- **Definition**: Pods can be assigned to specific nodes by adding the `nodeName` attribute in the pod's manifest file.
- **Key Points**:
    - Kubernetes automatically adds the `nodeName` attribute to the pod's `spec` when manually assigned.
    - Kubernetes **does not allow changing the node** of a running pod; overriding requires a **Binding API call**.
    - **Master nodes** are typically tainted to prevent the scheduler from placing pods on them.

#### **YAML Example for Manual Scheduling**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: manual-scheduling-pod
spec:
  nodeName: my-node
  containers:
    - name: my-container
      image: nginx
```

#### **CLI Commands for Manual Scheduling**

- Check available nodes:
    
    ```bash
    kubectl get nodes
    ```
    
- Describe a node to find details:
    
    ```bash
    kubectl describe node <node-name>
    ```
    
- Create the pod:
    
    ```bash
    kubectl create -f manual-scheduling.yaml
    ```
    

---

### **Labels and Selectors**

- **Labels**: Key-value pairs assigned to Kubernetes objects (like pods, nodes) for identification.
- **Selectors**: Used to filter and manage resources based on labels.

#### **CLI Commands**

- Add a label to a node:
    
    ```bash
    kubectl label nodes <node-name> <key>=<value>
    ```
    
- View labels:
    
    ```bash
    kubectl get nodes --show-labels
    ```
    
- Filter resources using a label selector:
    
    ```bash
    kubectl get pods -l <key>=<value>
    ```
    

#### **YAML Example**

```yaml
metadata:
  labels:
    app: webserver
```

---

### **Taints and Tolerations**

- **Taints**: Restrict nodes from accepting pods unless those pods have a matching toleration.
- **Tolerations**: Allow pods to schedule onto nodes with matching taints.

#### **CLI Commands**

- Add a taint to a node:
    
    ```bash
    kubectl taint nodes <node-name> key=value:taint-effect
    ```
    
- Remove a taint from a node:
    
    ```bash
    kubectl taint nodes <node-name> key=value:taint-effect-
    ```
    

#### **YAML Example for Tolerations**

```yaml
spec:
  tolerations:
    - key: app
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"
```

---

### **Node Selectors**

- **Purpose**: Assign pods to specific nodes using a label-based mechanism.
- **Limitations**:
    - Cannot use complex conditions like "not on these nodes" or multiple criteria.

#### **CLI Commands**

- Add a label to a node:
    
    ```bash
    kubectl label nodes <node-name> key=value
    ```
    

#### **YAML Example**

```yaml
spec:
  containers:
    - name: data-intensive-container
      image: nginx
  nodeSelector:
    size: large
```

---

### **Node Affinity**

- **Purpose**: To ensure pods are scheduled on desired nodes based on custom rules.
- **Types**:
    1. **PreferredDuringSchedulingIgnoredDuringExecution**: Best effort; if no matching node, the pod will still be scheduled.
    2. **RequiredDuringSchedulingIgnoredDuringExecution**: Strict requirement; if no matching node, the pod will not be scheduled.

#### **YAML Example**

```yaml
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: size
                operator: In
                values:
                  - large
                  - medium
      preferredDuringSchedulingIgnoredDuringExecution:
        - weight: 1
          preference:
            matchExpressions:
              - key: storage
                operator: In
                values:
                  - ssd
```

#### **CLI Commands**

- Check affinity rules in existing pods:
    
    ```bash
    kubectl describe pod <pod-name>
    ```
    

---

### **Resources & Limits**

- **Purpose**: Define CPU and memory usage for containers to optimize resource allocation.
    - **Requests**: Minimum resource guarantee for a container.
    - **Limits**: Maximum resource a container can use.

#### **YAML Example**

```yaml
spec:
  containers:
    - name: resource-limited-container
      image: nginx
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
```

#### **CLI Commands**

- View resource usage for pods:
    
    ```bash
    kubectl top pod
    ```
    
- View node capacity and usage:
    
    ```bash
    kubectl top node
    ```
    