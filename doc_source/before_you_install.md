# Before You Install<a name="before_you_install"></a>

Complete the following tasks before you install the AWS Application Discovery Service components\.

**Note**  
You must have an active AWS account to perform these procedures\. To create one, open [https://aws\.amazon\.com/](https://aws.amazon.com/), and choose **Create an AWS Account**\.

## Create an IAM User<a name="appdisc-iam-user"></a>

Services in AWS, such as Application Discovery Service, require that you provide credentials when you access them\. This allows the service to determine whether you have permissions to access its resources\. We recommend that you avoid accessing AWS with the credentials for your AWS account; instead, use AWS Identity and Access Management \(IAM\)\. Create an IAM user, and then add the user to an IAM group with administrative permissions and grant administrative permissions to this user\. You can then access AWS using a special URL and the credentials for the IAM user\.

For more information about setting up an IAM administrator, see [Creating Your First IAM Admin User and Group](http://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)\. For information about IAM, see [What Is IAM?](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\.

## Attach Required IAM User Policies<a name="appdisc-user-policy"></a>

Application Discovery Service uses the following IAM managed policies to control access to the service or components of the service\. For information about how to attach managed policies to an IAM user account, see [Working with Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html)\. 

**Note**  
The policies described here only provide access to the Application Discovery Service\. Application Discovery Service is now integrated with the [AWS Migration Hub](http://docs.aws.amazon.com/migrationhub/latest/ug/whatishub.html)\. For access to features of the Migration Hub other than Application Discovery, you must apply additional policies\. For more information, see [Authentication and Access Control for AWS Migration Hub](http://docs.aws.amazon.com/migrationhub/latest/ug/auth-and-access-control.html)\. 

**AWSApplicationDiscoveryServiceFullAccess**  
Grants the IAM user account access to the Application Discovery Service API\. With this policy, the user can configure Application Discovery Service, start and stop agents, start and stop agentless discovery, and query data from the AWS Discovery Service database\. This policy also grants the user access to Arsenal\. Arsenal is an agent service managed and hosted by AWS that forwards data to Application Discovery Service in the cloud\.

**AWSApplicationDiscoveryAgentAccess**  
Grants Application Discovery Agent access to register and communicate with Application Discovery Service\. This policy should be attached to any user whose credentials are to be used by Application Discovery Agent\.

**AWSAgentlessDiscoveryService**  
Grants the AWS Agentless Discovery Connector running in your VMware vCenter Server access to register, communicate with, and share connector health metrics with Application Discovery Service\. This policy must be attached to any user whose credentials are to be used by the connector\.

**Note**  
The `AWSAgentlessDiscoveryService` policy uses the following API actions: `awsconnector:RegisterConnector` and `awsconnector:GetConnectorHealth`\. For more information, see [API Actions of the AWSAgentlessDiscoveryService IAM Policy](#agentless-IAM-actions)\.

Each of the Application Discovery Service managed policies is shown here so that you can customize them as needed\.

**AWSApplicationDiscoveryServiceFullAccess**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                “discovery:*"
            ],
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

### API Actions of the AWSAgentlessDiscoveryService IAM Policy<a name="agentless-IAM-actions"></a>

The `AWSAgentlessDiscoveryService` IAM policy uses `awsconnector:RegisterConnector` and `awsconnector:GetConnectorHealth` to grant the AWS Agentless Discovery Connector access to register, communicate with, and share connector health metrics with Application Discovery Service\. More specifically, these API actions perform the following operations and return the following errors:

```
<operation name="RegisterConnector">
    <input target="RegisterConnectorRequest" /> Request type for RegisterConnector API.
    <output target="RegisterConnectorResponse" /> Response type for RegisterConnector API.
    <member name="snsTopicArn" target="String" /> Metrics SNS topic arn that is created/whitelisted for caller.
    <error target="AuthenticationFailureException" /> This exception is thrown if the credentials passed in the request could not be validated or user is not authorized to perform the operation.
    <error target="ServerInternalErrorException" /> This exception is thrown if there is erroneous logic in the service. It also includes all service dependency exceptions.
    <error target="ServiceUnavailableException" /> The request has failed due to a temporary failure of the server.
    <error target="ServerThrottleException" /> This exception is thrown if maximum number of request(for a given API) from an IAM user has been reached.
    <error target="InvalidParameterException" /> The request is missing required parameter(s) or has invalid parameter(s).
</operation>
                
<operation name="GetConnectorHealth">
    <input target="GetConnectorHealthRequest" /> Request type for GetConnectorHealth API.
    <member name="connectorId" target="String" /> Connector Id that will be used to verify identity of caller.
    <output target="GetConnectorHealthResponse" /> Response type for GetConnectorHealth API.
    <member name="serviceHealthList" target="ServiceHealthList" /> Contains all services' health information.
    <member target="ServiceHealth" /> The object that contains all health information for a given service.
    <member name="serviceName" target="String" /> The name of the service which was using connector health metrics publisher.
    <member name="healthList" target="HealthList" /> The list of health for the given service.
    <member target="Health" /> The object that represent a unique health metric that was published from the connector.
    <error target="AuthenticationFailureException" /> This exception is thrown if there is erroneous logic in the service. It also includes all service dependency exceptions.
    <error target="ServiceUnavailableException" /> The request has failed due to a temporary failure of the server.
    <error target="ServerThrottleException" /> This exception is thrown if maximum number of request(for a given API) from an IAM user has been reached.
    <error target="InvalidParameterException" /> The request is missing required parameter(s) or has invalid parameter(s).
</operation>
                
<structure name="Health"> The object that represent a unique health metric that was published from the connector.
    <member name="name" target="String" /> The name of the health that corresponds to "metric" field in Connector Metrics Publisher. The value of name is not user visible label that will show in Connector dashboard.
    <member name="value" target="String" /> The value for the health metric. It's a json that contains more information about how a health will display in connector dashboard.
    <member name="lastChecked" target="TimeStamp" /> The publish time for the last received metric.
</structure>
```