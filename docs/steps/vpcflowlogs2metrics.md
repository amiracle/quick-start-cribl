# VPC Flow Logs-to-metrics


## Overview of Use Case
This use case is to get VPC Flow logs into your system of record and then convert the log events to metric data. This is based on the [excellent blog post](https://cribl.io/blog/practical-logs-to-metrics-conversion-with-cribl/) by one of the Cribl Co-Founder Dritran Bitincka. There are two ways to collect VPCFlow Logs, depending on cost and latency concerns, you might want to go one way or the other. First, if cost is an issue and you don't mind receiving the data after a few minutes, collecting the logs via S3 is the best approach. However, if you need the logs more quickly, then pushing the data from CloudWatch Logs to an HTTP endpoint would be the recommended approach. There is a higher cost for going down this route which is explained [here](https://stackoverflow.com/questions/55472308/cloudwatch-log-store-costing-vs-s3-costing). 

This guide will walk you through setting up a VPC and then setup logging VPC Flow logs to a CloudWatch Log Group or an S3 bucket. Once the data is in, then we will configure Cribl LogStream to collect the data either via Push or Pull methods. 

#### Pull Deployment
- VPC with VPC Flow Logs enabled and sending to S3 Bucket
    - IAM Roles that can write from VPC to S3 Bucket
- SQS for S3 triggers
    - Create and enable SQS event when objects are created in S3 bucket

## Setup Cribl LogStream to Collect from S3 (Pull)

Click on `Configure`

![Configure](/docs/images/screenshots/s3bucket/s3dest/s3-dest-02.png)

Click on `Sources`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-01.png)

Click on `Amazon S3`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-02.png)

Click on `Add New`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-03.png)

Name the input, for example `vpcflowlogs`
- Input the name of the `SQS` queue that is triggered by the S3 bucket containing the VPC Flow Logs (e.g. `cribl-sqs`)
- Input the region these components are found located (e.g. US East 2 Ohio)

Click `Save`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-04.png)


> If you are using your own EC2 instances and levereaged the Cloudformation templates, the EC2 worker nodes will have an EC2 Role assigned to them. If you are using `Cribl Cloud` you will need to create an IAM user and insert the `Access Key` and `Secret Key` under the Authentication section with `Manual` selected:

>![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-05.png)

> You can also leverage the Assume Role capabilites to collect data from other AWS Accounts. You can read how to accomplish this from the Cribl Documentation site : https://docs.cribl.io/docs/usecase-aws-x-account 

Now you will need to `Commit` and `Deploy` the changes. Start by clicking on the `Commit` button in the upper right corner.

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-06.png)

Validate and comment on the changes, then click `Commit`.

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-07.png)

Now `Deploy` the changes.

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-08.png)

When promted, click `Yes`.

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-09.png)

Test to see if any data is flowing into your input by clicking the `Live` button. This takes time depending on how much data is flowing through your input.

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-10.png)

You can also click on `Save as Sample File` to save the caputed data for testing and validating the data in the `Pipeline` section.

Download the VPC Flow Logs to Metrics [Cribl Content Pack](/cribl/packs/aws_vpcflow_logs_to_metrics.crbl) 

Click on `Packs`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-11.png)

Click on `Add New`
    - Then click on `Import from File`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-12.png)

Select the content pack `aws_vpcflow_logs_to_metrics.crbl`.

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-13.png)

Name the new pack `aws_vpcflow_logs_to_metrics`.

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-14.png)

Click on `Commit`.

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-16.png)

Validate and comment on the changes, then click `Commit`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-15.png)

Click on `Deploy`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-17.png)

Click `Yes`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-18.png)

Your content pack should be installed. Click on `Configure` to see the components that make up the content pack. 

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-19.png)

Click on `Pipelines` within the pack

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-20.png)

Click on `AWS_VPCFlow_Logs`. Here you can see all the functions created for you to parse and convert VPC Flow Logs to Metrics.

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-21.png)

Make sure that your destination (e.g. Splunk) has an index named `metrics`, change it if necessary.

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-22.png)

Click on `Routes` and add a route. Click `Save`, `Commit` and then `Deploy` your changes. In this example, we are going to be sending the data to Splunk.

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-23.png)

Example Splunk Dashboard

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-24.png)
