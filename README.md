# Description
This repository will help you create AWS resources using Cloud formation template. This CFT supports rolling update. 

# Pre-requisites
1. Install AWS CLI

echo "Download the latest AWS Command Line Interface"
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip

echo "Install the AWS CLI into /bin/aws"
./awscli-bundle/install -i /usr/local/aws -b /bin/aws

echo "Validate that the AWS CLI works"
aws --version

echo "Clean up downloaded files"
rm -rf /root/awscli-bundle /root/awscli-bundle.zip

2. Save your AWS credentials to the $HOME/.aws/credentials file, making sure to replace YOURACCESSKEY and YOURSECRETACCESSKEY with your individual credentials.

export AWSKEY=<YOURACCESSKEY>
export AWSSECRETKEY=<YOURSECRETKEY>
export REGION=us-east-2

mkdir $HOME/.aws
cat << EOF >>  $HOME/.aws/credentials
[default]
aws_access_key_id = ${AWSKEY}
aws_secret_access_key = ${AWSSECRETKEY}
region = $REGION
EOF

# Deployment 
  

  1. Deploy the stack using AWS cli

  aws cloudformation create-stack --stack-name new-stack --template-body file:///resources/rollingupdate.yaml  --parameters  file:///resources/parameters.json
  
  2. Watch for status until the status is CREATE_COMPLETE
  
  watch -n 5 aws cloudformation describe-stacks --stack-name new-stack --query Stacks[0].StackStatus

  2. Host details 
  
  Add the list of Servers for which Mediawiki installation is required to /etc/ansible/hosts

  3. Installation using ansible

  ansible-playbook playbook_mediawiki.yaml -i inventory.txt
    
