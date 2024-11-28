### **Kubernetes Services Overview**

- **Purpose**:
    - Enable communication between applications (within the cluster and external).
    - Connect applications with users.
    - Provide a way to access pods, which may have dynamically assigned IPs.
- **Characteristics**:
    - Span across all nodes in the cluster.
    - Facilitate **loose coupling** across microservices.

---

### **1. NodePort Service**

- **Definition**:
    
    - Exposes a service on a static port of each node in the cluster.
    - Maps a node's port to the target pod's port.
- **Key Terms**:
    
    - **Port**: The service’s port (mandatory field).
    - **TargetPort**: The port of the pod that receives the traffic.
    - **NodePort**: The port on the node where the service is exposed (usually between 30000-32767).
- **Use Case**:
    
    - Accessing the service externally through the node’s IP and NodePort.
- **YAML Definition**:
    

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
    - port: 80          # Service's port
      targetPort: 8080  # Pod's port
      nodePort: 30007   # Node's port
```

- **CLI Commands**:
    
    - Create the service:
        
        ```bash
        kubectl create -f nodeport-service.yaml
        ```
        
    - Check the service:
        
        ```bash
        kubectl get svc
        ```
        
    - Access the service:
        
        ```bash
        http://<node-ip>:30007
        ```
        
- **Key Notes**:
    
    - Traffic is distributed across multiple pods using random algorithms.

---

### **2. ClusterIP Service**

- **Definition**:
    
    - Exposes the service within the cluster (internal communication only).
    - Assigns a virtual IP (ClusterIP) to the service.
    - Combines multiple pods into a single interface.
- **Use Case**:
    
    - Communication between microservices within the cluster.
- **YAML Definition**:
    

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-clusterip-service
spec:
  type: ClusterIP
  selector:
    app: myapp
  ports:
    - port: 80          # Service's port
      targetPort: 8080  # Pod's port
```

- **CLI Commands**:
    - Create the service:
        
        ```bash
        kubectl create -f clusterip-service.yaml
        ```
        
    - Access the service from inside the cluster:
        
        ```bash
        curl http://<service-name>:80
        ```
        

---

### **3. LoadBalancer Service**

- **Definition**:
    
    - Exposes the service externally using a supported cloud provider’s load balancer (e.g., AWS, GCP, Azure).
    - If used on local VMs or on-premises, it behaves like a NodePort service.
- **Use Case**:
    
    - Provide an external endpoint for users to access the application.
- **YAML Definition**:
    

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
    - port: 80          # Service's port
      targetPort: 8080  # Pod's port
```

- **CLI Commands**:
    - Create the service:
        
        ```bash
        kubectl create -f loadbalancer-service.yaml
        ```
        
    - Check the external IP assigned by the cloud provider:
        
        ```bash
        kubectl get svc
        ```
        

---

### **Service Type Comparison**

|**Type**|**Access**|**External Access**|**Use Case**|
|---|---|---|---|
|**ClusterIP**|Within the cluster only|No|Internal microservice communication.|
|**NodePort**|Node IP + NodePort|Yes|External access via node's IP.|
|**LoadBalancer**|External Load Balancer|Yes|Exposing services via cloud provider's load balancer.|

---

### **Key Notes**

- Combine services with **selectors** to direct traffic to pods with matching labels.
- Use **ingress controllers** for advanced routing and external access at the domain level.
- Services distribute traffic across pods using random or round-robin algorithms.