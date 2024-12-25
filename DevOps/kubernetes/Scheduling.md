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

> [!error] If the pod went out of memory limits, it will shutdown and throw OOM (Out Of Memory) error



---
### Daemon Sets
- Like replica sets, but runs one copy of a pod on each node.
- Mostly suitable for: `logging & monitoring agents as pods`, tools like kube-proxy can be part of daemon sets.
- Requires its own definition file.

> [!example]
#### Example Manifest for DaemonSet
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: example-daemonset
  labels:
    app: example-app
spec:
  selector:
    matchLabels:
      app: example-app
  template:
    metadata:
      labels:
        app: example-app
    spec:
      containers:
      - name: example-container
        image: nginx:latest
        resources:
          requests:
            cpu: "100m"
            memory: "128Mi"
          limits:
            cpu: "200m"
            memory: "256Mi"
````

> [!notes]

- Useful for cluster-wide utilities such as log collectors (`Fluentd`, `Logstash`) and node monitoring tools (`Prometheus Node Exporter`).
- Best practice: Use labels to manage pod selection for monitoring or cleanup operations.

> [!important] DaemonSets run on **all eligible nodes** by default but can be scoped to specific nodes using `nodeSelector`, `affinity`, or `taints and tolerations`.

---

### Static Pods

- `kubelet` will create pods for whatever pod definition files are present under the `/etc/kubernetes/manifests` directory on a node. It will also ensure that the pods are not failing.
- Static pods bypass the Kubernetes master; they are not managed by the API server or other control-plane components.
- Deployments or replica sets placed here will not work since they require processing by Kubernetes master node tools.

> [!example]

#### Example Manifest for Static Pod

Save this file as `/etc/kubernetes/manifests/static-pod-example.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: static-pod-example
  labels:
    app: static-app
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

> [!notes]

- Static pods are ideal for bootstrapping control-plane components such as:
    - `etcd`
    - `kube-apiserver`
    - `kube-controller-manager`
    - `kube-scheduler`
- Changes to static pod definitions require updating the manifest file on the node.

> [!important]
> - Static pods are monitored by `kubelet`, not the control plane.
> - Logs for static pods can be found using `journalctl` for the `kubelet` service.

---

### Additional Notes

> [!notes]
> - Static pods can be used for disaster recovery by deploying core Kubernetes components manually.
> - Use static pods sparingly, as they bypass Kubernetes' rich management and lifecycle features.
> - DaemonSets are managed and can be scaled dynamically, while static pods require direct interaction with node files.


---
### Multiple Schedules
- to place our own scheduler, we can make / config our own scheduler and package and apply as the default scheduler


- sample scheduler config file here
- only one can be master scheduler, so leader elect is begin used, 
- config for leader elect


- we also have to reference the custom scheduler that we created to pod definition file, ref goes here
- to see the schedule process, we can see it via events `kubectl get events -o wide`, and to see it's logs `kubectl logs scheduler 


### configuring sheduler provile
- we can set priority to a pod definitio for the scheduler to pick it up, for the scheduler queue
- pahse, 
	1. scheduling pahse ( scheduling queue) done via priority sort
	2. filter phase, use node resources plugin & nodename plugin to ched pod has node nome have name, node undrainable
	3. Scoring phase, done based on free space for selecting node, uses node resourdce fit
	4. Binding, default binder
- Extension point ( a thing with all the pahse to attach our plugins )
	- Scheduling
		- queueSort
	- Filtering
		- preFiltering
		- filter
		- postFilter
	- Scoring
		- preScore
		- Score
		- reserve
	- Binding
		- permit
		- prebind
		- bind
		- postbind
	
	