# Collecting CloudWatch Streaming Metrics from S3 buckets using Cribl LogStream
In this section we are going to setup Cribl LogStream to collect the CloudTrail data from the S3 bucket we just created by listening to the SQS notifications.

### Cribl LogStream Setup

These steps will setup Cribl LogStream to collect the CloudWatch Streaming Metrics from the S3 bucket created by the CloudFormation template triggered above. 

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-01.png)

- Setup Cribl S3 Source
    - Click on `Default Group`
        - Click on `Data` -> `Sources`
        - Click on `Amazon S3`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-01.png)

- Click `+Add New` S3 Sources
- Input ID : `cloudwatch-metrics`
- Queue: `cloudwatch-metrics`
- API key: Leave Blank since we are using EC2 Roles
- Secret key: Leave Blank since we are using EC2 Roles
- Region: select your region, e.g. US East (N.Virginia)
- Click `Save`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-02.png)

- Click on `Commit default`
- Enter update comments (e.g. `Added S3 CloudWatch Streaming Metrics`)

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-03.png)

- Click `Commit`
- Click `Deploy default`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-04.png)

- Click `Yes`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-05.png)

- Click `Live` under Status for the `cloudwatch-metrics` source

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-02.png)

- Note the status of your worker nodes
- Click on `Live Data`
- Click `Capture`
    - Set the `Capture Time (sec)` to `1000`
    - Set the `Capture Up to N Events` to `1000`
    - Click `Start`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-06.png)

Data can be collected for a sample file:
- Description: `CloudWatchMetrics`
- Tags: cloudwatch, aws
- Scroll down and click `Save Sample File`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-07.png)

- Click on `Pipelines`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-08.png)

- Click on `Add Pipeline`
    - Click on `Create Pipeline`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-10v2.png)

- Name the pipeline `cloudwatch_metrics`
- Description: `Collect CloudWatch Streaming Metrics`
- Click `Save`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-11v2.png)

Now you have a pipeline ready to add functions.

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-12.png)

- Add function : `parser`
- Description: `Parse CloudWatch Metrics`
- Filter: `true`
- Operation Mode* : `Extract`
- Type*: `JSON Object`
- Source Field: `_raw`
- List of Fields : `account_id` `dimensions_` `metric_name` `metric_stream_name` `namespace` `region` `value_` `timestamp` 

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-17v2.png)

- Add function: `Publish Metrics`
- Filter: `true`
- Add Metrics: (those are backticks before and after the Metric Name Expression)

| Event Field Name| Metric Name Expression|
------------------|-----------------------
value.count |  \`aws.${namespace}.count\`
value.max   |  \`aws.${namespace}.max\`
value.min   |  \`aws.${namespace}.min\`
value.sum   |  \`aws.${namespace}.sum\`

- Add Dimensions:
`region` `account_id` `dimensions_*` `unit`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-14v2.png)


- Add Function `Eval`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-15v2.png)

- Filter: `true`
- Evaluate Fields: 
    - Name: `index`
    - Value Expression: `'metrics'`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-16v2.png)

- Click `Save`

- Now lets validate that the data is being extracted. Select your data captured and let's apply the pipeline. On the right side, click on the "Preview Simple" and select your data capture file.

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-18v2.png)

Click on the `Out` box and you should see the fields highlighted. 

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-19v2.png)

Click on the gear and click on `Show Internal Fields`

(Before)
![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-20v2.png)

(After)
![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-21v2.png)

Now you can expand the internal fields to validate that all the data is being properly parsed.

Finally Click on `Routes` and create a route `Send Metrics to Splunk`
- Filter: `__inputId=='s3:cloudwatch-metrics'`
- Pipeline: `cloudwatch_metrics`
- Output: `splunk:splunk` (or your metric store destination)

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-24v2.png)

- Click on `Commit default` and `Deploy default` to deploy your changes. 

You should start to see your metrics in your dashboard of choice:

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-23v2.png)

