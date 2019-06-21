# Working with Data Exploration in Amazon Athena<a name="working-with-data-athena"></a>

After you enable Data Exploration in Amazon Athena, you can begin exploring and working with current, detailed data that was discovered by your agents in Amazon Athena\. You can query this data directly in Athena\. With the data, you can generate spreadsheets, run a cost analysis, port the query to a visualization program to diagram network dependencies, and more\.

The topics in this section provide instructions about the various ways you can work with your data in Amazon Athena to assess and plan for migrating your local environment to AWS\.

**Topics**
+ [Explore Data Directly in Amazon Athena](#explore-direct-in-ate)
+ [Visualize Amazon Athena Data](#port-query-to-visualization)
+ [Predefined Queries to use in Athena](#predefined-queries)

## Explore Data Directly in Amazon Athena<a name="explore-direct-in-ate"></a>

These instructions guide you to all your agent data directly in the Athena console\. If you don’t have any data in Athena or have not enabled Data Exploration in Amazon Athena, you will be prompted by a dialog box to enable Data Exploration in Amazon Athena as explained [here](ce-prep-agents.md)\.

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

## Visualize Amazon Athena Data<a name="port-query-to-visualization"></a>

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

## Predefined Queries to use in Athena<a name="predefined-queries"></a>

Here you will find a set of predefined queries of typical use cases, such as TCO analysis and network visualization\. You can use these queries as is or modify them to suit your needs\. Simply expand the query you want to use and follow these instructions:

**To use a predefined query**

1. In the navigation pane, choose **Servers**\.

1. Choose the **Explore data in Amazon Athena** link to be taken to your data in the Amazon Athena console\.

1. Expand one of the predefined queries listed below and copy it\.

1. Paste the query in the Athena's Query Editor window\.

1. Choose **Run Query**\.

### Obtain IP Addresses and Hostnames for Servers<a name="pq-helper-function"></a>

This helper function retrieves IP addresses and hostnames for a given server\. This view can be used in other queries\.

```
CREATE OR REPLACE VIEW hostname_ip_helper AS 
SELECT DISTINCT
  "os"."host_name"
, "nic"."agent_id"
, "nic"."ip_address"
FROM
  os_info_agent os
, network_interface_agent nic
WHERE ("os"."agent_id" = "nic"."agent_id")
```

### Identify Servers With or Without Agents<a name="pq-agents-installed-or-not"></a>

This query can help you perform data validation\. If you've deployed agents on a number of servers in your network, you can use this query to understand if there are other servers in your network without agents deployed on them\. In this query, we look into the inbound and outbound network traffic, and filter the traffic for private IP addresses, only\. That is, IP addresses starting with `192`, `10`, or `172`\.

```
SELECT DISTINCT "destination_ip" "IP Address" ,
         (CASE
    WHEN (
    (SELECT "count"(*)
    FROM network_interface_agent
    WHERE ("ip_address" = "destination_ip") ) = 0) THEN
        'no'
        WHEN (
        (SELECT "count"(*)
        FROM network_interface_agent
        WHERE ("ip_address" = "destination_ip") ) > 0) THEN
            'yes' END) "agent_running"
    FROM outbound_connection_agent
WHERE ((("destination_ip" LIKE '192.%')
        OR ("destination_ip" LIKE '10.%'))
        OR ("destination_ip" LIKE '172.%'))
UNION
SELECT DISTINCT "source_ip" "IP ADDRESS" ,
         (CASE
    WHEN (
    (SELECT "count"(*)
    FROM network_interface_agent
    WHERE ("ip_address" = "source_ip") ) = 0) THEN
        'no'
        WHEN (
        (SELECT "count"(*)
        FROM network_interface_agent
        WHERE ("ip_address" = "source_ip") ) > 0) THEN
            'yes' END) "agent_running"
    FROM inbound_connection_agent
WHERE ((("source_ip" LIKE '192.%')
        OR ("source_ip" LIKE '10.%'))
        OR ("source_ip" LIKE '172.%'))
```

### Analyze System Performance Data for Servers With Agents<a name="pq-agents-server-performance"></a>

You can use this query to analyze system performance and utilization pattern data for your on\-premises servers that have agents installed on them\. The query combines the `system_performance_agent` table with `os_info_agent` table to identify the hostname for each server\. This query returns the time series utilization data \(in 15 minute intervals\) for all the servers where agents are running\.

```
SELECT "OS"."os_name" "OS Name" ,
         "OS"."os_version" "OS Version" ,
         "OS"."host_name" "Host Name" ,
         "SP"."agent_id" ,
         "SP"."total_num_cores" "Number of Cores" ,
         "SP"."total_num_cpus" "Number of CPU" ,
         "SP"."total_cpu_usage_pct" "CPU Percentage" ,
         "SP"."total_disk_size_in_gb" "Total Storage (GB)" ,
         "SP"."total_disk_free_size_in_gb" "Free Storage (GB)" ,
         ("SP"."total_disk_size_in_gb" - "SP"."total_disk_free_size_in_gb") "Used Storage" ,
         "SP"."total_ram_in_mb" "Total RAM (MB)" ,
         ("SP"."total_ram_in_mb" - "SP"."free_ram_in_mb") "Used RAM (MB)" ,
         "SP"."free_ram_in_mb" "Free RAM (MB)" ,
         "SP"."total_disk_read_ops_per_sec" "Disk Read IOPS" ,
         "SP"."total_disk_bytes_written_per_sec_in_kbps" "Disk Write IOPS" ,
         "SP"."total_network_bytes_read_per_sec_in_kbps" "Network Reads (kbps)" ,
         "SP"."total_network_bytes_written_per_sec_in_kbps" "Network Write (kbps)"
FROM "sys_performance_agent" "SP" , "OS_INFO_agent" "OS"
WHERE ("SP"."agent_id" = "OS"."agent_id") limit 10;
```

### Track Outbound Communication Between Servers Based On Port Number and Process Details<a name="pq-analyze-outbound-connections"></a>

Before running this query, perform the following procedures\. Note that if you've already created the `iana_service_ports_import` table, you can skip the first procedure\.

**To create the iana\_service\_ports\_import table**

1. Download the IANA Port database CSV file from [Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) on *iana\.org*\.

1. Upload the file to Amazon S3\. For more information, see [How Do I Upload Files and Folders to an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html)\.

1. Create a new table in Athena named `iana_service_ports_import` and specify the Amazon S3 path to the object you uploaded in the previous step\. For more information, see [Step 2: Create a Table](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html#step-2-create-a-table) in the *Amazon Athena User Guide*\.

Now that the `iana_service_ports_import` table has been created, you create two help functions for tracking outbound traffic\. For information on how to create a view, see [CREATE VIEW](https://docs.aws.amazon.com/athena/latest/ug/create-view.html) in the *Amazon Athena User Guide*\. 

**To create outbound tracking helper functions**

1. Open the Athena console at [https://console\.aws\.amazon\.com/athena/](https://console.aws.amazon.com/athena/home)\.

1. Create the view `valid_outbound_ips_helper`\. This helper function gives the list of all distinct outbound source ip addresses and is as follows:

   ```
   CREATE OR REPLACE VIEW valid_outbound_ips_helper AS 
   SELECT DISTINCT "source_ip"
   FROM
     outbound_connection_agent;
   ```

1. Create the view `outbound_query_helper`\. This helper function determines the frequency of communication for outbound traffic and is as follows:

   ```
   CREATE OR REPLACE VIEW outbound_query_helper AS 
   SELECT
     "agent_id"
   , "source_ip"
   , "destination_ip"
   , "destination_port"
   , "agent_assigned_process_id"
   , "count"(*) "frequency"
   FROM
     outbound_connection_agent
   WHERE (("ip_version" = 'IPv4') AND ("destination_ip" IN (SELECT *
   FROM
     valid_outbound_ips_helper
   )))
   GROUP BY "agent_id", "source_ip", "destination_ip", "destination_port", "agent_assigned_process_id";
   ```

Now that you have the `iana_service_ports_import` table and your two helper functions, you can run the following query to get the details on the outbound traffic for each service, along with the port number and process details\.

```
SELECT DISTINCT
  "hin1"."host_name" "Source Host Name"
, "hin2"."host_name" "Destination Host Name"
, "o"."source_ip" "Source IP Address"
, "o"."destination_ip" "Destination IP Address"
, "o"."frequency" "Connection Frequency"
, "o"."destination_port" "Destination Communication Port"
, "p"."name" "Process Name"
, "ianap"."service name" "Process Service Name"
, "ianap"."description" "Process Service Description"
FROM
  outbound_query_helper o
, hostname_ip_helper hin1
, hostname_ip_helper hin2
, processes_agent p
, iana_service_ports_import ianap
WHERE ((((("o"."source_ip" = "hin1"."ip_address") AND ("o"."destination_ip" = "hin2"."ip_address")) AND ("p"."agent_assigned_process_id" = "o"."agent_assigned_process_id")) AND ("hin1"."host_name" <> "hin2"."host_name")) AND (("o"."destination_port" = TRY_CAST("ianap"."port number" AS integer)) AND ("ianap"."transport protocol" = 'tcp')))
ORDER BY "hin1"."host_name" ASC, "o"."frequency" DESC;
```

### Track Inbound Communication Between Servers Based On Port Number and Process Details<a name="pq-analyze-inbound-connections"></a>

Before running this query, perform the following procedures\. Note that if you've already created the `iana_service_ports_import` table, you can skip the first procedure\.

**To create the iana\_service\_ports\_import table**

1. Download the IANA Port database CSV file from [Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) on *iana\.org*\.

1. Upload the file to Amazon S3\. For more information, see [How Do I Upload Files and Folders to an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html)\.

1. Create a new table in Athena named `iana_service_ports_import` and specify the Amazon S3 path to the object you uploaded in the previous step\. For more information, see [Step 2: Create a Table](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html#step-2-create-a-table) in the *Amazon Athena User Guide*\.

Now that the `iana_service_ports_import` table has been created, you create two help functions for tracking inbound traffic\. For information on how to create a view, see [CREATE VIEW](https://docs.aws.amazon.com/athena/latest/ug/create-view.html) in the *Amazon Athena User Guide*\. 

**To create import tracking helper functions**

1. Open the Athena console at [https://console\.aws\.amazon\.com/athena/](https://console.aws.amazon.com/athena/home)\.

1. Create the view `valid_inbound_ips_helper`\. This helper function gives the list of all distinct inbound source ip addresses and is as follows:

   ```
   CREATE OR REPLACE VIEW valid_inbound_ips_helper AS 
   SELECT DISTINCT "source_ip"
   FROM
     inbound_connection_agent;
   ```

1. Create the view `inbound_query_helper`\. This helper function determines the frequency of communication for inbound traffic and is as follows:

   ```
   CREATE OR REPLACE VIEW inbound_query_helper AS 
   SELECT
     "agent_id"
   , "source_ip"
   , "destination_ip"
   , "destination_port"
   , "agent_assigned_process_id"
   , "count"(*) "frequency"
   FROM
     inbound_connection_agent
   WHERE (("ip_version" = 'IPv4') AND ("source_ip" IN (SELECT *
   FROM
     valid_inbound_ips_helper
   )))
   GROUP BY "agent_id", "source_ip", "destination_ip", "destination_port", "agent_assigned_process_id";
   ```

Now that you have the `iana_service_ports_import` table and your two helper functions, you can run the following query to get the details on the inbound traffic for each service, along with the port number and process details\.

```
SELECT DISTINCT
  "hin1"."host_name" "Source Host Name"
, "hin2"."host_name" "Destination Host Name"
, "i"."source_ip" "Source IP Address"
, "i"."destination_ip" "Destination IP Address"
, "i"."frequency" "Connection Frequency"
, "i"."destination_port" "Destination Communication Port"
, "p"."name" "Process Name"
, "ianap"."service name" "Process Service Name"
, "ianap"."description" "Process Service Description"
FROM
  inbound_query_helper i
, hostname_ip_helper hin1
, hostname_ip_helper hin2
, processes_agent p
, iana_service_ports_import ianap
WHERE ((((("i"."source_ip" = "hin1"."ip_address") AND ("i"."destination_ip" = "hin2"."ip_address")) AND ("p"."agent_assigned_process_id" = "i"."agent_assigned_process_id")) AND ("hin1"."host_name" <> "hin2"."host_name")) AND (("i"."destination_port" = TRY_CAST("ianap"."port number" AS integer)) AND ("ianap"."transport protocol" = 'tcp')))
ORDER BY "hin1"."host_name" ASC, "i"."frequency" DESC;
```

### Identify Running Software From Port Number<a name="pq-identify-software"></a>

This query can be used to identify the running software based on port numbers\. Note that this query requires you to download the IANA Port database, which can be downloaded from the IANA website at the following URL: [https://www\.iana\.org/assignments/service\-names\-port\-numbers/service\-names\-port\-numbers\.csv](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.csv )\.

Before running this query, you'll need to create the `iana_service_ports_import` table\. Note that if you've already created the table, you can skip this procedure\.

**To create the iana\_service\_ports\_import table**

1. Download the IANA Port database CSV file from [Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) on *iana\.org*\.

1. Upload the file to Amazon S3\. For more information, see [How Do I Upload Files and Folders to an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html)\.

1. Create a new table in Athena named `iana_service_ports_import` and specify the Amazon S3 path to the object you uploaded in the previous step\. For more information, see [Step 2: Create a Table](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html#step-2-create-a-table) in the *Amazon Athena User Guide*\.

```
SELECT DISTINCT
  "o"."host_name" "Host Name"
, "ianap"."service name" "Service"
, "ianap"."description" "Description"
, "con"."destination_port"
, "count"("con"."destination_port") "Destination Port Count"
FROM
  inbound_connection_agent con
, os_info_agent o
, iana_service_ports_import ianap
, network_interface_agent ni
WHERE ((((("con"."destination_ip" = "ni"."ip_address") AND (NOT ("con"."destination_ip" LIKE '172%'))) AND (("con"."destination_port" = "ianap"."port number") AND ("ianap"."transport protocol" = 'tcp'))) AND ("con"."agent_id" = "o"."agent_id")) AND ("o"."agent_id" = "ni"."agent_id"))
GROUP BY "o"."host_name", "ianap"."service name", "ianap"."description", "con"."destination_port"
ORDER BY "Destination Port Count" DESC
```