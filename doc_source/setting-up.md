# Setting Up AWS Application Discovery Service<a name="setting-up"></a>

Before you use AWS Application Discovery Service for the first time, complete the following tasks:
+ [Step 1: Sign Up for AWS](#setting-up-signup)
+ [Step 2: Create an IAM User](#setting-up-iam)
+ [Step 3: Provide Application Discovery Service Access to Non\-Administrator Users by Attaching Policies](#setting-up-user-policy)

Once you have completed the three steps of [Setting Up AWS Application Discovery Service](#setting-up), it is recommended that you read the section [Understanding and Using Service\-Linked Roles for Application Discovery Service](#using-service-linked-roles)\. There is no set\-up required for you to use this service\-linked role as it is automatically created for you when Continuous Export is  turned on by enabling [Data Exploration in Amazon Athena](explore-data.md)\. However it is important to understand the concept and this section also gives instructions for deleting the service\-linked role\.

## Step 1: Sign Up for AWS<a name="setting-up-signup"></a>

When you sign up for Amazon Web Services \(AWS\), you are charged only for the services that you use\. If you already have an AWS account, you can skip ahead to step 2\. If you don't have an AWS account, use the following procedure to create one\.

**To create an AWS account**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
If you previously signed in to the AWS Management Console using AWS account root user credentials, choose **Sign in to a different account**\. If you previously signed in to the console using IAM credentials, choose **Sign\-in using root account credentials**\. Then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code using the phone keypad\.

Note your AWS account number, because you'll need it for the next task\.

## Step 2: Create an IAM User<a name="setting-up-iam"></a>

Services such as AWS Application Discovery Service require that you provide credentials when you access them\. This way the service can determine whether you have permissions to access its resources\. We recommend that you don't use the AWS account root user credentials to make requests\. Instead, create an AWS Identity and Access Management \(IAM\) user, and grant that user full access\. We refer to these users as having administrator\-level credentials\. You can use the administrator\-level credentials to interact with AWS and perform tasks such as create an AWS S3 bucket, create additional IAM users, and grant permissions\. For more information, see [Root Account Credentials vs\. IAM User Credentials](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html) in the *AWS General Reference* and [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\. 

If you signed up for AWS but have not created an IAM user for yourself, you can create one using the IAM console\.

**To create an IAM user for yourself and add the user to an Administrators group**

1. Use your AWS account email address and password to sign in as the *[AWS account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)* to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user below and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane of the console, choose **Users**, and then choose **Add user**\.

1. For **User name**, type **Administrator**\.

1. Select the check box next to **AWS Management Console access**, select **Custom password**, and then type the new user's password in the text box\. You can optionally select **Require password reset** to force the user to create a new password the next time the user signs in\.

1. Choose **Next: Permissions**\.

1. On the **Set permissions** page, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** type **Administrators**\.

1. For **Filter policies**, select the check box for **AWS managed \- job function**\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Tags** to add metadata to the user by attaching tags as key\-value pairs\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users, and to give your users access to your AWS account resources\. To learn about using policies to restrict users' permissions to specific AWS resources, go to [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

**Note**  
An administrator account will by default inherit all the policies required for accessing Application Discovery Service\.
For a non\-administrator user, you can manually add the policies required to access Application Discovery Service\. Refer to [Step 3: Provide Application Discovery Service Access to Non\-Administrator Users by Attaching Policies](#setting-up-user-policy) for details\.

To sign in as this new IAM user, first sign out of the AWS Management Console\. Next, use the following URL, where *your\_aws\_account\_id* is your AWS account number without the hyphens\. For example, if your AWS account number is `1234-5678-9012`, then *your\_aws\_account\_id* is `123456789012`\)\.

```
https://your_aws_account_id.signin.aws.amazon.com/console/
```

Enter the IAM user name and password that you just created\. When you're signed in, the navigation bar displays ***your\_user\_name*****@*****your\_aws\_account\_id***\.

If you don't want the URL for your sign\-in page to contain your AWS account ID, you can create an account alias\. From the IAM dashboard, click **Create Account Alias** and enter an alias, such as your company name\. To sign in after you create an account alias, use the following URL:

```
https://your_account_alias.signin.aws.amazon.com/console/
```

To verify the sign\-in link for IAM users for your account, open the IAM console and check under **AWS Account Alias** on the dashboard\.

## Step 3: Provide Application Discovery Service Access to Non\-Administrator Users by Attaching Policies<a name="setting-up-user-policy"></a>

Application Discovery Service uses the IAM\-managed policies listed here to control access to the service or components of the service\. An administrator account will by default inherit all the policies required for accessing Application Discovery Service\. If your account is a non\-administrative account, in order to access Application Discovery Service, you need to request your administrator to add the below policies to your account\. For information about how to attach these managed policies to an IAM user account, see [Working with Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html) in the *IAM User Guide\.*

**AWSApplicationDiscoveryServiceFullAccess**  
Grants the IAM user account access to the Application Discovery Service and Migration Hub APIs\. With this policy, the user can configure Application Discovery Service, start and stop agents, start and stop agentless discovery, and query data from the AWS Discovery Service database\. 

**AWSApplicationDiscoveryAgentAccess**  
Grants the Application Discovery Agent access to register and communicate with Application Discovery Service\. Attach this policy to any user whose credentials are used by Application Discovery Agent\. This policy also grants the user access to Arsenal\. Arsenal is an agent service that is managed and hosted by AWS\. Arsenal forwards data to Application Discovery Service in the cloud\.

**AWSAgentlessDiscoveryService**  
Grants the AWS Agentless Discovery Connector that is running in your VMware vCenter Server the access to register, communicate with, and share connector health metrics with Application Discovery Service\. Attach this policy to any user whose credentials are used by the connector\.

**ApplicationDiscoveryServiceContinuousExportServiceRolePolicy**  
This policy is automatically added to your account when you turn on Data Exploration in Amazon Athena and have the `AWSApplicationDiscoveryServiceFullAccess` policy assigned\. It allows AWS Application Discovery Service to create Amazon Kinesis Data Firehose streams to transform and deliver data collected by AWS Application Discovery Service agents to an Amazon S3 bucket in your AWS account\. In addition, this policy creates an AWS Glue Data Catalog with a new database called *application\_discovery\_service\_database* and table schemas for mapping data collected by the agents\.

**AWSDiscoveryContinuousExportFirehosePolicy**  
This policy is required to use Data Exploration in Amazon Athena\. It allows Amazon Kinesis Data Firehose to write data collected from Application Discovery Service to Amazon S3\.

An administrator needs to attach the above policies to your user\. For `AWSDiscoveryContinuousExportFirehosePolicy` policy, the administrator needs to create a role named "AWSApplicationDiscoveryServiceFirehose" with Kinesis Data Firehose as a trusted entity and then attach the policy `AWSDiscoveryContinuousExportFirehosePolicy` to the role as show in the procedures below:

1. In the IAM console, choose **Roles** on the navigation pane\.

1. Choose **Create Role**\.

1. Choose **Kinesis**\.

1. Choose **Kinesis Firehose** as your use case\.

1. Choose **Next: Permissions**\.

1. Under **Filter Policies** search for **AWSDiscoveryContinuousExportFirehosePolicy**\.

1. Check the box beside **AWSDiscoveryContinuousExportFirehosePolicy** and then choose **Next: Review**\.

1. Enter **AWSApplicationDiscoveryServiceFirehose** as the role name and then choose **Create role**\.

Each of the Application Discovery Service managed policies is shown here so that you can customize them as needed\.

**AWSApplicationDiscoveryServiceFullAccess**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "mgh:*",
                "discovery:*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": [
                "iam:GetRole"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

**AWSApplicationDiscoveryAgentAccess**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "arsenal:RegisterOnPremisesAgent"
            ],
            "Resource": "*"
        }
    ]
}
```

**AWSAgentlessDiscoveryService**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "awsconnector:RegisterConnector",
                "awsconnector:GetConnectorHealth"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:GetUser",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::connector-platform-upgrade-info/*",
                "arn:aws:s3:::connector-platform-upgrade-info",
                "arn:aws:s3:::connector-platform-upgrade-bundles/*",
                "arn:aws:s3:::connector-platform-upgrade-bundles",
                "arn:aws:s3:::connector-platform-release-notes/*",
                "arn:aws:s3:::connector-platform-release-notes",
                "arn:aws:s3:::prod.agentless.discovery.connector.upgrade/*",
                "arn:aws:s3:::prod.agentless.discovery.connector.upgrade"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": [
                "arn:aws:s3:::import-to-ec2-connector-debug-logs/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "SNS:Publish"
            ],
            "Resource": "arn:aws:sns:*:*:metrics-sns-topic-for-*"
        },
        {
            "Sid": "Discovery",
            "Effect": "Allow",
            "Action": [
                "Discovery:*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "arsenal",
            "Effect": "Allow",
            "Action": [
                "arsenal:RegisterOnPremisesAgent"
            ],
            "Resource": "*"
        }
    ]
}
```

**ApplicationDiscoveryServiceContinuousExportServiceRolePolicy**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "glue:CreateDatabase",
                "glue:UpdateDatabase",
                "glue:CreateTable",
                "glue:UpdateTable",
                "firehose:CreateDeliveryStream",
                "firehose:DescribeDeliveryStream",
                "logs:CreateLogGroup"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": [
                "firehose:DeleteDeliveryStream",
                "firehose:PutRecord",
                "firehose:PutRecordBatch",
                "firehose:UpdateDestination"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:firehose:*:*:deliverystream/aws-application-discovery-service*"
        },
        {
            "Action": [
                "s3:CreateBucket",
                "s3:ListBucket",
                "s3:PutBucketLogging",
                "s3:PutEncryptionConfiguration"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:s3:::aws-application-discovery-service*"
        },
        {
            "Action": [
                "s3:GetObject"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:s3:::aws-application-discovery-service*/*"
        },
        {
            "Action": [
                "logs:CreateLogStream",
                "logs:PutRetentionPolicy"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:logs:*:*:log-group:/aws/application-discovery-service/firehose*"
        },
        {
            "Action": [
                "iam:PassRole"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:iam::*:role/AWSApplicationDiscoveryServiceFirehose",
            "Condition": {
                "StringLike": {
                    "iam:PassedToService": "firehose.amazonaws.com"
                }
            }
        },
        {
            "Action": [
                "iam:PassRole"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:iam::*:role/service-role/AWSApplicationDiscoveryServiceFirehose",
            "Condition": {
                "StringLike": {
                    "iam:PassedToService": "firehose.amazonaws.com"
                }
            }
        }
    ]
}
```

**AWSDiscoveryContinuousExportFirehosePolicy**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:GetTableVersions"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:AbortMultipartUpload",
                "s3:GetBucketLocation",
                "s3:GetObject",
                "s3:ListBucket",
                "s3:ListBucketMultipartUploads",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::aws-application-discovery-service-*",
                "arn:aws:s3:::aws-application-discovery-service-*/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:*:*:log-group:/aws/application-discovery-service/firehose:log-stream:*"
            ]
        }
    ]
}
```

## Understanding and Using Service\-Linked Roles for Application Discovery Service<a name="using-service-linked-roles"></a>

AWS Application Discovery Service uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Application Discovery Service\. Service\-linked roles are predefined by Application Discovery Service and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Application Discovery Service easier because you don’t have to manually add the necessary permissions\. Application Discovery Service defines the permissions of its service\-linked roles, and unless defined otherwise, only Application Discovery Service can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your Application Discovery Service resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

### Service\-Linked Role Permissions for Application Discovery Service<a name="service-linked-role-permissions"></a>

Application Discovery Service uses the service\-linked role named **AWSServiceRoleForApplicationDiscoveryServiceContinuousExport** – Enables access to AWS Services and Resources used or managed by AWS Application Discovery Service\.

The AWSServiceRoleForApplicationDiscoveryServiceContinuousExport service\-linked role trusts the following services to assume the role:
+ `continuousexport.discovery.amazonaws.com`

The role permissions policy allows Application Discovery Service to complete the following actions: 

glue  
 `CreateDatabase`   
 `UpdateDatabase`   
 `CreateTable`   
 `UpdateTable` 

firehose  
 `CreateDeliveryStream`   
 `DeleteDeliveryStream`   
 `DescribeDeliveryStream`   
 `PutRecord`   
 `PutRecordBatch`   
 `UpdateDestination` 

s3  
 `CreateBucket`   
 `ListBucket`   
 `GetObject` 

logs  
 `CreateLogGroup`   
 `CreateLogStream`   
 `PutRetentionPolicy` 

iam  
 `PassRole` 

This is the full policy showing which resources the above actions apply to:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "glue:CreateDatabase",
                "glue:UpdateDatabase",
                "glue:CreateTable",
                "glue:UpdateTable",
                "firehose:CreateDeliveryStream",
                "firehose:DescribeDeliveryStream",
                "logs:CreateLogGroup"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": [
                "firehose:DeleteDeliveryStream",
                "firehose:PutRecord",
                "firehose:PutRecordBatch",
                "firehose:UpdateDestination"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:firehose:*:*:deliverystream/aws-application-discovery-service*"
        },
        {
            "Action": [
                "s3:CreateBucket",
                "s3:ListBucket",
                "s3:PutBucketLogging",
                "s3:PutEncryptionConfiguration"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:s3:::aws-application-discovery-service*"
        },
        {
            "Action": [
                "s3:GetObject"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:s3:::aws-application-discovery-service*/*"
        },
        {
            "Action": [
                "logs:CreateLogStream",
                "logs:PutRetentionPolicy"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:logs:*:*:log-group:/aws/application-discovery-service/firehose*"
        },
        {
            "Action": [
                "iam:PassRole"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:iam::*:role/AWSApplicationDiscoveryServiceFirehose",
            "Condition": {
                "StringLike": {
                    "iam:PassedToService": "firehose.amazonaws.com"
                }
            }
        },
        {
            "Action": [
                "iam:PassRole"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:iam::*:role/service-role/AWSApplicationDiscoveryServiceFirehose",
            "Condition": {
                "StringLike": {
                    "iam:PassedToService": "firehose.amazonaws.com"
                }
            }
        }
    ]
}
```

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

### Creating a Service\-Linked Role for Application Discovery Service<a name="create-service-linked-role"></a>

You don't need to manually create a service\-linked role\. The AWSServiceRoleForApplicationDiscoveryServiceContinuousExport service\-linked role is automatically created when Continuous Export is implicitly turned on by a\) confirming options in the dialog box presented from the Data Collectors page after you choose “Start data collection”, or click the slider labeled, “Data exploration in Athena”, or b\) when you call the StartContinuousExport API using the AWS CLI\.  

**Important**  
This service\-linked role can appear in your account if you completed an action in another service that uses the features supported by this role\.  To learn more, see [A New Role Appeared in My IAM Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html#troubleshoot_roles_new-role-appeared)\.

#### Creating the Service\-Linked Role from the Migration Hub Console<a name="create-service-linked-role-service-console"></a>

You can use the Migration Hub console to create the AWSServiceRoleForApplicationDiscoveryServiceContinuousExport service\-linked role\.

**To create the service\-linked role \(console\)**

1. In the navigation pane, choose **Data Collectors**\.

1. Choose the **Agents** tab\.

1. Toggle the **Data exploration in Athena** slider to the On position\.

1. In the dialog box generated from the previous step, click the checkbox agreeing to associated costs and choose **Continue** or **Enable**\.

#### Creating the Service\-Linked Role from the AWS CLI<a name="create-service-linked-role-service-cli"></a>

You can use Application Discovery Service commands from the AWS Command Line Interface to create the AWSServiceRoleForApplicationDiscoveryServiceContinuousExport service\-linked role\.

This service\-linked role is automatically created when you start Continuous Export from the AWS CLI \(the AWS CLI must first be installed in your environment\)\.

**To create the service\-linked role \(CLI\) by starting Continuous Export from the AWS CLI**

1. Install the AWS CLI for your operating system \(Linux, macOS, or Windows\)\. See the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/) for instructions\.

1. Open the Command prompt \(Windows\) or Terminal \(Linux or macOS\)\.

   1. Type `aws configure` and press Enter\.

   1. Enter your AWS Access Key Id and AWS Secret Access Key\.

   1. Enter `us-west-2` for the Default Region Name\.

   1. Enter `text` for Default Output Format\.

1. Type the following command:

   ```
   aws discovery start-continuous-export
   ```

You can also use the IAM console to create a service\-linked role with the **Discovery Service \- Continuous Export** use case\. In the IAM CLI or the IAM API, create a service\-linked role with the `continuousexport.discovery.amazonaws.com` service name\. For more information, see [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. If you delete this service\-linked role, you can use this same process to create the role again\.

### Deleting a Service\-Linked Role for Application Discovery Service<a name="delete-service-linked-role"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up your service\-linked role before you can manually delete it\.

#### Cleaning Up the Service\-Linked Role<a name="service-linked-role-review-before-delete"></a>

Before you can use IAM to delete a service\-linked role, you must first delete any resources used by the role\.

**Note**  
If Application Discovery Service is using the role when you try to delete the resources, then the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

**To delete Application Discovery Service resources used by the AWSServiceRoleForApplicationDiscoveryServiceContinuousExport service\-linked role from the Migration Hub Console**

1. In the navigation pane, choose **Data Collectors**\.

1. Choose the **Agents** tab\.

1. Toggle the **Data exploration in Athena** slider to the Off position\.

**To delete Application Discovery Service resources used by the AWSServiceRoleForApplicationDiscoveryServiceContinuousExport service\-linked role from the AWS CLI**

1. Install the AWS CLI for your operating system \(Linux, macOS, or Windows\)\. See the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/) for instructions\.

1. Open the Command prompt \(Windows\) or Terminal \(Linux or macOS\)\.

   1. Type `aws configure` and press Enter\.

   1. Enter your AWS Access Key Id and AWS Secret Access Key\.

   1. Enter `us-west-2` for the Default Region Name\.

   1. Enter `text` for Default Output Format\.

1. Type the following command:

   ```
   aws discovery stop-continuous-export --export-id <export ID>
   ```

   1. If you don't know the export\-ID of the continuous export you want to stop, enter the following command to see the continuous export's ID:

     ```
     aws discovery describe-continuous-exports
     ```

1. Enter the follow command to ensure that Continuous Export has stopped by verifying its return status is "INACTIVE":

   ```
   aws discovery describe-continuous-export
   ```

#### Manually Delete the Service\-Linked Role<a name="slr-manual-delete"></a>

You can delete the AWSServiceRoleForApplicationDiscoveryServiceContinuousExport service\-linked role by using the IAM console, the IAM CLI, or the IAM API\. If you no longer need to use the Discovery Service \- Continuous Export features that require this service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

**Note**  
You must first clean up your service\-linked role before you can delete it\. See [Cleaning Up the Service\-Linked Role](#service-linked-role-review-before-delete)\.

 

 