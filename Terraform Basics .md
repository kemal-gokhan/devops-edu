# IaC

Ansible, Terraform, CloudFormation, Puppet   

terraform = .tf 

### Conf Management:

Ansible 

Puppet 

Saltstack

### Server Templating:

VM or Docker Immages + Immutable Infrastructure

Docker 

Packer 

HashiCorp  Vagrant

### Provisioning Tools

Deploy Immutable Infrastructure + Server DB anyhign

Terraform (VMWARE, AWS, GCP, AZURE, PHYSICAL)

Cloudformation

`curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -`

`sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"`

`sudo apt-get update && sudo apt-get install terraform`

registry.terraform.io

https://learn.hashicorp.com/collections/terraform/aws-get-started?utm_source=terraform_io_download

HCL has BLOCK AND PARAMETERS


hide secrets with the variables

### remote state

terraform.tfstate
terraformtfstate.backup

data "aws_ip_ranges" europena {
  regions = ["eu-west-1"]
}


### Interpolation

Variables: ${var.VARIABLE_NAM}
Resources: ${aws_instance.name.id}
Data Sources: ${data.template_file.name.rendered}

key = value
list: [0,1,2] unique and not repeatable
map: {"key" = "value"}


String VAR -- var.name -> ${var.VARIABLE_NAME}
MAP VAR -- var.MAP["key"] -> ${var.AMIS["us-east-1"]}
MAP VAR -- var.MAP["key"] -> ${lookup(var.AMIS, var.AWS_REGION)]}
LIST VARIABLE -- var.LIST OR var.LIST[i] -> ${var.subnets[i]} OR ${join(",", var.subnets)}

OUTPUTS -- module.NAME.output -> ${module.aws.vpc.vpcid}
COUNT -- count.FIELD -> $ {count.index}
PATH -- path.TYPE -> path.cwd(current directory) path.module (module path) path.root (root module path)
META -- terraform.FIELD -> terraform.env  "shows active workspace"

FILE -- ${file("rsakey.pub")} -> it reads rsakeypub

### FUNCTION: 

basename(path) -> basename("/home/kemal/file.txt") returns file.txt
element(list, index) -> element(module.vpc.public_subnets, count.index) returns a single element
index(list, elem) 
join(delim, list)
list(item1, item2) -> sample: join(":", list("a","b","c")) -> a:b:c
lookup(map, key [default]) -> lookup(map("k","v"), "k","not found") -> returns "v"
map(key,value) map("k", "v", "k2", "v2") -> {"k"="v", "k2"="v2"}
replace(string, search, replace) replace("aaab","a","b") -> bbbb
upper(string) upper("kemal") -> KEMAL
values(map) values(map("k","v","k2","v2") -> ["v", "v2"]




**<u>Block name - resource type (provider-re
  source) - resource name
</u>**    Arguments

First

`terraform init`

`terraform plan`

`terraform apply -yes`

`terraform show`

`terraform destroy`

Official Provider: AWS,Azure,GCP

Verified: Heroku,digitalocean

Community: Ucloud,netapp-gcp

`
    7  terraform init
    8  terraform plan
    9  terraform apply
`

```
resource "aws_instance" "ec2_instance" {

      ami       =  "ami-0eda277a0b884c5ab" 

      instance_type = "t2.large"

}

resource "aws_ebs_volume" "ec2_volume" {

      availability_zone = "eu-west-1"

      size  =    10

}
```

### Variable Example

list, tuple,object, map

```
variables block

description and types are optional

List (index) 

Map (key value)

type = list(string)

Set dont duplicate.

Tuples (you can use different in tuples)

variable "filename" 

data type: list, map, set, tuple, 



variable "name" {
     type = string
     default = "Mark"

}
variable "number" {
     type = bool
     default = true

}
variable "distance" {
     type = number
     default = 5

}
variable "jedi" {
     type = map
     default = {
     filename = "/root/first-jedi"
     content = "phanius"
     }

}

variable "gender" {
     type = list(string)
     default = ["Male", "Female"]
}
variable "hard_drive" {
     type = map
     default = {
          slow = "HHD"
          fast = "SSD"
     }
}
variable "users" {
     type = set(string)
     default = ["tom", "jerry", "pluto", "daffy", "donald", "jerry", "chip", "dale"]


}
```

```
variable "hard_drive" {

     type = map

     default = {

          slow = "HHD"

          fast = "SSD"

     }

var.hard_drive["slow"]

map

resource "local_file" "jedi" {
 filename = var.jedi["filename"]
 content = var.jedi["content"]
}

resource "local_file" "jedi" {

     filename = var.jedi["filename"]

     content = var.jedi["content"]

}
```

## Dictionary

A `**Dictionary**` is a data structure which maps the keys to values.

dict_1 = {'Yatharth':24, 'Akash':23, 'Jatin':19, 'Divyanshu':24}

## Set

A `**Set**` is a data structure which has an unordered collection of elements.

set_1 = {1, 'Yatharth', 'Delhi', 'Knoldus', 24}

## Tuples

A `**Tuple**` is a data structure which has an ordered sequence of elements.

tuple_1 = (1, 'Yatharth', 'Delhi', 'Knoldus', 24)

## Lists

A `**List**` is a data structure which has an ordered sequence of elements.

list_1 = [1, 'Yatharth', 'Delhi', 'Knoldus', 24]

### Using Variable

Environment variables can be used to set variables. The environment variables must be in the format TF_VAR_name and this will be checked last for a value.
export TF_VAR_region=us-west-1

cli -var flag is highest prioirities.

Use the -var-file option with a variable file. The file can be named anything but should always end in either .tfvars or .tfvars.json


### Terraform Commands
terraform apply
terraform destroy
terraform fmt #format
terraform get #download/update module
terraform graph #create a visual representation
terraform plan
terraform push
terraform refresh (remote)
terraform remote #conf remote state
terraform state #rename resource etc
terraform validate #validate syntax
terraform taint


map_public_ip_on_launch will give this subnet a public IP address, which means it'll be a public subnet

```
resource "aws_subnet" "subnet-1" {
    vpc_id = "${aws_vpc.main.id}"
    cidr_block = "10.0.203.0/24"
    map_public_ip_on_launch = "true"
    availability_zone = "us-west-1a"
}
```

If you need dynamic information, you can use datasources instead of variables. Datasources will pull their information from a remote API before it turns into a variable that can be used in *.tf files

The terraform.tfstate can be read easily by opening the terraform.tfstate file in a text editor. You will see that the contents is in the JSON format. It can even be changed manually (if you know what you're doing).

When you remove terraform.tfstate, at the time of writing, terraform doesn't recreate the terraform tfstate file itself automatically. You can use terraform import to manually recreate the terraform.tfstate.
### Resources Attributes

One output for others input.

```
 resource "time_static" "time_update" {
}
 resource "local_file" "time" {
     filename = "/root/time.txt"
     content = "Time stamp of this file is ${time_static.time_update.id} "
}
```

`terraform show`

depends_on for explicit dependency
When we use reference expressions to link resources, the dependency created is called implicit dependency

https://registry.terraform.io/providers/hashicorp/tls/latest/docs/resources/private_key

`terraform state <command>

```
resource "tls_private_key" "pvtkey" {
    algorithm = "RSA"
    rsa_bits = 4096
}

resource "local_file" "key_details" {
    filename = "/root/key.txt"
    content = tls_private_key.pvtkey.private_key_pem

}
```

```
resource "local_file" "whale" {
    filename    = "/root/whale"
    depends_on   = [local_file.krill]
}

resource "local_file" "krill" {
    filename    = "/root/krill"
}
```

You can explain output as well. `terraform output`

```
resource "random_pet" "my-pet" {

  length    = var.length 
}

output "pet-name" {

    value = random_pet.my-pet.id
    description = "Record the value of pet ID generated by the random_pet resource"
}

resource "local_file" "welcome" {
    filename = "/root/message.txt"
    content = "Welcome to Kodekloud."
}

output "welcome_message" {

    value = local_file.welcome.content (Value: content of the resource called welcome)
```

### Terraform State

after first terraform apply, it creates tfstate file to ensure it is exist or not.
State in not optional. Sensitional information like ip's. (cpu, mem)

Remote State.

### Commands

terraform validate
terraform fmt (usefull)
terraform show
terraform providers
terraform providers mirror path
terraform output
terraform refresh (manual update)
sudo install graphviz -y  & terraform graph | dot -Tsvg graph.svg

### Mutable vs Immutable

Immutable is unchanged infras. ngix v.1.17 -> you want to upgrade v1.19 so you can update per vm in sequence then you can come accross mistake while you update server. Hence, delete v1.17 and then install v1.18 (2 server) at the same time. Afterwards, terraform can delete the older version.

To sum up, it destroy first then create newer.

`create_before_destroy`

```
resource "local_file" "file" {

  filename        = var.filename

  file_permission = var.permission

  content         = random_string.string.id

}

resource "random_string" "string" {

  length = var.length

  keepers = {

    length = var.length

  }

  lifecycle {

    create_before_destroy = true

  }

}
```

```
resource "local_file" "file" {
  filename        = var.filename
  file_permission = var.permission
  content         = "This is a random string - ${random_string.string.id}"
  lifecycle {
    create_before_destroy = true
  }

}

resource "random_string" "string" {
  length = var.length
  keepers = {
    length = var.length
  }
  lifecycle {
    create_before_destroy = true
  }

}
```

lifecycle

create_before_destroy

resource "random_pet" "super_pet" {

  length = var.length

  prefix = var.prefix

  lifecycle {

    prevent_destroy = true

  }

}

### Data Sources

output "os-version" {

  value = data.local_file.os.filename

}

data "local_file" "os" {

  filename = "/etc/os-release"

}

### Meta Arguments

depends_on / lifecycle / count

length function is popular to use.

### Count

Since we used count, the resources are now created as `list`.

.tf

```
resource "local_file" "name" {

  filename          = var.users[count.index]

  sensitive_content = var.content

  count             = length(var.users)

}
```

variables.tf

```
variable "users" {

    type = list

}

variable "content" {

    default = "password: S3cr3tP@ssw0rd"

}
```

For each

```
#main tf
resource "local_file" "name" {
    filename = each.value
    for_each = toset(var.users)
    sensitive_content = var.content
}
#variables tf
variable "users" {
    type = list(string)
    default = [ "/root/user10", "/root/user11", "/root/user12", "/root/user10"]
}
variable "content" {
    default = "password: S3cr3tP@ssw0rd"

}
```

`terraform state list`

terraform init install plugin

Version change

```
terraform {
  required_providers {
    local = {
      source = "hashicorp/local"
      version = "2.0.0"
    }
  }
}
```

The following operators are valid:

= (or no operator): Allows only one exact version number. Cannot be combined with other conditions.

!=: Excludes an exact version number.

> , >=, <, <=: Comparisons against a specified version, allowing versions for which the comparison is true. "Greater-than" requests newer versions, and "less-than" requests older versions.

~>: Allows only the rightmost version component to increment. For example, to allow new patch releases within a specific minor release, use the full version number: ~> 1.0.4 will allow installation of 1.0.5 and 1.0.10 but not 1.1.0. This is usually called the pessimistic constraint operator.

aws --endpoint http://aws:4566 iam attach-user-policy --user-name mary --poicy-arn arn:aws:iam::aws:policy/AdministratorAccess
aws --endpoint http://aws:4566 iam create-group --group-name project-sapphire-developers 
aws --endpoint http://aws:4566 iam add-user-to-group --user-name jack --group-name project-sapphire-developers

### Remote backend

s3 for terraform state
dynamodb for state locking

terraform state list
terraform state show ...
state command show state after apply

terraform state list
terraform state mv random_pet.super_pet_1 random_pet.ultra_pet

provisioner "remote-exec" {

inline = [ ""sudo apt update", ...... ]

}

`local-exec` provisioner does not need a connection block as it does not connect to a remote instance to run tasks.
provisioner "local-exec" { #run this command
	command = "echo '15'"
	}
	
	


provisioners: "remote-exec" "local-exec" 

use user_data instead of execs

TF_LOG

TF_LOG_PATH

Trace is detailed option to see logs.

terraform taint <provider.project.name>

terraform untaint aws_instance.ProjectB

### Terraform Input

terraform import <resource_type>.<resource_name> <attribute>

terraform plan

modules provide ready config .tf files to deliver asap.

Sample

```
module "us_payroll" {

source = "../modules/payroll-app"

app_region = "us-east-1"

ami = " ami-2937219421"

}
```

Root Module +

`terraform console`
 `condition ? true_val : false_val`
 `lenght = var.length < 8 ? 8 : var.length

```
resource "aws_iam_user" "cloud" {
    name = split(":", var.cloud_users)[count.index]
    count = length(split(":",var.cloud_users))

}
```

```
 resource "aws_instance" "mario_servers" {
     ami = var.ami
     instance_type = var.name == "tiny" ? var.small : var.large
     tags = {
          Name = var.name

     }

}
```

terraform workspace list

default is  the default workspace.

terraform workspace select <name>



module "payroll_app" {
 source = "/root/terraform-projects/modules/payroll-app" app_region = lookup(var.region, terraform.workspace)
 ami = lookup(var.ami, terraform.workspace)
}
