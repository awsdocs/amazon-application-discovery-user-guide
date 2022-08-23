# AWS Application Discovery Service Identity\-Based Policy Examples<a name="security_iam_id-based-policy-examples"></a>

By default, IAM users and roles don't have permission to create or modify Application Discovery Service resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform specific API operations on the specified resources they need\. The administrator must then attach those policies to the IAM users or groups that require those permissions\.

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating Policies on the JSON Tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Granting full access to Application Discovery Service](#security_iam_id-based-policy-examples-ads-fullaccess)
+ [Granting access to discovery agents](#security_iam_id-based-policy-examples-ads-agentaccess)
+ [Granting permissions for agent data collection](#security_iam_id-based-policy-examples-ads-export-service)
+ [Granting permissions for data exploration](#security_iam_id-based-policy-examples-ads-export-firehose)
+ [Granting permissions to use the Migration Hub console network diagram](#security_iam_id-based-policy-examples-network-connection-graph)

## Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies are very powerful\. They determine whether someone can create, access, or delete Application Discovery Service resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started using AWS managed policies** – To start using Application Discovery Service quickly, use AWS managed policies to give your employees the permissions they need\. These policies are already available in your account and are maintained and updated by AWS\. For more information, see [Get started using permissions with AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-use-aws-defined-policies) in the *IAM User Guide*\.
+ **Grant least privilege** – When you create custom policies, grant only the permissions required to perform a task\. Start with a minimum set of permissions and grant additional permissions as necessary\. Doing so is more secure than starting with permissions that are too lenient and then trying to tighten them later\. For more information, see [Grant least privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.
+ **Enable MFA for sensitive operations** – For extra security, require IAM users to use multi\-factor authentication \(MFA\) to access sensitive resources or API operations\. For more information, see [Using multi\-factor authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.
+ **Use policy conditions for extra security** – To the extent that it's practical, define the conditions under which your identity\-based policies allow access to a resource\. For example, you can write conditions to specify a range of allowable IP addresses that a request must come from\. You can also write conditions to allow requests only within a specified date or time range, or to require the use of SSL or MFA\. For more information, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Granting full access to Application Discovery Service<a name="security_iam_id-based-policy-examples-ads-fullaccess"></a>

The AWSApplicationDiscoveryServiceFullAccess managed policy grants the IAM user account access to the Application Discovery Service and Migration Hub APIs\. 

An IAM user with this policy attached to their account can configure Application Discovery Service, start and stop agents, start and stop agentless discovery, and query data from the AWS Discovery Service database\. For more information about this policy, see [AWS managed policies for AWS Application Discovery Service](security-iam-awsmanpol.md)\. 

**Example AWSApplicationDiscoveryServiceFullAccess policy**  

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

## Granting access to discovery agents<a name="security_iam_id-based-policy-examples-ads-agentaccess"></a>

The AWSApplicationDiscoveryAgentAccess managed policy grants the Application Discovery Agent access to register and communicate with Application Discovery Service\. For more information about this policy, see [AWS managed policies for AWS Application Discovery Service](security-iam-awsmanpol.md)\.

Attach this policy to any user whose credentials are used by Application Discovery Agent\. 

This policy also grants the user access to Arsenal\. Arsenal is an agent service that is managed and hosted by AWS\. Arsenal forwards data to Application Discovery Service in the cloud\.

**Example AWSApplicationDiscoveryAgentAccess Policy**  

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

## Granting permissions for agent data collection<a name="security_iam_id-based-policy-examples-ads-export-service"></a>

The ApplicationDiscoveryServiceContinuousExportServiceRolePolicy managed policy allows AWS Application Discovery Service to create Amazon Kinesis Data Firehose streams to transform and deliver data collected by Application Discovery Service agents to an Amazon S3 bucket in your AWS account\.

In addition, this policy creates an AWS Glue Data Catalog with a new database called `application_discovery_service_database` and table schemas for mapping data collected by the agents\. 

For information about using this policy, see [AWS managed policies for AWS Application Discovery Service](security-iam-awsmanpol.md)\.

**Example ApplicationDiscoveryServiceContinuousExportServiceRolePolicy**  

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

## Granting permissions for data exploration<a name="security_iam_id-based-policy-examples-ads-export-firehose"></a>

The AWSDiscoveryContinuousExportFirehosePolicy policy is required to use Data Exploration in Amazon Athena\. It allows Amazon Kinesis Data Firehose to write data collected from Application Discovery Service to Amazon S3\. For information about using this policy, see [Creating the AWSApplicationDiscoveryServiceFirehose Role](security-iam-awsmanpol.md#security-iam-awsmanpol-create-firehose-role)\. 

**Example AWSDiscoveryContinuousExportFirehosePolicy**  

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

## Granting permissions to use the Migration Hub console network diagram<a name="security_iam_id-based-policy-examples-network-connection-graph"></a>

To grant access to the AWS Migration Hub console network diagram when creating an identity\-based policy that allows or denies access to Application Discovery Service or Migration Hub, you might need to add the `discovery:GetNetworkConnectionGraph` action to the policy\.

You must use the `discovery:GetNetworkConnectionGraph` action in new policies or update older policies if the following are both true for the policy:
+ The policy allows or denies access to Application Discovery Service or the Migration Hub\.
+ The policy grants access permissions using one more specific discovery actions like `discovery:action-name` rather than `discovery:*`\.

The following example shows how to use the `discovery:GetNetworkConnectionGraph` action in an IAM policy\.

**Example**  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": ["discovery:GetNetworkConnectionGraph"],
            "Resource": "*"
        }
    ]
}
```

For information about the Migration Hub network diagram, see [Viewing network connections in Migration Hub](https://docs.aws.amazon.com/migrationhub/latest/ug/network-diagram.html)\.