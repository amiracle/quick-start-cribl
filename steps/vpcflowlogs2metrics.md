# VPC Flow Logs-to-metrics

## Overview of Use Case
This use case is to get VPC Flow logs into your system of record and then convert the log events to metric data. This is based on the [excellent blog post](https://cribl.io/blog/practical-logs-to-metrics-conversion-with-cribl/) by one of the Cribl Co-Founder Dritran Bitincka. This guide will walk you through setting up a VPC and then setup logging VPC Flow logs to a CloudWatch Log Group. 

## Deployment Overview
This assumes that you have an AWS Account with an active VPC and are logging VPCFlow Logs to a CloudWatch Log Group. If you do not have that enabled or setup, you can follow the steps below to deploy a VPC and setup the logging of VPC Flow Logs to a CloudWatch Log Group.

> Setup a VPC and VPC Flow Logs collection in CloudWatch Logs:
> Click on this link : https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0 to create your VPC.
>
>
>Once this stack is complete, then click on this link to setup logging : https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-logs.yaml&stackName=cribl-vpcflow-logs-create&param_ParentVPCStack=cribl-vpc&param_RetentionInDays=1&param_TrafficType=ALL 
> 
> Check your CloudWatch Log Group and make sure the log group exists.

### Create HTTP/HEC Source in Cribl

### Setup Lambda Function 

6. Create HTTP/HEC Source in Cribl

7. Shape VPCFlow Logs and route them to Splunk metrics and S3 for archive

### 
