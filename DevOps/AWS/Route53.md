- Highly available, scalable, fully managed  & authoritative DNS `authoritive -> customer can update DNS records`
- Also a Domain Register
- Can check for health
- Only AWS services with 100% availability SLA


#### Record Types
- A - `host name to ipv4`
- AAAA - `host nae to ipv6`
- CNAME - `host name to another hostname`
	- target is a domain name which must have an A or AAAA record
- NS - Name servers for the hosted zone
- Alias Records - `specific to aws, maps to aws resources`


#### Hosted Zones
- Public Hosted Zones
- Private Hosted Zones
> $0.50 per month

### Time To Live
- cache expiration pushed to client side


### Routing Policies
- Simple
	- routes to single resource
	- if multiple - the resource will be chosen random
- Weighted
	- their sum need not to be zero
	- 0 means records to stop sending traffic to resource
- Failover
- Latency based
-  Geolocation
	- Different from Latency, `i.e mostly for content restriction & to comply with localization`

- Multi-Value Answer
- Geoproximity ( using Route 53 Traffic Flow feature )
> it's not same as load balancer, just only responds to DNS queries



### Health Checks
- Can be combined of multiple health checks int he association of `OR, AND & NOT`
- can have up to 256 child health checks
> R53 can't do health check for private endpoints, so we have to use CW metrics & alarm