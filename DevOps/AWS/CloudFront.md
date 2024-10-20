#CDN
- 216 Point of Presence globally `Global Edge network`
- DDoS protection, integration with Shield & AWS Web Application Firewall


### Origins
- S3 bucket
	- for distributing files & caching them at the edge
	- enhanced security with OAC
- Custom Origin (HTTP)
	- ALB
	- S3 websites


### Cache Invalidations
- can force an entire or partial cache refresh, bypassing TTL