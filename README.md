
# Quick Start for Cribl LogStream

## Overview :
This repo is meant to help new users to AWS and Cribl setup and configure these main use cases based on data from AWS:
- Logs to metrics
   - VPCFlow Logs
- Route Data to S3
- Replay Data from S3

But first, we will need to setup the AWS environment and it starts with an IAM Role, then setting up the VPC along with the logging for that VPC. Once the infrastructure is in place, we will then deploy a Stand Alone Cribl and Splunk Deployment. Finally, we will setup Cribl to collect data from AWS and forward it into Splunk as a metric. 

[Step 1 - EC2 Role Creation](/steps/step-1-cribl-ec2-role.md)
[Step 2 - VPC and Logging](steps/step-2-vpc-and-logging.md)
[Step 3 - Setup Cribl Instance](steps/step-3-setup-cribl.md)
[Step 4 - Lambda Function](/steps/step-4-lambdafunction.md)
