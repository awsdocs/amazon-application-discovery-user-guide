# View, Export, and Explore Server Data<a name="discovered_servers"></a>

The **Servers** page provides system configuration and performance data about each server instance known to the data collection tools\. You can view server information, sort servers with filters, tag servers with key\-value pairs, and export detailed server and system information\. 

**Topics**
+ [Viewing and Sorting Servers](#sort-view-servers)
+ [Tagging Servers](#tag-servers)
+ [Exporting Server Data](#export-server-data)
+ [Data Exploration in Athena](#explore-data-console)

## Viewing and Sorting Servers<a name="sort-view-servers"></a>

You can view information about the servers discovered by the data collection tools, and you can sort through the servers using filters\.

### Viewing Servers<a name="view-servers"></a>

You can get a general view and a detailed view of the servers discovered by the data collection tools\.  

**To view discovered servers**

1. In the navigation pane, choose **Servers**\. The discovered servers appear in the servers list\. 

1. For more detail about a server, choose its server link in the **Server info** column\. Doing so displays a screen that describes the server\.

The server's detail screen displays system information and performance metrics\. You can also find a button to export network dependencies and processes information\. To export detailed server information, see [Exporting Server Data](#export-server-data)\.

### Sorting Servers with Search Filters<a name="sort-servers"></a>

To easily find specific servers, apply search filters to sort through all the servers discovered by the collection tools\. You can search and filter on numerous criteria\.

**To sort servers by applying search filters**

1. In the navigation pane, choose **Servers**\.

1. Click inside the search bar, and choose a search criterion from the list\.

1. Choose an operator from the next list\.

1. Type in a case\-sensitive value for the search criterion you selected, and press Enter\.

1. Multiple filters can be applied by repeating steps 2 \- 4\.

## Tagging Servers<a name="tag-servers"></a>

To assist migration planning and help stay organized, you can create multiple tags for each server\. *Tags *are user\-defined key\-value pairs that can store any custom data or metadata about servers\. You can tag an individual server or multiple servers in a single operation\. Application Discovery Service tags are similar to AWS tags, but the two types of tag cannot be used interchangeably\. 

You can add or remove multiple tags for one or more servers from the main **Servers** page\. On a server's detail page, you can add or remove one or more tags for the selected server\. You can do any type of tagging task involving multiple servers or tags in a single operation\.  You can also remove tags\.<a name="add-tags"></a>

**To add tags to one or more servers**

1. In the navigation pane, choose **Servers**\.

1. In the **Server info** column, choose the server link for the server that you want to add tags for\. To add tags to more than one server at a time, click inside the check boxes of multiple servers\.

1. Choose **Add tag**\.

1. In the dialog box, type a value in the **Key** field, and optionally a value in the **Value** field\.

   Add more tags by choosing **Additional tag ** and adding more information\.

1. Choose **Add Tags**\. A green confirmation message will be displayed at the top of the screen\.

1. Optionally, tags can be added for an individual server from its detail page by choosing **Actions**, and then **Add tag** and repeating the above steps\.<a name="remove-tags"></a>

**To remove tags from one or more servers**

1. In the navigation pane, choose **Servers**\.

1. In the **Server info** column, choose the server link for the server that you want to remove tags from\. Click inside the check boxes of multiple servers to remove tags from more than one server at a time\.

1. For **Actions**, choose **Remove tag**\.

1. Select each tag you want to remove, or choose **select all**\.

1. Choose **Remove**\. A green confirmation message appears at the top of the screen\.

1. Optionally, tags can be removed for an individual server from its detail page by choosing **Actions**, and then **Remove tag** and repeating the above steps\.

## Exporting Server Data<a name="export-server-data"></a>

To export network dependencies and process information for one server at a time, you can use a server's detail screen\. You can find the export jobs for a server in a table located in the **Exports** section of the server's detail screen\. If no export jobs yet exist, the table is empty\. You can simultaneously export up to five collections of data\.

**Note**  
Exporting server data from the console is only available for data collected by an agent running on that server\. If you want to download data collected by a connector, see [Export System Performance Data for All Servers](export-data.md#export-data-api)\. Or, if you want to bulk export data for all servers where agents have been installed, see [Data Exploration in Amazon Athena](explore-data.md)\.<a name="export"></a>

**To export detailed server data**

1. In the navigation pane, choose **Servers**\.

1. In the **Server info** column, choose the ID of the server for which you want to export data\. 

1. In the **Exports** section at the bottom of the screen, choose **Export server details**\.

1. For **Export server details**, fill in **Start date** and **Time**\.   
**Note**  
The start time can't be more than 72 hours before the current time\.

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

## Data Exploration in Athena<a name="explore-data-console"></a>

Data Exploration in Amazon Athena is enabled by continuous export implicitly being turned on when you confirm options in its dialog box presented from the Data Collectors page after you choose “Start data collection”, or click the slider labeled, “Data exploration in Athena”\. 

After you've started Continuous Export and you’re ready to begin exploring data discovered by all your agents, you choose the “Explore data in Athena” link on the Servers page to go directly to Amazon Athena\.

**Topics**
+ [Prerequisite for Data Exploration in Amazon Athena](#ce-prep-agents-console)
+ [Working with Discovered Data in Amazon Athena](#working-with-data-athena)

### Prerequisite for Data Exploration in Amazon Athena<a name="ce-prep-agents-console"></a>

Before you can actually start exploring your discovered data in Athena, you first have to put your discovery agents in "continuous export" mode as a prerequisite by starting Continuous Export\. There are two ways to do this, through the console or by making API calls through the AWS CLI\. Instructions are provided below for both ways by expanding your method of choice:

Continuous Export is turned on when you choose "Start data collection", or click the slider labeled, "Data exploration in Athena" on the **Data Collectors** page of the Migration Hub console\.

**To start Continuous Export from your agents**

1. In the navigation pane, choose **Data Collectors**\.

1. Choose the **Agents** tab\.

1. Choose **Start data collection**, or if you already have data collection turned on, click the **Data exploration in Athena** slider\.

1. In the dialog box generated from the previous step, click the checkbox agreeing to associated costs and choose **Continue** or **Enable**\.

### Working with Discovered Data in Amazon Athena<a name="working-with-data-athena"></a>

 Once you have enabled Data Exploration in Amazon Athena, you can begin exploring and working with current, detailed data discovered by your agents in Amazon Athena\. You can query this data directly in Athena to do such things as generate spreadsheets, run a cost analysis, port the query to a visualization program to diagram network dependencies, and more\.

In this section the following topics will be covered providing instructions on the various ways you can work with your data in Amazon Athena to assess and plan for migrating your local environment to AWS:

**Topics**
+ [Explore Data Directly in Amazon Athena](#explore-direct-in-ate)
+ [Predefinied Queries to use in Athena](#predefined-queries)
+ [Visualize Amazon Athena Data](#port-query-to-visualization)

#### Explore Data Directly in Amazon Athena<a name="explore-direct-in-ate"></a>

These instructions will guide you to all of your agent data directly in the Athena console\. If you don’t have any data in Athena or have not enabled Data Exploration in Amazon Athena, you will be prompted by a dialog box to enable Data Exploration in Amazon Athena as explained [here](explore-data.md#ce-prep-agents)\.

**To explore agent discovered data directly in Athena**

1. In the navigation pane, choose **Servers**\.

1. Choose the **Explore data in Amazon Athena** link\.

   You will be taken to the Amazon Athena console where you will see:
   + The Query Editor window
   + In the navigation pane:
     + Database listbox which will have the default database pre\-listed as *application\_discovery\_service\_database*
     + Tables list consisting of seven tables representing the data sets grouped by the agents:
       + os\_info\_agent
       + network\_interface\_agent
       + sys\_performance\_agent
       + processes\_agent
       + inbound\_connection\_agent
       + outbound\_connection\_agent
       + id\_mapping\_agent

1. You are now ready to query the data in the Amazon Athena console by writing and running your own SQL queries in the Athena Query Editor to analyze details about your on\-premises servers\.

#### Predefinied Queries to use in Athena<a name="predefined-queries"></a>

Here you will find a set of predifined queries of typical use cases, such as TCO analysis and network visualization\. You can use these queries as is or modify them to suit your needs\. Simply expand the query you want to use and follow these instructions:

**To use a predefined query**

1. In the navigation pane, choose **Servers**\.

1. Choose the **Explore data in Amazon Athena** link to be taken to your data in the Athena console\.

1. Expand one of the predefined queries listed below and copy it\.

1. Place your cursor in Athena's Query Editor window and paste the query\.

1. Choose **Run Query**\.

##### Network Communication Between Servers Based On Port Number<a name="pq-net-com-srv"></a>

To obtain the network communication between servers based on a given port number, run the following query in the Amazon Athena Console:

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

##### Cost Analysis Based On System Performance<a name="pq-cost-anly-perfmnc"></a>

To obtain the system performance data for cost analysis, run the following query in the Amazon Athena Console:

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

 

#### Visualize Amazon Athena Data<a name="port-query-to-visualization"></a>

To visualize your data, a query can be ported to a visualization program such as Amazon QuickSight or other open\-source visualization tools such as Cytoscape, yEd, or Gelphi to render network diagrams, summary charts, and other graphical representations\. When this method is used, you connect to Athena through the visualization program so that it can access your collected data as a source to produce the visualization\.

**To visualize your Amazon Athena data using Amazon QuickSight**

1. Sign\-in to [Amazon QuickSight](https://aws.amazon.com/quicksight/)\.

1. Choose **Connect to another data source or upload a file**\.

1. Choose **Athena** which will produce the **New Athena data source** dialog box\.

1. Enter a name in the **Data source name** field\.

1. Choose **Create data source**\.

1. Select the **Agents\-servers\-os** table in the **Choose your table** dialog box and choose **Select**\.

1. In the **Finish data set creation** dialog box, select **Import to SPICE for quicker analytics** and choose **Visualize**\.

   Your visualization will be rendered\.