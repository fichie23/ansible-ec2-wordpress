# ansible-ec2-wordpress
This repository contains ansible playbook to provision an AWS EC2 with Wordpress installed

## Pre-requisites
- Install Python [guide](https://www.python.org/downloads/)
- Install pip [guide](https://pip.pypa.io/en/stable/installation/)
- Install ansible [guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- Install boto and boto3 using pip
  `pip install boto`
  `pip install boto3`
- Configure aws credentials in your local machine [guide](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)

## Manual resources need to be setup :
- Make sure your AWS user had enough permission to provision these resources : Security Groups, EC2 and Elastic Load Balancer
- Make sure your AWS environment has already VPC setup and has multiple AZ subnets supported
- Create an EC2 keypair for connect to the instances

## How to run

Run the command using below command :
`ansible-playbook ec2-wordpress-playbook.yml -e @inventories/group_vars/all.yml`

### Docker version
- Checkout the specific branch at `feature/dockerize`
- Install docker engine [guide](https://docs.docker.com/engine/install/)
- Set your aws credentials inside file `aws/credentials`
- Build docker image :
  `docker build -t ansible-ec2-wordpress .`
- Run docker image as container :
  `docker run -dp 80:80 ansible-ec2-wordpress`

## Provisioned resources
- Security Groups
  - ELB Security groups , to allow connection from your local machine IP
  - EC2 Security groups, to allow connection only from ELB
- 2 instances of Elastic Cloud Compute (EC2) , deployed in multiple Availability Zone
- Elastic Load Balancer, to support High Available environments

## Variables
Below are the description of the variable setup inside the ansible playbook inside `inventories/group_vars/all` used for provisioning the ec2 with pre-setup wordpress

| Variable Name | Type | Description
| --------- | ----------- | ----------- |
| aws_region | String | AWS Region where all the resources will be provisioned |
| vpc_id | String  | VPC where the resources will be provisioned | 
| subnets | List of String | List of subnets where EC2 will be deployed, suggested to use multi-az deployed subnets |
| sg_name | String | Security group provisioned for ELB, allow access only from your local machine IP | 
| ec2_sg_name | String | Security group provisioned for EC2, allow access only from ELB | 
| elb_name | String | Elastic Load Balancer Name |
| ec2_name | String | Elastic Cloud Compute (EC2) Instance Name |
| ec2_ami_id | String | ID of Amazon Machine Image (AMI) will be used, in this case we will use official wordpress certified image from Bitnami |
| ec2_keypair | String | Keypair will be used to connect to the instances |
| ec2_instance_type | String | Instance type of EC2 will be provisioned |

## Upcoming updates

Here are the list of undone things for this repository :
- Automate hourly post, utilize Wordpress API and cron jobs to make this possible 
- Automate manual process required above, for example : EC2 Keypair creation, IAM permission assignment etc