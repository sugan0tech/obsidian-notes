### How does Terraform preserve state?
When you provision infrastructure via Terraform it will create a state file named `terraform.tfstate`
This state file is a JSON data structure with a one-to-one mapping from resource instances to remote objects


- `terraform state list` List resources in the state
- `terraform state mv` Move an item in the state
	- rename a resource, 
	- `terraform state mv packet_device.worker packet_device.helper`
	- moving a resource to a module
- `terraform state pull` Pull current remote state and output to stdout
- `terraform state push` Update remote state from a local state
- `terraform state replace-provider` Replace provider in the state
- `terraform state rm` Remove instances from the state
- `terraform state show` Show a resource in the state

---
# State Backup

By default will be stored in `terraform.tfstate.backup` file locally