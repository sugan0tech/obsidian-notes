#### **Smallest deployable unit in Kubernetes**

- **Definition:** A pod is the smallest unit you can deploy in Kubernetes. It can have one or more tightly coupled containers that share the same network namespace and storage.

#### **Pod Characteristics**

- **Components:** Can include:
    - **Init container**: Runs first, sets up the environment.
    - **Helper container**: Performs auxiliary tasks (e.g., file processing).
    - **Main container**: Runs the primary application.
- **Helper container use cases**:
    - File processing for data created by users.
    - Sharing the same storage space with other containers under the same pod.

#### **Basic Commands for Pods**

1. **Create a pod:**
    
    ```bash
    kubectl run nginx --image=nginx
    ```
    
2. **List all pods:**
    
    ```bash
    kubectl get pods
    ```
    
3. **Get detailed information about a pod:**
    
    ```bash
    kubectl describe pod <pod-name>
    ```
    
4. **Delete a pod:**
    
    ```bash
    kubectl delete pod <pod-name>
    ```
    
5. **Log output from a pod:**
    
    ```bash
    kubectl logs <pod-name>
    ```
    
6. **Access a pod shell (if it has a shell like bash/sh):**
    
    ```bash
    kubectl exec -it <pod-name> -- /bin/bash
    ```
    

#### **Pods with YAML**

A pod configuration file (`pod-definition.yaml`) must include the following fields:

- **apiVersion**
- **kind**
- **metadata**
- **spec**

##### **Example YAML for a Pod**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: fe
spec:
  containers:
    - name: nginx-container
      image: nginx
```

#### **Create a Pod from YAML**

1. **Apply the configuration file:**
    
    ```bash
    kubectl create -f pod-definition.yaml
    ```
    
2. **Inspect the pod:**
    
    ```bash
    kubectl describe pod myapp-pod
    ```
    

---

### **ReplicaSets**

- **Purpose:** Ensures multiple instances of a pod run, and respawns them if they fail.
- **CLI Commands for ReplicaSets:**
    1. **Apply ReplicaSet from a file:**
        
        ```bash
        kubectl create -f replicaset.yaml
        ```
        
    2. **List all ReplicaSets:**
        
        ```bash
        kubectl get replicasets
        ```
        
    3. **Scale a ReplicaSet (increase/decrease the replicas):**
        
        ```bash
        kubectl scale replicaset <replicaset-name> --replicas=<number>
        ```
        
    4. **Delete a ReplicaSet:**
        
        ```bash
        kubectl delete replicaset <replicaset-name>
        ```
        

#### **Example YAML for a ReplicaSet**

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: nginx-container
          image: nginx
```

---

### **Deployments**

- **Purpose:** Used for rolling updates and managing application updates without downtime.
- **CLI Commands for Deployments:**
    1. **Apply Deployment from a file:**
        
        ```bash
        kubectl create -f deployment.yaml
        ```
        
    2. **List all Deployments:**
        
        ```bash
        kubectl get deployments
        ```
        
    3. **Get detailed information about a Deployment:**
        
        ```bash
        kubectl describe deployment <deployment-name>
        ```
        
    4. **Rollout a Deployment update:**
        
        ```bash
        kubectl rollout restart deployment <deployment-name>
        ```
        
    5. **Check the rollout status:**
        
        ```bash
        kubectl rollout status deployment <deployment-name>
        ```
        
    6. **Scale a Deployment:**
        
        ```bash
        kubectl scale deployment <deployment-name> --replicas=<number>
        ```
        
    7. **Delete a Deployment:**
        
        ```bash
        kubectl delete deployment <deployment-name>
        ```
        

#### **Example YAML for a Deployment**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: nginx-container
          image: nginx
```