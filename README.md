# aws-sts-token

## Prerequisites 
1. Install `ansible` on workstation (`pip install ansible`)
2. Install `awscli` on workstation (`pip install awscli`)
3. Install `awx.awx` collection on workstation (`ansible-galaxy collection install awx.awx`)
4. Configure aws credentials via `aws configure`
5. Have MFA enabled on your AWS account

## What does this aws-sts-token do?

I was getting annoyed having to update my short term AWS credentials within automation controller (formerly Ansible Tower), so I wanted a way where I could easily put in my MFA token and run a playbook/script that would then update that AWS credential automatically for me on my controller environment. I found [Jeff's blog talking about it](https://www.jeffgeerling.com/blog/2018/getting-aws-sts-session-tokens-mfa-aws-cli-and-kubectl-eks-automatically) but need some slight modifications to make it work to update my controller environment.


## How to use

Create a vars.yml file with the vars you will need to run:
```
---
controller_hostname: "controller.example.com"
controller_username: "admin"
controller_password: "changeme"
credential_name: "My AWS Credential"
aws_userarn: "<ARN_FROM_IAM>"
aws_profile: "default"
aws_sts_profile: "default"
```

NOTE: `credential_name` assumes you have an `AWS Web Services` Credential already created and labeled `My AWS Credential` otherwise it will create it.

To use it, you can download the contents of that file to `/usr/local/bin/aws-sts-token`, make the file executable (chmod +x /usr/local/bin/aws-sts-token), and run the command:

```
./aws-sts-token -e @vars.yml -e token_code=TOKEN
```
NOTE: `TOKEN` is the value from your MFA device. 
