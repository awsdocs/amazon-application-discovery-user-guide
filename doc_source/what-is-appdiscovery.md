# What Is AWS Application Discovery Service?<a name="what-is-appdiscovery"></a>

AWS Application Discovery Service collects and presents data to enable enterprise customers to understand the configuration, usage, and behavior of servers in their on\-premises environment\. Server data is stored securely by the service, where it can be tagged and grouped into applications to help with AWS migration planning\. The data can be exported for analysis in Excel or other cloud migration analysis tools\. For more information, see [AWS Application Discovery Service FAQ](https://aws.amazon.com/application-discovery/faqs/)\.

Application Discovery Service offers two modes of operation:

+ **Agentless discovery** mode is recommended for environments that use VMware vCenter Server\. This mode doesn't require you to install an agent on each host\. Agentless discovery gathers server information regardless of the operating systems, which minimizes the time required for initial on\-premises infrastructure assessment\. It collects static configuration data including server hostnames, IP addresses, MAC addresses, CPU allocation, network throughput, memory allocation, disk resource allocations, and DNS servers\. It also captures resource utilization metrics such as CPU usage and memory usage\.

+ **Agent\-based discovery** mode collects a richer set of data than agentless discovery by using Amazon software, the AWS Application Discovery agent, which you install on one or more hosts in your data center\. The agent captures infrastructure and server information including system configuration, system performance, running processes, and details of the network connections between systems\. The information collected by agents is secured at rest and in transit to the Application Discovery Service data store in the cloud\. Agent\-based discovery works in both VMware and non\-virtualized environments\. 

For VMware environments, we recommend that you run the Agentless Discovery Connector first to perform the initial infrastructure assessment, and then to install agents selectively on individual VMs to gather additional details\. For non\-VMware environments, including physical servers, you can use agent\-based discovery\.

You can run agent\-based and agentless discovery simultaneously\. Use agentless discovery to quickly complete the initial infrastructure assessment and then install agents on select hosts\.

**Important**  
Application Discovery Service doesn't gather sensitive information\. All data is handled according to the [AWS Privacy Policy](https://aws.amazon.com/privacy/)\. For additional security, you can operate Application Discovery Service offline to inspect collected data before it is shared with the service\.

For more information about the data that Application Discovery Service collects, see [AWS Application Discovery Service Components](appdisc-components.md)\.


+ [Prerequisites](appdisc-prereq.md)
+ [AWS Application Discovery Service Components](appdisc-components.md)
+ [Service Limitations](ads_service_limits.md)