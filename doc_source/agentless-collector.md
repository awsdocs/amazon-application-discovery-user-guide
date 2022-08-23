# Application Discovery Service Agentless Collector<a name="agentless-collector"></a>

Application Discovery Service Agentless Collector \(Agentless Collector\) is an on\-premises application that collects information through agentless methods about your on\-premises environment, including server profile information \(for example, OS, number of CPUs, amount of RAM\), and server utilization metrics\.  You install the Agentless Collector as a virtual machine \(VM\) in your VMware vCenter Server environment using an Open Virtualization Archive \(OVA\) file\. 

Agentless Collector has a modular architecture, which allows for use of multiple agentless collection methods\. Agentless Collector currently supports one\-module collection from VMware VMs\. Future modules will support network connection collection, collection from additional virtualization platforms, and operating system level collection\. 

Agentless Collector supports data collection for the AWS Application Discovery Service \(Application Discovery Service\), which helps you plan your migration to the AWS Cloud by collecting usage and configuration data about your on\-premises servers\. 

Application Discovery Service is integrated with AWS Migration Hub, which simplifies your migration tracking as it aggregates your migration status information into a single console\. You can view the discovered servers, obtain Amazon EC2 recommendations, visualize network connections, group servers into applications, and then track the migration status of each application from the Migration Hub console in your home Region\.

**Topics**
+ [Getting started with Agentless Collector](agentless-collector-gs.md)
+ [Data collected by Agentless Collector](agentless-collector-data-collected.md)
+ [Using the Agentless Collector console](agentless-collector-using.md)
+ [Manually updating Agentless Collector](agentless-collector-update.md)
+ [Troubleshooting Agentless Collector](agentless-collector-troubleshooting.md)