- Each container connected to a private virtual network `bridge`
- Each virtual network routes through NAT firewall on host IP
- All containers on a virtual network can talk to each other without `-p`

- We also have support to multiple network [drivers ](https://docs.docker.com/engine/network/drivers)
	- bridge

### We can make container
- to point to no network
- to point to multiple networks
- to skip & use host IP `--net=host`

### Default network behavior
```mermaid

graph TD
    subgraph "Host Network (192.168.1.0/24)"
        Host[Host Machine<br>IP: 192.168.1.100]
    end

    subgraph "Docker Default Bridge Network (172.17.0.0/16)"
        Container1[Container 1<br>IP: 172.17.0.2]
        Container2[Container 2<br>IP: 172.17.0.3]
    end

    Host <-- "NAT" --> Container1
    Host <-- "NAT" --> Container2
    Container1 <-- "Bridge Network" --> Container2


```

### Port routing with host
```mermaid
graph LR
    subgraph External_Network ["External Network"]
        Client["Client<br/>IP: Public"]
    end
    subgraph Host_Machine ["Host Machine<br/>IP: 192.168.1.100"]
        Host_OS["Host OS"]
        subgraph Docker_Environment ["Docker Environment"]
            Container["httpd Container<br/>IP: 172.17.0.2<br/>Port: 80"]
        end
    end
    Client -- "Request to<br/>192.168.1.100:8080" --> Host_OS
    Host_OS -- "Forward to<br/>172.17.0.2:80 via NAT" --> Container
    Container -- "Response" --> Host_OS
    Host_OS -- "Response" --> Client

```

### Via CLI Management
- show networks: `docker network ls`
- inspect a network: `docker newtwork inspect`
- create a network: `docker network create --driver`
- attach a network to container: `docker network connect`
 - detach a network from container: `docker network disconect`


### DNS
- docker uses container names as to id to talk with each other, automatic DNS resolution with container names
- routing based on round robin, here spinned up two `elasticsearch` servers, each access on round robin fashon via curl on same docker network
![[Pasted image 20241113120639.png]]

