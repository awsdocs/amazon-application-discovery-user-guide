# Troubleshooting AWS Application Discovery Service<a name="troubleshooting"></a>

In this section, you can find information about how to fix common issues with AWS Application Discovery Service\.

**Topics**
+ [Stop data collection by data exploration](#stop-data-collection)
+ [Remove the data collected by data exploration](#remove-collected-data)
+ [Fix common issues with data exploration in Amazon Athena](#troubleshoot-data-exploration)
+ [Troubleshooting failed import records](#troubleshooting-import-failed-records)

## Stop data collection by data exploration<a name="stop-data-collection"></a>

To stop data exploration, you can either switch off the toggle switch in the Migration Hub console under Discover > Data Collectors > Agents tab, or invoke the `StopContinuousExport` API\. It can take up to 30 minutes to stop the data collection, and during this stage, the toggle switch on the console and the `DescribeContinuousExport` API invocation will show the data exploration state as "Stop In Progress"\.

**Note**  
If after refreshing the console page, the toggle does not switch off and an error message is thrown or the `DescribeContinuousExport` API returns "Stop\_Failed" state, you can try again by switching the toggle switch off or calling the `StopContinuousExport` API\. If the "data exploration" still shows error and fails to successfully stop, please reach out to AWS support\.

Alternatively, you can manually stop data collection as described in the following steps\.

**Option 1: Stop Agent Data collection**

If you have already completed your discovery using ADS agents and no longer want to collect additional data in the ADS database repository:

1. From the Migration Hub console choose Discover > Data Collectors > Agents tab\.

1. Select all existing running agents and choose **Stop Data Collection**\.

   This will ensure that no new data is being collected by the agents in both the ADS data repository and your S3 bucket\. Your existing data remains accessible\.

**Option 2: Delete data exploration's Amazon Kinesis Data Streams**

If you want to continue collecting data by agents in ADS data repository, but don't want to collect data in your Amazon S3 bucket using data exploration, you can manually delete the Amazon Kinesis Data Firehose streams created by data exploration:

1. Log in to Amazon Kinesis from the AWS console and choose **Data Firehose** from the navigation pane\.

1. Delete the following streams created by the data exploration feature:
   + `aws-application-discovery-service-id_mapping_agent`
   + `aws-application-discovery-service-inbound_connection_agent`
   + `aws-application-discovery-service-network_interface_agent`
   + `aws-application-discovery-service-os_info_agent`
   + `aws-application-discovery-service-outbound_connection_agent`
   + `aws-application-discovery-service-processes_agent`
   + `aws-application-discovery-service-sys_performance_agent`

## Remove the data collected by data exploration<a name="remove-collected-data"></a>

**To remove data that's collected by data exploration**

1. Remove the discovery agent data stored in Amazon S3\.

   Data that's collected by AWS Application Discovery Service \(ADS\) is stored in an S3 bucket named `aws-application-discover-discovery-service-uniqueid`\. 
**Note**  
Deleting the Amazon S3 bucket or any of the objects in it while data exploration in Amazon Athena is enabled causes an error\. It continues to send new discovery agent data to S3\. The deleted data will no longer be accessible in Athena as well\.

1. Remove AWS Glue Data Catalog\.

   When data exploration in Amazon Athena is turned on, it creates an Amazon S3 bucket in your account to store the data that's collected by ADS agents at regular time intervals\. In addition, it also creates an AWS Glue Data Catalog to allow you to query the data stored in a Amazon S3 bucket from Amazon Athena\. When you turn off data exploration in Amazon Athena, no new data is stored in your Amazon S3 bucket, but data that was collected previously will persist\. If you no longer need this data and want to return your account to the state before data exploration in Amazon Athena was turned on\.

   1. Visit Amazon S3 from the AWS console and manually delete the bucket with the name "aws\-application\-discover\-discovery\-service\-uniqueid"

   1. You can manually remove the data exploration AWS Glue Data Catalog by deleting the *application\-discovery\-service\-database* database and all of these tables:
      + `os_info_agent`
      + `network_interface_agent`
      + `sys_performance_agent`
      + `processes_agent`
      + `inbound_connection_agent`
      + `outbound_connection_agent`
      + `id_mapping_agent`

**Removing your data from AWS Application Discovery Service**  
To have all your data removed from Application Discovery Service, contact [AWS Support](https://aws.amazon.com/contact-us/) and request full data deletion\.

## Fix common issues with data exploration in Amazon Athena<a name="troubleshoot-data-exploration"></a>

In this section, you can find information about how to fix common issues with data exploration in Amazon Athena\. 

**Topics**
+ [Data exploration in Amazon Athena fails to initiate because service\-linked roles and required AWS resources can't be created](#slr-failed-initialize)
+ [New Agent data doesn't show up in Amazon Athena](#new-agent-data-not-showing)
+ [You have insufficient permissions to access Amazon S3, Amazon Kinesis Data Firehose, or AWS Glue](#insufficient-permissions)

### Data exploration in Amazon Athena fails to initiate because service\-linked roles and required AWS resources can't be created<a name="slr-failed-initialize"></a>

 When you turn on data exploration in Amazon Athena, it creates the service\-linked role, `AWSServiceRoleForApplicationDiscoveryServiceContinuousExport`, in your account that allows it to create the required AWS resources for making the agent collected data accessible in Amazon Athena including an Amazon S3 bucket, Amazon Kinesis streams, and AWS Glue Data Catalog\. If your account does not have the right permissions for data exploration in Amazon Athena to create this role, it will fail to initialize\. Refer to [AWS managed policies for AWS Application Discovery Service](security-iam-awsmanpol.md)\. 

### New Agent data doesn't show up in Amazon Athena<a name="new-agent-data-not-showing"></a>

If new data does not flow into Athena, it has been more than 30 minutes since an agent started, and data exploration status is Active, check the solutions listed below:
+ AWS Discovery Agents

  Ensure that your agent's **Collection** status is marked as **Started** and the **Health** status is marked as **Running**\.
+ Kinesis Role

  Ensure that you have the `AWSApplicationDiscoveryServiceFirehose` role in your account\.
+ Kinesis Data Firehose Status

  Ensure that the following Kinesis Data Firehose delivery streams are working correctly:
  + `aws-application-discovery-service/os_info_agent`
  + `aws-application-discovery-service-network_interface_agent`
  + `aws-application-discovery-service-sys_performance_agent`
  + `aws-application-discovery-service-processes_agent`
  + `aws-application-discovery-service-inbound_connection_agent`
  + `aws-application-discovery-service-outbound_connection_agent`
  + `aws-application-discovery-service-id_mapping_agent`
+ AWS Glue Data Catalog

  Ensure that the `application-discovery-service-database` database is in AWS Glue\. Make sure that the following tables are present in AWS Glue:
  + `os_info_agent`
  + `network_interface_agent`
  + `sys_performance_agent`
  + `processes_agent`
  + `inbound_connection_agent`
  + `outbound_connection_agent`
  + `id_mapping_agent`
+ Amazon S3 Bucket

  Ensure that you have an Amazon S3 bucket named `aws-application-discovery-service-uniqueid` in your account\. If objects in the bucket have been moved or deleted, they will not show up properly in Athena\.
+ Your on\-premises servers

  Ensure that your servers are running so that your agents can collect and send data to AWS Application Discovery Service\.

### You have insufficient permissions to access Amazon S3, Amazon Kinesis Data Firehose, or AWS Glue<a name="insufficient-permissions"></a>

If you are using AWS Organizations, and initialization for data exploration in Amazon Athena fails, it can be because you don’t have permissions to access Amazon S3, Amazon Kinesis Data Firehose, Athena or AWS Glue\.

You will need an IAM user with administrator permissions to grant you access to these services\. An administrator can use their account to grant this access\. See [AWS managed policies for AWS Application Discovery Service](security-iam-awsmanpol.md)\.

To ensure that data exploration in Amazon Athena works correctly, do not modify or delete the AWS resources created by data exploration in Amazon Athena including the Amazon S3 bucket, Amazon Kinesis Data Firehose Streams, and AWS Glue Data Catalog\. If you accidentally delete or modify these resources, please stop and start Data Exploration and it will automatically create these resources again\. If you delete the Amazon S3 bucket created by data exploration, you may lose the data that was collected in the bucket\.

## Troubleshooting failed import records<a name="troubleshooting-import-failed-records"></a>

Migration Hub import allows you to import details of your on\-premises environment directly into Migration Hub without using the Discovery Connector or Discovery Agent\. This gives you the option to perform migration assessment and planning directly from your imported data\. You can also group your devices as applications and track their migration status\.

When importing data, it's possible that you'll encounter errors\. Typically, these errors occur for one of the following reasons:
+ **An import\-related quota was reached** – There is a quota associated with import tasks\. If you make an import task request that would exceeds the quotas, then the request will fail and return an error\. For more information, see [AWS Application Discovery Service Quotas](ads_service_limits.md)\.
+ **An extra comma \(,\) was inserted into the import file** – Commas in \.CSV files are used to differentiate one field from the next\. Having a comma appear within a field is unsupported, because it will always split a field\. This can cause a cascade of formatting errors\. Be sure that commas are only used between fields, and are not otherwise used in your import files\.
+ **A field has a value outside of its supported range** – Some fields, like `CPU.NumberOfCores` must have a range of values they support\. If you have more or less than this supported range, then the record will fail to be imported\.

If any errors occur with your import request, you can resolve them by downloading your failed records for your import task, and resolve the errors in the failed entries CSV file, and do the import again\.

------
#### [ Console ]

**To download your failed records archive**

1. Sign into the AWS Management Console, and open the Migration Hub console at [https://console.aws.amazon.com/migrationhub](https://console.aws.amazon.com/migrationhub)\.

1. From the left\-side navigation, under **Discover**, choose **Tools**\.

1. From **Discovery Tools**, choose **view imports**\.

1. From the **Imports** dashboard, choose the radio button associated an import request with some number of **Failed records**\.

1. Choose **Download failed records** from above the table on the dashboard\. This will open your browser's download dialog box for downloading the archive file\.

------
#### [ AWS CLI ]

**To download your failed records archive**

1. Open a terminal window, and type the following command, where `ImportName is the name of the import task with the failed entries that you want to correct.`:

   ```
   aws discovery describe-import-tasks - -name ImportName
   ```

1. From the output, copy the entire contents of the value returned for `errorsAndFailedEntriesZip`, without the surrounding quotes\.

1. Open a web browser, and paste in the contents into the URL text box and press `ENTER`\. This will download the failed records archive, compressed in a \.zip format\.

------

Now that you've downloaded your failed records archive, you can extract the two files within and correct the errors\. Note that if your errors are tied to service\-based limits, you'll either need to request a limit increase, or delete enough of the associated resources to get your account under the limit\. The archive has the following files:
+ **errors\-file\.csv** – This file is your error log, and it tracks the line, column name, `ExternalId`, and a descriptive error message for each failed record of each failed entry\.
+ **failed\-entries\-file\.csv** – This file contains only the failed entries from your original import file\.

To correct the non\-limit\-based errors you've encountered, use the `errors-file.csv` to correct the issues in the `failed-entries-file.csv` file, and then import that file\. For instructions on importing files, see [Importing Data](discovery-import.md#start-data-import)\.