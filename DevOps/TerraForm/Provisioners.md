1. **`file`** for pushing files from local to the resource

   ```json
   resource "aws_instance" "web" {
     #
     # Copies the myapp.conf file to /etc/myapp.conf
     provisioner "file" {
       source      = "conf/myapp.conf"    # Local file path
       destination = "/etc/myapp.conf"    # Remote file destination
     }

     # Copies the string in content into /tmp/file.log
     provisioner "file" {
       content     = "AMI used: ${self.ami}"
       destination = "/tmp/file.log"
     }
   }
   ```

2. **`local-exec`** to execute any command on the local machine itself

   ```json
   resource "aws_instance" "web" {
     provisioner "local-exec" {
       command = "echo ${self.private_ip} >> private_ips.txt"
     }

     # Example of using local-exec to run a local script
     provisioner "local-exec" {
       command = "bash ./scripts/cleanup.sh"
     }
   }
   ```

3. **`remote-exec`** to execute any command on the VM resource

   ```json
   resource "aws_instance" "web" {
     provisioner "remote-exec" {
       inline = [
         "sudo apt-get update",
         "sudo apt-get install -y nginx",
       ]
     }

     # Example of using remote-exec with scripts
     provisioner "remote-exec" {
       scripts = [
         "/setup-users.sh",
         "/home/andrew/Desktop/bootstrap"
       ]
     }

     # Using remote-exec to upload and execute a remote script
     provisioner "remote-exec" {
       inline = [
         "chmod +x /home/ubuntu/my_script.sh",
         "sudo /home/ubuntu/my_script.sh"
       ]
     }
   }
   ```
---