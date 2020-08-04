# Terraform 

Is a **infrastructure as code** tool focused on facilitated tha administration of resources (mainly cloud) being able to have version control over it. It uses a `.tfstate` file to keep a mapping between your terraform scripts and the current Id's and other specific data related to the resources.

The main advantages offered by Terraform are:
* Easily repeatable
* Easy readable
* Operational certainty with “terraform plan"
* Standardized environment builds
* Quickly provisioned development environments
* Disaster recovery

Another ones
* Platform agnostic
* State management
* Operator Confidence

Before start using `terraform` some prerequisites must be acomplished.

* AWS CLI
* AWS credentials configured locally

First, you'll need to set up your AWS credentials in orther to let CLI seeing them, frequently you will do it using [`aws configure`](aws_cli/configure.md) command, but with **terraform 0.12 I found some issues trying to use shared credentials with profiles** so I ended using environment variables. [More information here](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)

Some frequent commands to be used when you work with terraform are:

```bash
> terraform init
# Once you have defined your `tf` file. This command will get the right plugins (providers) to find and manage the resources.

> terraform fmt
# Format your configuration, the commands will return the files it formatted.

> terraform validate
# Helps to make sure your configuration is syntactically valid and internally consistent, it will check and report errors within modules, attribute names and value types.

> terraform apply
# Check the infrastructure description and show the execution plan to follow in order to match that description with your real infrastructure. And you are asked for confirmation before to execute it.

> terraform show
# After applying your changes to the infra you can inspect the tfstate file using this command, you will be able to see IDs and other data about the real infrastructure.
```

**Note**: Terraform 0.11 and earlier require running `terraform plan` before `terraform apply`. Use `terraform version` to confirm your running version.

If you need to make some work over the state you can use `terraform state` combined with one of its subcommands:

```bash
> terraform state list
# Display a list of the resources handled by the state.
```

Destructive actions are also part of terraform:

```bash
> terraform destroy
# Terminates all resources listed in the tfstate file. And only those resources described in tf files will be affected.
```