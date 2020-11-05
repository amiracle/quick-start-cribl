# Send Data to S3 bucket

## Overview of Use Case
This use case will show you how to setup Cribl LogStream to send data from any source to an S3 bucket. The CloudFormation template from the AWS Marketplace listing automatically creates an S3 bucket with an IAM policy that allows reading from and writing to the bucket. The AWS side of this deployment has already been configured, we only need to setup a route and pipeline in Cribl LogStream to send the data to the bucket.

## Setting up Cribl LogStream

### Enable Cribl Internal Sources

- Log into your Cribl LogStream instance by going to  http://cribl_ip:9000/inputs where cribl_ip is your instance ip.
 
![Cribl Start Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/1-clsmp-s3.png)
_Figure 1. Initial Login Screen for Cribl LogStream_

- Click on Data in the toolbar and select Sources:

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/2-clsmp-s3.png)

- Click on the __Cribl Internal__ Data Source

![Cribil Interal Source Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/3-clsmp-s3.png)

- Turn on the CriblLogs source by enabling the source:

- Disabled : 

![Cribl Off Source](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/4-clsmp-s3.png) 

- Enabled : 

 ![Cribl On Source](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/5-clsmp-s3.png)

    - Click Save

### Enable S3 Destination 

- Click on the S3 Destination

![S3 Destination](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/6-clsmp-s3.png)

- By default, you should see a _default-s3_ location. This was created by the CloudFormation template.

![Default S3 Destination](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/7-clsmp-s3.png)

> Optional Step:
> 
> You can modify the settings for your S3 Destination to include sending the data in as _raw_ and enabling compression to _gzip_ . 
> ![Example Settings for S3](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/8-clsmp-s3.png)
>
> Just click save when you're done modifying the Destination.

### Create Pipeline

- Click on Pipelines from the top menu bar:

![Pipeline Menu](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/9-clsmp-s3.png)

- Click on __Add Pipeline__ and then _Create Pipeline_

![Create Pipeline](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/10-clsmp-s3.png)

- Create your Pipeline with these settings: 

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/11-clsmp-s3.png)

- Create an __EVAL__ function:

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/12-clsmp-s3.png)

- Set the function filter to _true_ and click save.

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/13-clsmp-s3.png)

- Now let's take a sample of the events:

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/14-clsmp-s3.png)

- Click Capture and see the results: 

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/15-clsmp-s3.png)

- Save the sample events as a Sample File

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/16-clsmp-s3.png)

- Review your Pipeline page, it should look like this. If so, you're ready to move on to create a __Route__

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/17-clsmp-s3.png)

### Create and attach a Route

- Click on __Add Route__ 

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/18-clsmp-s3.png)

- Add the details for the __Route__ and click _Save_

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/19-clsmp-s3.png)

### Now lets check the monitoring screen

- Click on the __Monitoring__ link in the toolbar

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/20-clsmp-s3.png)

- Click on _Sources_ and see that the sources are sending data.

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/21-clsmp-s3.png)

- Click on _Routes_ and see that the routes are seeing events.

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/22-clsmp-s3.png)

- Click on _Pipelines_ and oyu can see the data going through the pipelines.

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/23-clsmp-s3.png)

- Click on _Destinations_ to see the data landing in the S3 bucket.

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/24-clsmp-s3.png)

- Click on the _Live_ button to see the __Status__ of the data flowing through the S3 Destination.

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/25-clsmp-s3.png)

- Click on _Live Data_ and you can see the data streaming through.

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/26-clsmp-s3.png)

- Click on _Logs_ and you can see if there are any errors with this destination. 

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/27-clsmp-s3.png)

- Finally log into your AWS environment and check out the S3 bucket, there should be events landing in your S3 bucket.

![Pipeline Settings](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/28-clsmp-s3.png)
