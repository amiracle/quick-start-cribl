
# Quick Start for Cribl LogStream

## Problem Statement:
Setting up AWS and the necessary identity policy and role for Cribl LogStream can be difficult especially if you are not too familiar with Amazon Web Services. 

## Solution:
I’ve put together a simple method to launch a cloudformation template that will automatically create the IAM EC2 role and the proper IAM policy to use with your deployment. 

## How to use the code:
1. Log into your AWS Account (SAML or via the console is fine.)

2. Next, click on this link : https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquick-start-cribl.s3.amazonaws.com%2FCriblEc2Role.json&stackName=Quick-Start-Cribl-Create-EC2Role

3. Follow the CloudFormation instructions and click “Create Stack.” 

4. Attach the newly created EC2 Role EC2CriblRole to your Cribl workers / master node.

## Code:
Here is the raw JSON code you can add to your own CloudFormation Template. Just copy it and run it as a stack in your or your customer AWS account. 

```json 
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "Cloudformation template for Cribl to collect data and send data into an AWS S3 bucket.",
    "Resources": {
      "CriblEC2Role": {
         "Type": "AWS::IAM::Role",
         "Properties": {
            "AssumeRolePolicyDocument": {
               "Version" : "2012-10-17",
               "Statement": [ {
                  "Effect": "Allow",
                  "Principal": {
                     "Service": [ "ec2.amazonaws.com" ]
                  },
                  "Action": [ "sts:AssumeRole" ]
               } ]
            },
            "Path": "/"
         }
      },
      "RolePolicies": {
         "Type": "AWS::IAM::Policy",
         "Properties": {
            "PolicyName": "criblS3",
            "PolicyDocument": {
               "Version" : "2012-10-17",
               "Statement": [ {
                  "Effect": "Allow",
                  "Action": [
                        "s3:GetObject",
                        "s3:ListBucket",
                        "s3:PutObject",
                        "sqs:CreateQueue",
                        "sqs:GetQueueUrl",
                        "sqs:ListQueues",
                        "sqs:SendMessage",
                        "sqs:SendMessageBatch",
                        "sqs:SetQueueAttributes"
                                ],
                  "Resource": [
                        "arn:aws:s3:::*",
                        "arn:aws:sqs:::*"
                                ]
               } ]
            },
            "Roles": [ {
               "Ref": "CriblEC2Role"
            } ]
         }
      },
      "RootInstanceProfile": {
         "Type": "AWS::IAM::InstanceProfile",
         "Properties": {
            "Path": "/",
            "Roles": [ {
               "Ref": "CriblEC2Role"
            } ]
         }
      }
   }
}  
```

## Modifications:
You can modify the Resources in the JSON policy to only point to specific S3 buckets or SQS queues. Just add the ARN (Amazon Resource Name) in the Policy, like this:

“Resource”: [

“arn:aws:s3:::thisismycribldata”,

“arn:aws:sqs:::thisismysqsqueue”

],