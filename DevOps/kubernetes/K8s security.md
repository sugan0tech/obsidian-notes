Here’s an enhanced version of your Kubernetes (K8s) notes, structured for clarity and completeness:

---

## Kubernetes Authentication and Authorization Overview

### **Authentication**

Authentication methods supported by Kubernetes to access the API server:

1. **Username & Password**
    
    - Define a `user.csv` file with fields like:
        
        ```
        password,username,user_id,optional_group_name
        ```
        
    - Pass it to the API server with the flag:
        
        ```
        --basic-auth-file=<path_to_csv>
        ```
        
2. **Username & Tokens**
    
    - Similar to the username/password method, but use tokens instead.
    - Pass the token in the API call header as:
        
        ```
        Authorization: Bearer <token>
        ```
        
3. **Certificates**
    
    - Use client certificates signed by a Certificate Authority (CA) to authenticate.
    - This is commonly used for Kubernetes components like `kubelet`, `kubectl`, and others.
4. **External Authentication Providers**
    
    - Examples: LDAP, OpenID Connect, or custom webhooks.
5. **Service Accounts**
    
    - Managed by Kubernetes for pods and controllers.
    - Tokens are automatically mounted inside pods.

> **Note:** Kubernetes manages **Service Accounts** but does not allow managing **User Accounts** (e.g., creating or listing them). User management is external to Kubernetes.

---

### **Kubernetes Authentication Flow**

1. All operations on the API server are authenticated.
2. The `kubeconfig` file auto-fetches request credentials, so specifying the server, token, or certificate manually for every request is unnecessary.
3. Each node uses a certificate to enable secure communication (TLS) with the API server.

### **Certificates in Kubernetes**

- Components requiring certificates:
    - Clients: `kubectl`, `scheduler`, `kube-proxy`, `kube-controller-manager`.
    - Servers: `etcd`, `kubelet`.
- **Certificate Authority (CA):**
    - Required for signing certificates.
    - It's possible to have separate CAs for components like etcd.

### **Adding a User with Certificate**

To onboard a user with a certificate for API server access:

1. Generate a private key:
    
    ```bash
    openssl genrsa -out dev-user.key 2048
    ```
    
2. Create a Certificate Signing Request (CSR):
    
    ```bash
    openssl req -new -key dev-user.key -out dev-user.csr -subj "/CN=dev-user/O=developers"
    ```
    
3. Submit the CSR to the Kubernetes CA for approval:
    
    ```bash
    kubectl certificate approve <csr_name>
    ```
    
4. Generate the signed certificate:
    
    ```bash
    openssl x509 -req -in dev-user.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out dev-user.crt -days 365
    ```
    
5. Add the user details to `kubeconfig`:
    
    ```bash
    kubectl config set-credentials dev-user --client-certificate=dev-user.crt --client-key=dev-user.key
    ```
    
6. Assign roles using RBAC (covered below).

---

### **API Groups**

Kubernetes API groups organize resources:

1. **Core API (`/api/v1`)**
    - Resources: `namespaces`, `pods`, `secrets`, `services`, `configmaps`, `nodes`, etc.
2. **Named API Groups (`/apis`)**
    - Example groups:
        - `/apps/v1`: `deployments`, `replicasets`, `statefulsets`.
        - `/networking.k8s.io/v1`: `networkpolicies`.
        - `/storage.k8s.io/v1`: `storageclasses`.
        - `/authentication.k8s.io/v1`: Authentication resources.
        - `/certificates.k8s.io/v1`: Certificate management.

---

### **Authorization**

Kubernetes authorization modes determine access after authentication:

1. **Node Authorization**
    - Grants permissions to nodes for performing their specific operations.
2. **Attribute-Based Access Control (ABAC)**
    - Define access policies in a JSON file.
    - Deprecated in favor of RBAC.
3. **Role-Based Access Control (RBAC)**
    - Defines roles and binds them to users or groups.
4. **Webhook**
    - External services determine authorization.

#### **Authorization Modes**

- `AlwaysAllow`: All requests are allowed (default).
- `AlwaysDeny`: All requests are denied.

---

### **RBAC (Role-Based Access Control)**

RBAC allows fine-grained access control by defining roles and binding them to subjects (users, groups, or service accounts).

#### **Role Definition Example**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["create"]
```

#### **RoleBinding Example**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devuser-developer-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```

#### **Verify Role Assignments**

To verify if a user has the necessary permissions:

```bash
kubectl auth can-i <action> <resource> --as <user>
```

Examples:

```bash
kubectl auth can-i create pods --as dev-user
kubectl auth can-i delete nodes --as dev-user
```


#### **Service Accounts**
- for bots
- may be for prometheous, jenkins
- some command s with service accounts , creation and steps
- flow of account creation, token generation, lkingin that token to that servcie account
- each namespace has it's default service acc, that default account secret is attached to the pod
---

### Additional Notes

- Kubernetes communicates securely between all components using TLS encryption.
- Service accounts are commonly used for automation or bots, while users are typically developers or administrators.
