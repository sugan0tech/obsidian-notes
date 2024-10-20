---
CW: CloudWatch
---
### CloudWatch Metrics
- Available for every aws service
- Metric is a variable to monitor
- Metrics belong to namespaces ( per service )
- Dimension is attribute of metric ( instance id, env )
- up to 30 dimensions per metric
- metrics have timestamps
- Can create CustomWatch Custom Metrics ( i.e can do for ram )

#### CW Metric Streams
- Continually stream CloudWatch metrics to a destination of your choices
- near-real-time delivery & low latency
- can access it via 3rd party like `New Relic`
![[Pasted image 20241017060917.png]]
---
### CloudWatch Logs
- Place to store application logs
- structure:
	- `LogGroups` : represents an app ( typical )
		- `LogStream`
- Encrypted by default
- can be forwareded to 
	- S3
	- Kinesis Data Stream
	- Firehouse
	- Lambda

### Sources
- SDK
- ECS
- EB from app
- AWS Lambda
- VPC flow logs
- API Gateway
- Cloud Trail
- Route53: ( DNS queries log )


`Logs -> LogGroups -> Streams -> crate metrics namespace & metrics -> metric alarm`



---
### CloudWatch Logs for EC2
- by default no logs from EC2 will go to Cloud Watch
- need to use an Agent to send the logs
- Requires IAM permission
- supports `On Premise`


### Agents
- Logs Agents
	- only send to CloudWatch Logs
- Unified Agent
	- can gather additional system-level metrics ( RAM, Processes)
	- Can collect logs & sends to CW logs
	- Centralized configuration using SSM

#### Unified Agent - Metrics
- CPU ( active, guest, idle ...)
- Disk Metrics 
- RAM
- Netstat
- Processes
- Swap Space


---
### CloudWatch Alarms
- Triggers notification for any metric
- usage
	- Stop, Terminate, Reboot or recover EC2 instances
	- Trigger Auto Scaling Action
	- Send to SNS ( where we can do any thing )
- Composite Alarms
	- Supports AND & OR conditions `eg notify me only if cpu is high & network is low`

---
### Amazon EventBridge
- Schedule: Cron Jobs
- Event Pattern: Event rules to react to a service
- Trigger lambda functions
![[Pasted image 20241017072309.png]]


---
### CloudWatch Container Insights
- ECS, EKS & K8s on EC2
- Logs for Containers
### CloudWatch Lambda Insights
- for Lambda
### CloudWatch Contributor Insights
- See metrics about top-N contributors

### CloudWatch Application Insights
- Stack based Java, .Net, IIS server, db..
- Includes aws resources
- Automated dashboard
- powered by segemaker


---
### CloudTrail
- governance, compilance and audit
- enabled by default
- logs of `Console, SDK, CLI & AWS Services`
![[Pasted image 20241017080704.png]]


#### CloudTrail Events
- Management Events
	- Anything modifies resources & aws account
> Can seperate Read Events from Write Events ( delete, update )
- Data Events
	- Not logged by default
	- like `S3: GetObject, DeleteObject`
	- Lambda Function execution

#### CloudTrail Insights
- $\$
- to  detect unusual activity

### CloudTrail Events Retention
- events are stored for 90 days
- for beyond, log them to S3 and use Athena


--- 
### CloudTrail Event Bridge Integrations

#### Intercept API Calls eg
![[Pasted image 20241017083514.png]]