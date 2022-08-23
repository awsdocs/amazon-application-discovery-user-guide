# AWS Application Discovery Service User Guide

-----
*****Copyright &copy; Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What is AWS Application Discovery Service?](what-is-appdiscovery.md)
+ [Setting up Application Discovery Service](setting-up.md)
   + [Sign Up for Amazon Web Services](setting-up-signup.md)
   + [Create IAM Users](setting-up-iam.md)
   + [Sign in to the Migration Hub console and choose a home Region](setting-up-choose-home-region.md)
+ [AWS Application Discovery Agent](discovery-agent.md)
   + [Prerequisites for Discovery Agent](gen-prep-agents.md)
   + [Install Discovery Agent on Linux](install_on_linux.md)
   + [Install on Windows](install_on_windows.md)
   + [Data collected by Discovery Agent](agent-data-collected.md)
   + [Start or stop Discovery Agent data collection](start-agent-data-collection.md)
+ [Application Discovery Service Agentless Collector](agentless-collector.md)
   + [Getting started with Agentless Collector](agentless-collector-gs.md)
      + [Prerequisites for Agentless Collector](agentless-collector-gs-prerequisites.md)
      + [Step 1: Create an IAM user for Agentless Collector](agentless-collector-gs-iam-user.md)
      + [Step 2: Download the Agentless Collector](agentless-collector-gs-download-ova.md)
      + [Step 3: Deploy Agentless Collector](agentless-collector-gs-deploy.md)
      + [Step 4: Access the Agentless Collector console](agentless-collector-gs-access-console.md)
      + [Step 5: Configure Agentless Collector](agentless-collector-gs-configure.md)
      + [Step 6: Set up Agentless Collector data collection module](agentless-collector-gs-data-collection.md)
         + [VMware vCenter Agentless Collector data collection module](agentless-collector-gs-data-collection-vcenter.md)
      + [Step 7: View collected data in the Migration Hub console](agentless-collector-gs-view-collected-data.md)
   + [Data collected by Agentless Collector](agentless-collector-data-collected.md)
      + [Data collected by the Agentless Collector VMware vCenter data collection module](agentless-collector-data-collected-vmware.md)
   + [Using the Agentless Collector console](agentless-collector-using.md)
      + [The Agentless Collector dashboard](agentless-collector-dashboard.md)
      + [Editing Agentless Collector settings](agentless-collector-edit-configure.md)
      + [Editing VMware vCenter credentials](agentless-collector-vcenter-edit.md)
   + [Manually updating Agentless Collector](agentless-collector-update.md)
   + [Troubleshooting Agentless Collector](agentless-collector-troubleshooting.md)
+ [Migration Hub Import](discovery-import.md)
+ [View, export, and explore discovered data](view-and-export.md)
   + [View collected data using the Migration Hub console](view-data.md)
   + [Export collected data](export-data.md)
   + [Data Exploration in Amazon Athena](explore-data.md)
      + [Enabling Data Exploration in Amazon Athena](ce-prep-agents.md)
      + [Working with Data Exploration in Amazon Athena](working-with-data-athena.md)
+ [AWS Application Discovery Service Console Walkthroughs](console-walkthrough.md)
   + [Main Dashboard](dashboard.md)
   + [Data collection tools](data_collection.md)
   + [View, export, and explore server data](discovered_servers.md)
+ [Using the Application Discovery Service API to query discovered configuration items](discovery-api-queries.md)
+ [Security in AWS Application Discovery Service](security.md)
   + [Identity and Access Management for AWS Application Discovery Service](security-iam.md)
      + [How AWS Application Discovery Service Works with IAM](security_iam_service-with-iam.md)
      + [AWS managed policies for AWS Application Discovery Service](security-iam-awsmanpol.md)
      + [AWS Application Discovery Service Identity-Based Policy Examples](security_iam_id-based-policy-examples.md)
      + [Using Service-Linked Roles for Application Discovery Service](using-service-linked-roles.md)
         + [Service-Linked Role Permissions for Application Discovery Service](service-linked-role-permissions.md)
         + [Creating a Service-Linked Role for Application Discovery Service](create-service-linked-role.md)
         + [Deleting a Service-Linked Role for Application Discovery Service](delete-service-linked-role.md)
      + [Troubleshooting AWS Application Discovery Service Identity and Access](security_iam_troubleshoot.md)
   + [Logging and monitoring in AWS Application Discovery Service](logging-monitoring.md)
      + [Logging Application Discovery Service API Calls with AWS CloudTrail](logging-using-cloudtrail.md)
+ [AWS Application Discovery Service Quotas](ads_service_limits.md)
+ [Troubleshooting AWS Application Discovery Service](troubleshooting.md)
+ [Document History for AWS Application Discovery Service](doc-history.md)
+ [AWS glossary](glossary.md)
+ [Appendix](appendix.md)
   + [Appendix: Transition from Discovery Connector to Agentless Collector](discovery-collector-transition.md)
   + [Appendix: AWS Agentless Discovery Connector](discovery-connector.md)
      + [Data Collected by the Discovery Connector](agentless-data-collected.md)
      + [Download the Discovery Connector](setting-up-agentless.md)
      + [Deploy the Discovery Connector](deploy-connector-appliance.md)
      + [Configure the AWS Discovery Connector](configure-connector.md)
      + [Discovery Connector Data Collection](start-connector-data-collection.md)
      + [Troubleshooting the Discovery Connector](agentless-troubleshooting.md)