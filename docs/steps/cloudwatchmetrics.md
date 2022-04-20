# CloudWatch Streaming Metric Data Collection


## Overview of Use Case
This use case will cover collecting metrics from AWS CloudWatch using their streaming feature via S3 buckets. Follow the steps in the AWS Post to enable your CloudWatch Streaming Metrics. https://aws.amazon.com/blogs/aws/cloudwatch-metric-streams-send-aws-metrics-to-partners-and-to-your-apps-in-real-time/ 


### AWS Setup Overview
We are going to create the following components in your AWS Environment: 
- CloudWatch Metric Streams enabled and sending to S3 Bucket
    - IAM Roles that can write from CloudTrail to S3 Bucket
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

Name the input, for example `cloudwatch_metrics`
- Input the name of the `SQS` queue that is triggered by the S3 bucket containing the VPC Flow Logs (e.g. `cribl-metrics-sqs`)
- Input the region these components are found located (e.g. US East 2 Ohio)

Click `Save`

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm01.png)


> If you are using your own EC2 instances and levereaged the Cloudformation templates, the EC2 worker nodes will have an EC2 Role assigned to them. If you are using `Cribl Cloud` you will need to create an IAM user and insert the `Access Key` and `Secret Key` under the Authentication section with `Manual` selected:

>![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-05.png)

> You can also leverage the Assume Role capabilites to collect data from other AWS Accounts. You can read how to accomplish this from the Cribl Documentation site : https://docs.cribl.io/docs/usecase-aws-x-account 

Now you will need to `Commit` and `Deploy` the changes. Start by clicking on the `Commit` button in the upper right corner.

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm02.png)

Validate and comment on the changes, then click `Commit`.

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm03.png)

Now `Deploy` the changes.

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm04.png)

Click `Yes`

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm05.png)

Test to see if any data is flowing into your input by clicking the `Live` button. This takes time depending on how much data is flowing through your input.

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm06.png)

You can also click on `Save as Sample File` to save the caputed data for testing and validating the data in the `Pipeline` section.

Download the VPC Flow Logs to Metrics [Cribl Content Pack](/cribl/packs/aws_cloudwatch_streaming_metrics.crbl) 

Click on `Packs`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-11.png)

Click on `Add New`
    - Then click on `Import from File`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-12.png)

Select the content pack `aws_cloudwatch_streaming_metrics.crbl`.

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm07.png)

Name the new pack `aws_cloudwatch_streaming_metrics`.

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm08.png)

Click on `Commit`.

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm09.png)

Validate and comment on the changes, then click `Commit`

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm10.png)

Click on `Deploy`

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm11.png)

Click `Yes`

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm13.png)

Your content pack should be installed. Click on `Configure` to see the components that make up the content pack. 

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm12.png)

Click on `Pipelines` within the pack

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm14.png)

Click on `AWS_CloudWatch_Streaming_Metrics`. Here you can see all the functions created for you to parse and create the metrics from AWS. 

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm15.png)

Make sure that your destination (e.g. Splunk) has an index named `metrics`, change it if necessary.

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm16.png)

Example Splunk Dashboard

![Sources](/docs/images/screenshots/s3bucket/cwmetrics/cwm17.png)
