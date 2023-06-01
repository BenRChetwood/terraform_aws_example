# Using Terraform with AWS
a simple example for setting up resources on AWS using Terraform


To get up and running I've followed the instructions here.
I've added the relevant info to this readme too, in case this page becomes unreachable
https://courses.devopsdirective.com/terraform-beginner-to-pro/lessons/02-terraform-overview-and-set-up/02-terraform-set-up-with-aws#install-and-set-up-terraform-with-aws

# Useful terminology
AMI
-The AMI resource allows the creation and management of a completely-custom Amazon Machine Image
Terraform Provider
-Providers are a logical abstraction of an upstream API. They are responsible for understanding API interactions and exposing resources. This is what allows Terraform to connect with your platform of choice.
Terraform Registry 
-Where you can find information about the many providers, such as AWS https://registry.terraform.io/providers/hashicorp/aws/latest
-resources you can define for each, such as S3 buckets https://registry.terraform.io/modules/terraform-aws-modules/s3-bucket/aws/latest
	
# Installing Terraform
Excellent instructions for installing and verifying Terraform can be found here
https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli

# Authenticating with AWS
The tutorial will ask that you create an AWS user with a collection of permissions. As we data engineers are not able to create IAM users even in Sandbox, we can just authenticate using our own federated user in whatever way you prefer.
*If you're going to play with this config and terraform configs - ensure you only do so in a sandbox account.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Basic usage

### Creating a simple TF config
in the repo from the tutorial, an example terraform config is provided, that defines a v simple setup (see main.tf) which contains just the AWS provider and an EC2 instance resource (using an Ubuntu 20.04 AMI and the t2.micro instance type.)

### Initialising Terraform Backend
Navigate to the directory where the main.tf file resides, and run ```terraform init```

This will initialise the Terraform backend and state storage., using whichever provider plugins have been defined (in this case "hashicorp/aws").

### What does terraform init actually do?
Initialising the backend will usually be a one-time action, which writes a lock file (.terraform.lock.hcl) to the the directory where init was run. 

Some OS (eg Ubuntu) may default to hiding these files, if you want to see them you can use keyboard shortcut "Ctrl + L". You will also notice that a directory ".terraform" has been created by the init command.
Alternatively, if you prefer cli you can use ```tree -a``` before and after ```terraform init``` to see the files and directories that init creates.

Run terraform init to download the necessary providers and store them in the .terraform directory. 
The .terraform.lock.hcl file contains information about the installed dependencies and providers

Modules, reusable Terraform code bundles, are also downloaded and stored in the .terraform directory.


### Viewing Changes
Run ```terraform plan``` to view the changes Terraform will make to your infrastructure.

### Making changes / Creating resources
Run ```terraform apply``` to create the specified resources. Confirm the action when prompted.

* Note that if you use the provided config in main.tf, the ec2 instance will be created in us-east-1. So if you want to have a look in the console at the created resource, don't forget to change region.

### Deleting resources
To clean up resources and avoid unnecessary costs, run ```terraform destroy``` and confirm the action when prompted.

# Terraform state management

### State file - resource objects
This is what terraform knows of the resources it has deployed. 
In this example, it will contain details of the EC2 that was deployed, including all sorts of metadata, such as arn which contains the instance id generated when deployed.
the state is stored as a JSON file
It will contain sensitive info, like database passwords that were generated when resources built, so treat the file in a suitable way.
One way of dealing with sensitive data, whilst also making the file available for multiple people to work with, is to store it in s3. This is a common way of using Terraform.

### State file - data objects
There's also a concept of data objects within the state file, also represented in JSON. This can contain API data or static data for use elsewhere.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Ways of working

### Local backend
- Good
	- simple (running ```terraform init``` on your local machine as described above)
- Bad
	- sensitive values stored in plain text
	- Difficult to collaborate
	- Manual
### Remote backend
- Good
	- Sensitive values encrypted
	- Collaborative
	- Automation is possible
- Bad
	- More complex, though not really more that AWS IAC in Cloudformation
		
		
	
	

