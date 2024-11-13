# Docker: Revolutionizing Software Development with Containerization 🐳

Docker introduced **containerization** to the software development world, offering key benefits:

### Key Advantages of Docker

- **Isolation**: Applications and dependencies are encapsulated, ensuring they run consistently across environments.
- **Environment Consistency**: Packages applications with their dependencies for uniform behavior across development, testing, and production stages.
- **Speed**: Containers share the host kernel, enabling rapid startup times and efficient resource usage.

---

### Docker vs. Virtual Machines: A Comparison

| Feature                  | Docker Containers                           | Virtual Machines            |
|--------------------------|---------------------------------------------|-----------------------------|
| **Isolation**            | Process-level, shares OS kernel            | Full OS, kernel per VM      |
| **Size**                 | Lightweight (~MBs)                         | Heavier (~GBs)              |
| **Startup Time**         | Quick (seconds)                            | Slower (minutes)            |
| **Resource Utilization** | More efficient, shared resources           | Less efficient, dedicated resources |
| **Environment Consistency** | High, across all stages (dev, test, prod) | Moderate, depends on VM configurations |

---

### Evolution of Docker Command-Line Interface (CLI) 🖥️

Docker's CLI has evolved for better **usability** and **clarity**.

- **Previous Syntax**: Commands were straightforward, e.g., `docker run`, `docker build`.
- **Current Syntax**: Commands are more modular, with subcommands for organization.

  - **Old**: `docker run` ➔ **New**: `docker container run`
  - **Old**: `docker build` ➔ **New**: `docker image build`

**This structure improves clarity**, aligning with Docker's commitment to a clean, organized CLI.

---

### Essential Docker Files

#### **Dockerfile**
A **Dockerfile** is a set of instructions to build an image. It’s now a standard for container configurations, recognized by the [Open Container Initiative (OCI)](https://opencontainers.org).

#### **.dockerignore**
The **.dockerignore** file specifies files/directories to exclude from the build context, optimizing the build by preventing unnecessary files from being sent to the Docker daemon.

---

### Docker’s Core Concepts

- **Layering**: Each command in a Dockerfile creates a new image layer, enabling efficient storage and caching. Unchanged layers can be reused, improving speed and reducing data redundancy.
  
- **Volumes**: Volumes allow data to persist outside the container’s lifecycle. They are crucial for data persistence and sharing between containers.

  ```bash
  sudo docker run --name sampleapp_1 -v $(pwd):/app -v /app/node_modules -p 3232:3232 sampleapp
  ```
  - `-v $(pwd):/app`: Mounts the current directory to `/app` in the container.
  - `-v /app/node_modules`: Creates a volume for the `node_modules` directory.
  - `-p 3232:3232`: Maps port 3232 on the host to port 3232 on the container.

---

### Docker Pruning: Cleaning Up Unused Resources 🧹

Over time, Docker can accumulate unused data. Pruning commands help maintain a clean environment:

- **Remove stopped containers**:
  ```bash
  docker container prune
  ```
- **Remove unused images**:
  ```bash
  docker image prune
  ```
- **Remove unused networks**:
  ```bash
  docker network prune
  ```
- **Remove unused volumes**:
  ```bash
  docker volume prune
  ```
- **Remove all unused resources**:
  ```bash
  docker system prune
  ```

---

### CLI Process Monitoring & Management 🔍

- **`container top`**: List all processes in a container.
- **`container inspect`**: Display detailed container configuration.
- **`container stats`**: Show real-time performance stats.

### Accessing the Shell in Containers 🐚

- **Start a new container interactively**:
  ```bash
  docker container run -it <image>
  ```
- **Run commands in an existing container**:
  ```bash
  docker container exec -it <container> <program>
  ```
- **Attach to a stopped container**:
  ```bash
  docker container start -ai <container>
  ```

> **Flags**: `-t` for pseudo-TTY, `-i` for interactive mode.

---

### Popular Base Distributions for Docker Containers 🐧

- [[Alpine]]: Extremely lightweight, optimized for containers.
- [[CentOs]]: Stable and widely used, upstream of Red Hat.
- [[NIX]]: Configuration-focused, uses Nix package manager for containers.

---

This structure provides a **clear, organized view** of Docker's features and commands, making it easy to understand Docker’s key concepts and efficiently manage containerized environments.