# In This Project 
- we will use auto-scaling and load balancer to manage the traffic
- we will use cloudformation to create the infrastructures
- github and github action for CICD
- whenever there is change in the code new instances must be created with recent changes and old instances must be down with zero downtime

## Prerequisite
- github Account
- AWS Management Console
- AWS Access keys

## Clone the repository
```sh
git clone https://github.com/Abishekgautam44/ec2-automation.git
```
## Download and Configure the AWS CLI
```sh
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version ## verify the version
aws confiure
aws sts get-caller-identity ## verify the configuration

```
## Generate key pair
manually from  aws management console 
or
```sh
aws ec2 create-key-pair \
    --key-name MyKeyPair \
    --query 'KeyMaterial' \
    --output text > MyKeyPair.pem
chmod 400 MyKeyPair.pem

##check if it is generated on the aws
aws ec2 describe-key-pairs --key-names MyKeyPair
```
## Add Secret to the github 
Under settings > secrets and variables <br>
choose actions and Add new Repository Secret
- Name AWS_ACCESS_KEY_ID
- secret <@alue>
- Name AWS_SECRET_ACCESS_KEY
- secret <@Value>

## Deploy the cloudformation template
```sh
aws cloudformation deploy --template-file infra/max-app.yml --stack-name MaxInfra --capabilities CAPABILITY_IAM --parameter-overrides KeyName=MyKeyPair InstanceType=t2.micro AppName=max-app HtmlContent=$HTML_CONTENT BuildId=$TIMESTAMP
```
### Note: make sure your cloudformation template parameter and deploy file contain same keyPair