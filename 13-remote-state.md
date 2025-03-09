The `terraform_remote_state` resource can be used to retrieve data populated.
The [Get Started VPC](https://github.com/daily-ops/quick-start-aws-vpc-with-terraform) demonstrates the use of remote state where each component can be maintained by different team. The backend can be other supported [backend](https://developer.hashicorp.com/terraform/language/backend) type such as s3, and etc. 

Quick to note that accessing the state in HCP Terraform Cloud Workspaces will require [tfe_workspace](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/data-sources/workspace) datasource. There is also a comprehensive guide available [here](https://support.hashicorp.com/hc/en-us/articles/1500008832622-Accessing-A-Workspace-s-State-Data)

```
data "terraform_remote_state" "vpc" {
    backend = "local"
    config = {
        path = "../vpc/terraform.tfstate"
    }
}

data "terraform_remote_state" "sg" {
    backend = "local"
    config = {
        path = "../sg/terraform.tfstate"
    }
}

data "terraform_remote_state" "iam" {
    backend = "local"
    config = {
        path = "../iam/terraform.tfstate"
    }
}

data "aws_ami" "ubuntu_22_04" {
    most_recent = true

    owners = ["099720109477"]

    filter {
        name = "name"
        values = ["ubuntu-minimal/images/hvm-ssd/ubuntu-jammy-22.04-amd64-minimal-*"]
    }
}

resource "aws_iam_instance_profile" "public" {
    name = "public-ec2-profile"
    role = keys(data.terraform_remote_state.iam.outputs.public-ec2-role)[0]
}

resource "aws_instance" "public" {
    for_each = data.terraform_remote_state.vpc.outputs.public_subnets
    ami = data.aws_ami.ubuntu_22_04.id
    instance_type = "t2.micro"
    availability_zone = each.key
    subnet_id = each.value
    key_name = var.ssh_key_name
    vpc_security_group_ids = [data.terraform_remote_state.sg.outputs.public_sg_id]
    user_data = <<-EOF
        #!/bin/bash
        sudo apt update
        sudo apt install -y awscli
    EOF
    iam_instance_profile = aws_iam_instance_profile.public.name
}

resource "aws_instance" "private" {
    for_each = data.terraform_remote_state.vpc.outputs.private_subnets
    ami = data.aws_ami.ubuntu_22_04.id
    instance_type = "t2.micro"
    availability_zone = each.key
    subnet_id = each.value
    key_name = var.ssh_key_name
    vpc_security_group_ids = [data.terraform_remote_state.sg.outputs.private_sg_id]
}
```
