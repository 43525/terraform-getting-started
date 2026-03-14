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
## 1. Introducing Iac and Terraform
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

> Using Azure Hand-On, 
``` console
cloud [ ~ ]$  RGROUP='1-8c47860b-playground-sandbox'
az vm create \
    --resource-group $RGROUP \
    --name MyVm \
    --image Ubuntu2404 \
    --size Standard_D2s_v5 \
    --admin-username adminuser \
    --generate-ssh-keys \
    --location eastus

cloud [ ~ ]$  ssh adminuser@52.240.54.93
adminuser@MyVm:~$ 
```
Installing terraform, from `https://developer.hashicorp.com/terraform/downloads`
``` console
adminuser@MyVm:~$ 
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform

...
Unpacking terraform (1.14.7-1) ...
Setting up terraform (1.14.7-1) ...
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

## 2. Terraform Configuration Files and Syntax
### Globomabtics Scenario
### Terraform Configurations
### Terraform Objects and Blocks
### Terraform Block Types
### Terraform the Base Configuration

## 3. Deploy Infrastructure with Terraform
### Initializing the Configuration
### Running Terraform Init
### Olanning the Deployment
### Deployinh the Infrastructure
### Destroying the Infrastructure

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
