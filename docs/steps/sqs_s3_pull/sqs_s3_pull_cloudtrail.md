# Collecting CloudTrail Data from S3 buckets using Cribl LogStream
In this section we are going to setup Cribl LogStream to collect the CloudTrail data from the S3 bucket we just created by listening to the SQS notifications.

### Cribl LogStream Setup

These steps will setup Cribl LogStream to collect the CloudTrail logs from the S3 bucket created by the CloudFormation template triggered above. 

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-01.png)

Click on `Configure`

![Configure](/docs/images/screenshots/s3bucket/s3dest/s3-dest-02.png)

Click on `Sources`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-01.png)

Click on `Amazon S3`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-02.png)

Click on `Add New`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-03.png)

Name the input, for example `cloudtrail`
- Input the name of the `SQS` queue that is triggered by the S3 bucket containing the VPC Flow Logs (e.g. `cribl-cloudtrail-sqs-s3-<unique_id>`)
- Input the region these components are found located (e.g. US East 2 Ohio)

Click `Save`

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-01.png)

Click `Commit`

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-02.png)

Comment on the changes and then click `Commit`.

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-03.png)

Click `Deploy`

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-04.png)

Click `Yes`

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-05.png)

Test to see if any data is flowing into your input by clicking the `Live` button. This takes time depending on how much data is flowing through your input.

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-06.png)

You can also click on `Save as Sample File` to save the caputed data for testing and validating the data in the `Pipeline` section.

Download the VPC Flow Logs to Metrics [Cribl Content Pack](/cribl/packs/aws_cloudtrail.crbl) 

Click on `Packs`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-11.png)

Click on `Add New`
    - Then click on `Import from File`

![Sources](/docs/images/screenshots/s3bucket/vpcflow/sqs-s3-cls-12.png)

Select the content pack `aws_cloudtrail.crbl`.

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-07.png)

Name the new pack `aws_cloudtrail`.

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-08.png)

Click on `Commit` 

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-09.png)

Validate and comment on the changes, then click `Commit`

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-10.png)

Click on `Deploy`

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-11.png)

Click on `Yes`

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-12.png)

Your content pack should be installed. Click on `Configure` to see the components that make up the content pack.

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-13.png)

Click on `Pipeline` within the pack.

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-14.png)

Click on  `AWS_CloudTrail`. Here you can see all the functiosn created for you to parse and send CloudTrail data to your destination. 

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-15.png)

Click on `Routes` and add a route. Click `Save`, `Commit` and then `Deploy` your changes. In this example, we are going to be sending the data to Splunk.

![Sources](/docs/images/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-16.png)