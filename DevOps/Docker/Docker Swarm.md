- To automate container lifecycle
- brings different host machine into a single managable unit

- not enabled by default


---

## Docker Swarm Upgrade Process

### Diagram 1: Nginx Service with Replicas
```mermaid
flowchart LR
    service["Service: 3 nginx replicas"]
    task1["Task: nginx.1"] --> container1["Container: nginx:latest"]
    task2["Task: nginx.2"] --> container2["Container: nginx:latest"]
    task3["Task: nginx.3"] --> container3["Container: nginx:latest"]
    service --> task1
    service --> task2
    service --> task3
    subgraph "Swarm Manager"
        task1
        task2
        task3
    end
```

swarm replaces `docker run`,  `service` -> `replicas`

---

### Diagram 2: Swarm Architecture
![[Pasted image 20241119143851.png]]
```mermaid
graph TD
    subgraph "Raft Consensus Group"
        manager1["Manager"]
        manager2["Manager"]
        manager3["Manager"]
        manager1 --> manager2
        manager2 --> manager3
        manager3 --> manager1
        state["Internal Distributed State Store"]
        state --> manager1
        state --> manager2
        state --> manager3
    end
    subgraph "Gossip Network"
        worker1["Worker"]
        worker2["Worker"]
        worker3["Worker"]
        worker4["Worker"]
        worker5["Worker"]
        manager1 --> worker1
        manager2 --> worker2
        manager3 --> worker3
        manager1 --> worker4
        manager2 --> worker5
    end
```

---


- raft database, 
- manager - worker
- has CA & secure communiation across nodes
- default disk encryption for raft database