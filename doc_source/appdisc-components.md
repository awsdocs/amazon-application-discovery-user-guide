# AWS Application Discovery Service Components<a name="appdisc-components"></a>

AWS Application Discovery Service uses a combination of AWS software agents and cloud\-based services to identify, map, and store an inventory of the assets in your computing environment\.


+ [Arsenal](#appdisc-arsenal)
+ [AWS Agentless Discovery Connector](#appdisc-agentless)
+ [AWS Application Discovery Agent](#awsagent)
+ [Related Services and Partner Tools](#appdisc-related)
+ [Processing and Database Components](#aws-app-discsrv)

## Arsenal<a name="appdisc-arsenal"></a>

Arsenal is an agent service managed and hosted by AWS that sends data from AWS Application Discovery Agents and the AWS Agentless Discovery Connector to Application Discovery Service in the cloud\. The word *arsenal* is included in some URLs and IAM policies\. 

## AWS Agentless Discovery Connector<a name="appdisc-agentless"></a>

Agentless discovery uses the AWS Agentless Discovery Connector to communicate with Arsenal\. Install the connector as a virtual machine \(VM\) in your VMware vCenter Server environment using an Open Virtualization Archive \(OVA\) file\. When you start the connector, it registers with Arsenal, and queries Arsenal for configuration information\. When you send a command for the connector to start collecting data, it connects to VMWare vCenter Server and collects information about all the VMs and hosts managed by this specific vCenter\. The collected data is sent to Arsenal using Secure Sockets Layer \(SSL\) encryption\. The connector is configured to automatically upgrade when new versions of the connector become available\. You can change this configuration setting at any time\. 

Agentless discovery relies on VMware metadata, and can therefore collect information about any server running in your VMware environment, regardless of operating system\. 

### Data Collected by Agentless Discovery<a name="agentless-data"></a>

Agentless discovery does not collect information about your applications\. It only collects information about your VMware vCenter Server hosts and VMs, including performance data about those hosts and VMs\. The information collected by agentless discovery is shown in the following tables\. Items in **bold** are only captured if VMware vCenter Server tools are installed\. Agentless discovery attempts to collect all the following data; however, in some situations, vCenter might not have the data, as noted in the following tables\.


**Inventory Data about VMs in vCenter**  

| Data | Data availability | 
| --- | --- | 
| Timestamp | Guaranteed | 
|  OSType  | If available | 
|  SystemRelease  | If available | 
| MoRefID \(Unique vCenter Managed Object Reference ID\) | Guaranteed | 
| instanceUuid \(Unique ID for a virtual machine, not for the host system\) | If available | 
| FolderPath \(VM folder path in vCenter\) | Guaranteed | 
| Name\(Name of the vCenter VM or host\) | Guaranteed | 
|  Hostname  | If available | 
| Hypervisor | Guaranteed | 
| Manufacturer | Guaranteed | 
| ToolsStatus \(VMware tools status\) | If available | 
| HostSystem \(MoRefId of the VM or host system\) | Guaranteed for VM | 
| Datacenter \(MoRefID of the data center where the system is located\) | Guaranteed | 
| Type\(Host or VM\) | Guaranteed | 
| vCenterId \(Unique vCenter ID\) | Guaranteed | 
| smBiosId | If available | 
| MacAddress | Guaranteed | 
|  IpAddress  | If available | 
| Network \- List\(A VMware object representation of a network\) | If available | 
| macAddress \(For the network\) | Guaranteed if the network exists | 
| portGroupName \(For the network\) | If available | 
| portGroupId \(For the network\) | If available | 
| virtualSwitchName | If available | 
| Name \(Network name specified by the user\) | If available | 
| CPUType \(vCPU for a VM, actual model for a host\) | If available | 


**Performance Data for VMs in vCenter**  

| Data | Guaranteed or If Available | 
| --- | --- | 
| Timestamp | Guaranteed | 
| MoRefID \(Managed Object Reference ID of the system producing the metrics\) | Guaranteed | 
| Type \(Host or VM\) | Guaranteed | 
| vCenterId \(Unique ID of the vCenter\) | Guaranteed | 
| smBiosId | If available | 
| PowerState | Guaranteed | 
| MemorySize \(Memory size of the VM/host\) | If available | 
| MemoryReservation \(Reservation set for a VM\) | If available | 
|  ActiveRAM\(Average RAM over the polling period\) | If available | 
|  MaxActiveRam\(Max RAM over polling period\) | If available | 
| NetworkCards | If available | 
| Name \(Name associated with the metrics collected\) | If available | 
|  BytesReadPerSecond \(Average over the polling period\) | If available | 
|  BytesWrittenPerSecond\(Average over the polling period\) | If available | 
|  TotalUsage\(Average transmitted/received over the polling period\) | If available | 
|  MaxTotalUsage\(Max transmitted/received over the polling period\) | If available | 
| Disks | If available | 
| DeviceID \(Name associated with metrics collected; for a virtual device, it is the SCSI ID\) | If available | 
| Name | If available | 
| Capacity | If available | 
| scsi \(For mapping performance metrics to a virtual disk\) | If available | 
| BytesReadPerSecond \(Average over the polling period\) | If available | 
| BytesWrittenPerSecond \(Average over the polling period\) | If available | 
| ReadOpsPerSecond \(Average over the polling period\) | If available | 
| WriteOpsPerSecond \(Average over the polling period\) | If available | 
| Cpus | If available | 
|  Name\(Name associated with the metrics collected\) | If available | 
| UsagePct | If available | 
|  UsageMHz \(Average over the polling period\) | If available | 
|  MaxUsageMHz \(Max over the polling period\) | If available | 
| numCores | If available | 
| speedMHz | If available | 
| reservationMHz \(Reservation set for a VM\) | If available | 

## AWS Application Discovery Agent<a name="awsagent"></a>

The AWS Application Discovery Agent is AWS software that you install on on\-premises servers and VMs targeted for discovery and migration\. Agents run on Linux and Windows and collect server configuration and activity information about your applications and infrastructure\. You can also install the agent on Amazon EC2 instances\. When you start an agent, it registers with Arsenal and frequently pings the service for configuration information\. When you send a command that tells an agent to start collecting data, it collects an extensive amount of data for the host or VM where it resides, including TCP connections to other hosts or VMs, which can help you map your IT assets without having to install an agent on every host or VM\. Agents are configured to upgrade automatically when new versions become available\. You can change this configuration setting at any time\.

The agent does not require device drivers or kernel modules and operates only in user space\. This follows industry best practices and reduces the risk of system instability\.

After you install agents, you manage them using the Application Discovery Service API\. The API includes actions to start and stop data collection\. You can also retrieve information about agents, including the host names where agents reside, their health, and the version number of each agent\. For more information, see the [Application Discovery Service API Reference](http://docs.aws.amazon.com/application-discovery/latest/APIReference/)\.

### Data Collected by the Discovery Agent<a name="agent-data"></a>

The Application Discovery agent collects the following information about each system where it is installed:

+ System identification information \(hostname, IP addresses, MAC addresses, operating system name and version\)

+ System resource specifications \(CPU, RAM, storage, etc\.\)

+ System\-level resource utilization

+ Running processes

+ TCP/IP \(v4 and v6\) connections

## Related Services and Partner Tools<a name="appdisc-related"></a>

You have the flexibility to choose the discovery and migration tools that you need by integrating with AWS Partner tools using the Application Discovery Service API\. For more information, see the [Application Discovery Service API Reference](http://docs.aws.amazon.com/application-discovery/latest/APIReference/)\.

After completing discovery of your virtualized inventory, use AWS Server Migration Service to perform an incremental, automated migration of your VMs to the Amazon EC2 cloud\. For information, see the [AWS SMS User Guide](http://docs.aws.amazon.com/server-migration-service/latest/userguide/)\. 

You can use the AWS VM Import/Export tools to import VM images manually from your local virtualization environment into AWS and convert them into ready\-to\-use Amazon EC2 Amazon Machine Images \(AMIs\) or instances\. For more information, see [Importing a VM as an Instance Using VM Import/Export](http://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-instance-import.html)\.

## Processing and Database Components<a name="aws-app-discsrv"></a>

AWS Application Discovery Agents and the AWS Agentless Discovery Connector send data to Application Discovery Service, which includes a processing component that ingests the data and identifies \(maps\) IT assets\. The service also includes the AWS Discovery database, a repository for discovered and mapped IT assets called *configuration items*\.

Data in the Discovery database is encrypted at rest\. Encryption keys for the data are managed using the AWS KMS\.

**Note**  
The AWS Discovery database is not a general purpose, enterprise configuration management database \(CMDB\)\. You can't save snapshots of discovered resources or track resource changes\. The service does not alert you when resource configurations change\. Similarly, though the service does collect performance data, it is not a general purpose, health monitoring solution\.

When you use Application Discovery Service, you can specify filters and query specific configuration items in the AWS Discovery database\. The service supports server, process, and connection configuration items\. This means you can specify a value for the following keys and query your IT assets\. 

**Note**  
Server Performance, shown below, is an attribute of Server\.


| Server | Process | Connection | ServerPerformance | 
| --- | --- | --- | --- | 
| cpuType | name | sourceIp | numCores | 
| hostName | commandLine | sourceProcess | numCpus | 
| hypervisor | path | destinationIp | numDisks | 
| osName | avgFreeRAMInKB | destinationPort | numNetworkCards | 
| osVersion | minFreeRAMInKB | dstProcess | totalDiskSizeInKB | 
| transportProtocol | avgDiskReadsPerSecondInKB | count | totalDiskFreeSizeInKB | 
| lastReportedTime |  | ipVersion | totalRAMInKB | 
| avgDiskWritesPerSecondInKB | 
| avgDiskReadIOPS | 
| avgDiskWriteIOPS | 
| maxDiskReadsPerSecondInKB | 
| maxDiskWritesPerSecondInKB | 
| maxDiskReadIOPS | 
| maxDiskWriteIOPS | 
| avgNetworkReadsPerSecondInKB | 
| avgNetworkWritesPerSecondInKB | 
| maxNetworkReadsPerSecondInKB | 
| maxNetworkWritesPerSecondInKB | 