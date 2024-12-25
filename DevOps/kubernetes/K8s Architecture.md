#DevOps 
#Docker 
## Core Concepts

### 1. **Cluster Architecture**

- Kubernetes follows a **Master-Worker architecture**.
- The **Master Node** manages the cluster by scheduling, monitoring, and orchestrating containers.
- The **Worker Nodes** host and run application containers.

---

### 2. **Master Node Components**

#### **ETCD Cluster**

- Acts as the key-value store for Kubernetes.
- Stores cluster data such as configurations, secrets, and states.

> [!note] Critical Component  
> ETCD is the backbone of Kubernetes, storing the state of the entire cluster. If ETCD fails, the cluster's state may be lost.

#### **Kube-API Server**

- The main entry point for all administrative tasks.
- Exposes Kubernetes APIs and manages communication between other master components and worker nodes.

> [!important] Containerized Component  
> The **kube-apiserver** can be deployed as a container on the master node.

#### **Kube-Scheduler**

- Responsible for placing containers on worker nodes based on scheduling policies (e.g., resource requirements).

#### **Controller Manager**

- Manages control loops to ensure the desired state is maintained.
    - **Node Controller:** Handles node lifecycle events, such as onboarding new nodes.
    - **Replication Controller:** Ensures the correct number of pod replicas are running.
- request scheduler to create a container if the container failes

---

### 3. **Worker Node Components**

#### **Container Runtime Engine**

- Runs containers using a runtime such as:
    - **Docker**
    - **Rocket (rkt)**

#### **Kubelet**

- An agent that runs on each worker node.
- Listens to the API server and executes instructions such as starting/stopping containers.
- Sends periodic status reports back to the API server.

> [!example] Kubelet Reports  
> Kubelet periodically reports the node's health and running container statuses to the kube-apiserver.

#### **Kube-Proxy**

- Maintains network rules and facilitates communication between pods and services across nodes.

> [!note] Networking Role  
> The **kube-proxy** is key for enabling seamless pod-to-pod communication within the cluster.

---

## Kubernetes Network

- The **Kube-Proxy** ensures connectivity between worker nodes, enabling them to communicate and work collaboratively.

---

## Deployment of Master and Worker Components

> [!info] Containerized Components
> 
> - On the **Master Node**, the following can run as containers:
>     - **kube-apiserver**
>     - **kube-scheduler**
>     - **controller manager**
> - On the **Worker Nodes**, the following can be containerized:
>     - **kubelet**
>     - **kube-proxy**

---
## **Mermaid Diagram**

```mermaid
flowchart TD
    subgraph Master_Node["Master Node"]
        direction TB
        ETCD[ETCD Cluster]
        API[Kube-API Server]
        Scheduler[Kube-Scheduler]
        Controller_Manager[Kube Controller Manager]
        API --> Scheduler
        API --> Controller_Manager
        API --> ETCD
    end

    subgraph Worker_Node_1["Worker Node"]
        direction TB
        Kubelet_1[Kubelet]
        Proxy_1[Kube-Proxy]
        Runtime_1[Container Runtime: Docker or rkt]
        Kubelet_1 --> Proxy_1
        Proxy_1 --> Runtime_1
    end

    subgraph Worker_Node_2["Worker Node"]
        direction TB
        Kubelet_2[Kubelet]
        Proxy_2[Kube-Proxy]
        Runtime_2[Container Runtime: Docker or rkt]
        Kubelet_2 --> Proxy_2
        Proxy_2 --> Runtime_2
    end

    Master_Node --> Worker_Node_1
    Master_Node --> Worker_Node_2

```

### Simple flow of pod creation via kubectl

```mermaid
flowchart TD
    subgraph kubectl
        A[kubectl / API Call]
    end

    subgraph Master_Node
        subgraph API_Server
            B[Receive Request from kubectl]
            C[Authenticate User and Validate Request]
            H[Notify Target Node's Kubelet]
        end

        subgraph ETCD
            D[Store Desired Pod Spec]
            G[Update Pod Specification with Node Assignment]
        end

        subgraph Scheduler
            E[Watch ETCD for Unscheduled Pods]
            F[Assign Node to Pod]
        end
    end

    subgraph Worker_Node
        I[Kubelet Provisions Pod]
        J[Kubelet Updates Pod Status]
    end

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> B
    B --> H
    H --> I
    I --> J
    J --> B
```

### Descriptions:

1. **kubectl / API Call**: A request to create a pod is sent by the user or system.
2. **API Server**: The API server handles the request and authenticates/validates it.
3. **ETCD**: Stores the desired state of the pod.
4. **Scheduler**: Monitors ETCD for pods without node assignments, schedules them to a suitable node, and updates the ETCD.
5. **Kubelet**: On the selected node, provisions the pod, and updates its status back to ETCD through the API Server.

---
### Components
- [[services]]
- [[K8s Controllers]]
- [[Pods]]
- [[Scheduling]]
- [[k8s namespaces]]
