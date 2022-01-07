### CloudFormation Steps

- Step 1 - Create Stack - 
   - Download this [Template](/templates/cribl-single-template.yaml) 
      - This will take you to your AWS Account and drop you in a CloudFormation Stack deploy screen. Click "Next" to start the stack creation.
- Step 2 - Specify stack details 
> Tips for deploying in your AWS Environment. If this is a new AWS account, you will need to create the following components: 
> - An SSH Key pair to log into your EC2 instance. Please follow [this guide for help on creating your key pair.](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
> - A VPC with publicly accessible IP's. Here is a [CloudFormation template that will create the VPC for you.](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquick-start-cribl.s3.amazonaws.com%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0) 
> - A Subnet ID. This will be created by using the CloudFormation template above.
> Security Group CIDR Tips. 
> - Web Access CIDR should be open to either a range of IP's that you want to grant access. You can also open it to the world if you do not have any restrictions for public access to the webport. An example CIDR would be 0.0.0.0/0.
> - Advanced Settings amild is only used if you are issued this by Cribl Support.
   
   - Step 3 Configure stack options
      - This step has some optional sections. All the defaults can be left as-is and you can click "Next." 
      - You can modify the options in this section if you know what you're doing or have a company policy around tagging AWS resources. 

   - Step 4 Review
      - Review all the steps, acknowledge the access capabilities at the bottom of this page and click "Create Stack." 
Once the CloudFormation stack has been created, click on the Outputs tab of your Stack and you will see the LogStream WebAccess Credentials, the URL and the stackname. Your WebURL 