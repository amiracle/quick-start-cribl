# Cribl Stream - Rapid Adoption
This Rapid Adoption guide was created by [Cribl](https://cribl.io) to help automate the deployment of [Cribl Stream](https://cribl.io/templates/) in your AWS environment. These  automated reference deployments use AWS CloudFormation templates to deploy EC2 instances, IAM policies and S3 buckets, following AWS best practices. 

## Deployment
This Rapid Adoption deployment builds a new AWS environment consisting of the infrastructure resources required to provision Cribl Stream. 

### Steps 
1. If you don't already have an AWS account, sign up at https://aws.amazon.com, and sign into your account.
2. Subscribe to the free offering of [Cribl Stream on the AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-3wsytwvqb65gg?sr=0-1&ref_=beagle&applicationId=AWSMPContessa). Once you're subscribed, continue to the next step. 
3.  Select your deployment method:

| VPC | ARM64 | x86_64 |
| --- | ---- | ---- |
| Deploy in an existing VPC + CloudTrail Environment| [Cribl Stream ARM64](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=Cribl-Stream&templateURL=https://cribl-rapid-adoption-us-east-1.s3.us-east-1.amazonaws.com/templates/cribl-single-arm64.entrypoint.template.yaml) | [Cribl Stream x86_64](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=Cribl-Stream&templateURL=https://cribl-rapid-adoption-us-east-1.s3.us-east-1.amazonaws.com/templates/cribl-single-x86.entrypoint.template.yaml) |
| Deploy in new VPC with Flow Logs and CloudTrail to s3 enabled | [Cribl Stream ARM64](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=Cribl-Stream&templateURL=https://cribl-rapid-adoption-us-east-1.s3.us-east-1.amazonaws.com/templates/cribl-single-arm64-new-vpc-logging.template.yaml) | [Cribl Stream x86_64](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=Cribl-Stream&templateURL=https://cribl-rapid-adoption-us-east-1.s3.us-east-1.amazonaws.com/templates/cribl-single-x86-new-vpc-logging.template.yaml) | 

4. Deploy the stack in your environment, make sure to check the region as this defaults to **Northern Virginia (us-east-1)**. 
5. Log into Cribl Stream with the credential supplied in the nested **CriblDeploy** "Outputs" tab on your CloudFormation stack.

---
>#### Tips for Deployment

>By default this CloudFormation template will deploy in **Northern Virginia (us-east-1)**, but if you want to change the region simply change the region in your CloudFormation drop down and then in the template itself update the ***QSS3BucketRegion*** variable from ***us-east-1*** to your region of choice.
This deployment takes approximately 10 minutes to complete. For more information and step-by-step deployment instructions, see the deployment guide.

>Make sure to select TWO Availability Zones (AZ) for your deployment. Selecting one will cause the template to fail.
---

## Use Cases
Cribl Stream can optimize the collection of :
 - [VPC Flow Logs](/docs/steps/vpcflowlogs2metrics.md)
 - [CloudWatch Streaming Metrics](/docs/steps/cloudwatchmetrics.md)  
 - [CloudTrail logs](/docs/steps/cloudtrail.md) 
 - [Collect and send to an S3 bucket](/docs/steps/s3bucket.md) 

## Pricing

You are responsible for the cost of the AWS services used while running this Rapid Adoption reference deployment. There is no additional cost for using the Rapid Adoption. For Cribl Stream pricing information, see the [Cribl website](https://cribl.io/cribl-Stream-pricing/).

## Architecture

![Architecture](docs/images/Cribl_AWS_Single.png)

To post feedback, submit feature ideas, or report bugs, use the [**Issues**](https://github.com/amiracle/quick-start-cribl/issues) section of this [GitHub repo](https://github.com/aws-quickstart/quickstart-cribl-Stream).

To submit code for this Rapid Adoption, see the [AWS Rapid Adoption Contributor's Kit](https://aws-quickstart.github.io/).

### Additional Cribl Resources
- [Cribl Community](https://cribl.io/community) 
- [Cribl Resources](https://cribl.io/resources)
- [Cribl Docs on Single Instance Deployments](https://docs.cribl.io/docs/deploy-single-instance)
- [Cribl Docs on Distributed Deployments](https://docs.cribl.io/docs/deploy-distributed)
- [Cribl Docs on sizing and scaling instances](https://docs.cribl.io/docs/scaling)
- [Cribl Docs on AWS Cross-Account Data Collection](https://docs.cribl.io/templates/usecase-aws-x-account)
- [Cribl Docs Sources](https://docs.cribl.io/templates/sources)
- [Cribl Docs Destinations](https://docs.cribl.io/templates/destinations)
- [Cribl Integrations](https://cribl.io/integrations/)