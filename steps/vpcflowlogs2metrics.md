# VPC Flow Logs-to-metrics

## Overview of Use Case
This use case is to get VPC Flow logs into your system of record and then convert the log events to metric data. This is based on the [excellent blog post](https://cribl.io/blog/practical-logs-to-metrics-conversion-with-cribl/) by one of the Cribl Co-Founder Dritran Bitincka. This guide will walk you through setting up a VPC and then setup logging VPC Flow logs to a CloudWatch Log Group. 

## VPC and Logging Setup
Lets setup the AWS VPC and configure the logging for your VPC. 

## Deployment Steps
1. Log into your AWS Account.

2. Next, click on this link : https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquick-start-cribl.s3.amazonaws.com%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0 to create your VPC.

3. Once this stack is complete, then click on this link to setup logging : https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquick-start-cribl.s3.amazonaws.com%2Fvpc-flow-logs.yaml&stackName=cribl-vpcflow-logs-create&param_ParentVPCStack=cribl-vpc&param_RetentionInDays=1&param_TrafficType=ALL 

4. Validate that the newly created VPC and VPCFlow Logging group exist in your AWS account.

5. Create Lambda function to send CloudWatch Logs into HEC
