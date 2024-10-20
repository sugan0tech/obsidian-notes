### AWS KMS
- `Key Management Service`
- Seamless integration with aws services
- Stores Encryption keys for us

#### KMS keys
- Symmetric ( AES-256 )
	- Integrated services with KMS use symmetric key
	- KMS api call's can't be done for KMS key retrival without encrypted access
- Asymmetric ( RSA  & ECC pair keys )
	- mostly of outside aws access
- ==Region Specific==
- Multi Region Key
	- Fix a primary location for a key & replicate in it other regions

- Types
	- AWS Owned Keys ( free & default )
	- AWS Managed Keys ( free & aws/service-name )
	- Customer Managed Keys ==created== ( $1/mon )
	- Customer Managed keys imported ( $1/mon )
	- + pay for API calls  to KMS ( $0.03 / 10,000 calls )
	
- Automatic Key rotation
	- AWS Managed automatic ( 1 - year rotation )
	- Customer managed ( must be enabled )
	- Imported keys ( manual rotation )

- Key policies
	- Can restrict access to specific type of users



---
### SSM Parameter Store
- Serverless
- seamless encryption using KMS
- version tracking of configuration & secrets
- Parameters will be stored in ==file structured hierarchy==

#### Tires
| Feature                                      | Standard Tier                    | Advanced Tier                               |
| -------------------------------------------- | -------------------------------- | ------------------------------------------- |
| **Parameter Limit per Account**              | 10,000                           | 100,000                                     |
| **Parameter Size Limit**                     | 4 KB                             | 8 KB                                        |
| **Cost**                                     | Free for standard parameters     | $0.05 per advanced parameter per month      |
| **Secure String Parameters**                 | Yes                              | Yes                                         |
| **API Throughput (Transactions per Second)** | 40 TPS for all operations        | 1,000 TPS for GetParameter (other ops: 100) |
| **Versioning**                               | 100 versions per parameter       | 100 versions per parameter                  |
| **Policies (Expiration, No Change, etc.)**   | Not Supported                    | Supported                                   |
| **Data Types**                               | String, StringList, SecureString | String, StringList, SecureString            |
| **Parameter History**                        | Not Supported                    | Retains all versions (100 by default)       |
| **Parameter Policies**                       | Not Supported                    | Supported (allows setting expiration, etc.) |
| **API Request Rate**                         | 40 transactions per second (TPS) | Up to 1,000 TPS                             |
| **Automation Actions**                       | Limited                          | Full range of actions                       |
| **Higher Availability and Durability**       | No                               | Yes                                         |


#### AWS SSM Parameter Store Commands

 1. Retrieve a Single Parameter
```bash
aws ssm get-parameter --name "parameter_name"
```
 2. Retrieve a Single Parameter with Decryption
```bash
aws ssm get-parameter --name "parameter_name" --with-decryption
```
 3. Retrieve Multiple Parameters by Name
```bash
aws ssm get-parameters --names "param_name_1" "param_name_2" --with-decryption
```
 4. Retrieve Parameters by Path
```bash
aws ssm get-parameters-by-path --path "/my-app/config/" --recursive --with-decryption
```
 5. Retrieve Parameters with a Limit
```bash
aws ssm get-parameters-by-path --path "/my-app/config/" --recursive --max-items 5
```

---

### Secret Manager
- For secrets
- Comes with auto rotation
- well integration with RDS
- Can be used in app clients ( java, js, py sdks)

---
###  Web Application Firewall WAF
- Layer 7 firewall
- Deploy on 
	- ALB ==No support for NLB since it's layer 4==
	- CloudFront
	- API Gateway
	- AppSync GraphQL Api
	- Cognito User Pool

#### Process
- Define ACL ( Web Access Control List )
	- Definte IP sets
	- Http header filter for XSS
	- Size constraints
	- Geo match ( allow or block )
	- Rate based rules


---
### AWS Shield
- Specific for DDoS Protection


## Tiers

| Feature                                 | AWS Shield Standard               | AWS Shield Advanced                             |
| --------------------------------------- | --------------------------------- | ----------------------------------------------- |
| **Cost**                                | Free                              | $3000/per-month/per-org                         |
| **DDoS Protection**                     | Yes (automatic for all AWS users) | Yes (enhanced protections)                      |
| **Protection for**                      | All AWS services                  | EC2, ELB, CloudFront, Route 53                  |
| **Attack Detection**                    | Automatic                         | Automatic with advanced threat intelligence     |
| **Response Time**                       | Automatic, immediate              | 24/7 access to AWS DDoS Response Team (DRT)     |
| **DDoS Cost Protection**                | Not included                      | Included for scaling costs due to attacks       |
| **Real-time Metrics and Reporting**     | Basic via CloudWatch              | Detailed reports via AWS CloudWatch, Health API |
| **Web Application Firewall (WAF)**      | Requires separate configuration   | Integration with AWS WAF for deeper protections |
| **Global Threat Environment Dashboard** | Not available                     | Available (access to detailed attack insights)  |

---
### AWS FireWall manager
- To manage all teh Firewall rules in all of accounts of an organization
- 