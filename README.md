###### 69
# Terraform: Getting Started
Ref: https://github.com/ned1313/getting-started-terraform

PluralSight Ref: https://app.pluralsight.com/ilx/video-courses/terraform-getting-started-5/course-overview

Systems administrators and DevOp engineers have always been charged to do more with less. Defining infrastructure in code and automating its deployment helps improve operational efficiency and lower administrative overhead.

In this course, Terraform: Getting Started, you'll learn foundational knowledge of Hashicorp's Terraform software, a toolset for infrastructure automation. 
First, you'll discover what infrastructure as code is and how Terraform implements it. 
Next, you’ll explore Terraform configuration files and the workflow for deploying infrastructure using Terraform. Finally, you’ll learn how Terraform state supports managing the full lifecycle of your infrastructure. 
When you're finished with this course, you'll have the base knowledge required to get started with using Terraform.

1. Introducing Iac and Terraform
2. Terraform Configuration Files and Syntax
3. Deploy Infrastructure with Terraform
4. Using Inputs and Outputs
5. Using Expressions and Functions
6. Exploring Terraform State

 . . . . . . . . . . . . . . . . . . . [Go to Top :arrow_up:](#69)
###### module 1
## Introducing Iac and Terraform
### Iac Fundamentals
Provisioning instrastructure through software to achieve consistent and preditable environments.

Core Concepts of IaC
- Defined in code
- Stored inDe source control
- Declarative vs. imperative
- Idempotent and consistent

Imperative
```
# Make me a taco
get shell
get beans
get cheese
get letture
get salsa

put beans in shell
put cheese on beans
put lettuce on cheese
put salsa on lettuce
```
Declarative
```
# Make me a taco
food "taco" "bean_taco" {
  ingredients = [
    "beans", 'cheese',
    "lettuce", "salsa"
  ]
}
```
#### Idempotent
Let's say my niece, who also loves tacos, has asked me to get her a taco. And I do it. I say, here, here's a taco. 

Now, in an idempotent world, if she asks me again to make her a taco, I will go, hey, you already have a taco. I'm not going to make her another taco because I'm aware of her state and the fact that she already has a taco. 

If she gives me the same instruction again, I'm not going to do anything because her instruction already matches the state of the world she wants. 
She wants a taco, and she has the taco. 
In a non‑idempotent world, each time she told me to make her a taco, I would make and give her another taco. 

Terraform implements idempotence by recording the state of your environment and comparing it to your configuration. As long as the two match, no changes will be made.

### What is Terraform?
HashiCorp Terraform
- Infrastructure automation tool
- Terraform Community
- Platform agnostic
- Single binary compiled from Go
- Declarative syntax
- HashiCorp Configuration Language (HCK) or JSON
- No centrlal server or agent

Core Components
- Terraform binary
- Configuration files
- Provider plugins
- State data

### Terraform Workflow
1. Write . Author the code
2. Plan . Compare code against state
3. Apply . Apply changes and record results

### Installing Terraform
- Download the binary for your platform
- Add to your PATH environment variable
- Share and enjoy!

#### Installation and CLI Basics
Learn about installing Terraform and using the basic command line interfaces.
- github: **`https://github.com/ned1313/getting-started-terraform`** ⭐
  - [m1_commands.sh](commands/m1_commands.sh)
  - You can find the installer info for Terraform here:
    `https://developer.hashicorp.com/terraform/downloads`

> Using Amazon Hand-On  
> [Tutorial 2: Launch a test EC2 instance and connect to it]( https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/tutorial-launch-a-test-ec2-instance.html)  
> Create key pair > ED25519 for Linux

On Amazon Cloud Shell, Upload the ssh key first
``` console
$ chmod 400 keyvm1.pem 
$ ssh -i keyvm1.pem ubuntu@ec2-13-220-90-17.compute-1.amazonaws.com
ubuntu@ip-172-31-27-35:~$ 
```
On my mac, but on next difficult to sudo yum 
``` console
antw@Mac-mini terraform-getting-started % mv ~/Downloads/*.pem .
antw@Mac-mini terraform-getting-started % chmod 400 keyvm1.pem 
antw@Mac-mini terraform-getting-started % ssh -i keyvm1.pem ubuntu@ec2-13-220-90-17.compute-1.amazonaws.com
```

Installing terraform, from `https://developer.hashicorp.com/terraform/downloads`  
⭐ Amazon Linux
``` console
[ec2-user@ip-172-31-29-13 ~]$  sudo yum install -y yum-utils shadow-utils

[ec2-user@ip-172-31-29-13 ~]$  sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo

[ec2-user@ip-172-31-29-13 ~]$  sudo yum install terraform
...
Installed:
  git-2.50.1-1.amzn2023.0.1.x86_64                      git-core-2.50.1-1.amzn2023.0.1.x86_64                 git-core-doc-2.50.1-1.amzn2023.0.1.noarch          
  perl-Error-1:0.17029-5.amzn2023.0.2.noarch            perl-File-Find-1.37-477.amzn2023.0.7.noarch           perl-Git-2.50.1-1.amzn2023.0.1.noarch              
  perl-TermReadKey-2.38-9.amzn2023.0.2.x86_64           perl-lib-0.65-477.amzn2023.0.7.x86_64                 terraform-1.14.7-1.x86_64                          

Complete!
[ec2-user@ip-172-31-29-13 ~]$ 
```

``` console
[ec2-user@ip-172-31-29-13 ~]$ terraform version
Terraform v1.14.7
on linux_amd64
[ec2-user@ip-172-31-29-13 ~]$ terraform -h
...
Main commands:
  init          Prepare your working directory for other commands
  validate      Check whether the configuration is valid
  plan          Show changes required by the current configuration
  apply         Create or update infrastructure
  destroy       Destroy previously-created infrastructure
...
```
0️⃣  **`git clone https://github.com/ned1313/Getting-Started-Terraform.git`**
``` console
[ec2-user@ip-172-31-29-13 ~]$ git clone https://github.com/ned1313/Getting-Started-Terraform.git
```

#### Change the Prompt to be Shorter   
To stop the path from being long when you ls, you can shorten your Bash prompt (PS1).
- **Use \W instead of \w**: In your `~/.bashrc`, change PS1 to show only the current directory name, not the whole path.   Example: `export PS1='\W $ '`
- **Use PROMPT_DIRTRIM**: To keep paths long but capped, add `PROMPT_DIRTRIM=1` to your `~/.bashrc`. 
⭐
``` console
$ cat ~/.bashrc
$ sudo tee -a ~/.bashrc << EOF
PROMPT_DIRTRIM=1
EOF
```

 . . . . . . . . . . . . . . . . . . . [Go to Top :arrow_up:](#69)
###### module 2
## Terraform Configuration Files and Syntax
### Globomantics Scenario
The best way to learn is to start with a real‑world example. 

For this course, pretend that you've just started as an IT ops admin at Globomantics, a global risk assessment company.

For the moment, the application is a basic web app running on a virtual machine. 
Globomantics has recently started using AWS for deploying new applications, and you've been asked to spin up this environment in the us‑east‑1 region. 

This seems like an ideal project to take Terraform for a test run. 
In fact, Sally has already found a basic Terraform deployment file she thinks you could get started with. 

The base configuration Sally found includes the following infrastructure components. 
In the us‑east‑1 region of AWS, a VPC with a single public subnet, and inside that subnet is a single EC2 instance that's running NGINX as a web server. 
The configuration also includes all the necessary routing resources and a security group to allow web traffic to reach the web server. 

### Terraform Configurations

Terraform Configuration Files
- Terraform looks for all the files in the current working directory that have the file extension .tf.json.
- Terraform does not look for additional files in subdirectories,

HashiCorp Configuration Language
- Human readable and writable
- Comment support
- Functions and complex expressions

### Terraform Objects and Blocks
Block Syntax
```
block_type "label" "label" {
  argument_name = expression
  nested_block {
    argument_name = expression
  }
}
```
Use in aws
``` tf
resource "aws_instance" "web_server" {
  name = "web-server"
  ebs_volume {
    size == 40
  }
}
```
Terraform Object Reference  
<resource_type>.<name_label>.<attribute>  
aws_instance.web_server.name

ref: https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance

### Terraform Block Types
Terraform Block Types
- Terraform
- Providers
- Resources
- Data sources

Other Block Types
- Variable
- Output
- Local

### Reviewing the Base Configuration
```
adminuser@MyVm:~/Getting-Started-Terraform$ cd base_web_app
adminuser@MyVm:~/.../base_web_app$ ls
main.tf
```
[main.tf](base_web_app/main.tf)
``` tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

....
```

###### module 3
## Deploy Infrastructure with Terraform

 cd ~/Getting-Started-Terraform/base_web_app 

### Initializing the Configuration
terraform init
- Prepare configuration for use
- Find required provider plug-ins
- Configure state backend
  - Uses working directory by default
- Rerun when state backend, providers, or modules change

### Running Terraform Init
1️⃣ [m3_commands.sh](commands/m3_commands.sh)  
``` console
[ec2-user@ip-172-31-29-13 Getting-Started-Terraform]$ mkdir globo_web_app
[ec2-user@ip-172-31-29-13 Getting-Started-Terraform]$ cp ./base_web_app/main.tf ./globo_web_app/main.tf
[ec2-user@ip-172-31-29-13 Getting-Started-Terraform]$ ls
CHANGELOG.md  LICENSE  README.md  base_web_app  commands  globo_web_app  m4_solution  m5_solution  m6_solution  s3_bucket_create
```
2️⃣ **`terraform init`**
``` console
[ec2-user@ip-172-31-29-13 Getting-Started-Terraform]$ cd globo_web_app

[ec2-user@ip-172-31-29-13 globo_web_app]$ terraform init
Initializing the backend...
Initializing provider plugins...
- Finding hashicorp/aws versions matching "~> 5.0"...
- Installing hashicorp/aws v5.100.0...
- Installed hashicorp/aws v5.100.0 (signed by HashiCorp)
Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```
This will create a lock file
``` console
[ec2-user@ip-172-31-29-13 globo_web_app]$ ls -a
.  ..  .terraform  .terraform.lock.hcl  main.tf
[ec2-user@ip-172-31-29-13 globo_web_app]$ ls .terraform/providers/registry.terraform.io/hashicorp/aws
5.100.0
```

### Planning the Deployment
terraform plan
- Loads configuration from working directory
- Loads state data into memory
- Compares configuration and state
- Creates an execution plan
  - Save with -out option

3️⃣  **`aws configure`**
``` console
[ec2-user@ip-172-31-29-13 globo_web_app]$ aws configure
AWS Access Key ID [****************SVV3]: 
AWS Secret Access Key [****************Zoqp]: 
Default region name [None]: 
Default output format [None]: 
...
```
4️⃣  run the plan command to see what Terraform will do.
``` console
[ec2-user@ip-172-31-29-13 globo_web_app]$  terraform plan -out m3.tfplan
data.aws_ssm_parameter.amzn2_linux: Reading...
...
Plan: 7 to add, 0 to change, 0 to destroy.

Saved the plan to: m3.tfplan
```

Terraform Plan Symbols
- `+` - Resource or attribute will be created
- `-` - Resource or attribute will be destroyed
- `~` - Resource or attribute will be modified in place
- `-/+` - Resource or attribute will be recreated

Check out the plan
``` console
[ec2-user@ip-172-31-29-13 globo_web_app]$  terraform show m3.tfplan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
...
Plan: 7 to add, 0 to change, 0 to destroy.
```

### Deploying the Infrastructure
terraform apply
- Accepts a saved plan file
- Executes changes from plan
- Updates state data contents
- Generates a plan if none submitted

apply without any options first to see the interactive prompt
``` console
globo_web_app $ terraform apply
data.aws_ssm_parameter.amzn2_linux: Reading...
...
Plan: 7 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: no

Apply cancelled.
```

5️⃣  Now use the plan file to apply the changes.
``` console
[ec2-user@ip-172-31-29-13 globo_web_app]$  terraform apply m3.tfplan
aws_vpc.app: Creating...
aws_vpc.app: Still creating... [00m10s elapsed]
aws_vpc.app: Creation complete after 12s [id=vpc-0a7ef1988f9414c73]
aws_subnet.public_subnet1: Creating...
aws_internet_gateway.app: Creating...
aws_security_group.nginx_sg: Creating...
aws_internet_gateway.app: Creation complete after 0s [id=igw-04a9a30b913e5f67a]
aws_route_table.app: Creating...
aws_route_table.app: Creation complete after 1s [id=rtb-0842b8c22ad20f2a0]
aws_security_group.nginx_sg: Creation complete after 3s [id=sg-0f044570db659a985]
aws_subnet.public_subnet1: Still creating... [00m10s elapsed]
aws_subnet.public_subnet1: Creation complete after 11s [id=subnet-0a3fe6738bdeb4089]
aws_route_table_association.app_subnet1: Creating...
aws_instance.nginx1: Creating...
aws_route_table_association.app_subnet1: Creation complete after 0s [id=rtbassoc-03e082c212525c6c4]
aws_instance.nginx1: Still creating... [00m10s elapsed]
aws_instance.nginx1: Creation complete after 13s [id=i-0d9369e1a397cd6c3]

Apply complete! Resources: 7 added, 0 changed, 0 destroyed.
```
🤔 Grab the Public IP Address from the Instances (without name). In the browser: http://34.239.124.4/

### Destroying the Infrastructure
terraform destroy
- Destroys all resources from state
- Use caution!
- Alias for `terraform apply -destroy`
- Create plan with `terraform plan -destroy`

⭐ If you are done, you can tear things down to save $$
``` console
[ec2-user@ip-172-31-29-13 globo_web_app]$  terraform destroy
aws_vpc.app: Refreshing state... [id=vpc-0a7ef1988f9414c73]
data.aws_ssm_parameter.amzn2_linux: Reading...
...
Plan: 0 to add, 0 to change, 7 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes
...
Destroy complete! Resources: 7 destroyed.
[ec2-user@ip-172-31-29-13 globo_web_app]$
```

## 4. Using Inputs and Outputs
### Globomantics Scenario
Sally Sue is excited that you got the environment up so quickly, but the folks over in ops have some requests about how the environment is deployed. 

Potential Improvements
- Replace hard coaded values
- Generate output of public DNS hostname
- Generate outputs for VPC and subnets

### Input Variables
Variable Syntax, `main.tf`
```
variable "name_label" {}

variable "name_label" {
  type         = value
  description  = "string"
  default      = value
}
```
Example
```
variable "billing_tag" {}

variable "aws_region" {
  type         == string
  description  = "Region to use for AWS resources"
  default      = "us-east-1"
}
```
Terraform Variable Reference
- var.<name_label>
- var.aws_region

Terraform Data Types
- Primitive: String, number, bool
- Collection: List, set, map
- Structural: Typle, object

Data Type Examples, list
```
variable "aws_region" {
  type         = list(string)
  description  = "Region to use for AWS resources"
  default      = ["us-east-1", "us-east-2", "us-west-1", "us-wesr-2"]
}
```
Referenccing Collection Values
- var.<name_label>[<element_number>]
- var.aws_region[0]

Data Type Examples, list
```
variable "aws_instance_size" {
  type         = map(string)
  description  = "Instance sizes to use in AWS"
  default      = {
    small  = "t3.micro"
    medium = "m5.large"
    large  = "m4-xlarge"
  }
}
```
Referenccing Collection Values
- var.<name_label>.<key_name>  or  var.<name_label>["key_name"]
- var.aws_instance__sizes.small  or var.aws_instance__sizes["small"]

### Adding Variables to the Configuration
Ref: commands/m4_commands.sh

**`variables.tf`**  
`[ec2-user@ip-172-31-19-92 globo_web_app]$ vim variables.tf`
``` tf
variable "aws_region" {
  description = "The AWS region to deploy resources in"
  type        = string
  default     = "us-east-1"
}

variable "vpc_cidr_block" {
  description = "The CIDR block for the VPC"
  type        = string
  default     = "10.0.0.0/16"
}

variable "vpc_enable_dns_hostnames" {
  description = "Enable DNS hostnames in the VPC"
  type        = bool
  default     = true
}

variable "vpc_subnet_cidr" {
  description = "The CIDR block for the public subnet"
  type        = string
  default     = "10.0.0.0/24"
}

variable "map_public_ip_on_launch" {
  description = "Whether to map public IPs on launch for the subnet"
  type        = bool
  default     = true
}

variable "http_port" {
  description = "The HTTP port for the application"
  type        = number
}

variable "ec2_instance_type" {
  description = "The type of EC2 instance to launch"
  type        = string
}
```
main.tf
``` tf
provider "aws" {
  region     = var.aws_region
}
...
resource "aws_vpc" "app" {
  cidr_block           = var.vpc_cidr_block
  enable_dns_hostnames = var.vpc_enable_dns_hostnames
}
...
resource "aws_subnet" "public_subnet1" {
  cidr_block              = var.vpc_subnet_cidr
  vpc_id                  = aws_vpc.app.id
  map_public_ip_on_launch = var.map_public_ip_on_launch
}
...
  ingress {
    from_port   = var.http_port
    to_port     = var.http_port
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
...
resource "aws_instance" "nginx1" {
  ami                         = nonsensitive(data.aws_ssm_parameter.amzn2_linux.value)
  instance_type               = var.ec2_instance_type
  subnet_id                   = aws_subnet.public_subnet1.id
  vpc_security_group_ids      = [aws_security_group.nginx_sg.id]
  user_data_replace_on_change = true
```

### Output Values
- Printed to terminal after apply
- Stored in state data
- Used by child modules

Outputs Syntax, main.tf
```
output "name_label" {
  value        = value
  description  = "string"
}
```

**`outputs.tf`**  
`[ec2-user@ip-172-31-19-92 globo_web_app]$ vim outputs.tf`
``` tf
output "aws_instance_public_dns" {
  value       = aws_instance.nginx1.public_dns
  description = "Public DNS hostname of the EC2 instance"
}

output "vpc_id" {
  value       = aws_vpc.app.id
  description = "ID of the created VPC"
}

output "public_subnet_id" {
  value       = aws_subnet.public_subnet1.id
  description = "ID of the public subnet"
}
```

### Format and Variable
- Fix formatting to match HCL spec
  - `terraform fmt`
- Check syntax and logic
  - `terraform validate`


Example if adding more space, in main.tf
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version        = "~> 5.0"
    }
...
```
`terraform fmt -check` - to check for format
``` console
[ec2-user@ip-172-31-19-92 globo_web_app]$ terraform fmt -check
main.tf
```
`terraform fmt` - to clecn up
``` console
[ec2-user@ip-172-31-19-92 globo_web_app]$ terraform fmt
main.tf
```
Let intentionally introduce error at main.tf
```
resource "aws_vpc" "app" {
  cidr_block           = var.vpc_cidr_block_dne
  enable_dns_hostnames = var.vpc_enable_dns_hostnames
}
```

**`terraform validate`**
``` console
[ec2-user@ip-172-31-19-92 globo_web_app]$ terraform validate
╷
│ Error: Reference to undeclared input variable
│ 
│   on main.tf line 38, in resource "aws_vpc" "app":
│   38:   cidr_block           = var.vpc_cidr_block_dne
│ 
│ An input variable with the name "vpc_cidr_block_dne" has not been declared. This variable can be
│ declared with a variable "vpc_cidr_block_dne" {} block.
╵
[ec2-user@ip-172-31-19-92 globo_web_app]$
```
After fixed it back
``` console
[ec2-user@ip-172-31-19-92 globo_web_app]$ terraform validate
Success! The configuration is valid.
```

### Supplying Variable Values
Now that our configuration is ready and validated, we need to provide values for variables that don't have defaults. 

- Default value - If you don't submit a value in any of those ways and no default is set, Terraform will prompt you for a value at runtime.
- `-var` flag, `-var-file` flag - to specify either a variable in line or point to a file that has key value pairs for your variable values
- `terraform.tfvars`, `terraform.tfvars.json` - automatically loaded from the working directory if it's found
- `*.auto.tfvars`, `*.auto.tfvars.json` - Terraform will automatically find those files and load the values that are inside
- `TF-VAR_` -  you can pass variable values using shell environment variables. To do so, you start the environment variable with TF_VAR_ and then the name label of the input variable. 

Can you combine them? Yes, yes, you can.
And what if you set the same variable in multiple ways?
Well, Terraform follows an order of precedence with the last one winning.
You can use this to strategically override values for testing. 

### Deploying the Upload Configuration
Looking through our list of input variables, we need values for the HTTP port and EC2 instance type variables.

```
...
variable "http_port" {
  description = "The HTTP port for the application"
  type        = number
}

variable "ec2_instance_type" {
  description = "The type of EC2 instance to launch"
  type        = string
}
```
**`terraform.tfvars`**  
`[ec2-user@ip-172-31-19-92 globo_web_app]$ vim terraform.tfvars`
``` tf
http_port = 80
ec2_instance_type = "t3.micro"
```
> different from video
``` console
[ec2-user@ip-172-31-19-92 globo_web_app]$ terraform plan -out m4.tfplan
data.aws_ssm_parameter.amzn2_linux: Reading...
...
Changes to Outputs:
  + aws_instance_public_dns = (known after apply)
  + public_subnet_id        = (known after apply)
  + vpc_id                  = (known after apply)
```
``` console
[ec2-user@ip-172-31-19-92 globo_web_app]$ terraform apply m4.tfplan
...
Apply complete! Resources: 7 added, 0 changed, 0 destroyed.
Outputs:
aws_instance_public_dns = "ec2-100-26-149-51.compute-1.amazonaws.com"
public_subnet_id = "subnet-093d3778e1a9140ec"
vpc_id = "vpc-0da49c10f750b657c"
[ec2-user@ip-172-31-19-92 globo_web_app]$ 
```
> http://3.84.207.158/ not working


## 5. Using Expressions and Functions
### Globomantics Scenario
### Local Values
### Adding Locals
### Functions and Expressions
### Testing Functions with Terraform Console
### Adding Functions to the COnfiguration
### Applying the Updates

## 6. Exploring Terraform State
### What's is Terraform State?
### State Storage Options
### Globomantics Scenario and State Manipulation
### Migrating State Data
### Next Steps

---
 . . . . . . . . . . . . . . . . . . . [Go to Top :arrow_up:](#69)
