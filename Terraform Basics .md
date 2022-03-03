# IaC

Ansible, Terraform, CloudFormation, Puppet    

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

KEY = VALUE

Block name - resource type(provider-rosource) - resource name
    Arguments

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
