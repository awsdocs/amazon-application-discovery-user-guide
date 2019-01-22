# Getting Started with AWS Application Discovery Service<a name="getting-started"></a>

In this section, you can learn how to get started with AWS Application Discovery Service\. The topics explain how to access the Application Discovery Service console and the two ways to discover data on your local servers\.

**Topics**
+ [Assumptions](#gs-assumptions)
+ [Accessing AWS Application Discovery Service](#access-via-console-and-api)
+ [Two Ways to Start Collecting Data](#collect-two-ways)
+ [AWS Agentless Discovery Connector](discovery-connector.md)
+ [AWS Application Discovery Agent](discovery-agent.md)
+ [Migration Hub Import](discovery-import.md)

## Assumptions<a name="gs-assumptions"></a>

To use Application Discovery Service, the following is assumed\.
+ You have signed up for AWS\. For more information, see [Setting Up AWS Application Discovery Service](setting-up.md)
+ The Application Discovery Service tools only send their status if you have authorized them\.
+ For a list of AWS Regions where you can use Application Discovery Service, see the [Amazon Web Services General Reference](https://docs.aws.amazon.com/general/latest/gr/rande.html#migrationhub-region)\.

## Accessing AWS Application Discovery Service<a name="access-via-console-and-api"></a>

Use AWS Application Discovery Service to help you plan your migration to the AWS Cloud\. Collect use and configuration data about your on\-premises servers\. Use the AWS Migration Hub console to view servers, group them into applications, and then track the migration status\. You can find AWS Application Discovery Service at [AWS Application Discovery Service](http://console.aws.amazon.com/discovery/home)\.

Use the AWS Application Discovery Service API to export system data and network connections for your servers\. This information helps you to group your servers for migration planning\. For more information about the API, see the [Application Discovery Service API Reference](https://docs.aws.amazon.com/application-discovery/latest/APIReference/)\. 

Also, use the AWS SDKs to create applications that interact with Application Discovery Service\. The AWS SDKs for Java, \.NET, and PHP wrap the Application Discovery Service API to simplify your programming tasks\. For more information, see [Sample Code Libraries](http://aws.amazon.com/code)\.

## Two Ways to Start Collecting Data<a name="collect-two-ways"></a>

To begin collecting data, use either the **Discovery Connector** or the **Discovery Agent**\. A description follows in each topic that will help you decide which discovery tool to use\. Each of the following topics also guides you through installation and deployment\.

**Topics**
+ [AWS Agentless Discovery Connector](discovery-connector.md)
+ [ AWS Application Discovery Agent](discovery-agent.md)