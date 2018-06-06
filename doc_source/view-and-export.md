# View and Export Discovered Data<a name="view-and-export"></a>

The AWS Discovery Connector and AWS Discovery Agent both provide system performance data based on average and peak utilization\. You can use the system performance data collected to perform a high\-level TCO \(total cost of ownership\)\. Discovery Agents collect more detailed data including time series data for system performance information, inbound and outbound network connections, and processes running on the server\. You can use this data to understand network dependencies between servers and group the related servers as applications for migration planning\. 

In this section you'll find instructions on how to view and work with data discovered by Discovery Connectors and Discovery Agents from both the console and the AWS CLI\.

**Topics**
+ [View Collected Data Using the Console](#view-data)
+ [Export Collected Data](#export-data)
+ [Explore Agent Collected Data in Athena](#explore-data)

## View Collected Data Using the Console<a name="view-data"></a>

After starting the data collection process of your Discovery Connector or Discovery Agent, you can use the console to view their collected data about your servers and VMs\. Data appears in the console approximately 15 minutes after turning on data collection\. This data can also be viewed in a csv  format by exporting the collected data by making API calls through the AWS CLI\. Exporting collected data is covered in the next section [Export Collected Data](#export-data)\. 

**To view collected data about discovered servers**

1. In the console's navigation pane, choose **Servers**\. The discovered servers appear in the servers list\.

1. For details comprised of the collected data, choose the server name link in the **Server info** column\. Doing so displays a screen that describes detail information such as system information, performance metrics, and more\.

To learn more about using the console to view, sort, and tag servers discovered by your Discovery Connectors or Discovery Agents, see [ AWS Application Discovery Service Console Walkthroughs](console-walkthrough.md)\.

## Export Collected Data<a name="export-data"></a>

After starting the data collection process of your Discovery Connector or Discovery Agent, you can export thier collected data  about your servers and VMs\. This data can be exported either by interacting with the console or by making API calls through the AWS CLI depending on which discovery tool you used to collect data\.
+ **Discovery Agent**, you can export the collected data either from the console or from the AWS CLI\.
+ **Discovery Connector**, you can only export the collected data from the AWS CLI\.

Instructions are provided below for both ways by expanding your method of choice:

### Export System Performance Data for All Servers<a name="export-data-api"></a>

Collected data from all the Discovery Connectors and Discovery Agents running on your hosts and VMs can be bulk exported from the AWS CLI\. If not already installed, the AWS CLI must first be installed in your environment\.

**To install the AWS CLI and export collected data**

1. If you have not already done so, install the AWS CLI appropriate to your OS type \(Windows or Mac/Linux\)\. See the [AWS Command Line Interface User Guide](http://docs.aws.amazon.com/cli/latest/userguide/) for instructions\.

1. Open the Command prompt \(Windows\) or Terminal \(MAC/Linux\)\.

   1. Type `aws configure` and press Enter\.

   1. Enter your AWS Access Key Id and AWS Secret Access Key\.

   1. Enter `us-west-2` for the Default Region Name\.

   1. Enter `text` for Default Output Format\.

1. Type the following command to generate an export ID:

   ```
   aws discovery start-export-task
   ```

1. Using the export ID generated in the previous step, type the following command to generate an S3 URL as a value for the parameter `"configurationsDownloadUrl"`:

   ```
   aws discovery describe-export-tasks --export-ids <export ID>
   ```

1. Copy the URL generated in the previous step and paste it in a browser to download the zip file with collected data of the discovered servers\.

### Export Agent Collected Data Using the Console<a name="export-data-console"></a>

Exporting agent collected data from the console is limited to one agent when you are on the detail page for a specific server\. There, you can find the server's export jobs listed at the bottom of the screen, underneath **Exports**\. If no export jobs yet exist, the table is empty\. You can execute up to five exports of server data at a time\.

**To export collected data about a discovered server**

1. In the navigation pane, choose **Servers**\.

1. In the **Server info** column, choose the link for the server that you want to export data for\. 

1. In the **Exports** section at the bottom of the screen, choose **Export server details**\.

1. For **Export server details**, fill in **Start date** and **Time**\.   
**Note**  
The start time can't be more than 72 hours prior from the current time\.

1. Choose **Export** to start the job\. The initial status is **In\-progress**; to update the status, click the refresh icon for the **Exports** section\.

1. When the export job is complete, choose **Download** and save the \.zip file\.

1. Unzip the saved file\. A set of \.csv files contains the export data, similar to the following:
   + *<AWS account ID>*\_destinationProcessConnection\.csv
   + *<AWS account ID>*\_networkInterface\.csv
   + *<AWS account ID>*\_osInfo\.csv
   + *<AWS account ID>*\_process\.csv
   + *<AWS account ID>*\_sourceProcessConnection\.csv
   + *<AWS account ID>*\_systemPerformance\.csv

   You can open the \.csv files in Microsoft Excel and review the exported server data\. 

   Among the files, you can find a JSON file containing data about the export task and its results\.

## Explore Agent Collected Data in Athena<a name="explore-data"></a>

All the data collected by agents can also be exported and uploaded to your S3 bucket using the [AWS Discovery Utilities](https://github.com/awslabs/aws-discovery-utils) scripts\. These scripts enable bulk export of system performance, network connection, and process data for all the agent discovered servers, and transform it for use in [Amazon Athena](https://aws.amazon.com/athena) using an S3 bucket for storage\. The AWS Discovery Utilities package includes the following scripts:
+ **export\.py** \- performs a bulk export of data from all servers discovered by agents in CSV format\.
+ **convert\_csv\.py** \- converts the CSV files to Parquet format and uploads them to your S3 bucket\.
+ **discovery\_athena\.ddl** \- once the Parquet files are in S3, you modify the statements in discovery\_athena\.ddl to reference your S3 bucket\. Then, run this script from the Athena console within a new or existing database\.

After you have downloaded, modified, and run all of the above scripts, you can query the data in the Athena console using SQL commands\. Instructions are provided below on how to do this along with sample SQL queries\.

**Note**  
The **export\.py** script will take longer to run if you have a large amount of discovered data\.
Before using Athena, please reference [Amazon Athena Pricing](https://aws.amazon.com/athena/pricing/)\. 

**To explore collected data in Athena**

1. Download the AWS Discovery Utilities scripts from [here\.](https://github.com/awslabs/aws-discovery-utils)

1. Before running the scripts downloaded in the previous step, be sure to read the **README\.md** file to ensure you understand the set up and have the correct environment configured, such as having *python* and *boto3* installed\.

1. Run all three of the AWS Discovery Utilities scripts in the order specified and as instructed in the **README\.md** file\.
**Note**  
Running the *convert\_csv\.py* script involves passing parameters, and you must modify the statements in *discovery\_athena\.ddl* to reference the correct S3 bucket before running it in the Athena console\. All of this detail is provided in **README\.md**\.

1. After you have succesfully run all of the scripts from the previous step, you are now ready to query the data in the Athena console using SQL commands\.
**Note**  
If you are not familiar with Amazon Athena, read [Get Started with Amazon Athena](https://aws.amazon.com/athena) before proceeding\.

   To analyze details about your on\-premises servers, you can write your own SQL queries in Athena\. Following are some example queries for reference\. 

   1. To obtain the network communication between servers based on a given port number, run the following query in the Athena Console:

      ```
      WITH valid_ips AS
          (SELECT DISTINCT source_ip
          FROM source_process_connection ), outer_query AS
          (SELECT agent_id,
               source_ip,
               destination_ip,
               destination_port,
               count(*) AS frequency
          FROM source_process_connection
          WHERE ip_version = 'IPv4'
                  AND destination_ip IN
              (SELECT *
              FROM valid_ips)
              GROUP BY  agent_id, source_ip, destination_ip, destination_port )
          SELECT source_ip AS Source,
               'Port ' || cast(destination_port AS varchar(20)) AS Edge, destination_ip AS Target, Frequency
      FROM outer_query;
      ```

   1. To obtain the system performance data for cost analysis, run the following query in the Athena Console:

      ```
      SELECT DISTINCT SP.AGENT_ID,
               OS.OS_NAME,
               OS.OS_VERSION,
               MAX(SP.total_num_cores) AS Cores,
               MAX(SP.total_num_cpus) AS CPU,
               MAX(SP.total_disk_size_in_gb) AS StorageTotal,
               MAX(SP.total_disk_free_size_in_gb) AS StorageFree,
               MAX(SP.total_ram_in_mb) AS RAM,
               MAX(SP.total_disk_read_ops_per_sec) AS IOPS_Read,
               MAX(SP.total_disk_bytes_written_per_sec_in_kbps) AS IOPS_Write
      FROM SYSTEM_PERFORMANCE AS SP, OS_INFO AS OS
      WHERE SP.AGENT_ID = OS.AGENT_ID
      GROUP BY  SP.AGENT_ID, OS.OS_NAME, OS.OS_VERSION;
      ```

**Removing your data from AWS Application Discovery Service**  
If you’d like to have all your data removed from Application Discovery Service, please contact [AWS Support](https://aws.amazon.com/contact-us/) and request full data deletion from Application Discovery Service\.