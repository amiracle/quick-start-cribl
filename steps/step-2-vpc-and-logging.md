## Step 2 - VPC and Logging Setup
Lets setup the AWS VPC and configure the logging for your VPC. 

## Overview :
Cribl has put together a simple method to launch a cloudformation template that will automatically create the IAM EC2 role and the proper IAM policy to use with your deployment. 

## How to use the code:
1. Log into your AWS Account (SAML or via the console is fine.)

2. Next, click on this link : https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquick-start-cribl.s3.amazonaws.com%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0 to create your VPC.

3. Once this stack is complete, then click on this link to setup logging : https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquick-start-cribl.s3.amazonaws.com%2Fvpc-flow-logs.yaml&stackName=cribl-vpcflow-logs-create&param_ParentVPCStack=cribl-vpc&param_RetentionInDays=1&param_TrafficType=ALL 

4. Validate that the newly created VPC and VPCFlow Logging group exist in your AWS account.

### Move onto Step 3 - Setup Cribl