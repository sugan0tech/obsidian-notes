## Managing Secrets in Terraform

### Using Environment Variables

```json
provider "aws" {
  access_key = var.aws_access_key
  secret_key = var.aws_secret_key
  token      = var.aws_session_token
}

# Variables for AWS credentials
variable "aws_access_key" {}
variable "aws_secret_key" {}
variable "aws_session_token" {}
```

- By default, Terraform will **prompt** for these inputs if not provided.
- To **avoid prompts** and **set credentials securely**, use environment variables in the format:
  
  ```bash
  export TF_VAR_aws_access_key="your-access-key"
  export TF_VAR_aws_secret_key="your-secret-key"
  export TF_VAR_aws_session_token="your-session-token"
  ```

Using through \*.tfvars file
---

This format keeps the content concise and aligned with typical Obsidian notes, making it easy to review and apply the setup.
---

Available IAC tools
1. Infrastructure Orchestration
	- [[TerraForm]]
	- [[CloudFormation]]
2.  Configuration Management
	- [[Ansible]]
	- [[Chef]]
	- [[Puppet]]

