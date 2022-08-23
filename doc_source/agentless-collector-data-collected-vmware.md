# Data collected by the Agentless Collector VMware vCenter data collection module<a name="agentless-collector-data-collected-vmware"></a>

The following information describes the data collected by the Application Discovery Service Agentless Collector \(Agentless Collector\) VMware vCenter data collection module\. For information about setting up data collection, see [How to set up the Agentless Collector data collection module for VMware vCenter](agentless-collector-gs-data-collection-vcenter.md#agentless-collector-gs-vcenter)\.

**Table legend for Agentless Collector VMware vCenter collected data:**
+ Collected data is in measurements of kilobytes \(KB\) unless stated otherwise\.
+ Equivalent data in the Migration Hub console is reported in megabytes \(MB\)\.
+ Data fields denoted with an asterisk \(\*\) are available only in the \.csv files produced from the Application Discovery Service API export function\. 

  The Agentless Collector supports data export using the AWS CLI\. To export collected data using the AWS CLI, follow the instructions described under **Export System Performance Data for All Servers** on the page [ Export Collected Data](https://docs.aws.amazon.com/application-discovery/latest/userguide/export-data.html) in the *Application Discovery Service User Guide*\. 
+ The polling period is in intervals of approximately 60 minutes\.
+ Data fields denoted with a double asterisk \(\*\*\) currently return a *null* value\.


| Data field | Description | 
| --- | --- | 
| applicationConfigurationId\* | ID of the migration application the VM is grouped under | 
| avgCpuUsagePct | Average percentage of CPU usage over polling period | 
| avgDiskBytesReadPerSecond | Average number of bytes read from disk over polling period | 
| avgDiskBytesWrittenPerSecond | Average number of bytes written to disk over polling period | 
| avgDiskReadOpsPerSecond\*\* | Average number of read I/O operations per second null | 
| avgDiskWriteOpsPerSecond\*\* | Average number of write I/O operations per second | 
| avgFreeRAM | Average free RAM expressed in MB | 
| avgNetworkBytesReadPerSecond | Average amount of throughput of bytes read per second | 
| avgNetworkBytesWrittenPerSecond | Average amount of throughput of bytes written per second | 
| configId | ID assigned by Application Discovery Service to the discovered VM | 
| configType | Type of resource discovered | 
| connectorId | ID of the virtual appliance | 
| cpuType | vCPU for a VM, actual model for a host | 
| datacenterId | ID of the vCenter | 
| hostId\* | ID of the VM host | 
| hostName | Name of host running the virtualization software | 
| hypervisor | Type of hypervisor | 
| id | ID of server | 
| lastModifiedTimeStamp\* | Latest date and time of data collection before data export | 
| macAddress | MAC address of the VM | 
| manufacturer | Maker of the virtualization software | 
| maxCpuUsagePct  | Max\. percentage of CPU usage during polling period | 
| maxDiskBytesReadPerSecond | Max\. number of bytes read from disk over polling period | 
| maxDiskBytesWrittenPerSecond | Max\. number of bytes written to disk over polling period | 
| maxDiskReadOpsPerSecond\*\* | Max\. number of read I/O operations per second | 
| maxDiskWriteOpsPerSecond\*\* | Max\. number of write I/O operations per second | 
| maxNetworkBytesReadPerSecond | Max\. amount of throughput of bytes read per second | 
| maxNetworkBytesWrittenPerSecond | Max\. amount of throughput of bytes written per second | 
| memoryReservation\* | Limit to avoid overcommitment of memory on VM | 
| moRefId | Unique vCenter Managed Object Reference ID | 
| name\* | Name of VM or network \(user specified\) | 
| numCores | Number of independent processing units within CPU | 
| numCpus | Number of central processing units on VM | 
| numDisks\*\* | Number of disks on VM | 
| numNetworkCards\*\* | Number of network cards on VM | 
| osName | Operating system name on VM | 
| osVersion | Operating system version on VM | 
| portGroupId\* | ID of group of member ports of VLAN | 
| portGroupName\* | Name of group of member ports of VLAN | 
| powerState\* | Status of power | 
| serverId | Application Discovery Service assigned ID to the discovered VM | 
| smBiosId\* | ID/version of the system management BIOS | 
| state\* | Status of the virtual appliance | 
| toolsStatus | Operational state of VMware tools  | 
| totalDiskSize | Total capacity of disk expressed in MB | 
| totalRAM | Total amount of RAM available on VM in MB | 
| type | Type of host | 
| vCenterId | Unique ID number of a VM | 
| vCenterName\* | Name of the vCenter host | 
| virtualSwitchName\* | Name of the virtual switch | 
| vmFolderPath | Directory path of VM files | 
| vmName | Name of the virtual machine | 