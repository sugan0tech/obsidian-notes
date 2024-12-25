# Helm Charts: A Package Manager for Kubernetes

### What is Helm?

Helm is a package manager for Kubernetes, used to package collections of YAML files and distribute them to repositories. A **Helm Chart** is a bundle of YAML files that can be created, customized, and pushed to a Helm repository.

### Common Use Cases

- Database setup
- ElasticSearch setup
- Monitoring applications

---

## Helm Chart Structure

```plaintext
mychart/
├── Chart.yaml       # Metadata about the chart (e.g., name, version)
├── values.yaml      # Default values for the template files
├── charts/          # Dependencies for the chart
└── templates/       # Template YAML files with placeholders
```

---

## Templating Engine

Helm provides a templating engine for defining a common blueprint using placeholders. These placeholders are dynamically replaced with actual values defined in the `values.yaml` file.

### Example `values.yaml`

```yaml
name: my-app
container:
  name: my-app-container
  image: my-app-image
  port: 9001
```

### Template YAML (`templates/pod.yaml`)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: {{ .Values.name }}
spec:
  containers:
    - name: {{ .Values.container.name }}
      image: {{ .Values.container.image }}
      ports:
        - containerPort: {{ .Values.container.port }}
```

---

## Applying Helm Charts

To deploy a Helm chart to a Kubernetes cluster:

1. Package the chart:
    
    ```bash
    helm package mychart
    ```
    
2. Install the chart:
    
    ```bash
    helm install my-release mychart
    ```
    
3. Override values with a custom file:
    
    ```bash
    helm install --values=my-values.yaml my-release mychart
    ```
    

### Example `my-values.yaml`

```yaml
container:
  port: 8080
```

---

## Dependencies

Helm supports **chart dependencies**, managed in the `charts/` directory or declared in the `Chart.yaml` file.

### Example `Chart.yaml` with Dependencies

```yaml
apiVersion: v2
name: mychart
version: 1.0.0
dependencies:
  - name: postgresql
    version: 10.0.0
    repository: https://charts.bitnami.com/bitnami
```

Install the dependencies:

```bash
helm dependency update
```

---

## Release Management

Helm simplifies release management with commands like:

- `helm list`: List all releases
- `helm upgrade`: Upgrade an existing release
- `helm rollback`: Roll back to a previous release

---

## CI/CD Integration

Helm is practical for CI/CD pipelines as you can dynamically replace values during the build or deployment stage.

Example:

```bash
helm install --set image.tag=1.0.1 my-release mychart
```
