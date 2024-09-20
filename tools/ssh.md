- Secure shell protocol, ( Tunneling protocol )
> Tunneling - defines as encapsulated connection, often as a direct one on one connection between two devices across a mess of network or external medium.
- on top of [[TCP]]
- defaults on port 22
- Common utilities of `OpenSSH`, first introduced on BSD systems as secure alternative for telnet and remote invocation


##### Use Cases
- Remote Access
- File transfer ( secure unless of [[FTP]] )


### Usage
- Connect to a remote server:
```bash
   ssh username@remote_host
```
- With specific ssh key & port
```bash
    ssh -i path/to/key_file username@remote_host -p 2220
```
- forwarding a specific port, from device of localhost port as `9999`, with server `example.org:80`
```bash
    ssh -L 9999:example.org:80 -N -T username@remote_host
```


## Making a container ssh connectable
For Arch Linux, follow these steps to make the device SSH connectable:

### 1. **Install OpenSSH**
```bash
sudo pacman -S openssh
```

### 2. **Start and Enable SSH Service**
Start the SSH service to run immediately, and enable it to start at boot:
```bash
sudo systemctl start sshd
sudo systemctl enable sshd
```

### 3. **Check SSH Status**
Verify if the SSH service is running:
```bash
sudo systemctl status sshd
```

### 4. **Configure Firewall (if applicable)**
If you're using a firewall (e.g., `ufw` or `iptables`), allow SSH traffic:
- For `ufw`:
    ```bash
    sudo ufw allow ssh
    sudo ufw enable
    ```
- For `iptables`:
    ```bash
    sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
    ```

### 5. **Access the Device**
Once the SSH server is running, you can connect to the device from another machine using:
```bash
ssh username@device_ip
```

Now the device should be SSH connectable.


> By default , that plain old ssh will forwards towards password based authentication of a user

Secure SSH best practices
- Instead of Password auth, go for SSH key pair one,  ( like sharing keys )
- we can customize `/etc/ssh/sshd_config`  for specific user & ssh key pair usages
```bash
PasswordAuthentication no
ChallengeResponseAuthentication no
PermitRootLogin no
AllowUsers specific_user1 specific_user2
# can change ports too
Port 2222
```
> Also note some peeps need to update their firewall `sudo ufw allow 2222/tcp`
- providing access to specific keys, add their keys on `~/.ssh/authorized_keys`


### Monitoring
`sudo tail -f /var/log/auth.log`
