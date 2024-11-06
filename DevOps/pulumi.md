#IAC 

- IAC as a SDK, supports `Go, Java, C#, Python & Js`
- project will be managed ( state ) by pulumi cli 

## State

By default pulumi maintains it's state in remote backed, `pulumi cloud`. So with this we can see the resources & status through app.pulumi dashboard itself

> Since the default begaviour points towards pulumi cloud, need to have pulumi acc

to have pulumi state in local
```bash
pulumi login --local
```

- To see about the backend 
```bash
pulumi whoami -v
```

- With remote backend, we can point to any data source
```bash
pulumi login s3://<your-bucket-name>
```

---

## Stacks ( workspaces )
A **stack** in Pulumi represents an isolated, independently configurable instance of your infrastructure. Stacks are commonly used to denote different phases of development (such as development, staging, and production) or feature branches (such as feature-x-dev). Each stack has its own state file, allowing you to manage multiple environments or configurations within the same project.

[Pulumi](https://www.pulumi.com/docs/iac/concepts/stacks/)

By utilizing stacks, you can:

- **Isolate Environments:** Maintain separate configurations and resources for different environments, ensuring changes in one environment don't affect others.
    
- **Manage Configurations Independently


- Setting up state
```bash
pulumi stack init prod pulumi up -s prod
```

---
## SDK
Sample code
```go
func main() {
	pulumi.Run(func(ctx *pulumi.Context) error {
		// Create an AWS resource (S3 Bucket)
		bucket, err := s3.NewBucketV2(ctx, "suga-pulumi-bucket", nil)
		if err != nil {
			return err
		}

		// Export the name of the bucket
		ctx.Export("bucketName", bucket.ID())
		return nil
	})
}
```