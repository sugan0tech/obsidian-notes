#GlobalService
### Users & Groups
- Root Account
- Users ( peeps under an organization )
	- users can be without a group 
	- users can be in multiple groups
- Groups ( only contains users not groups )


> IAM provides permissions to user/groups with assigned JSON documents called policies

> ==Least Privilege Principle== aws key thing to consider while giving permission to a user, we can further do this by (==AccessAdvisor== now Last Accessed )by removing stuffs which being not used from the policy

### IAM Policies
- consists of
	- Version: '2012-10-17' date as timestamp
	- Id: "policy id"
	- Statements ( one ore multiple statements )
		- sid: statment id ( opt )
		- Effect: ( "Allow" or "Deny" )
		- Principal: account/user/role to policy applied to
		- Action: list of actions ( allows & denies )
		- Resource: list of resources where the action applied to

### IAM Password Policies
- Better password improves security
	- can define 
		- minimum pass length
		- can allow or deny user to change their passwords
		- require users to change their passwords on time
		- prevent password re-use
		- MFA


### User Access
- AccessKeys ( secret )
- Access Key ID ( as username )
- Secrety Access Key ( as password )

### Roles for Services
- eg:
	- EC2 instance roles
	- Lambda function roles

