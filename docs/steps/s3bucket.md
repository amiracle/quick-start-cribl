# Send Data to S3 bucket

## Overview of Use Case
This use case will show you how to setup Cribl LogStream to send data from any source to an S3 bucket. The CloudFormation template from the AWS Marketplace listing automatically creates an S3 bucket with an IAM policy that allows reading from and writing to the bucket. The AWS side of this deployment has already been configured, we only need to setup a route and pipeline in Cribl LogStream to send the data to the bucket.


## Enabling up Cribl LogStream Data

### Validate S3 Destination 

![Cribl Sources Screen](/docs/images/screenshots/s3bucket/s3dest/s3-dest-01.png)

- Hover over the left toolbar
    - Click on `Configure`

![Cribl Sources Screen](/docs/images/screenshots/s3bucket/s3dest/s3-dest-02.png)

- Click on `Destinations`

![Cribl Sources Screen](/docs/images/screenshots/s3bucket/s3dest/s3-dest-03.png)

- Click on `Amazon S3`

![Cribl Sources Screen](/docs/images/screenshots/s3bucket/s3dest/s3-dest-04.png)

- Click on the `s3-default` S3 destination

![Cribl Sources Screen](/docs/images/screenshots/s3bucket/s3dest/s3-dest-05.png)

- Notice the s3 bucket has already been pre-populated and can accept data.

![Cribl Sources Screen](/docs/images/screenshots/s3bucket/s3dest/s3-dest-06.png)

- Click on the `Test` tab

![Cribl Sources Screen](/docs/images/screenshots/s3bucket/s3dest/s3-dest-07.png)

- Click `Run Test` and see the `Success` in the Test Results

![Cribl Sources Screen](/docs/images/screenshots/s3bucket/s3dest/s3-dest-08.png)

- Log into your AWS S3 view and see the data in the S3 bucket

![Cribl Sources Screen](/docs/images/screenshots/s3bucket/s3dest/s3-dest-09.png)