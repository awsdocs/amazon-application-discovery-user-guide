# Getting Started with AWS Application Discovery Service<a name="getting-started"></a>

In this section, you can find information about how to get started with AWS Application Discovery Service\. Included are topics about how to access the Application Discovery Service console and the two ways of discovering data on your local servers\.

**Topics**
+ [Assumptions](#gs-assumptions)
+ [Accessing AWS Application Discovery Service](#access-via-console-and-api)
+ [Two Ways to Start Collecting Data](#collect-two-ways)
+ [AWS Agentless Discovery Connector](discovery-connector.md)
+ [AWS Application Discovery Agent](discovery-agent.md)

## Assumptions<a name="gs-assumptions"></a>

To use Application Discovery Service, the following is assumed:
+ You have signed up for AWS\. For more information, see [Setting Up AWS Application Discovery Service](setting-up.md)
+ The Application Discovery Service tools only send their operational status if you have authorized, that is, connected, them\.
+ For a list of AWS Regions where you can use Application Discovery Service, see the [Amazon Web Services General Reference](http://docs.aws.amazon.com/general/latest/gr/rande.html#migrationhub-region)\.

## Accessing AWS Application Discovery Service<a name="access-via-console-and-api"></a>

You can use AWS Application Discovery Service to help you plan your migration to the AWS Cloud by collecting usage and configuration data about your on\-premises servers\. Using the integrated AWS Migration Hub console, you can view the discovered servers, group them into applications, and then track the migration status of each application\. You can find AWS Application Discovery Service at [AWS Application Discovery Service](http://console.aws.amazon.com/discovery/home)\.

Additionally, you can use the AWS Application Discovery Service API to export system data and network connections about your discovered servers\. This information helps you to group your servers into applications for migration planning\. For more information about the API, see the [Application Discovery Service API Reference](http://docs.aws.amazon.com/application-discovery/latest/APIReference/)\. 

You can also use the AWS SDKs to develop applications that interact with Application Discovery Service\. The AWS SDKs for Java, \.NET, and PHP wrap the underlying Application Discovery Service API to simplify your programming tasks\. For information about downloading the SDK libraries, see [Sample Code Libraries](http://aws.amazon.com/code)\.

## Two Ways to Start Collecting Data<a name="collect-two-ways"></a>

To begin collecting data, use either the **Discovery Connector** or the **Discovery Agent**\. A detailed description follows in each topic that will help you decide which discovery tool to use\. Each of the following topics also has instructions to guide you through installation and deployment\.

**Topics**
+ [AWS Agentless Discovery Connector](discovery-connector.md)
+ [ AWS Application Discovery Agent](discovery-agent.md)