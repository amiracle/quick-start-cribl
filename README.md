# Cribl LogStream on Amazon - Quick Start
This Quick Start guide was created by [Cribl](https://cribl.io) to help automate the deployment of [Cribl LogStream](https://cribl.io/logstream/) in your AWS environment. These  automated reference deployments use AWS CloudFormation templates to deploy EC2 instances, IAM policies and S3 buckets, following AWS best practices. 

## Deployment
This Quick Start deployment builds a new AWS environment consisting of the infrastructure resources required to provision Cribl LogStream. 

| VPC | ARM64 | x86_64 |
| --- | ---- | ---- |
| Deploy in an existing VPC | [Cribl LogStream ARM64](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/template?stackName=Cribl-LogStream&templateURL=https://aws-quickstart-cribl-logstream-us-west-2.s3.us-west-2.amazonaws.com/logstream/cribl-single-template-arm64.yaml) | [Cribl LogStream x86_64](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/template?stackName=Cribl-LogStream&templateURL=https://aws-quickstart-cribl-logstream-us-west-2.s3.us-west-2.amazonaws.com/logstream/cribl-single-template.yaml) |
| Deploy in a new VPC | [Cribl LogStream ARM64](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/template?stackName=Cribl-LogStream&templateURL=https://aws-quickstart-cribl-logstream-us-west-1.s3.us-west-1.amazonaws.com/logstream/cribl-single-template-vpc-arm64.yaml) | [Cribl LogStream x86_64](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/template?stackName=Cribl-LogStream&templateURL=https://aws-quickstart-cribl-logstream-us-west-1.s3.us-west-1.amazonaws.com/logstream/cribl-single-template-vpc.yaml) |
| Deploy in new VPC with Flow Logs to s3 enabled | [Cribl LogStream ARM64](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/template?stackName=Cribl-LogStream&templateURL=https://aws-quickstart-cribl-logstream-us-west-2.s3.us-west-2.amazonaws.com/logstream/cribl-single-template-vpc-logging-arm64.yaml) | [Cribl LogStream x86_64](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/template?stackName=Cribl-LogStream&templateURL=https://aws-quickstart-cribl-logstream-us-west-2.s3.us-west-2.amazonaws.com/logstream/cribl-single-template-vpc-logging.yaml) |

This deployment takes approximately 10 minutes to complete. For more information and step-by-step deployment instructions, see the [deployment guide](https://github.com/amiracle/quick-start-cribl/blob/main/docs/Cribl%20LogStream%20User%20Deployment%20Guide%20for%20AWS%20-%20Google%20Docs.pdf)

## Use Cases
Cribl LogStream can optimize the collection of :
 - [VPC Flow Logs](/docs/steps/vpcflowlogs2metrics.md)
 - [CloudWatch Streaming Metrics](/docs/steps/cloudwatchmetrics.md)  
 - [CloudTrail logs](/docs/steps/cloudtrail.md) 
 - [Collect and send to an S3 bucket](/docs/steps/s3bucket.md) 

## Pricing

You are responsible for the cost of the AWS services used while running this Quick Start reference deployment. There is no additional cost for using the Quick Start. For Cribl LogStream pricing information, see the [Cribl website](https://cribl.io/cribl-logstream-pricing/).

## Architecture

![Architecture](/docs/images/Cribl_AWS_Single.png)

To post feedback, submit feature ideas, or report bugs, use the [**Issues**](https://github.com/amiracle/quick-start-cribl/issues) section of this [GitHub repo](https://github.com/amiracle/quick-start-cribl).

To submit code for this Quick Start, see the [AWS Quick Start Contributor's Kit](https://aws-quickstart.github.io/).

### Additional Cribl Resources
- [Cribl Community](https://cribl.io/community) 
- [Cribl Resources](https://cribl.io/resources)
- [Cribl Docs on Single Instance Deployments](https://docs.cribl.io/docs/deploy-single-instance)
- [Cribl Docs on Distributed Deployments](https://docs.cribl.io/docs/deploy-distributed)
- [Cribl Docs on sizing and scaling instances](https://docs.cribl.io/docs/scaling)