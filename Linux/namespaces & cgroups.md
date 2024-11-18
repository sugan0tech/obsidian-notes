> [!info] **Development Took Over a Decade**  
> Namespaces implementation started around **2002**; the last one, **user namespaces**, was completed in February 2013, in kernel 3.18.

---

## **Docker and Linux Containerization**
Docker is a widely-used open-source project that leverages **Linux kernel features** to provide process isolation and resource management.

### **Key Features**
- Written primarily in **Go**.
- Initially developed by **dotCloud**, which was later renamed Docker, Inc.
- Started with **LXC** (Linux Containers) and transitioned to its own library for improved performance and isolation.
- Supported by **850+ contributors** and an extensive ecosystem of tools and registries.

### **Core Technologies**
- **Namespaces**: Provides isolation of processes, networks, and filesystems.
- **cgroups (Control Groups)**: Manages and limits resource allocation.

> [!tip] **Resources**  
> - GitHub: [Docker Project](https://github.com/docker/docker)  
> - Public Docker Registry: For container images.

---

## **Historical Collaboration**
- **RedHat** announced a technical partnership with dotCloud in **September 2013**, showcasing industry support for Docker.

---

## **Background - Linux Containers**
### **LXC**
- Originated from a French company later acquired by **IBM** in 2005.
- Fully rewritten as an open-source project and maintained by **Canonical developers**.

### **Systemd and Containers**
- **systemd-nspawn**: A lightweight containerization tool for debugging.
- Inspired by RedHat, **SELinux** enhancements were added for better security.

---

> [!warning] **Namespaces in Linux**  
> Namespaces isolate resources, restricting the visibility of processes and system resources to specific containers.

### **Types of Namespaces**
1. **mnt** (Mount): Isolates filesystem mount points.  
2. **pid** (Process): Isolates process IDs.  
3. **net** (Network): Isolates network interfaces, ports, and routing tables.  
4. **ipc** (Inter-Process Communication): Isolates shared memory and message queues.  
5. **uts** (Unix Time-sharing System): Isolates hostnames and NIS domain names.  
6. **user** (User): Isolates user and group IDs (UIDs/GIDs).

---

> [!note] **Namespace System Calls**  
1. **`clone()`**:  
   - Creates a new process and attaches it to a new namespace.  
   - Docker uses this to start isolated container processes.  
2. **`unshare()`**:  
   - Modifies the namespace of the current process.  
   - Docker uses this for namespace customization.

---

> [!info] **cgroups (Control Groups)**  
Control Groups handle resource allocation and accounting in Linux, allowing Docker to impose resource limits.

### **Key Features**
1. **Resource Management**:  
   - Manages CPU, memory, and I/O limits for containers.  
2. **Accounting**:  
   - Tracks resource usage for monitoring.  
3. **Isolation**:  
   - Ensures fair distribution of resources.

---

> [!example] **Viewing Namespaces and cgroups**  
- **Namespaces**: Use `ls -al /proc/<pid>/ns` to view namespaces associated with a process.  
- **cgroups**: Check `/sys/fs/cgroup/docker/<container_id>` for container-specific resource limits.