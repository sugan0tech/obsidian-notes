---
tags: []
---

- For Better & Unified auditing across all the aws resources
- Records configuration and it's change over time
> Some important use cases can be 
> `How many unauthorized ssh-access occured to my instances?`
> `Do any of my buckets have public access?`
> `Has any of my ALB configuration changed over time?`

- ==Per Region Service==

- No Free Tier, $ 0.0003 per config recorded in a region & $0.0001 per config rule `eval`/`per region`


### Config Rules
- 75 predefined AWS managed Config rules
- custom
	- eg: `evaluate if my EBS have type gp2`, `evaluage is my ec2 instances are t2.micro`



### Use case flow
![[Pasted image 20241020101852.png]]