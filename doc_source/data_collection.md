# Data collection<a name="data_collection"></a>

The **Data collection** page displays a tab for each type of data collection tool supported, currently **Agents** \([application discovery agents](http://docs.aws.amazon.com/application-discovery/latest/userguide/appdisc-components.html#awsagent)\) and **Connectors** \([agentless discovery connectors](http://docs.aws.amazon.com/application-discovery/latest/userguide/appdisc-components.html#appdisc-agentless)\)\. On this page, you can:

+ [View and search data collection tools](#view_tools)

+ [Start data collection for both agents and connectors](#start_data_collection)

+ [Stop data collection](#stop_data_collection)

**Note**  
You must explicitly start data collection for discovery to begin\.

The table of available tools provides the following information about each:

+ **Agent ID** or **Connector ID**

+ **Hostname**

+ **Collection status**

  The supported states can be understood as follows: 

  +  **STARTED**—The collection tool has started collecting and sending data to Discovery service\. 

  +  **START\_SCHEDULED**—The data collection has been scheduled to be started\. The next time collection tool contacts AWS, it will start sending data to the Discovery Service and the collection status will change to **STARTED**\. 

  +  **STOPPED**—The collection tool has stopped sending data to the Discovery service\.

  +  **STOP\_SCHEDULED**—The data collection has been scheduled to be stopped\. The next time collection tool contacts AWS, it will stop sending data to the Discovery service and the Collection status will change to **STOPPED**\.

+ **Health**

+ **IP address**

+ **Version** \(of the collection tool\)

+ **Registered time** \(when the collection tool was created\)

+ **Last health ping time**

The following procedures demonstrate how to carry out typical data\-related management tasks\. Though these examples focus on discovery agents, the steps for agentless discovery connectors are nearly identical\.<a name="view_tools"></a>

**To view and filter data collection tools**

1. In the navigation menu, choose **Data collection**\.

1. Choose **Agents** to view a table of installed application discovery agents\. Each entry provides detailed information about an agent, such as its ID and host name\. 

1. To filter the display, choose the menu\-driven filter bar, and select one of the available fields in the resulting menu:

   + **Collection status**

   + **Health**

   + **Host name**

   + **IP address**

   + **Agent ID**

1. Select one of the available operators:

   + **==**

   + **\!=**

1. Select a field value\. These vary based on the filter selected earlier\. For **Health**, you see a menu with the following possible values: 

   + **HEALTHY**

   + **RUNNING**

   + **UNHEALTHY**

   + **UNKNOWN**

   + **BLACKLISTED**

   + **SHUTDOWN**

The table now displays only the entries that match your filter criterion\. You can also define multiple filters, delete filters, and bypass the filter menus by typing into the filter bar directly\. For more information about agent health status and collection status, see [Querying Discovered Configuration Items](http://docs.aws.amazon.com/application-discovery/latest/APIReference/discovery-api-queries.html) in the *Application Discovery Service API Reference*\.<a name="start_data_collection"></a>

**To start data collection for both agents and connectors**

1. In the navigation menu, choose **Data collection**, **Agents**\.

1. In the table, select the check box associated with each of the agents to start\.

1. Choose **Start data collection**\. In the **Collection status** field, note that the status of each of your selected collection tools changes to either **START\_SCHEDULED** or **STARTED**\. The next time each of your selected collection tools contacts AWS, it collects and sends data to Application Discovery Service\. <a name="stop_data_collection"></a>

**To stop data collection**

1. In the navigation menu, choose **Data collection**, **Agents**\.

1. In the table, select the check box associated with each of the agents to stop\.

1. Choose **Stop data collection**\. In the **Collection status** field, note that the status of each of your selected collection tools is now either **STOP\_SCHEDULED** or **STOPPED**\. The next time each of your selected collections tools contacts AWS, it stops sending discovery data to Application Discovery Service\. The status of each selected collection tool changes to **STOPPED** after data collection has halted\.