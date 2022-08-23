# Step 1: Create an IAM user for Agentless Collector<a name="agentless-collector-gs-iam-user"></a>

To use Agentless Collector, in the AWS account that you used in [Sign in to the Migration Hub console and choose a home Region](setting-up-choose-home-region.md), you must create an AWS Identity and Access Management \(IAM\) user that is set up to use the predefined IAM policy [AWSApplicationDiscoveryAgentlessCollectorAccess](security-iam-awsmanpol.md#security-iam-awsmanpol-AWSApplicationDiscoveryAgentlessCollectorAccess)\. You attach this IAM policy when you create the IAM user\.

We recommend that you create a non\-administrative IAM user to use with Agentless Collector\. When creating non\-administrative IAM users, follow the security best practice [ Grant Least Privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege), granting users minimum permissions\. 

**To create a non\-administrator IAM user to use with Agentless Collector**

1. In AWS Management Console, navigate to the IAM console, using the AWS account that you used to set the home Region in [Sign in to the Migration Hub console and choose a home Region](setting-up-choose-home-region.md)\.

1. Create a non\-administrator IAM user by following the instructions for creating a user with the console as described in [Creating an IAM user in your AWS account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) in the *IAM User Guide*\. 

   While following the instructions in the *IAM User Guide*:
   + When on the step about selecting the type of access, select **Programmatic access**\. Note, while not recommended, only select **AWS Management Console access** if you plan to use the same IAM user credentials for accessing the AWS console\. 
   + When on the step about the **Set permission** page, choose the option to **Attach existing policies to user directly**\. Then select the `AWSApplicationDiscoveryAgentlessCollectorAccess` managed IAM policy from the list of policies\.
   + When on the step about viewing the user's access keys \(access key IDs and secret access keys\), follow the guidance in the **Important** note about saving the user's new access key ID and secret access key in a safe and secure place\. You'll need these access keys in [Step 5: Configure Agentless Collector](agentless-collector-gs-configure.md)\.