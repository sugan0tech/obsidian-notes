---
IAC: Infrastructure As Code
---
#IAC
### What is brings into the table
- to create reusable code that can deploy identical set of infra in repeatable fashion
- Cloud as automation configs 
- example use case
	- set of resources ( VM, DB, S3, AWS users) must be created with exact similar configuration in Dev, Staging and Production environment
- Idempotent ( manages state, changed config only updates no recreating ) 
- Low risk of human errors
- version control for infra struct
- speed of infrastructure management

> For each provider terraform provides proper docs, way better than any course : https://registry.terraform.io/providers/hashicorp/aws/latest/docsbb

---
#### Creating a EC2

```json
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}

```
---
## Mutable vs Immutable Infrastructure

- Mutable: A Virtual Machine (VM) is deployed and then a Configuration Management tool like Ansible, Puppet, Chef, Salt or Cloud-Init is used to configure the state of the server
- Immutable: A VM is launched and provisioned, and then and it is turned into a Virtual Image, stored in image repository, that image is used to deployed VM instances
---
## Lifecycle
1. Code
2. Init ( initialize and update latest providers )
3. Plan ( speculate what will change )
	- Represents in execution plan ( change sets)
		- `+` -> addition of resource
		- `~` -> change of resource
		- `-` -> deletion of resource
4. Validate ( validate against types & values are valid )
5. Apply (Execute provison infra)
6. Destroy

> We can visualize the resource relations with this command, using `graphviz` tool set
```bash
terraform graph | dot -Tsvg > graph.svg
```

[[Provisioners]]
[[Terraform Security]]

## Better management practicres

Here’s a summarized version that you can add to your Obsidian notes:

1. **Locals vs Variables**:
   - **Variables**: Accept external input, can be passed between modules. Useful for dynamic values (e.g., `instance_type`).
   - **Locals**: Store calculated or static values within the module. Used for avoiding repetition in code (e.g., `locals { common_tags = {...}}`).

2. **Other Variable Stores**:
   - **Environment Variables**: Pass values with `TF_VAR_` prefix (e.g., `export TF_VAR_instance_type="t2.large"`).
   - **Workspaces**: Manage different environments (e.g., dev/prod) with isolated state files.
   - **Remote State**: Use S3, Terraform Cloud, or other backends to manage state centrally for collaboration.

3. **Organizing Large Terraform Configurations**:
   - **Module-Based Structure**: Reusable modules for specific resources (e.g., EC2, VPC).
   - **Environment-Based Directories**: Separate directories or workspaces for `dev`, `prod`, `staging`.
   - **Separate State Files**: Isolate state for different environments/regions (e.g., use S3 backend with different keys).
   - **Use `terraform.tfvars` Files**: Separate variable files for environments (e.g., `prod.tfvars`, `dev.tfvars`).
   - **Remote State & Terraform Cloud**: Use for managing state across teams with collaboration features.

4. **Best Practices**:
   - **Modular Design**: Break configurations into reusable, self-contained modules.
   - **Environment Segregation**: Use different workspaces or directories for isolated environments.
   - **Version Pinning**: Pin Terraform and provider versions to avoid unexpected updates.
   - **Remote State Management**: Store and manage state in a centralized location with versioning.
   - **Automation**: Integrate Terraform into CI/CD pipelines for automated deployments.
   - **Security**: Use IAM roles, policies, and enforce compliance with policy tools like Sentinel or OPA.
   - **Documentation**: Keep clear documentation of modules, variables, and environments.

---

