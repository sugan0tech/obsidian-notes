---
HCL: Hashicorp Configuration Language
---
### Terraform HCL (HashiCorp Configuration Language)

HashiCorp Configuration Language (HCL) offers a middle ground between configuration files (YAML, JSON) and programming languages (Java, Python). HCL is used in Terraform for writing infrastructure-as-code with human-readable configuration files.

---

### Elements of HCL

1. **Blocks**  
   Used to define the structure, containing nested configurations.
   ```hcl
   resource "aws_instance" "example" {
     ami           = "ami-123456"
     instance_type = "t2.micro"
   }
   ```

2. **Arguments**  
   Set values for configuration options inside blocks.
   ```hcl
   resource "aws_instance" "example" {
     instance_type = "t2.micro"  # instance_type is an argument
   }
   ```

3. **Expressions**  
   Used to define values by referencing variables, functions, or other elements.
   ```hcl
   output "instance_id" {
     value = aws_instance.example.id  # Expression referencing resource attribute
   }
   ```

---

### Input Variables

Define reusable values to customize the infrastructure.

```hcl
variable "region" {
  description = "The AWS region"
  default     = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = var.instance_type
}
```

---

### Output Values

Used to extract information from the Terraform state.

- `terraform output` – Lists all outputs.
- `terraform output <val_name>` – Shows a specific output.
- `terraform output -json` – Outputs in JSON format.

```hcl
output "instance_ip" {
  value = aws_instance.example.public_ip
}
```

---

### Data Sources

Used to retrieve information from outside of Terraform.

```hcl
data "aws_ami" "example" {
  most_recent = true
  owners      = ["self"]

  filter {
    name   = "name"
    values = ["example-*"]
  }
}
```

---

### Resource Meta-Arguments

1. **`depends_on`** – Specifies explicit dependencies.
   ```hcl
   resource "aws_instance" "example" {
     depends_on = [aws_security_group.example]
   }
   ```

2. **`count`** – Creates multiple instances of a resource.
   ```hcl
   resource "aws_instance" "example" {
     count         = 3
     instance_type = "t2.micro"
   }
   ```

3. **`for_each`** – Iterates over a map or set.
   ```hcl
   resource "aws_instance" "example" {
     for_each      = { "web" = "t2.micro", "db" = "t2.large" }
     instance_type = each.value
   }
   ```

4. **`provider`** – Specifies a non-default provider configuration.
   ```hcl
   provider "aws" {
     region = "us-west-2"
   }
   ```

5. **`lifecycle`** – Manages resource lifecycle.
   ```hcl
   resource "aws_instance" "example" {
     lifecycle {
       create_before_destroy = true
     }
   }
   ```

6. **`provisioner` and `connection`** – Executes scripts or other actions post-creation.
   ```hcl
   resource "aws_instance" "example" {
     provisioner "local-exec" {
       command = "echo ${self.public_ip} >> ip_addresses.txt"
     }
   }
   ```

---

### Terraform Expressions

1. **Types and Values**  
   Terraform supports basic types (string, number, bool) and complex types (list, map, object).
   ```hcl
   variable "example" {
     type = string
   }
   ```

2. **Strings and Templates**  
   Uses interpolation syntax.
   ```hcl
   "Hello, ${var.name}!"
   ```

3. **References to Values**  
   Refers to variables, resources, or outputs.
   ```hcl
   value = var.region
   ```

4. **Operators**  
   Supports arithmetic, comparison, and logical operators.
   ```hcl
   condition = var.age > 18 ? "adult" : "minor"
   ```

5. **Function Calls**  
   Built-in functions for various operations.
   ```hcl
   length(var.list)
   ```

6. **Conditional Expressions**  
   Evaluates conditions.
   ```hcl
   value = var.enabled ? "enabled" : "disabled"
   ```

7. **For Expressions**  
   Iterates over collections.
   ```hcl
   [for i in var.list : i * 2]
   ```

8. **Splat Expressions**  
   Extracts attributes from multiple instances.
   ```hcl
   aws_instance.example[*].id
   ```

9. **Dynamic Blocks**  
   Generates nested configuration blocks dynamically.
   ```hcl
   resource "aws_security_group" "example" {
     dynamic "ingress" {
       for_each = var.ingress_rules
       content {
         from_port   = ingress.value.from
         to_port     = ingress.value.to
         protocol    = ingress.value.protocol
         cidr_blocks = ingress.value.cidr
       }
     }
   }
   ```

10. **Type Constraints**  
    Specifies variable types.
    ```hcl
    variable "names" {
      type = list(string)
    }
    ```

11. **Version Constraints**  
    Ensures compatibility with specified versions.
    ```hcl
    terraform {
      required_version = ">= 1.0.0"
    }
    ```
