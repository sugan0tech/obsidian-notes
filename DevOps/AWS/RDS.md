#RelationDatabaseService

### can manage
- Postgres
- MySql
- MariaDB
- Oracle
- Microsoft SQL Server
- IBM DB2
- Auroara ( by aws )


#### Advantage over using RDS than EC2
- Automated provisioning OS patching
- Continuous backups
- Monitoring dashboards
- RR 
> RDS RR in ==same region== no FEE
- Multi AZ setup for Disaster Recovery
	- All under same DNS & the backup will be in ==SYNC replication ==
- Maintenance windows for upgrades
- Scaling capability ( vertical & horizontal )
- Storage backed by EBS

> But RDS don't have SSH options


### Storage Auto Scaling
- RDS will auto scale depends on needs 


### RDS Custom
- Customizable RDS only for `Oracle & SQL Server`
- Confugure OS settings , install patches, enable native features
- Can access underlying EC2 instances with ssh


### Amazon Aurora
- Not an opensource
- compatable with Postgress & MySql
- Cloud Optimised , gives better with performance
- Groups up to 128TB from 10GB
- Can have 15 RR, pretty faster than MySql
- Costs 20% more than RDS

> has 6 copies of data in 3AZ , 4/6 for reads & 3/6 for reads


### RDS Backups
- Automated Backups
	- Daily full backup of db
	- ability to restore to any point in time ( oldest backup to 5 mns ago )
	- 1 to 35 days retention, set 0 to disable automated backups
	> in Aurora can't be disabled & instant recovery
- Manual DB Snapshots
	- Manually triggered by the user
	- Retention of backup is as long as you want

> We can restore MySQL RDS db from S3



### Amazon ElastiCache 
- for in-memory databases
- better read internsive workloads
- `make your application stateless`
- AWS takes care of OS maintanance, patching, optimizations, setup, configuration, monitoring, failure recovery and backups
- Types: 
	- Redis
		- `Muti AZ`
		- `supporst Sets & Sorted Sets`
		- `RR for better reads`
		- `Presistant`
	- Memcached
		- Multi-node partitioning of data ( sharding )
		- No high availability ( replication )
		- Non presistant
		- No backup & restore
		- Multi threaded architecture

> Using ElastiCache requires heavy application code changes



---
## Picking Better Database
### Questions to choose the right database based on your architecture:


• Read-heavy, write-heavy, or balanced workload? Throughput needs? Will it change, does it need to scale or fluctuate during the day?
• How much data to store and for how long? Will it grow? Average object size?How are they accessed?
• Data durability? Source of truth for the data?
• Latency requirements? Concurrent users?
• Data model? How will you query the data? Joins? Structured? Semi-Structured?
• Strong schema? More flexibility? Reporting? Search? RDBMS / NoSQL?
• License costs? Switch to Cloud Native DB such as Aurora?