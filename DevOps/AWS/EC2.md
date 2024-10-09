EC2 - Elastic Compute Cloud
### Services associated with EC2
- data store: EBS
- Load balancer: ELB
- Scaling: ASG ( Auto Scaling Group )


### Instance types
1. General Purpose
	- Kinds: `Mac, T4g, T3, T3a, T2, M5, M5n, M4, A1`
2. Compute Optimized
	- Kinds: `c6g, c6gn, cs, c5a, c5n, c4`
3. Memory Optimised
	- High performance on dbs
	- distributed cache-stores
	- real-time processing with big unstructured data
	- kinds: `R6g, R5, R5a, X1e, X1, High Memory, z1d`
4. Storage Optimized
	- DB's
	- cache for in-memory dbs ( redis )
	- warehouse
	- dfs
	- kinds: `i3, i3en, D2, D3, D3en, H1`
- Decoding of EC2 types `m5.2xlarge`
	- m: instance class
	- 5: generation
	- 2xlarge: size of the instance


### Security Groups
- Firewall for EC2 instances can be integrated along with VPC
- Better to have dedicated security group for ssh access
- Regulates on
	- port access
	- authorized ip ranges
	- control of inbound and outbound traffic
	> We can have security groups as inbound roules, i.e granting other instances with those security groups to connect via network irrespective of ip
	![[Pasted image 20241007114349.png]]


### EC2 purchase options
- On-demand instances ( pay for what we use )
- Reserved ( 1 & 3 years, upto 72% discounts  )
- Savings Plan ( commitment on usage )
- Spot Instances ( cheaper but less reliable )
	- requires max price
	- suitable for batch jobs
- Dedicated hosts ( entire physical server , most expensive)
- Dedicated instances ( no customers will share your hardware )
- Capacity Reservations ( reserved capacity , no discounts & cancel anytime )
> ![[Pasted image 20241007123104.png]]


### Placement Groups
- To gain control over EC2 instance placement strategy
	- Strategies
		- Cluster: into  low-latency grp in single AZ
		- Spread: minimizing risks so spread across different hardware in multiple AZ
		- Partition - partition spread across racks
- path to do `launch instance -> advanced options -> placement group name`

### Elastic Network Interfaces (ENI)
- Logical version of Network Interface Card in a `VPC`
- Will be attached via Eth0 to the EC2
- bound to specific AZ
- ![[Pasted image 20241007145635.png]] 

### EC2 hibernate
- in-memory state is preserved
- instant boot, ( means os is not stopped )
- ram is written in root EBS volume
- use cases: long-running process
- supported instance families `c3, c4, c5, i3, M3, M4, R3, R4, T2, T3`
- system ram should be <150bb
- should be not more than 60 days


--- 
### Storage Options

#### EBS Volumes
- Elastic Block Store, a network drive
- only be mounted one instance at a time
- Since it's on network, will be having latency bottlenecks, but can be easily detached and attached to another instance quickly
- ==locked to AZ==
- transfering btwn different AZ can be done via `snapshot`

#### EBS Volume types
- `gp2 / gp3 SSD` general purpose ==bootvolume==
- `io1 / io2 SSd` low latency ( for critical workloads ) ==bootvolume==
- `ST HDD` lost cost, designed for frequently access  `T - throughput optimised`
- `sc HDD` lost cost, designed for less frequent access `C - cold HDD`

#### EBS Multi Attach
- EBS volumes can be attached to multiple EC2, all instances can do R/W at the same time, since EBS can be attached to multiple instances we use `EBS multi attach`
- Should be using file system that's cluster-aware `XFS & EXT4`

### EBS Snapshot
- Snapshot Archive - 75% cheaper than usual snapshot, restoring cost is high
- Recycle bin, with specified retention ( 1 day to 1 year )
- Fast Snapshot Restore (FSR), Forceful initialization of snapshot, no latency on the first use ( expensive of all )


### EFS - Elastic File System
- 3x expensive than gd2 EBS
- Pay per use
- works on multi-AZ
- Highly Available & scalable
- only works iwth linux based AMI

#### EFS - Performance & Storage Classes
- Perfornamce Mode:
	- General Purpose (default)
	- Max I/O - higher latency & throughtput
- throughtput Mode
	- Bursting 
	- Provisioned
	- Elastic ( for unpredictable workloads )

- Storage Calsses
	- Standard - for frequently accessed files
	- Infrequent access ( EFS-IA )
	- Archive - 50% cheaper
	> we can implement life cycle such that un accessed files from EFS-Standard can move to EFS-IA upon conditions ![[Pasted image 20241008100238.png]]


## EC2 Instance Store
- Physically attached disk
- Better I/O performance
- Highly coupled with EC2, not as durable as EBS
- can be better for buffer / cache / temp content
- risk of data loss if hardware fails
--- 
### Scalability & High Availability

#### Vertical scalablity ( scale up / down)
- increasing size of instances i.e `t2.micro to t2.macro`
#### Horizontal Scalability ( scale out / in)
- Adding more instances, instead of single instance resources 
- Implies Distributed Systems

#### High Availability
- Ability to survive datacenter loss
### ELB - Elastic Load Balancer, aws managed LB

#####  why LB
- spreads loads
- expose single point of access ( DNS )
- does regular health checks on instances
- High availability across zones


#### Health Checks
- ELB health checks done on :4567/health on an ec2
- if not 200 (Ok) then instances in unhealthy


#### Types
- Classic Load Balancer ( deprecated )
	- supports: Http, Https, TCP, SSL
- Application Load Balancer
	- supports: Http, Https, websocket
- Network Load Balancer
	- Tcp, Tls & UDP
	- ==High performant==
- Gateway Load Balancer 
	- operates on IP level
	- ==Mostly of security & firewall==
> Note: LB's can be set as private & public

#### Application Load Balancer ALB ( V2 )
- Routing: via `path, hostname, query strings`
- Better for micro services & container based applications
- has port mapping redirect to a dynamic port in ECS
- `Target Groups`: group of instances of 
	1. EC2 instances
	2. ECS tasks
	3. Lambda functions
	4. IP Addresses
	> Note:  in ALB health checks are one as target group level,
	> as for the EC2 instance to know about clients creds i.e ip & port, i can fetch those only from the headers `X-forwarded-For` & `X-Forwarded-Port` for port
	
#### Security
- LB has public access, where as the instances only set allowed source from LB

### Network load balancer 
- On network leve `TCP & UDP`
- High performance
- Ultra low latency


### Sticky Sessions
- each client's request will stick to particular instance path
- sticky sessions will be done with the help of cookies

#### Cross - Zone Load Balancing
![[Pasted image 20241008181632.png]]
- Application Load Balancer
	- Enabled by default ( can be disabled at the Target Group level )
	- No charges for inter AZ data ( usually happens with other services )
- Network Load Balancer
	- charges for inter AZ data
---
## Auto Scaling Group

### Scaling Policies
- Dynamic Scaling
	- Target Tracking Scaling
		- easy, i.e config as avg cpu at 40%
	- Simple / Step scaling
		- when cloud watch alarm is triggered ( cpu > 70% ) then add 2 units
		- when cpu < 30%
	
- Scheduled scaling
	- Scaling based on known usage patterns
	- eg: increase min capacity to 10 at 5 pm on Friday's
- Predictive Scaling
	- ![[Pasted image 20241009105644.png]]
### Template properties
- AMI + instances Type
- EC2 user data
- EBS volume
- Security Groups
- SSH Key pair
- IAM roles
- Network or Subnet info
- Load Balancer info
