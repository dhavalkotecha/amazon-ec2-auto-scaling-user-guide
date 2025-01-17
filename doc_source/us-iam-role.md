# IAM Role for Applications That Run on Amazon EC2 Instances<a name="us-iam-role"></a>

Applications that run on Amazon EC2 instances need credentials to access other AWS services\. To provide these credentials in a secure way, use an IAM role\. The role supplies temporary permissions that the application can use when it accesses other AWS resources\. The role's permissions determine what the application is allowed to do\.

For more information, see [Using an IAM Role to Grant Permissions to Applications Running on Amazon EC2 Instances](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) in the *IAM User Guide*\.

For instances in an Auto Scaling group, you must create a launch configuration or template and choose an instance profile to associate with the instances\. An instance profile is a container for an IAM role that allows Amazon EC2 to pass the IAM role to an instance when the instance is launched\. First, create an IAM role that has all of the permissions required to access the AWS resources\. Then, create the instance profile and assign the role to it\. For more information, see [Using Instance Profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) in the *IAM User Guide*\.

**Note**  
When you use the IAM console to create a role for Amazon EC2, the console guides you through the steps for creating the role and automatically creates an instance profile with the same name as the IAM role\. 

## Prerequisites<a name="us-iam-role-prereq"></a>

Create the IAM role that your application running on Amazon EC2 can assume\. Choose the appropriate permissions, so that the application that is subsequently given the role can access the resources that it needs\. <a name="create-iam-role-console"></a>

**To create an IAM role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, **Create new role**\.

1. On the **Select role type** page, choose **EC2** and the **EC2** use case\. Choose **Next: Permissions**\. 

1. On the **Attach permissions policy** page, choose the AWS managed policies that contain the required permissions\.

1. On the **Review** page, type a name for the role and choose **Create role**\. 

## Create a Launch Configuration<a name="us-iam-role-create-launch"></a>

When you create the launch configuration using the AWS Management Console, on the **Configure Details** page, select the role from **IAM role**\. For more information, see [Creating a Launch Configuration](create-launch-config.md)\.

When you create the launch configuration using the [https://docs.aws.amazon.com/cli/latest/reference/autoscaling/create-launch-configuration.html](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/create-launch-configuration.html) command from the AWS CLI, specify the name of the instance profile as follows:

```
aws autoscaling create-launch-configuration --launch-configuration-name my-lc-with-instance-profile \
--image-id ami-01e24be29428c15b2 --instance-type t2.micro \
--iam-instance-profile my-instance-profile
```

## Create a Launch Template<a name="us-iam-role-create-lt"></a>

When you create the launch template using the AWS Management Console, in the **Advanced Details** section, select the role from **IAM instance profile**\. For more information, see [Creating a Launch Template for an Auto Scaling Group](create-launch-template.md)\.

When you create the launch template using the [https://docs.aws.amazon.com/cli/latest/reference/ec2/create-launch-template.html](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-launch-template.html) command from the AWS CLI, specify the name of the instance profile as follows:

```
aws ec2 create-launch-template --launch-template-name my-lt-with-instance-profile --version-description version1 \
--launch-template-data '{"ImageId":"ami-01e24be29428c15b2","InstanceType":"t2.micro","IamInstanceProfile":{"Name":"my-instance-profile"}}'
```