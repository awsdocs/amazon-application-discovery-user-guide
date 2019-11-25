# Getting Started with AWS Application Discovery Service<a name="getting-started"></a>

In this section, you can learn how to get started with AWS Application Discovery Service\. The topics explain how to access the Application Discovery Service console and the two ways to discover data on your local servers\.

**Topics**
+ [Assumptions](#gs-assumptions)
+ [Access to AWS Application Discovery Service](#access-via-console-and-api)
+ [Two Ways to Start Collecting Data](#collect-two-ways)
+ [AWS Agentless Discovery Connector](discovery-connector.md)
+ [AWS Application Discovery Agent](discovery-agent.md)
+ [Migration Hub Import](discovery-import.md)

## Assumptions<a name="gs-assumptions"></a>

To use Application Discovery Service, the following is assumed\.
+ You have signed up for AWS\. For more information, see [Setting Up AWS Application Discovery Service](setting-up.md)
+ You have selected a Migration Hub home region\. For more information, see [the documentation regarding home regions\.](https://docs.aws.amazon.com/migrationhub/latest/ug/home-region.html)

Here's what to expect:
+ The Migration Hub home region is the only region where Application Discovery Service stores your discovery and planning data\.
+ Discovery agents, connectors, and imports can be used in your selected Migration Hub home region only\.
+ For a list of AWS Regions where you can use Application Discovery Service, see the [Amazon Web Services General Reference](https://docs.aws.amazon.com/general/latest/gr/rande.html#migrationhub-region)\.

## Access to AWS Application Discovery Service<a name="access-via-console-and-api"></a>

 AWS Application Discovery Service helps you plan your migration to the AWS Cloud\. It gives you these capabilities:
+ Collect data regarding usage and configuration for your on\-premises servers\.
+ View servers and group them into applications from the AWS Migration Hub console\.
+ Track the migration status on the Migration Hub console in your home region\.

 You can find AWS Application Discovery Service at [AWS Application Discovery Service](http://console.aws.amazon.com/discovery/home)\.

The AWS Application Discovery Service API can export system data and network connections for your servers\. This information helps you to group your servers for migration planning\. For more information about the API, see the [Application Discovery Service API Reference](https://docs.aws.amazon.com/application-discovery/latest/APIReference/)\.

The AWS SDKs assist you to create applications that interact with Application Discovery Service\. The AWS SDKs for Java, \.NET, and PHP wrap the Application Discovery Service API to simplify your programming tasks\. For more information, see [Sample Code Libraries](http://aws.amazon.com/code)\.

## Two Ways to Start Collecting Data<a name="collect-two-ways"></a>

To begin collecting data, use either the **Discovery Connector** or the **Discovery Agent**\. A description follows in each topic that will help you decide which discovery tool to use\. Each of the following topics also guides you through installation and deployment\.

**Topics**
+ [AWS Agentless Discovery Connector](discovery-connector.md)
+ [ AWS Application Discovery Agent](discovery-agent.md)