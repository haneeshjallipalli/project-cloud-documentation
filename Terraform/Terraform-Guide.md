Terraform:
  -Terraform installation:
  -AWS CLI installed: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
  -Configured: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html

1) aws configure


2)
AWS Access Key ID [None]: YOUR_ACCESS_KEY_ID
AWS Secret Access Key [None]: YOUR_SECRET_ACCESS_KEY
Default region name [None]: us-west-2
Default output format [None]: json

3)aws ec2 describe-instances

=> aws ec2 start-instances --instance-ids i-1234567890abcdef0

=> aws ec2 stop-instances --instance-ids i-1234567890abcdef0


3) main.tf
```
provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "example-terraform" {
  ami           = "ami-078701cc0905d44e4"  # This is a free tier eligible t2.micro Amazon Linux 2 AMI in the us-west-2 region.
  instance_type = "t2.micro"

  tags = {
    Name = "example-terraform"
  }
}
```

4) terraform init
5) terraform plan
6) terraform apply
7) terraform destroy
8) current state: terraform.tfstate


