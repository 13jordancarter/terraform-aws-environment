# Terraform AWS Environment

## Project Overview

This project demonstrates how to provision and manage AWS infrastructure using Terraform. The environment was built using Infrastructure as Code (IaC) to create a VPC, public subnet, internet gateway, route table, security group, and Amazon Linux EC2 instance.

The goal of this project was to practice defining AWS resources in Terraform, managing dependencies between resources, configuring basic network connectivity, and using the Terraform workflow to deploy, inspect, and destroy infrastructure.

## Architecture

                    AWS
                     │
                    VPC
               10.0.0.0/16
                     │
              Public Subnet
               10.0.1.0/24
                     │
             ┌───────┴───────┐
             │               │
      Route Table        EC2 Instance
             │               │
      Internet Gateway   Security Group
                             │
                       HTTP / HTTPS

## Terraform Resources

| Resource                      | Purpose                                     |
| ----------------------------- | ------------------------------------------- |
| `aws_vpc`                     | Creates the VPC                             |
| `aws_subnet`                  | Creates the public subnet                   |
| `aws_internet_gateway`        | Provides internet connectivity              |
| `aws_route_table`             | Routes internet-bound traffic               |
| `aws_route_table_association` | Associates the route table with the subnet  |
| `aws_security_group`          | Controls EC2 network traffic                |
| `aws_instance`                | Creates the EC2 Linux server                |
| `aws_ssm_parameter`           | Retrieves the current Amazon Linux 2023 AMI |


## Networking

The VPC provides the overall network.
The subnet is where the EC2 instance resides.
The Internet Gateway provides a path to/from the internet.
The route table sends 0.0.0.0/0 traffic to the Internet Gateway.
The subnet is associated with that route table.
The EC2 instance receives a public IP

VPC
10.0.0.0/16

        ↓

Public Subnet
10.0.1.0/24

        ↓

Route Table
0.0.0.0/0 → Internet Gateway

        ↓

EC2
Public IPv4 address


## Security Group

The security group is stateful, with returning traffic allowed connections are automatically permitted. We did not add port 22 SSH due it not being needed for this lab. 

-Inbound
| Protocol | Port | Source      | Purpose |
| -------- | ---: | ----------- | ------- |
| TCP      |   80 | `0.0.0.0/0` | HTTP    |
| TCP      |  443 | `0.0.0.0/0` | HTTPS   |

-Outbound
All protocols
All ports
0.0.0.0/0

## Deployment

Prerequisites:
An AWS account
AWS CLI installed and authenticated
Terraform installed
Git installed

Deployment of this Terraform code included developing the code for each resource based off Terraform Registry documentation and connecting the AWS providers in the providers.tf file. I then built out the main.tf file with resources to build the AWS netowrk environment. I used the variables.tf file to add variables into the main.tf file to add more flexibility with the deployment and to make it more reusable for future deployments of other environments like a development or staging environment. There is also the outputs.tf file that can be run to show what resource specific items were built like public IP address and instace ID of the EC2 instance.

To deploy, steps to complete this were running the terraform commands of init, fmt, validate, plan, and apply.  

## Destroying the Environment

To remove the resources that were built by this terraform project, I ran the terraform destroy command. This checked the local state file for this project and removed any of the resources attached to this. 

## Project Structure

## Project Structure

terraform-aws-environment/
├── main.tf
├── providers.tf
├── variables.tf
├── outputs.tf
├── .gitignore
├── .terraform.lock.hcl
└── README.md


- main.tf - Defines the AWS resources, including the VPC, subnet, internet gateway, route table, security group, and EC2 instance.

- providers.tf - Configures the AWS provider and specifies the required Terraform provider.

- variables.tf - Defines configurable values such as the AWS region, VPC CIDR block, subnet CIDR block, and EC2 instance type.

- outputs.tf - Displays useful information about deployed resources, including resource IDs and the EC2 public IP address.

- .gitignore - Excludes local Terraform files, state files, and variable files from Git version control.

- .terraform.lock.hcl -  Records the selected provider versions to support consistent Terraform runs.

- README.md - Documents the project architecture, resources, networking, security rules, and deployment and cleanup procedures.

