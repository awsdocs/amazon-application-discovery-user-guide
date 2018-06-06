# Setting Up AWS Application Discovery Service<a name="setting-up"></a>

Before you use AWS Application Discovery Service for the first time, complete the following tasks:
+ [Step 1: Sign Up for AWS](#setting-up-signup)
+ [Step 2: Create an IAM User](#setting-up-iam)
+ [Step 3: Attach Required IAM User Policies](#setting-up-user-policy)

## Step 1: Sign Up for AWS<a name="setting-up-signup"></a>

When you sign up for Amazon Web Services \(AWS\), you are charged only for the services that you use\. If you already have an AWS account, you can skip ahead to step 2\. If you don't have an AWS account, use the following procedure to create one\.

**To create an AWS account**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
This might be unavailable in your browser if you previously signed into the AWS Management Console\. In that case, choose **Sign in to a different account**, and then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a PIN using the phone keypad\.

Note your AWS account number, because you'll need it for the next task\.

## Step 2: Create an IAM User<a name="setting-up-iam"></a>

Services such as AWS Application Discovery Service, require that you provide credentials when you access them\. This way the service can determine whether you have permissions to access its resources\. We recommend that you don't use the AWS account root user credentials to make requests\. Instead, create an AWS Identity and Access Management \(IAM\) user, and grant that user full access\. We refer to these users as having administrator\-level credentials\. You can use the administrator\-level credentials to interact with AWS and perform tasks such as create an AWS S3 bucket, create additional IAM users, and grant permissions\. For more information, see [Root Account Credentials vs\. IAM User Credentials](http://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html) in the *AWS General Reference* and [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\. 

If you signed up for AWS but have not created an IAM user for yourself, you can create one using the IAM console\.

**To create an IAM user for yourself and add the user to an Administrators group**

1. Use your AWS account email address and password to sign in as the *[AWS account root user](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)* to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user below and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](http://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane of the console, choose **Users**, and then choose **Add user**\.

1. For **User name**, type **Administrator**\.

1. Select the check box next to **AWS Management Console access**, select **Custom password**, and then type the new user's password in the text box\. You can optionally select **Require password reset** to force the user to create a new password the next time the user signs in\.

1. Choose **Next: Permissions**\.

1. On the **Set permissions for user** page, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, type **Administrators**\.

1. For **Filter**, choose **Job function**\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users, and to give your users access to your AWS account resources\. To learn about using policies to restrict users' permissions to specific AWS resources, go to [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

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

## Step 3: Attach Required IAM User Policies<a name="setting-up-user-policy"></a>

Application Discovery Service uses the IAM\-managed policies listed here to control access to the service or components of the service\. For information about how to attach these managed policies to an IAM user account, see [Working with Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html) in the *IAM User Guide\.*

**AWSApplicationDiscoveryServiceFullAccess**  
Grants the IAM user account access to the Application Discovery Service and Migration Hub APIs\. With this policy, the user can configure Application Discovery Service, start and stop agents, start and stop agentless discovery, and query data from the AWS Discovery Service database\. 

**AWSApplicationDiscoveryAgentAccess**  
Grants the Application Discovery Agent access to register and communicate with Application Discovery Service\. Attach this policy to any user whose credentials are used by Application Discovery Agent\. This policy also grants the user access to Arsenal\. Arsenal is an agent service that is managed and hosted by AWS\. Arsenal forwards data to Application Discovery Service in the cloud\.

**AWSAgentlessDiscoveryService**  
Grants the AWS Agentless Discovery Connector that is running in your VMware vCenter Server the access to register, communicate with, and share connector health metrics with Application Discovery Service\. Attach this policy to any user whose credentials are used by the connector\.

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