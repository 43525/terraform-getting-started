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

> Using Amazon Hand-On,  > EC2 > Instances > Connect

Installing terraform, from `https://developer.hashicorp.com/terraform/downloads`  
Amazon Linux
``` console
globo_web_app $  sudo yum install -y yum-utils shadow-utils

globo_web_app $  sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo

globo_web_app $  sudo yum install terraform
...
Terraform has been successfully initialized!
```

``` console
adminuser@MyVm:~$ terraform version
Terraform v1.14.7
on linux_amd64
adminuser@MyVm:~$ terraform -h
...
Main commands:
  init          Prepare your working directory for other commands
  validate      Check whether the configuration is valid
  plan          Show changes required by the current configuration
  apply         Create or update infrastructure
  destroy       Destroy previously-created infrastructure
...
```
get software 
``` console
adminuser@MyVm:~$ git clone https://github.com/ned1313/Getting-Started-Terraform.git
```
##### Change the Prompt to be Shorter   
To stop the path from being long when you ls, you can shorten your Bash prompt (PS1).
- **Use \W instead of \w**: In your `~/.bashrc`, change PS1 to show only the current directory name, not the whole path.   Example: `export PS1='\W $ '`
- **Use PROMPT_DIRTRIM**: To keep paths long but capped, add `PROMPT_DIRTRIM=1` to your `~/.bashrc`. 

```
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
[m3_commands.sh](commands/m3_commands.sh)
``` console
adminuser@MyVm:~/Getting-Started-Terraform$ mkdir globo_web_app
adminuser@MyVm:~/Getting-Started-Terraform$ cp ./base_web_app/main.tf ./globo_web_app/main.tf
adminuser@MyVm:~/Getting-Started-Terraform$ ls
CHANGELOG.md  README.md     commands       m4_solution  m6_solution
LICENSE       base_web_app  globo_web_app  m5_solution  s3_bucket_create
```
`terraform init`
``` console
adminuser@MyVm:~/Getting-Started-Terraform$ cd globo_web_app

globo_web_app$  terraform init
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
globo_web_app$ 
```
This will create a lock file
``` console
globo_web_app$  ls -a
.  ..  .terraform  .terraform.lock.hcl  main.tf
globo_web_app $ ls .terraform/providers/registry.terraform.io/hashicorp/aws
5.100.0
```

### Planning the Deployment
terraform plan
- Loads configuration from working directory
- Loads state data into memory
- Compares configuration and state
- Creates an execution plan
  - Save with -out option

``` console
globo_web_app $ aws configure
AWS Access Key ID [****************SVV3]: 
AWS Secret Access Key [****************Zoqp]: 
Default region name [None]: 
Default output format [None]: 
globo_web_app $ 
```
run the plan command to see what Terraform will do.
``` console
globo_web_app $ terraform plan -out m3.tfplan
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
globo_web_app $ terraform show m3.tfplan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
...
Plan: 7 to add, 0 to change, 0 to destroy.
globo_web_app $ 
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

Now use the plan file to apply the changes.
``` console
globo_web_app $ terraform apply m3.tfplan
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
globo_web_app $ 
```
Grab the Public IP Address from the Instances (without name). In the browser: http://34.239.124.4/

### Destroying the Infrastructure
terraform destroy
- Destroys all resources from state
- Use caution!
- Alias for `terraform apply -destroy`
- Create plan with `terraform plan -destroy`

If you are done, you can tear things down to save $$
``` console
globo_web_app $ terraform destroy
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
globo_web_app $ 
```

## 4. Using Inputs and Outputs
### Globomantics Scenario
### Input Variables
### Adding Variables to the Configuration
### Output Values
### Format and Variable
### Supplying Variable Values
### Deploying the Upload Configuration

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
