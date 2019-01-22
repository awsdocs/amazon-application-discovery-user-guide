# Data Exploration in Amazon Athena<a name="explore-data"></a>

Data Exploration in Amazon Athena allows you to analyze the data collected from all the discovered on\-premises servers by Discovery Agents at one single place\. Once Data Exploration in Amazon Athena is enabled from the Migration Hub console \(or by using the StartContinousExport API\) and the data collection for agents is turned on, data collected by agents will automatically get stored in your S3 bucket at regular intervals\.

You can then visit Amazon Athena to run pre\-defined queries to analyze the time\-series system performance for each server, the type of processes that are running on each server and the network dependencies between different servers\. In addition, you can write your own custom queries using Amazon Athena, upload additional existing data sources such as configuration management database \(CMDB\) exports, and associate the discovered servers with the actual business applications\. You can also integrate the Athena database with Amazon QuickSight to visualize the query outputs and perform additional analysis

**Topics**
+ [Enabling Data Exploration in Amazon Athena](#ce-prep-agents)
+ [Working with Discovered Data in Amazon Athena](#working-with-data-athena)

## Enabling Data Exploration in Amazon Athena<a name="ce-prep-agents"></a>

Before you can actually see and start exploring your discovered data in Amazon Athena, Data Exploration in Amazon Athena must first be enabled by Continuous Export implicitly being turned on when you choose "Start data collection", or click the toggle labeled, "Data exploration in Amazon Athena" on the **Data Collectors** page of the Migration Hub console\. Data Exploration in Amazon Athena can also be enabled by Continuous Export explicitly being turned on through an API call from the AWS CLI\.  Instructions are provided below for both ways by expanding your method of choice:

### Enable Data Exploration in Amazon Athena Using the Migration Hub Console<a name="start-ce-console"></a>

Data Exploration in Amazon Athena is enabled by Continuous Export implicitly being turned on when you choose "Start data collection", or click the toggle labeled, "Data exploration in Amazon Athena" on the **Data Collectors** page of the Migration Hub console\.

**To enable Data Exploration in Amazon Athena from the console**

1. In the navigation pane, choose **Data Collectors**\.

1. Choose the **Agents** tab\.

1. Choose **Start data collection**, or if you already have data collection turned on, click the **Data exploration in Amazon Athena** toggle\.

1. In the dialog box generated from the previous step, click the checkbox agreeing to associated costs and choose **Continue** or **Enable**\.

**Note**  
Your agents are now running in "continuous export" mode which will enable you to see and work with your discovered data in Amazon Athena\. The first time this is enable it may take up to 30 minutes for your data to appear in Amazon Athena\.

### Enable Data Exploration in Amazon Athena Using the AWS CLI<a name="start-ce-api"></a>

Data Exploration in Amazon Athena is enabled by Continuous Export explicitly being turned on through an API call from the AWS CLI\. To do this, the AWS CLI must first be installed in your environment\.

**To install the AWS CLI and enable Data Exploration in Amazon Athena**

1. Install the AWS CLI for your operating system \(Linux, macOS, or Windows\)\. See the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/) for instructions\.

1. Open the Command prompt \(Windows\) or Terminal \(Linux or macOS\)\.

   1. Type `aws configure` and press Enter\.

   1. Enter your AWS Access Key Id and AWS Secret Access Key\.

   1. Enter `us-west-2` for the Default Region Name\.

   1. Enter `text` for Default Output Format\.

1. Type the following command:

   ```
   aws discovery start-continuous-export
   ```

**Note**  
Your agents are now running in "continuous export" mode which will enable you to see and work with your discovered data in Amazon Athena\. The first time this is enable it may take up to 30 minutes for your data to appear in Amazon Athena\.

## Working with Discovered Data in Amazon Athena<a name="working-with-data-athena"></a>

After you enable Data Exploration in Amazon Athena, you can begin exploring and working with current, detailed data that was discovered by your agents in Amazon Athena\. You can query this data directly in Athena\. With the data, you can generate spreadsheets, run a cost analysis, port the query to a visualization program to diagram network dependencies, and more\.

The topics in this section provide instructions about the various ways you can work with your data in Amazon Athena to assess and plan for migrating your local environment to AWS\.

**Topics**
+ [Explore Data Directly in Amazon Athena](#explore-direct-in-ate)
+ [Predefined Queries to use in Athena](#predefined-queries)
+ [Visualize Amazon Athena Data](#port-query-to-visualization)

### Explore Data Directly in Amazon Athena<a name="explore-direct-in-ate"></a>

These instructions guide you to all your agent data directly in the Athena console\. If you don’t have any data in Athena or have not enabled Data Exploration in Amazon Athena, you will be prompted by a dialog box to enable Data Exploration in Amazon Athena as explained [here](#ce-prep-agents)\.

**To explore agent discovered data directly in Athena**

1. In the navigation pane, choose **Servers**\.

1. Choose the **Explore data in Amazon Athena** link\.

   You will be taken to the Amazon Athena console where you will see:
   + The **Query Editor** window
   + In the navigation pane:
     + Database list box, which will have the default database pre\-listed as *application\_discovery\_service\_database*
     + Tables list consisting of seven tables representing the data sets grouped by the agents\.
       + **os\_info\_agent**
       + **network\_interface\_agent**
       + **sys\_performance\_agent**
       + **processes\_agent**
       + **inbound\_connection\_agent**
       + **outbound\_connection\_agent**
       + **id\_mapping\_agent**

1. Query the data in the Amazon Athena console by writing and running your own SQL queries in the Athena Query Editor\. Analyze details about your on\-premises servers\.

### Predefined Queries to use in Athena<a name="predefined-queries"></a>

This section has predefined queries of typical use cases, such as TCO analysis and network visualization\. Use these queries as is or modify them to suit your needs\. Simply expand the query you want to use and follow these instructions\.

**To use a predefined query**

1. In the navigation pane, choose **Servers**\.

1. Choose the **Explore data in Amazon Athena** link to be taken to your data in the Athena console\.

1. Expand one of the predefined queries listed below and copy it\.

1. Place your cursor in Athena's Query Editor window and paste the query\.

1. Choose **Run Query**\.

#### Network Communication Between Servers Based On Port Number<a name="pq-net-com-srv"></a>

To find the network communication between servers based on a given port number, run the following query in the Amazon Athena console\.

```
WITH valid_ips AS
(SELECT DISTINCT source_ip
FROM outbound_connection_agent ), outer_query AS
(SELECT agent_id,
source_ip,
destination_ip,
destination_port,
count(*) AS frequency
FROM outbound_connection_agent
WHERE ip_version = 'IPv4'
AND destination_ip IN
(SELECT *
FROM valid_ips)
GROUP BY agent_id, source_ip, destination_ip, destination_port )
SELECT source_ip AS Source,
'Port ' || cast(destination_port AS varchar(20)) AS Edge, destination_ip AS Target, Frequency
FROM outer_query;
```

#### Cost Analysis Based On System Performance<a name="pq-cost-anly-perfmnc"></a>

To find the system performance data for cost analysis, run the following query in the Amazon Athena console\.

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
FROM sys_performance_agent AS SP, OS_INFO_AGENT AS OS
WHERE SP.AGENT_ID = OS.AGENT_ID
GROUP BY SP.AGENT_ID, OS.OS_NAME, OS.OS_VERSION;
```

 

### Visualize Amazon Athena Data<a name="port-query-to-visualization"></a>

To visualize your data, a query can be ported to a visualization program such as Amazon QuickSight or other open\-source visualization tools such as Cytoscape, yEd, or Gelphi\. Use these tools to render network diagrams, summary charts, and other graphical representations\. When this method is used, you connect to Athena through the visualization program so that it can access your collected data as a source to produce the visualization\.

**To visualize your Amazon Athena data using Amazon QuickSight**

1. Sign\-in to [Amazon QuickSight](https://aws.amazon.com/quicksight/)\.

1. Choose **Connect to another data source or upload a file**\.

1. Choose **Athena** which will produce the **New Athena data source** dialog box\.

1. Enter a name in the **Data source name** field\.

1. Choose **Create data source**\.

1. Select the **Agents\-servers\-os** table in the **Choose your table** dialog box and choose **Select**\.

1. In the **Finish data set creation** dialog box, select **Import to SPICE for quicker analytics** and choose **Visualize**\.

   Your visualization will be rendered\.

**Removing your data from AWS Application Discovery Service**  
To have all your data removed from Application Discovery Service, contact [AWS Support](https://aws.amazon.com/contact-us/) and request full data deletion\.