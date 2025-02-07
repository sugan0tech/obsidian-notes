### Covered 

- [[AWS Monitoring]]
- [[CloudFront]]
- [EC2](./EC2.md)
- [IAM](./IAM.md)
- [RDS](./RDS.md)
- [Route53](./Route53.md)
- [S3](./S3.md)
- [VPC](./VPC.md)

### To get info about user
1. add user creds to shell
```bash
export AWS_ACCESS_KEY_ID="your-access-key-id"
export AWS_SECRET_ACCESS_KEY="your-secret-access-key"
export AWS_SESSION_TOKEN="your-session-token"

```
2. use this command to get `aws sts get-caller-identity
3. Then the output will give these creds
```json
{
    "UserId": "ABCDEF1234567890EXAMPLE",
    "Account": "123456789012",
    "Arn": "arn:aws:sts::123456789012:user/your-user-name"
}
```
`