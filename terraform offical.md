To deploy infrastructure with Terraform:

Scope - Identify the infrastructure for your project.
Author - Write the configuration for your infrastructure.
Initialize - Install the plugins Terraform needs to manage the infrastructure.
Plan - Preview the changes Terraform will make to match your configuration.
Apply - Make the planned changes.

The terraform destroy command terminates resources managed by your Terraform project. This command is the inverse of terraform apply in that it terminates all the resources specified in your Terraform state. 
It does not destroy resources running elsewhere that are not managed by the current Terraform project.

The current configuration includes a number of hard-coded values. Terraform variables allow you to write configuration that is flexible and easier to re-use.

Add a variable to define the instance name.

Create a new file called variables.tf

Create a file called outputs.tf in your learn-terraform-aws-instance directory.

Add the configuration below to outputs.tf to define outputs for your EC2 instance's ID and IP address.