# Push Events to HTTP Endpoint Using Lambda Function

## Finish AWS Setup
Once your VPC has been created, then click on this link to setup VPC Flow logging to a CloudWatch Log Group: https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-logs.yaml&stackName=cribl-vpcflow-logs-create&param_ParentVPCStack=cribl-vpc&param_RetentionInDays=1&param_TrafficType=ALL


After deploying our Cribl Distributed Deployment, we will create an HTTP source and generate a token for this source. Once we have that information, then we will setup our AWS environment. We will start with creating a VPC and enable VPC Flow Logs to be sent to a CloudWatch Log Group. From there, we will create a Lambda Function that will trigger on CloudWatch Logs. This will send the events over HTTP into our Cribl LogStream deployment. 

### Cribl LogStream Setup
Log into your Cribl LogStream instance:

![Step-1 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-07.png)

- Go to __Default Group__ -> _Data_ -> _Sources_: 

![Step-2 Add Source](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-08.png)

- Select __Splunk HEC__ source:

![Step-3 Select Splunk HEC](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-09.png)

- Click Add New:

![Step-4 Create new HEC Endpoint](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-10.png)

- Name your input (e.g. _cribl_) and set the port to __8088__, click Next:

![Step-5 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-11.png)

- Add an _Authentication Token_:

![Step-6 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-12.png)

- Click _Generate_ and get the token:

![Step-7 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-13.png)

- Copy the token and give it a description: 

![Step-8 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-14.png)

- Add the following fields and then click _Save_

Name | Value
--- | ---
sourcetype | 'aws:cloudtrail'
index | main
--- 

![Step-9 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-15.png)


- Now you should have an endpoint ready to collect data from AWS:

![Step-8 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-18.png)

>
>
> AWS Tip, make sure you have __port 8088__ added to your security _Inbound Rules_:
>
> ![Security Group Check](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-19.png)
>
>(EC2 Instance->Click on Security Groups)
>


### AWS Setup Overview
We are going to create the following components in your AWS Environment:
- VPC with VPC Flow Logs enabled and sending to CloudWatch Logs Group
    - IAM Roles that can write from VPC to CloudWatch Logs
- Lambda function to send data to Cribl HTTP endpoint
    - Create and enable trigger from CloudWatch logs trigger

#### VPC Creation 
> Click on the following CloudFormation template to create a VPC within your AWS Deployment: https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0.
>
>
>Once this stack is complete, then click on this link to setup VPC Flow logging: https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-logs.yaml&stackName=cribl-vpcflow-logs-create&param_ParentVPCStack=cribl-vpc&param_RetentionInDays=1&param_TrafficType=ALL 
> 
> Check your CloudWatch Log Group and make sure the log group exists.

#### Lambda Function Creation

- Click on Create Function in the AWS Lambda console:

![Step-1 Lambda](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-01.png)

- Click on _Use a Blueprint_ then search for __splunk__ in the search box. Select the _splunk-cloudwatch-logs-processor_ function:

![Step-2 Lambda](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-03.png)

- Give the function a name, for example _CloudWatchToHTTP_ and leave the default __Create a new role with basic Lambda permissions__

![Step-3 Lambda](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-04.png)

- Select your CloudWatch Log group to trigger off of, in this example its _vpcflowlogs_, set the filter name to _vpcflowlogs_ and leave the filter pattern blank. 

![Step-4 Lambda](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-05.png)

- Set your SPLUNK_HEC_URL and SPLUNK_HEC_TOKEN to your Cribl instance:

| Key | Value |
| --- | --- |
| SPLUNK_HEC_URL | http://cribl_instance:8088/services/collector |
| SPLUNK_HEC_TOKEN | 11111111 |

> 
> Replace cribl_instance with your instance's IP Address
> 
> Replace 111111111 with your HEC token
> 
> Click Create function

![Step-5 Lambda](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-06.png)

- Click on CloudWatch Logs and make sure they are set to _Enabled_:

![Step-6 Lambda](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-20.png)


### Cribl Validation Steps
Go back into your Cribl Instance and click Sources -> Splunk HEC. Click on the Live Button under Status.

- Click on _Live_ and see the data flowing into the HTTP Endpoint 

![Step-8 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-18.png)

- Click Capture and see the sample events flowing into Cribl: 

![Step-10 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-16.png)

- Click on Monitoring -> Sources -> splunk_hec and see the metrics:

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-17.png)

### Send Data to Splunk Instance
We are going to setup a Splunk Destination and send the HEC Data to a Splunk forwarder endpoint (TCP_9997). Let's start by clicking on Data -> Destinations:

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-21.png)

- Select the _Splunk Single Instance_ Destination: 

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-22.png)

- Click _+ Add New_

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-23.png)

- Enter your Splunk servers IP Address and click Save

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-24.png)

- Click on Pipelines 

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-26.png)

- Click on Create Pipeline

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-27.png)

- Name your pipeline _vpcflowlogs_

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-28.png)

- Click _+ Add Function_

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-29.png)

- Type and select _Parser_ 

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-30.png)

- Under filter type _sourcetype=='aws:cloudwatchlogs:vpcflow'_ and click __Save__

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-31.png)

- Click on _+ Add Function_ and type _Publish Metrics_ 

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-32.png)

- Under filter type _sourcetype=='aws:cloudwatchlogs:vpcflow'_ 
- Under __Metrics__ add :

__Event Field Name__ | __Metric Name Expression__
--- | ---
bytes | 'net.bytes'
packets | 'net.packets'

- Under Dimensions type in : 
    > !_* !end !start *  – which basically means create dimensions out of all fields except for those start with _ (underscore), end and start.
    >

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-33.png)

- Click on _+ Add Function_ and type _Eval_
- Under filter type _sourcetype=='aws:cloudwatchlogs:vpcflow'_
- Under _Evaluate Fields_ Type in _index_ and _'metrics_test'_ 

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-34.png)

- Review your routes:
- All your Cribl Logs are going to S3 and your vpcflow logs are first going to S3 and the metrics are going to Splunk. 

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-36.png)

- Log into your Splunk instance and go to the Analytics dashboard. You should be seeing your VPCFlow Metric data flowing into the metric_test metric index.

![Step-11 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-35.png)