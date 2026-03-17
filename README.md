###### 69

Updated to alvintwng/genCloud > auto/terraform/terraform-getting-started

# Terraform: Getting Started
Ref: https://github.com/ned1313/getting-started-terraform

PluralSight Ref: https://app.pluralsight.com/ilx/video-courses/terraform-getting-started-5/course-overview

Systems administrators and DevOp engineers have always been charged to do more with less. Defining infrastructure in code and automating its deployment helps improve operational efficiency and lower administrative overhead.

In this course, Terraform: Getting Started, you'll learn foundational knowledge of Hashicorp's Terraform software, a toolset for infrastructure automation. 
First, you'll discover what infrastructure as code is and how Terraform implements it. 
Next, you’ll explore Terraform configuration files and the workflow for deploying infrastructure using Terraform. Finally, you’ll learn how Terraform state supports managing the full lifecycle of your infrastructure. 
When you're finished with this course, you'll have the base knowledge required to get started with using Terraform.

1. [Introducing Iac and Terraform](#module-1)
2. [Terraform Configuration Files and Syntax](#module-2)
3. [Deploy Infrastructure with Terraform](#module-3)
4. [Using Inputs and Outputs](#module-4)
5. [Using Expressions and Functions](#module-5)
6. [Exploring Terraform State](#module-6)

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
> 1. EC2 > Create key pair, Name: `keyvm1` > Key pair type: ED25519 for Linux > `mv ~/Downloads/*.pem .`
> 2. EC2 > Instances > Launch Instances > Name: `My Vm1` > Amazon Linux >Key Pair: keyvm1 > Launch
> 3. Click the instance (i-076f3edbde4bb4283) > Connect > ssh ...

If using the Amazon Cloud Shell. Upload the ssh key.
``` console
$ chmod 400 keyvm1.pem 
$ ssh -i keyvm1.pem ubuntu@ec2-13-220-90-17.compute-1.amazonaws.com
ubuntu@ip-172-31-27-35:~$ 
```
On my mac
``` console
antw@Mac-mini 2025gen %  mv ~/Downloads/*.pem .
antw@Mac-mini 2025gen %  chmod 400 keyvm1.pem 
antw@Mac-mini 2025gen %  ssh -i keyvm1.pem ubuntu@ec2-13-220-90-17.compute-1.amazonaws.com
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
[ec2-user@ip-172-31-29-4 ~]$  git clone https://github.com/ned1313/Getting-Started-Terraform.git
[ec2-user@ip-172-31-29-4 ~]$  cd Getting-Started-Terraform
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
``` console
adminuser@MyVm:~/Getting-Started-Terraform$ cd base_web_app
adminuser@MyVm:~/.../base_web_app$ ls
main.tf
```
base_web_app/main.tf (removed uncessary comments)
``` tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

######### PROVIDERS #########
provider "aws" {
  region     = "us-east-1"
}

######### DATA #########
data "aws_ssm_parameter" "amzn2_linux" {
  name = "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2"
}

######### RESOURCES #########
# NETWORKING #
resource "aws_vpc" "app" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true

}

resource "aws_internet_gateway" "app" {
  vpc_id = aws_vpc.app.id

}

resource "aws_subnet" "public_subnet1" {
  cidr_block              = "10.0.0.0/24"
  vpc_id                  = aws_vpc.app.id
  map_public_ip_on_launch = true
}

# ROUTING #
resource "aws_route_table" "app" {
  vpc_id = aws_vpc.app.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.app.id
  }
}

resource "aws_route_table_association" "app_subnet1" {
  subnet_id      = aws_subnet.public_subnet1.id
  route_table_id = aws_route_table.app.id
}

# SECURITY GROUPS #
# Nginx security group 
resource "aws_security_group" "nginx_sg" {
  name   = "nginx_sg"
  vpc_id = aws_vpc.app.id

  # HTTP access from anywhere
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # outbound internet access
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# INSTANCES #
resource "aws_instance" "nginx1" {
  ami                    = nonsensitive(data.aws_ssm_parameter.amzn2_linux.value)
  instance_type          = "t3.micro"
  subnet_id              = aws_subnet.public_subnet1.id
  vpc_security_group_ids = [aws_security_group.nginx_sg.id]
  user_data_replace_on_change = true

  user_data = <<EOF
#! /bin/bash
sudo amazon-linux-extras install -y nginx1
sudo service nginx start
sudo rm /usr/share/nginx/html/index.html
sudo cat > /usr/share/nginx/html/index.html << 'WEBSITE'
<html>
<head>
    <title>Taco Team Server</title>
</head>
<body style="background-color:#1F778D">
    <p style="text-align: center;">
        <span style="color:#FFFFFF;">
            <span style="font-size:100px;">Welcome to the website! Have a &#127790;</span>
        </span>
    </p>
</body>
</html>
WEBSITE
EOF

}
```

 . . . . . . . . . . . . . . . . . . . [Go to Top :arrow_up:](#69)
###### module 3
## Deploy Infrastructure with Terraform

### Initializing the Configuration
terraform init
- Prepare configuration for use
- Find required provider plug-ins
- Configure state backend
  - Uses working directory by default
- Rerun when state backend, providers, or modules change

### Running Terraform Init
1️⃣ [m3_commands.sh](commands/m3_commands.sh)  

```
cd ~/Getting-Started-Terraform
mkdir globo_web_app
cp ./base_web_app/main.tf ./globo_web_app/main.tf
```
``` console
[ec2-user@ip-172-31-29-13 Getting-Started-Terraform]$ mkdir globo_web_app
[ec2-user@ip-172-31-29-13 Getting-Started-Terraform]$ cp ./base_web_app/main.tf ./globo_web_app/main.tf
[ec2-user@ip-172-31-29-13 Getting-Started-Terraform]$ ls
CHANGELOG.md  LICENSE  README.md  base_web_app  commands  globo_web_app  m4_solution  m5_solution  m6_solution  s3_bucket_create
[ec2-user@ip-172-31-29-13 Getting-Started-Terraform]$  cd globo_web_app
```
2️⃣ **`terraform init`**
``` console
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

> get the keys from AWS Sandbox
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
``` console
[ec2-user@ip-172-31-17-96 globo_web_app]$ ls -a
.  ..  .terraform  .terraform.lock.hcl  m3.tfplan  main.tf  terraform.tfstate
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

``` console
[ec2-user@ip-172-31-17-96 globo_web_app]$ ls -a
.   .terraform           m3.tfplan  terraform.tfstate
..  .terraform.lock.hcl  main.tf    terraform.tfstate.backup
[ec2-user@ip-172-31-17-96 globo_web_app]$ cat terraform.tfstate
{
  "version": 4,
  "terraform_version": "1.14.7",
  "serial": 17,
  "lineage": "25a7af69-7960-b8a3-fb67-f9eea4ce7c5e",
  "outputs": {},
  "resources": [],
  "check_results": null
}
```

 . . . . . . . . . . . . . . . . . . . [Go to Top :arrow_up:](#69)
###### module 4
## Using Inputs and Outputs
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
Ref: [m4_commands.sh](commands/m4_commands.sh)

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
`vim main.tf`
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

Outputs Syntax
```
output "name_label" {
  value        = value
  description  = "string"
}
```

`vim` **`outputs.tf`**  
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

⭐ **`terraform validate`**
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

`vim` **`terraform.tfvars`**  
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
> Browser: `http://ec2-100-26-149-51.compute-1.amazonaws.com/`  
> Welcome to the website! Have a 🌮

``` console
[ec2-user@ip-172-31-17-96 globo_web_app]$ ls -a
.   .terraform           m3.tfplan  main.tf     terraform.tfstate         terraform.tfvars
..  .terraform.lock.hcl  m4.tfplan  outputs.tf  terraform.tfstate.backup  variables.tf
[ec2-user@ip-172-31-17-96 globo_web_app]$ cat terraform.tfstate
{
  "version": 4,
  "terraform_version": "1.14.7",
  "serial": 25,
  "lineage": "25a7af69-7960-b8a3-fb67-f9eea4ce7c5e",
  "outputs": {
 ...
```

  . . . . . . . . . . . . . . . . . . . [Go to Top :arrow_up:](#69)
###### module 5
## Using Expressions and Functions
### Globomantics Scenario
Potential Improvement
- Default tags and naming convention
- Move startup script to a file
- Make public DNS a ful URL

### Local Values
- Internal temporary values
- Replace repeated values
- Data transformtaion

Locals Syntax, main.tf
```
locals {
  key = value
}
```
Example
```
locals {
  instance_prefix = "globo"
  common_tags = {
    company      = "Globomantics"
    project      = var.project
    billing_code = var.billing.code
  }
}
```
Terraform Locals Reference
- local.<key>
- local.instance_prefix
- local.common_tags.company

### Adding Locals
Globomantics needs common tags on all resources and a naming prefix. They want four tags, 

Common Tags
- Company (Globomantics)
- Project
- Environment
- BillingCode

Variables to Create
- Company
- Project
- Environment
- Billing code

Ref: [m5_commands.sh](commands/m5_commands.sh)  

Added these variables into the configuration  
`vim variables.tf`
``` tf
...
variable "company_name" {
  description = "The name of the company"
  type        = string
  default     = "Globomantics"
}

variable "project" {
  description = "The name of the project"
  type        = string
}

variable "environment" {
  description = "The environment for the deployment (e.g., dev, staging, prod)"
  type        = string
}

variable "billing_code" {
  description = "The billing code for the project"
  type        = string
}
```

##### Interpolation, `${var.project}-${var.environment}`  
Interpolation is a way to ask Terraform to evaluate an expression and turn the result into a string. 
We already have quotes here indicating that we're constructing a string. 

Inside the string, we first want to add a reference to the project.
We do that by adding a dollar sign and then curly braces. 
Inside the curly braces, I'll put var.project. 
This syntax tells Terraform that it should take whatever it finds between the curly braces, interpret the expression, and render it as a string. 

Next, we're going to add a dash after the curly braces and another reference to the environment value stored in the variable environment. 
Now we've created a string from our two variables that is of the form project‑environment. 
With our common_tags local value defined, let's apply these tags to our resources. 

Added these variable to local.tf   
`vim locals.tf`
``` tf
locals {
  common_tags = {
    Company     = var.company_name
    Project     = var.project
    Environment = var.environment
    BillingCode = var.billing_code
  }
  naming_prefix = "${var.project}-${var.environment}"
}
```

Update main.tf  
`vim main.tf`
``` tf
# NETWORKING #
resource "aws_vpc" "app" {
  cidr_block           = var.vpc_cidr_block
  enable_dns_hostnames = var.vpc_enable_dns_hostnames

  tags = local.common_tags
}
```
Note that the aws_route_table_association resource doesn't support tags, so skip that one. 

Add variables into terraform.tfvars  
`vim terraform.tfvars`
``` tf
http_port         = 80
ec2_instance_type = "t3.micro"
project           = "tacowagon"
environment       = "dev"
billing_code      = "8675309"
```

### Functions and Expressions
Terraform Expressions
- Literal expressions
- Object or attribute references
- Arithmetic and logical operators
- Conditional expressions
- For expressions

Terraform Functions
- Built-in to Terraform
  - func_name(arg1, arg2, arg3, ...)
- Grouped by category

Function to Use (for Globomantics)
- Startup script
  - templatefile(file_path, { map of variables })
- Consisten naming
  - lower(local.naming_prefix)
- Add tags to common tags
  - merge(local.common.tags, { map of additional tags})

### Testing Functions with Terraform Console
**`terraform console`**, exit by ctrl-d
``` console
[ec2-user@ip-172-31-19-92 globo_web_app]$ terraform console
> min(4,5,16)
4
> lower("TACOCAT")
"tacocat"
> local.common_tags
{
  "BillingCode" = "8675309"
  "Company" = "Globomantics"
  "Environment" = "dev"
  "Project" = "tacowagon"
}
> exit
[ec2-user@ip-172-31-19-92 globo_web_app]$ 
```

### Adding Functions to the Configuration
``` console
mkdir templates
cd templates
vim startup_script.tpl
```
startup_script, moves from the man.tf, and add the placeholder
``` sh
#! /bin/bash
sudo amazon-linux-extras install -y nginx1
sudo service nginx start
sudo rm /usr/share/nginx/html/index.html
sudo cat > /usr/share/nginx/html/index.html << 'WEBSITE'
<html>
<head>
    <title>Taco Team Server - ${environment}</title>
</head>
<body style="background-color:#1F778D">
    <p style="text-align: center;">
        <span style="color:#FFFFFF;">
            <span style="font-size:100px;">Welcome to the ${environment} website! Have a &#127790;</span>
        </span>
    </p>
</body>
</html>
WEBSITE
```
`cd .. ` && `vim main.tf`
``` tf
...
resource "aws_vpc" "app" {
  cidr_block           = var.vpc_cidr_block
  enable_dns_hostnames = var.vpc_enable_dns_hostnames

  tags = merge(local.common_tags, { Name = lower("${local.naming_prefix}-vpc") })
}
...
  user_data_replace_on_change = true

  user_data = templatefile("./templates/startup_script.tpl", {
    environment = var.environment
  })

}
```
Check the function
``` console
[ec2-user@ip-172-31-19-92 globo_web_app]$ terraform console
> merge(local.common_tags, { Name = lower("${local.naming_prefix}-vpc") })
{
  "BillingCode" = "8675309"
  "Company" = "Globomantics"
  "Environment" = "dev"
  "Name" = "tacowagon-dev-vpc"
  "Project" = "tacowagon"
}
>  exit
```
`vim outputs.tf` edit the DNS value
``` tf
output "aws_instance_public_dns" {
  description = "Public DNS hostname of the EC2 instance"
  value       = "http://${aws_instance.nginx1.public_dns}:${var.http_port}"
}
...
```

### Applying the Updates
``` console
[ec2-user@ip-172-31-19-92 globo_web_app]$ terraform plan -out m5.tfplan
...
      ~ user_data                            = "cb0913cf765ab03a0e0efe30a565273bbb8826c4" -> "ccb34672460b6a9ae4871e7212579f51cd686b70" # forces replacement
      + user_data_base64                     = (known after apply)
...
Plan: 1 to add, 1 to change, 1 to destroy.

Changes to Outputs:
  ~ aws_instance_public_dns = "ec2-54-196-70-138.compute-1.amazonaws.com" -> (known after apply)

─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Saved the plan to: m5.tfplan

To perform exactly these actions, run the following command to apply:
    terraform apply "m5.tfplan"
[ec2-user@ip-172-31-19-92 globo_web_app]$ 
```
``` console
[ec2-user@ip-172-31-19-92 globo_web_app]$ terraform apply m5.tfplan
aws_instance.nginx1: Destroying... [id=i-08109ca1e270add39]
...
aws_vpc.app: Modifying... [id=vpc-0822023bc137ffbfd]
aws_vpc.app: Modifications complete after 0s [id=vpc-0822023bc137ffbfd]
aws_instance.nginx1: Creating...
aws_instance.nginx1: Still creating... [00m10s elapsed]
aws_instance.nginx1: Creation complete after 13s [id=i-032ff9181c6cc4e3b]

Apply complete! Resources: 1 added, 1 changed, 1 destroyed.

Outputs:

aws_instance_public_dns = "http://ec2-100-30-238-205.compute-1.amazonaws.com:80"
public_subnet_id = "subnet-05158e5b5e97065a5"
vpc_id = "vpc-0822023bc137ffbfd"
[ec2-user@ip-172-31-19-92 globo_web_app]$
```
Clickable Browser ipaddr: http://ec2-100-30-238-205.compute-1.amazonaws.com:80  
> Welcome to the dev website! Have a 🌮

``` console
[ec2-user@ip-172-31-19-92 globo_web_app]$ ls -a
.           .terraform.lock.hcl  m4.tfplan  outputs.tf         terraform.tfstate.backup
..          locals.tf            m5.tfplan  templates          terraform.tfvars
.terraform  m3.tfplan            main.tf    terraform.tfstate  variables.tf
```

 . . . . . . . . . . . . . . . . . . . [Go to Top :arrow_up:](#69)
###### module 6
## Exploring Terraform State
### What's is Terraform State?
You can think of state as Terraform's memory. 
State data contains entries for your resources, data sources, and outputs. 
State data is what maps the resources and data sources in your configuration to the target environment.

Within state data, each resource and data source is mapped from the identifier in the configuration to a unique value for that object in the real world. 
The unique identifier depends on the object type. 
For instance, the AWS instance resource uses the EC2 instance ID. 
Also included are all the attributes of the resource or the data source. 

Outputs hold the output name and the value associated with that output. 
The state data also contains metadata about the version of Terraform used, the version of the state data format, and the serial number of the current state data. 
These are internal implementation details that you don't need to worry about. 

Terraform state data is stored in a JSON format.

Each time Terraform generates an execution plan, it loads the configuration and state data into memory and refreshes the attribute values for resources and data sources by querying the deployment environment. 
Then it compares the contents of state data and the contents of the configuration.
Terraform's goal is to make state match the desired configuration. 

The output of this process is the execution plan with the necessary changes. 
When Terraform is executing an operation that interacts with state data like a Terraform plan or an apply, it places a lock on the data so no other instance of Terraform can make changes. 

Imagine if the state data was in a shared location and two admins tried to make conflicting changes at the same time. 
That's not good. Locking prevents that situation from arising. 
If the state is currently locked, Terraform will let you know and provide information about the lock, like who placed it and when it was placed. 

### State Storage Options
Backend Options
- Local backend
  - Default setting
- Remote backend
  - S3, Azure Storage, Google CLoud Storage
  - HCP Terraform and consul
- Features
  - Locking
  - Workspaces
- Data encryption
- Access control

Backend Block, backend.tf
```
terraform {
  backend "backend_type" {
    IDENTIFIER = LITERAL_VALUE
  }
}
```
Example
```
terraform {
  backend "s3" {
    bucket      = "globo-state-1234"
    key         = "dev/terraform.tfsate"
    region      = "us-east-1"
    access_key  = "HSGDGSJD"
    secret_key  = "aadasdasd"
  }
}
```
Some settings like the access_key, secret_key, and region can be sourced from environment variables or AWS CLI settings, so we can remove those three from the block.
You may also not know the exact details of the back end or want to make the configuration reusable across multiple environments. 
Instead of hard coding any of the information, we can instead supply the values as part of the terraform init command. 
What we're left with is just a backend block of type s3. This is called a partial configuration. 
```
terraform {
  backend "s3" {
    bucket      = "globo-state-1234"
    key         = "dev/terraform.tfsate"
  }
}
```
Partial Configuration  
- Using in-line settings
  - terraform init -backend-config="bucket=globo-state-12345"
- Using a backend config file
  - terraform init -backend-config="backend-settings.txt"

### Globomantics Scenario and State Manipulation
As the web app project picks up steam, John from operations would like to make sure that others can collaborate on the infrastructure and that the state data is safeguarded from data loss. 

Storing it on your workstation is not meeting those requirements, so instead, you've agreed to move the state data to an S3 bucket. 
John will provision and configure the bucket for you and provide you with the region, bucket name, and key to use. 
It's up to you to update the configuration to support it and migrate the state data off your workstation. 
You've also decided to use a partial configuration to support future environments. 

State Data Migration
- Use S3 bucket provided by Ops
- Migrate existing state data
- Use a partial configuration

State Inspection Commands
- Display all state data
  - show will print the contents of state to the terminal in a human‑readable format. Add the ‑json flag to have the output be in JSON format instead.  
  - `terraform show` 
- List all resources and data sources
  - show you all the resources and data sources in state. The output will be the identifiers of each object.
  - terraform state list 
- List all attributes of a single object
  - Based on that output, you can run the terraform state show command along with the identifier of interest to see more information about that single object.
  - terraform state show ADDR 
  - terraform state show aws_vps.main
- Show outputs stored in state
  - Without any arguments, it will print all the defined outputs to the terminal. You can add an output name to the command to only have that output value printed.
  - terraform output NAME

In addition to the state commands, there are three block types that allow you to manipulate the contents of state.

Moved Block, moved.tf
```
moved {
  from = ADDRESS
  to = ADDRESS
}
```
Import BLock, imports.tf
```
import {
  to = ADDRESS
  id = "UNIQUE_IDENTIFIER"
}
```
Removed Block, removed.tf
```
removed {
  from = ADDRESS

  lifecycle {
    prevent_destroy = false
  }
}
```

### 🤔  Migrating State Data
Ref: [m6_commands.sh](commands/m6_commands.sh)  

`cd ../s3_bucket_create`
``` console
[ec2-user@ip-172-31-29-4 globo_web_app]$ cd ..
[ec2-user@ip-172-31-29-4 Getting-Started-Terraform]$ cd s3_bucket_create
[ec2-user@ip-172-31-29-4 s3_bucket_create]$ ls
main.tf  outputs.tf  terraform.tf  terraform.tfvars.example  variables.tf
```
Initialize and apply the S3 bucket.  
> Take note on bucket_name, and bucket_region.
``` console
[ec2-user@ip-172-31-29-4 s3_bucket_create]$ terraform init

[ec2-user@ip-172-31-29-4 s3_bucket_create]$ terraform plan -out bucket.tfplan

[ec2-user@ip-172-31-29-4 s3_bucket_create]$ terraform apply bucket.tfplan
...
bucket_name = "taco-wagon20260316085944190600000001"
bucket_region = "us-east-1"
[ec2-user@ip-172-31-29-4 s3_bucket_create]$ ls
bucket.tfplan  outputs.tf    terraform.tfstate         variables.tf
main.tf        terraform.tf  terraform.tfvars.example
```
🤔 We'll get back two outputs, the bucket_name and the region. We'll use that information to set up the remote back end. 

Now that we're adding more to the terraform block, perhaps we should move it to its own file. 
It's pretty common to see a terraform.tf file in most configurations. 
So I'm going to create a new terraform.tf file and copy the terraform block out of the main.tf file. 

Now we can add the *back‑end block* inside of the terraform block. 
The type of back end is s3, and inside the block, I'll set the bucket name to the output from the configuration. 
I'll copy and paste that value, and now I'm going to set the region to us‑east‑1.
The other value I can set is the key, but we may want to use this configuration for multiple environments, so we can leave that key unset here and pass the value during initialization. 

The block is all set, but I'm not quite ready to use it, so I'm going to comment it out for now, and before we migrate the state, let's try out some of the state commands.

`cd ../globo_web_app`  
`vim terraform.tf` , cut and paste from main.tf. And add these two outputs to bucket "s3". Commented.
``` tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  #backend "s3" {
  #  bucket = "taco-wagon20260316085944190600000001"
  #  region = "us-east-1"
  #}
}
```
`vim main.tf`. Can commented off, or delete these.
```
#terraform {
#  required_providers {
#    aws = {
#      source  = "hashicorp/aws"
#      version = "~> 5.0"
#    }
#  }
#}
```
##### To navigate the data state
From vscode  
`terraform-getting-started/globo_web_app/terraform.tfstate`
``` tf
{
  "version": 4,
  "terraform_version": "1.14.7",
  "serial": 8,
  "lineage": "c0f900ca-c6fb-07b5-286d-5fcb7759ad21",
  "outputs": {
    "aws_instance_public_dns": {
      "value": "ec2-54-224-136-192.compute-1.amazonaws.com",
      "type": "string"
    },
...
```

`cd ../globo_web_app` `terraform show`   
Well, that is certainly easier to read than the JSON data, but it's also a little overwhelming. 
``` console
[ec2-user@ip-172-31-29-4 s3_bucket_create]$ cd ../globo_web_app
[ec2-user@ip-172-31-29-4 globo_web_app]$ terraform show
...
Outputs:

aws_instance_public_dns = "http://ec2-3-239-246-204.compute-1.amazonaws.com:80"
public_subnet_id = "subnet-068132660411cf68e"
vpc_id = "vpc-04a91dea284a7f04d"
```
`terraform state list` to get a list of all the resource and data source identifiers. 
``` console
[ec2-user@ip-172-31-29-4 globo_web_app]$ terraform state list
data.aws_ssm_parameter.amzn2_linux
aws_instance.nginx1
aws_internet_gateway.app
aws_route_table.app
aws_route_table_association.app_subnet1
aws_security_group.nginx_sg
aws_subnet.public_subnet1
aws_vpc.app
```
The object we're interested in is aws_instance.nginx1. So we can now run terraform state show and copy and paste that address from the list. 
It's still a lot of information, but definitely better than trying to parse a ton of JSON.
``` console
[ec2-user@ip-172-31-29-4 globo_web_app]$ terraform state show aws_instance.nginx1
```

##### To migrate our state data
The process of migrating. 
First, update your configuration with the necessary backend block, which we've already done, and then you run terraform init. 
Start by uncommenting that backend block so we can make use of it. 

`cd ../globo_web_app` `vim terraform.tf` , uncommenting bucket "s3"
``` tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  backend "s3" {
    bucket = "taco-wagon20260316085944190600000001"
    region = "us-east-1"
  }
}
```

Update the globo_web_app backend to use the S3 bucket.  
Since we didn't specify a value for the key argument in the backend block, I'll add in the flag ‑backend‑config=, and then inside of there, put the key value pair, which is "key=dev.tfstate".

`terraform init -backend-config="key=dev.tfstate"`
``` console
[ec2-user@ip-172-31-29-4 globo_web_app]$ terraform init -backend-config="key=dev.tfstate"
Initializing the backend...
Do you want to copy existing state to the new backend?
  Pre-existing state was found while migrating the previous "local" backend to the
  newly configured "s3" backend. No existing state was found in the newly
  configured "s3" backend. Do you want to copy this state to the new "s3"
  backend? Enter "yes" to copy and "no" to start with an empty state.

  Enter a value: yes


Successfully configured the backend "s3"! Terraform will automatically
use this backend unless the backend configuration changes.
Initializing provider plugins...
- Reusing previous version of hashicorp/aws from the dependency lock file
- Using previously-installed hashicorp/aws v5.100.0

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
[ec2-user@ip-172-31-29-4 globo_web_app]$ 
```
To confirm
- `terraform.tfstate` will be empty
  ``` console
  [ec2-user@ip-172-31-28-65 globo_web_app]$ cat terraform.tfstate
  [ec2-user@ip-172-31-28-65 globo_web_app]$ 
  ```
- `terraform show`, we'll get valid response

``` console
[ec2-user@ip-172-31-28-65 globo_web_app]$  terraform show
...
Outputs:

aws_instance_public_dns = "ec2-54-224-136-192.compute-1.amazonaws.com"
public_subnet_id = "subnet-0f9f2dafa868a1884"
vpc_id = "vpc-007817db769066e85"
```
> note on the aws_instance_public_dns.

`terraform plan`
``` console
[ec2-user@ip-172-31-28-65 globo_web_app]$ terraform plan
...
No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found no
differences, so no changes are needed.
[ec2-user@ip-172-31-28-65 globo_web_app]$ 
```
Browser: http://ec2-54-224-136-192.compute-1.amazonaws.com/  
`Welcome to the website! Have a 🌮`

``` console
[ec2-user@ip-172-31-17-96 globo_web_app]$ ls -a
.           .terraform.lock.hcl  m4.tfplan  outputs.tf    terraform.tfstate         variables.tf
..          locals.tf            m5.tfplan  templates     terraform.tfstate.backup
.terraform  m3.tfplan            main.tf    terraform.tf  terraform.tfvars
```

> #### If all done, and to quit.
Tear down the deployments to save costs

Globo Web App 
- **`terraform destroy -auto-approve`**

the S3 bucket
- `cd ../s3_bucket_create` 
  `terraform destroy -auto-approve`


### Next Steps
The point of this course was to give you a solid foundation and enough knowledge to get up and running with Terraform. 

The other courses in the learning path focus on different aspects of Terraform, and you have a chance to choose your own adventure. 
I'd recommend starting with Terraform: Variables and Outputs. 
We touched on both in this course, but we didn't even get into variable validation, creating complex data types or setting values as sensitive. 
If you're interested in building reusable Terraform configurations, then the Modules course would be perfect for learning how to consume modules from the public registry and write your own. 

If you're looking to dig deeper into the Terraform language, then the Terraform: Advanced HCL course is where you should go next. 
You'll learn about using count and for_each meta‑arguments, building for expressions, and leveraging provider‑defined functions. 


---
 . . . . . . . . . . . . . . . . . . . [Go to Top :arrow_up:](#69)
