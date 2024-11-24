
#IAC 

- IAC as a SDK, supports `Go, Java, C#, Python & Js`
- project will be managed ( state ) by pulumi cli 

## Generating project
	Pulumi comes with a awesome build in cli, which does all the background jobs of setting up our fav lang with SDK installed.

#### Let's dive into this with Go
1. for setting up pulumi with go
```bash
pulumi new go # provider agnostic, for one with prebuilt aws
pulumi new aws-go # will prompt for region to operate
```
2. this will prompt for 
	1. project name 
	2. project description
	3. stack name i.e `environment as dev, qa, prod`
3. will do 
	1. `go mod init` & setups the project
	2. seeds the dependencies in go.mod & runs `go mod tidy` to resolve and download these dependencies
	3. Finally template go file as `main.go`
4. Generates:
	2. `Pulumi.<your_stack>.yaml`, if you have aws region & others config will reside there
	3. `Pulumi.yaml` project info

--- 

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

## Stacks ( workspaces as in TF )
A **stack** in Pulumi represents an isolated, independently configurable instance of your infrastructure. Stacks are commonly used to denote different phases of development (such as development, staging, and production) or feature branches (such as feature-x-dev). Each stack has its own state file, allowing you to manage multiple environments or configurations within the same project.

> [!note]  recommended name format `<organization>/<project>/<stack>`

[Pulumi](https://www.pulumi.com/docs/iac/concepts/stacks/)

By utilizing stacks, you can:

- **Isolate Environments:** Maintain separate configurations and resources for different environments, ensuring changes in one environment don't affect others.
    
- **Manage Configurations Independently


- Setting up state

```bash
pulumi stack init staging
```
- other commands
	- `pulumi stack ls`
	- `pulumi stack select jane-dev`
	- `pulumi stack tag ls` - list tags, `pulumi stack tag set name val` 
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


### Input & outputs
## Stack outputs
A stack can export values as stack outputs. These outputs are shown during an update, can be easily retrieved with the Pulumi CLI, and are displayed in Pulumi Cloud. They can be used for important values like resource IDs, computed IP addresses, and DNS names.

To export values from a stack, use the following definition in the top-level of the entrypoint for your project:
```go
ctx.Export("bucketName", bucket.Id())
```

- to get stack output of current project
`pulumi stack output <otp-output-name>`, includes a `--json` 

- custom stack value output, apart from resource as str & types
```go
ctx.Export("x", pulumi.String("hello"))
ctx.Export("o", pulumi.Map(map[string]pulumi.Input{
    "num": pulumi.Int(42),
}))
```

### Reading outputs from stack [references](https://www.pulumi.com/docs/iac/concepts/stacks/#reading-outputs-from-stack-references)

Stack references support two ways of reading outputs from the referenced stack:

- `getOutput` returns an `Output` that provides gated access to the output value. The output value can be accessed and transformed with methods like `Output.apply`. This is useful when the output is used as an input to another resource.
- `getOutputDetails` returns an `OutputDetails` object that provides direct access to the output value. This is useful when you want to process the output directly in your code.


> [!warning] deleting stack, 1. remove the resource associated with it `pulumi delete` & `pulumi stack rm` - deletes history & stack.yaml too


>[!important] unlike terraform pulumi managest backent state withing s3 itself
