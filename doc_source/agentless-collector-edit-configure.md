# Editing Agentless Collector settings<a name="agentless-collector-edit-configure"></a>

You configured the collector when you first set up Application Discovery Service Agentless Collector \(Agentless Collector\) as described in [Step 5: Configure Agentless Collector](agentless-collector-gs-configure.md)\. The following procedure describes how to edit Agentless Collector configuration settings\.

**To edit the collector configuration settings**
+ Choose the **Edit collector settings** button on the Agentless Collector dashboard\.

  On the **Edit collector settings** page, perform the following:

  1. For **Collector name**, enter a name to identify the collector\. The name can contain spaces but it cannot contain special characters\.

  1. Under **Destination AWS account for discovery data**, enter the AWS access key and secret key for the AWS account to specify as the destination account to receive the data discovered by the collector\. For information about the requirements for the IAM user, see [Step 1: Create an IAM user for Agentless Collector](agentless-collector-gs-iam-user.md)\.

     1. For **AWS access\-key**, enter the access key of the AWS account IAM user that you're specifying as the destination account\.

     1. For **AWS secret\-key**, enter the secret key of the AWS account IAM user that you're specifying as the destination account\.

  1. Under **Agentless Collector password**, change the password to use to authenticate access to the Agentless Collector\.

     1. For **Agentless Collector password**, enter a password to use to authenticate access to the Agentless Collector\.

     1. For **Re\-enter Agentless Collector password**, for verification enter the password again\.

  1. Choose **Save configurations**\.

Next, you'll see [The Agentless Collector dashboard](agentless-collector-dashboard.md)\.