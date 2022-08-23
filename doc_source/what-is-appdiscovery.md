# What is AWS Application Discovery Service?<a name="what-is-appdiscovery"></a>

AWS Application Discovery Service helps you plan your migration to the AWS cloud by collecting usage and configuration data about your on\-premises servers\. Application Discovery Service is integrated with AWS Migration Hub, which simplifies your migration tracking as it aggregates your migration status information into a single console\. You can view the discovered servers, group them into applications, and then track the migration status of each application from the Migration Hub console in your home Region\.

All discovered data is stored in your AWS Migration Hub home Region\. Therefore, you must set your home Region in the Migration Hub console or with CLI commands before performing any discovery and migration activities\. Your data can be exported for analysis in Microsoft Excel or AWS analysis tools such as Amazon Athena and Amazon QuickSight\.

Using Application Discovery Service APIs, you can export the system performance and utilization data for your discovered servers\. Input this data into your cost model to compute the cost of running those servers in AWS\. Additionally, you can export data about the network connections that exist between servers\. This information helps you determine the network dependencies between servers and group them into applications for migration planning\.

**Note**  
Your home Region must be set in AWS Migration Hub before you begin the process of discovery, because your data will be stored in your home Region\. For more information about working with a home Region, see [Home Region](https://docs.aws.amazon.com/migrationhub/latest/ug/home-region.html)\.

Application Discovery Service offers two ways of performing discovery and collecting data about your on\-premises servers:
+ **Agentless discovery** can be performed by deploying the Application Discovery Service Agentless Collector \(Agentless Collector\) \(OVA file\) through your VMware vCenter\. After Agentless Collector is configured, it identifies virtual machines \(VMs\) and hosts associated with vCenter\. Agentless Collector collects the following static configuration data: Server hostnames, IP addresses, MAC addresses, disk resource allocations\. Additionally, it collects the utilization data for each VM and computes average and peak utilization for metrics such as CPU, RAM, and Disk I/O\.
+ **Agent\-based discovery** can be performed by deploying the AWS Application Discovery Agent on each of your VMs and physical servers\. The agent installer is available for Windows and Linux operating systems\. It collects static configuration data, detailed time\-series system\-performance information, inbound and outbound network connections, and processes that are running\.

Application Discovery Service integrates with application discovery solutions from AWS Partner Network \(APN\) partners\. These third\-party solutions can help you import details about your on\-premises environment directly into Migration Hub, without using any agentless collector or discovery agent\. Third\-party application discovery tools can query AWS Application Discovery Service, and they can write to the Application Discovery Service database using the public API\. In this way, you can import data into Migration Hub and view it, so that you can associate applications with servers and track migrations\. 

## VMware Discovery<a name="more-discovery"></a>

If you have virtual machines \(VMs\) that are running in the VMware vCenter environment, you can use the Agentless Collector to collect system information without having to install an agent on each VM\. Instead, you load this on\-premises appliance into vCenter and allow it to discover all of its hosts and VMs\. 

Agentless Collector captures system performance information and resource utilization for each VM running in the vCenter, regardless of what operating system is in use\. However, it cannot “look inside” each of the VMs, and as such, cannot figure out what processes are running on each VM nor what network connections exist\. Therefore, if you need this level of detail and want to take a closer look at some of your existing VMs in order to assist in planning your migration, you can install the Discovery Agent on an as\-needed basis\.

Also, for VMs hosted on VMware, you can use both the Agentless Collector and Discovery Agent to perform discovery simultaneously\. For details regarding the exact types of data each discovery tool will collect, see [Data collected by Agentless Collector](agentless-collector-data-collected.md) and [Data collected by Discovery Agent](agent-data-collected.md)\.

## Compare Agentless Collector and Discovery Agent<a name="compare-tools"></a>

The following table provides a quick comparison of the Application Discovery Service data collection tools\.


|  | Agentless Collector | Discovery Agent | 
| --- |--- |--- |
| Supported server types | 
| --- |
| VMware virtual machine  | Yes | Yes | 
| Physical server  | No | Yes | 
| Deployment | 
| --- |
| Per server | No | Yes | 
| Per vCenter | Yes | No | 
| Collected data | 
| --- |
| Static configuration data | Yes | Yes | 
| VM utilization metrics | Yes | No | 
| Time series performance information | No | Yes \(Export only\) | 
| Network inbound/outbound connections | No | Yes \(Export only\) | 
| Running processes | No | Yes \(Export only\) | 
|  **Supported OS**  | Any OS running in VMware vCenter V5\.5\+ |  For the list of supported Linux and Windows operating systems, see [Prerequisites for Discovery Agent](gen-prep-agents.md)\.  | 

## Assumptions<a name="assumptions"></a>

To use Application Discovery Service, the following is assumed:
+ You have signed up for AWS\. For more information, see [Setting up Application Discovery Service](setting-up.md)\.
+ You have selected a Migration Hub home Region\. For more information, see [the documentation regarding home Regions](https://docs.aws.amazon.com/migrationhub/latest/ug/home-region.html)\.

Here's what to expect:
+ The Migration Hub home Region is the only Region where Application Discovery Service stores your discovery and planning data\.
+ Discovery agents, connectors, and imports can be used in your selected Migration Hub home Region only\.
+ For a list of AWS Regions where you can use Application Discovery Service, see the [Amazon Web Services General Reference](https://docs.aws.amazon.com/general/latest/gr/rande.html#migrationhub-region)\.