# Pull Events from SQS and S3 with Cribl LogStream (VPC Flow Logs)
After deploying our Cribl LogStream Distributed deployment, we are going to setup VPC Flow Logs and send them into an S3 bucket. That S3 bucket will trigger an SQS event anytime VPC Flow Logs PUT data into the S3 bucket. Cribl LogStream will pickup the events and send them into your logging solution of choice.

## Cribl LogStream Setup - S3 Source

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-01.png)

These steps will setup Cribl LogStream to collect the VPC Flow Logs from the S3 bucket created by the CloudFormation template triggered above. 

- Setup Cribl S3 Source
    - Click on `Default Group`
        - Click on `Data` -> `Sources`
        - Click on `Amazon S3`

![Cribl S3 Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-02.png)

- Click `+Add New` S3 Sources
- Input ID : `vpcflowlogs`
- Queue: `cribl-sqs`
- API key: Leave Blank since we are using EC2 Roles
- Secret key: Leave Blank since we are using EC2 Roles
- Region: select your region, e.g. US East (N.Virginia)
- Click Save

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-03.png)

- Disable Assume Role for S3 
- Click on `Commit default`

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-04.png)

- Enter update comments (e.g. `Added S3 VPC Flow Collection`)
- Click `Commit`

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-05a.png)

- Click `Deploy default`

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-06.png)

- Click `Yes`

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-05b.png)

- Click `Live`

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-07.png)

- See the status of your worker nodes

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-08.png)

- Click on `Charts` to see if data is flowing

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-09.png)

- Click on `Live Data` to see sample events flowing

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-10.png)

- Save the captured data for future use
- Tag the events `vpcflow` and give it a description `VPCFlowLogs` (Optional)

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-11.png)

- Exit this screen and click on `Pipelines`

## Cribl LogStream Setup - Configure Pipeline

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-12.png)

- Click Pipelines
    - Click `Add Pipeline`
        - Select `Create Pipeline`
            - Id : `vpcflowlogs`
            - Click `Save`

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-13.png)

- Click `Add Function`
    - Type `Parser` and Select it
        - Filter : `__inputId=='s3:vpcflowlogs'`
        - Description : `Parse VPC Flow Logs`
            - Operation Mode: `Extract`
                - Type: `Extended Log File Format`
                - Library: `AWS VPC Flow Logs`

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-14.png)

- Click `Add Function`
    - Type `Publish Metrics` and Select it
        - Filter: `__inputId=='s3:vpcflowlogs'`
        - Description: `convert logs to metrics`
        - Metrics: 
            - Event Field Name: `+ Add Metric`
                - bytes : `'net.bytes'`
                - packets : `'net.packets'`
            - Dimensions: `!_*` `!end` `!start` `*`
                
![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-15.png)
                
- Click `Add Function`
    - Type `Eval` and Select it
    - Filter: `__inputId=='s3:vpcflowlogs'`
        - Description: `Send to metric store`
        - Evaluate Fields:
            - Name: `index` 
            - Value Expression: `metrics` 
- Click `Save`

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-16.png)

- Click `Routes`
    - Add Routes
        - Route Name: `VPC Flow Logs Metrics`
        - Filter: `__inputId=='s3:vpcflowlogs'`
        - Pipeline: Select `vpcflowlogs`
        - Output: `splunk:splunk`
        - Description: `Send VPC Flow Logs Metrics to Splunk`

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-17.png)

- Click `Save`
- Click `Commit default`
- Click `Deploy default`

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-18.png)

- Click `Commit`

Check your metrics system and see the results:

![Cribl S3 Sources 2](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/vpcflow/sqs-s3-cls-19.png)