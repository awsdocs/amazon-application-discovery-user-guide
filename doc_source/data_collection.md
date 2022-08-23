# Data collection tools<a name="data_collection"></a>

Application Discovery Service Agentless Collector \(Agentless Collector\) and AWS Application Discovery Agent \(Discovery Agent\) are the data collection tools that AWS Application Discovery Service \(Application Discovery Service\) uses to help you discover your existing infrastructure\. The following topics explain how to download and deploy these discovery data collection tools, [Getting started with Agentless Collector](agentless-collector-gs.md) and [AWS Application Discovery Agent](discovery-agent.md)\.

These data collection tools store their data in the Application Discovery Service's repository, providing details about each server and the processes running on them\. When either of these tools is deployed, you can start, stop, and view the collected data from the AWS Migration Hub \(Migration Hub\) console\.

**Topics**
+ [Starting and stopping data collectors](#start-stop-data_collection)
+ [Viewing and sorting data collectors](#sort-data-collectors)

## Starting and stopping data collectors<a name="start-stop-data_collection"></a>

If you deployed AWS Application Discovery Agent \(Discovery Agent\), you can start or stop the data collection process on the **Data Collectors** page of the AWS Migration Hub \(Migration Hub\) console\.

**To start or stop data collection tools**

1. Using your AWS account, sign in to the AWS Management Console and open the Migration Hub console at [https://console\.aws\.amazon\.com/migrationhub/](https://console.aws.amazon.com/migrationhub/)\.

1. In the Migration Hub console navigation pane under **Discover**, choose **Data collectors**\.

1. Choose the **Agents** tab\.

1. Select the check box of the collection tool you want to start or stop\.

1. Choose **Start data collection** or **Stop data collection**\.

## Viewing and sorting data collectors<a name="sort-data-collectors"></a>

If you deployed many data collectors, you can sort the displayed list of deployed collector's on the **Data Collectors** page of the console\. You sort the list by applying filters in the search bar\. You can search and filter on most of the criteria specified in the **Data Collectors** list\.

The following table shows the search criteria that you can use for **Agents**, including operators, values, and a definition of the values\.


| Search Criterion | Operator | Value: Definition | 
| --- | --- | --- | 
| Agent ID |  ==  |  Any agent ID selected from the pre\-populated list from which a collection tool is installed\.  | 
| Hostname |  == \!=  |  For agents, any host name selected from the pre\-populated list of hosts where an agent is installed\.  | 
|  Collection status  |  == \!=  |  Started: Data is being collected and sent to Application Discovery Service Start scheduled: Data collection is scheduled to start\. Data will be sent to Application Discovery Service on next ping, and status will change to **Started**\. Stopped: Data is not being collected or sent to Application Discovery Service\. Stop scheduled: Data collection is scheduled to stop\. Data will stop being sent to Application Discovery Service on next ping, and status will change to **Stopped**\.  | 
|  Health  |  == \!=  |  Healthy: Data collection isn't turned on\. The tool is functioning normally\. Unhealthy: The tool is in an error state\. Data isn't being collected or reported\. Unknown: No connection established in over an hour\. Shutdown: The tool last communicated "shutting down" due to a system, service, or daemon shut down\. If a reboot or tool upgrade occurred, status will change to another state at the first reporting cycle\. Running: Data collection is turned on\. The tool is functioning normally\.  | 
| IP address |  == \!=  |  Any IP address selected from the pre\-populated list where a collection tool is installed\.  | 

The following table shows the search criteria that you can use for **Agentless collectors**, including operators, values, and a definition of the values\.


| Search Criterion | Operator | Value: Definition | 
| --- | --- | --- | 
| ID |  ==  |  Any agentless collector ID selected from the pre\-populated list from which a collection tool is installed\.  | 
| Hostname |  == \!=  |  For agentless collectors, any host name selected from the pre\-populated list of hosts where an agentless collectors is installed\.  | 
|  Status  |  == \!=  |  Collecting data: Data collection is turned on\. The tool is functioning normally\. Ready to configure— Data collection isn't turned on\. The tool is functioning normally\. Requires attention— The tool is in an error state and needs attention\. Unknown: No connection established in over an hour\. Shut down: The tool last communicated "shutting down" due to a system, service, or daemon shut down\. If a reboot or tool upgrade occurred, status will change to another state at the first reporting cycle\.  | 
| IP address |  == \!=  |  Any IP address selected from the pre\-populated list where a collection tool is installed\.  | 

**To sort data collectors by applying search filters**

1. Using your AWS account, sign in to the AWS Management Console and open the Migration Hub console at [https://console\.aws\.amazon\.com/migrationhub/](https://console.aws.amazon.com/migrationhub/)\.

1. In the Migration Hub console navigation pane under **Discover**, choose **Data Collectors**\.

1. Choose either the **Agentless collectors** or **Agents** tab\.

1. Click inside the search bar and choose a search criterion from the list\.

1. Choose an operator from the next list\.

1. Choose a value from the last list\.