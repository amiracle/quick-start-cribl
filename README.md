# Cribl LogStream on Amazon
> This Quick Start guide was created by [Cribl](https://cribl.io) to help automate the deployment of Cribl LogStream in your AWS environment. These  automated reference deployments use AWS CloudFormation templates to deploy EC2 instances, IAM policies and S3 buckets, following AWS best practices. 

## Overview 
Cribl has put together a simple method to launch a CloudFormation template that will automatically create the IAM EC2 role and the proper IAM policy to use with your deployment. We will be breaking down the permissions used, AWS resources deployed and their associated costs. This process will take around 5 minutes and you will have a running Cribl LogStream deployment.  

## Cribl Cloud
If you want to just deploy a Cribl LogStream Standard instance running on AWS, you can also head over to [Cribl.Cloud](https://cribl.cloud) and sign up for your own free tenant. 

## Architecture 
Deploying this Quick Start with default parameters will build a single EC2 instance Leader Instance. The instance will be launched within the VPC and (AZ's) you specify. This deployment will also create an S3 bucket that can be used as a source or destination. The EC2 Role and the associated policy will also be created as a part of this CloudFormation template and assigned to the Worker Nodes.  

![Cribl Single Deplyoment](docs/images/Cribl_AWS_Single.png)

_Figure 1: Cribl LogStream Single Instance architecture deployment in AWS_

As shown in Figure 1, the Quick Start CloudFormation template sets up the following:

* In AWS: 
   * EC2 Instance
   * Security Groups
   * S3 Bucket
   * IAM Role and Policy
> IAM Policy Best practices 
>
> Per the [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) the policy created by the CloudFormation template creates an EC2 role with the following IAM Policy:

```yaml 
iamDefaultWorkerRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: !Sub '${AWS::StackName}-logstream-worker-role'
      Description: Cribl LogStream default IAM role
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service:
                - ec2.amazonaws.com
            Action:
              - sts:AssumeRole
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
        - arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
      Policies:
        - PolicyName: S3Destinations
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - s3:PutObject
                  - s3:GetObject
                  - s3:ListBucket
                  - s3:GetBucketLocation
                Resource:
                  - !Sub '${s3DefaultDestinationBucket.Arn}'
                  - !Sub '${s3DefaultDestinationBucket.Arn}/*'
        - PolicyName: S3Sources
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - s3:GetObject
                  - s3:GetBucketLocation
                Resource:
                  - !Sub '${s3DefaultDestinationBucket.Arn}'
                  - !Sub '${s3DefaultDestinationBucket.Arn}/*'
      Tags:
        - Key: Name
          Value: Cribl LogStream default IAM role
  iamDefaultWorkerInstanceProfile:
    Type: AWS::IAM::InstanceProfile
    Properties:
      Path: /
      Roles:
        - !Ref 'iamDefaultWorkerRole'
```

>This IAM Role ties the actions of listing, reading and writing to the specific S3 bucket created during this process. Then this policy is attached to the EC2 Worker instance rather than using Access / Secret keys for authentication. 

## Cost and licenses
You are responsible for the cost of the AWS services used while running this Quick Start reference deployment. There are no additional costs for using the Quick Start. The Cribl LogStream license included in your deployment will default to the LogStream Free 1TB license. For more information about licensing, please refer to the [Cribl LogStream Pricing Page](https://cribl.io/cribl-logstream-pricing/).

The AWS CloudFormation template for this Quick Start includes configuration parameters that you can customize. Some of these settings may affect the cost of your deployment. For cost estimates, see our estimates under "Pricing Information" in the AWS Marketplace listing. 

> Estimated Infrastructure Cost assuming US East (N.Virginia) and a c5a.large instance is $45.88/month. Prices may different depending on instance type, region and other factors. Please check with your AWS billing reports or use the [AWS Calculator](https://calculator.aws/#/) to more accuratly predict your costs. 

## Planning the deployment

### Specialized knowledge
This Quick Start assumes familiarity with Amazon EC2, S3, IAM and CloudFormation. 

### AWS Account
If you don't already have an AWS Account, create one at https://aws.amazon.com and follow the instructions.

## Deployment Steps
1. Sign into your AWS Account.

2. Deploy the CloudFormation Template: [QuickStart Cribl Deployment](https://github.com/criblio/aws-quickstart-cribl-logstream/blob/main/templates/cribl-single-template.yaml)

3. Log into Cribl LogStream with the credential supplied in the "Outputs" tab on your CloudFormation stack.

4. Start collecting and sending data to your S3 bucket.

> Here is the [step-by-step](docs/steps/cloudformation.md) instructions for the cloudformation template deployment. 

## Log into Cribl LogStream

Navigate to http://<cribl_logstream_ip>:9000 and log in using admin / <ec2_instance_id> . Once you have logged into your instance take a look around and we can start diving into the use cases.

### Use Cases

- [Send Data to S3 bucket](docs/steps/s3bucket.md)

### Advanced Use Cases
- [VPC Flow Logs to Metrics](docs/steps/vpcflowlogs2metrics.md)
- [CloudTrail Collection](docs/steps/cloudtrail.md)
- [CloudWatch Metric Collection](docs/steps/cloudwatchmetrics.md)

## Feedback
Please submit feedback, feature ideas or report bugs using the [Issues](https://github.com/amiracle/quick-start-cribl/issues) section of this GitHub repository. If you would like to submit code, please review the Quick Start Contributor's Guide.

## Remove Deployment

Once you have tested this deployment and would like to remove these artifacts from your deployment, simply remove / delete the CloudFormation template you deployed in your region. If you sent data into the S3 bucket, wou will need to remove S3 buckets manually. First empty the buckets and then delete it. 

## Additional resources

### Cribl Resources
- [Cribl Community](https://cribl.io/community) 
- [Cribl Resources](https://cribl.io/resources)
- [Cribl Docs on Single Instance Deployments](https://docs.cribl.io/docs/deploy-single-instance)
- [Cribl Docs on Distributed Deployments](https://docs.cribl.io/docs/deploy-distributed)
- [Cribl Docs on sizing and scaling instances](https://docs.cribl.io/docs/scaling)

### AWS resources

* [Getting Started Resource Center](https://aws.amazon.com/getting-started/)
* [AWS General Reference](https://docs.aws.amazon.com/general/latest/gr/)
* [AWS Glossary](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html)

### AWS services

* [AWS CloudFormation](https://docs.aws.amazon.com/cloudformation/)
* [Amazon EC2](https://aws.amazon.com/ec2/)
* [IAM](https://docs.aws.amazon.com/iam/)
* [Amazon VPC](https://docs.aws.amazon.com/vpc/)
* [Amazon Kinesis](https://docs.aws.amazon.com/kinesis/)
