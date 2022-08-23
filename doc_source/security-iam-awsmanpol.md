# AWS managed policies for AWS Application Discovery Service<a name="security-iam-awsmanpol"></a>







To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.









## AWS managed policy: AWSApplicationDiscoveryServiceFullAccess<a name="security-iam-awsmanpol-AWSApplicationDiscoveryServiceFullAccess"></a>

The `AWSApplicationDiscoveryServiceFullAccess` policy grants an IAM user account access to the Application Discovery Service and Migration Hub APIs\. 

An IAM user account with this policy attached can configure Application Discovery Service, start and stop agents, start and stop agentless discovery, and query data from the AWS Discovery Service database\. For an example of this policy, see [Granting full access to Application Discovery Service](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-ads-fullaccess)\.

## AWS managed policy: AWSApplicationDiscoveryAgentlessCollectorAccess<a name="security-iam-awsmanpol-AWSApplicationDiscoveryAgentlessCollectorAccess"></a>

The `AWSApplicationDiscoveryAgentlessCollectorAccess` managed policy grants the Application Discovery Service Agentless Collector \(Agentless Collector\) access to register and communicate with the Application Discovery Service, and communicate with other AWS services\.

This policy must be attached to the IAM user whose credentials are used to configure the Agentless Collector\.

**Permissions details**

This policy includes the following permissions\.


+ `aresenal` – Allows the collector to register with the Application Discovery Service application\. This is necessary to be able to send collected data back to AWS\.
+ `ecr-public` – Allows the collector to make calls to the Amazon Elastic Container Registry Public \(Amazon ECR Public\) where the latest updates are found for the collector\.
+ `mgh` – Allows the collector to call AWS Migration Hub to retrieve the home region of the account used to configure the collector\. This is necessary to know which region the collected data should be sent to\.
+ `sts` – Allows the collector to retrieve a service bearer token so that the collector can make calls to Amazon ECR Public to get the latest updates\.

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
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr-public:DescribeImages"
            ],
            "Resource": "arn:aws:ecr-public::446372222237:repository/6e5498e4-8c31-4f57-9991-13b4b992ff7b"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr-public:GetAuthorizationToken"

            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "mgh:GetHomeRegion"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "sts:GetServiceBearerToken"
            ],
            "Resource": "*"
        }
    ]
}
```

## AWS managed policy: AWSApplicationDiscoveryAgentAccess<a name="security-iam-awsmanpol-AWSApplicationDiscoveryAgentAccess"></a>

The `AWSApplicationDiscoveryAgentAccess` policy grants the Application Discovery Agent access to register and communicate with Application Discovery Service\.

You attach this policy to any user whose credentials are used by Application Discovery Agent\. 

This policy also grants the user access to Arsenal\. Arsenal is an agent service that is managed and hosted by AWS\. Arsenal forwards data to Application Discovery Service in the cloud\. For an example of this policy, see [Granting access to discovery agents](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-ads-agentaccess)\.

## AWS managed policy: AWSAgentlessDiscoveryService<a name="security-iam-awsmanpol-AWSAgentlessDiscoveryService"></a>

The `AWSAgentlessDiscoveryService` policy grants the AWS Agentless Discovery Connector that is running in your VMware vCenter Server access to register, communicate with, and share connector health metrics with Application Discovery Service\. 

You attach this policy to any user whose credentials are used by the connector\.

## AWS managed policy: ApplicationDiscoveryServiceContinuousExportServiceRolePolicy<a name="security-iam-awsmanpol-ApplicationDiscoveryServiceContinuousExportServiceRolePolicy"></a>

If your IAM account has the `AWSApplicationDiscoveryServiceFullAccess` policy attached, `ApplicationDiscoveryServiceContinuousExportServiceRolePolicy` is automatically attached to your account when you turn on Data Exploration in Amazon Athena\.

This policy allows AWS Application Discovery Service to create Amazon Kinesis Data Firehose streams to transform and deliver data collected by AWS Application Discovery Service agents to an Amazon S3 bucket in your AWS account\. 

In addition, this policy creates an AWS Glue Data Catalog with a new database called *application\_discovery\_service\_database* and table schemas for mapping data collected by the agents\. For an example of this policy, see [ Granting permissions for agent data collection](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-ads-export-service)\.

## AWS managed policy: AWSDiscoveryContinuousExportFirehosePolicy<a name="security-iam-awsmanpol-AWSDiscoveryContinuousExportFirehosePolicy"></a>

The `AWSDiscoveryContinuousExportFirehosePolicy` policy is required to use Data Exploration in Amazon Athena\. It allows Amazon Kinesis Data Firehose to write data collected from Application Discovery Service to Amazon S3\. For information about using this policy, see [Creating the AWSApplicationDiscoveryServiceFirehose Role](#security-iam-awsmanpol-create-firehose-role)\. For an example of this policy, see [Granting permissions for data exploration](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-ads-export-firehose)\.

## Creating the AWSApplicationDiscoveryServiceFirehose Role<a name="security-iam-awsmanpol-create-firehose-role"></a>

An administrator attaches managed policies to your IAM user account\. When using the `AWSDiscoveryContinuousExportFirehosePolicy` policy, the administrator must first create a role named **AWSApplicationDiscoveryServiceFirehose** with Kinesis Data Firehose as a trusted entity and then attach the `AWSDiscoveryContinuousExportFirehosePolicy` policy to the role, as shown in the following procedure\.

**To create the **AWSApplicationDiscoveryServiceFirehose** IAM role**

1. In the IAM console, choose **Roles** on the navigation pane\.

1. Choose **Create Role**\.

1. Choose **Kinesis**\.

1. Choose **Kinesis Firehose** as your use case\.

1. Choose **Next: Permissions**\.

1. Under **Filter Policies** search for **AWSDiscoveryContinuousExportFirehosePolicy**\.

1. Select the box beside **AWSDiscoveryContinuousExportFirehosePolicy**, and then choose **Next: Review**\.

1. Enter **AWSApplicationDiscoveryServiceFirehose** as the role name, and then choose **Create role**\.





## Application Discovery Service updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for Application Discovery Service since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the [Document History for AWS Application Discovery Service](doc-history.md) page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [AWSApplicationDiscoveryAgentlessCollectorAccess](#security-iam-awsmanpol-AWSApplicationDiscoveryAgentlessCollectorAccess) – New policy made available with the Agentless Collector launch   |  Application Discovery Service added the new managed policy `AWSApplicationDiscoveryAgentlessCollectorAccess` that grants the Agentless Collector access to register and communicate with the Application Discovery Service, and communicate with other AWS services\.  | August 16, 2022 | 
|  Application Discovery Service started tracking changes  |  Application Discovery Service started tracking changes for its AWS managed policies\.  | March 1, 2021 | 