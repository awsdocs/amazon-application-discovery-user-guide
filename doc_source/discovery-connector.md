# AWS Agentless Discovery Connector<a name="discovery-connector"></a>

Agentless discovery uses the AWS Discovery Connector\. The AWS Discovery Connector is a VMware appliance that can collect information only about VMware virtual machines \(VMs\)\. This mode doesn't require you to install a connector on each host\. You install the Discovery Connector as a VM in your VMware vCenter Server environment using an Open Virtualization Archive \(OVA\) file\. Because the Discovery Connector relies on VMware metadata to gather server information regardless of operating system, it minimizes the time required for initial on\-premises infrastructure assessment\.

After you deploy and configure the Discovery Connector, it registers with the Application Discovery Service endpoint, *https://arsenal\.us\-west\-2\.amazonaws\.com/*, and pings the service at regular intervals, approximately every 60 minutes, for configuration information\. When you start the connector's data collecting process, it connects to VMware vCenter Server where it collects information about all the VMs and hosts managed by this specific vCenter\.

The collected data is sent to the Application Discovery Service using Secure Sockets Layer \(SSL\) encryption\. The connector is configured to automatically upgrade when new versions of the connector become available\. You can change this configuration setting at any time\. 

**Topics**
+ [Data Collected by the Discovery Connector](#agentless-data-collected)
+ [Download the Discovery Connector](#setting-up-agentless)
+ [Deploy the Discovery Connector](#deploy-connector-appliance)
+ [Configure the AWS Discovery Connector](#configure-connector)
+ [Start Discovery Connector Data Collection](#start-connector-data-collection)

## Data Collected by the Discovery Connector<a name="agentless-data-collected"></a>

The Discovery Connector collects information about your VMware vCenter Server hosts and VMs, including performance data about those hosts and VMs\. However, you can capture this data only if VMware vCenter Server tools are installed\. See [Step 3: Provide Application Discovery Service Access to Non\-Administrator Users by Attaching Policies](setting-up.md#setting-up-user-policy) for Discovery Connector installation prerequisites\.

Following, you can find an inventory of the information collected by the Discovery Connector\.

**Table legend for Discovery Connector collected data:**
+ Collected data is in measurements of kilobytes \(KB\) unless stated otherwise\.
+ Equivalent data in the Migration Hub console is reported in megabytes \(MB\)\.
+ Data fields denoted with an asterisk \(\*\) are only available in the \.csv files produced from the connector's API export function\.
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
| configId | Application Discovery Service assigned ID to the discovered VM | 
| configType | Type of resource discovered | 
| connectorId | ID of the Discovery Connector virtual appliance | 
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
| state\* | Status of the Discovery Connector virtual appliance | 
| tagKey | User\-defined key to store custom data or metadata about servers | 
| tagValue | User\-defined value to further define a key's custom data or metadata about servers | 
| toolsStatus | Operational state of VMware tools \(See [Viewing and Sorting Data Collectors](data_collection.md#sort-data-collectors) for a complete list\.\) | 
| totalDiskSize | Total capacity of disk expressed in MB | 
| totalRAM | Total amount of RAM available on VM in MB | 
| type | Type of host | 
| vCenterId | Unique ID number of a VM | 
| vCenterName\* | Name of the vCenter host | 
| virtualSwitchName\* | Name of the virtual switch | 
| vmFolderPath | Directory path of VM files | 
| vmName | Name of the virtual machine | 

## Download the Discovery Connector<a name="setting-up-agentless"></a>

**Download, Set Up, and Start Collecting Data**  
To set up agentless discovery, you must download and deploy the Discovery Connector, which is a virtual appliance, on a VMware vCenter Server host in your on\-premises environment\. The Discovery Connector is an Open Virtualization Archive \(OVA\) file that you must install in your on\-premises VMware environment\.

**Reminder**  
Discovery Connector supports VMware vCenter versions V5\.5, V6, and V6\.5\.

Beginning with this section and those that follow on this page, you will be instructed how to download, deploy, configure, and start collecting data using the Discovery Connector\.

**To download the Discovery Connector OVA file and verify its checksum\.**

1. Sign in to vCenter as a VMware administrator and switch to the directory where you want to download the Discovery Connector OVA file\.

1. Download the [Discovery Connector OVA](https://s3-us-west-2.amazonaws.com/aws.agentless.discovery.connector.bundle/latest/AWSDiscoveryConnector.ova)\.

1. Depending on which hashing algorithm you use in your system environment, download either the [MD5](https://s3-us-west-2.amazonaws.com/aws.agentless.discovery.connector.bundle/latest/AWSDiscoveryConnector.ova.md5) or [SHA256](https://s3-us-west-2.amazonaws.com/aws.agentless.discovery.connector.bundle/latest/AWSDiscoveryConnector.ova.sha256) to get the file containing the checksum value\. Use this value to verify the `AWSDiscoveryConnector.ova` file downloaded in the preceding step\.

1. Depending on your variation of Linux, run the version appropriate MD5 command or SHA256 command to verify the cryptographic signature of the `AWSDiscoveryConnector.ova` file as shown following: 

   ```
   $ md5sum AWSDiscoveryConnector.ova
   MD5 (AWSDiscoveryConnector.ova) = 60c6f85b1db16afd5d6a837a3e7aa8c8
   $ sha256sum AWSDiscoveryConnector.ova
   SHA256(AWSDiscoveryConnector.ova)= 0eb383b8199e336ce7e8ce9a32f4ad8ad7fca4e61a2651afd6cd1b57dded65fd
   ```

   Verify that the checksum value returned from the command you ran is equal to the respective value displayed in the example above\.

## Deploy the Discovery Connector<a name="deploy-connector-appliance"></a>

Deploy the downloaded OVA file of the Discovery Connector in your VMware environment\.

**To deploy the Discovery Connector**

1. Sign in to vCenter as a VMware administrator\.

1. Choose **File**, **Deploy OVF Template**, select the ova file you downloaded in the previous section, and complete the wizard\.

1. On the **Disk Format** page, select one of the thick provision disk types\. We recommend that you choose **Thick Provision Eager Zeroed**, because it has the best performance and reliability\. However, it requires several hours to zero out the disk\. Do not choose **Thin Provision**\. This option makes deployment faster but significantly reduces disk performance\. For more information, see [Types of supported virtual disks](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1022242) in the VMware documentation\.

1. Locate and open the context \(right\-click\) menu for the newly deployed template in the vSphere client inventory tree and choose **Power**, **Power On**\.

1. Open the context \(right\-click\) menu for the template again and choose **Open Console**\. The console displays the IP address of the connector console\. Make note of the IP address as you'll need it in order to complete the connector setup process\.

## Configure the AWS Discovery Connector<a name="configure-connector"></a>

To finish the setup process, open a web browser and complete the following procedure and optional tasks within this section\.

**To configure the connector using the VMWare console**

1. In a web browser, type the following URL in the address bar:  **https://***<ip\_address>***/**, where *ip\_address* is the IP address of the connector console that you saved earlier\. 

1. Choose **Get started now** and follow the wizard steps\.

1. In **Step 5: Discovery Connector Set Up** of the wizard steps, choose **Configure vCenter credentials**:

   1. For **vCenter Host**, type the hostname or IP address of your VMware vCenter Server host\.

   1. For **vCenter Username**, type the name of a local or domain user that the connector uses to communicate with vCenter\. For domain users, use the form *domain*\\*username* or *username*@*domain*\.

   1. For **vCenter Password**, type the local or domain user password\.

   1. Choose **Ignore security certificate** to bypass SSL certificate validation with vCenter\.

1. Choose **Configure AWS credentials** and type the credentials for the IAM user who is assigned the `AWSAgentlessDiscoveryService` IAM policy that you created in [Step 3: Provide Application Discovery Service Access to Non\-Administrator Users by Attaching Policies](setting-up.md#setting-up-user-policy), and then choose **Next**\.

1. Choose **Configure where to publish data** and select suitable publishing options\. Choose **Next** and you should see the AWS Agentless Discovery Connector console\.

**Topics**
+ [Configure a static IP address for the connector](#connector_static_ip)
+ [Control the Scope of Data Collection](#data-collection-scope)
+ [Disabling Auto\-Upgrades on AWS Discovery Connector](#connector_auto_upgrade)
+ [Troubleshooting the Discovery Connector](#agentless-troubleshooting)

### Configure a static IP address for the connector<a name="connector_static_ip"></a>

This optional procedure is required if your environment requires that you use a static IP address\.

**To Configure a static IP address for the connector**

1. Open the connector's virtual machine console and log in as **ec2\-user** with the password **ec2pass**\. Supply a new password if prompted\.

1. Run the command **sudo setup\.rb** and enter the password for *ec2\-user* when prompted to display the configuration menu\.

1. Enter **2** to select **Reconfigure network settings**\. This displays current network information and a submenu for making changes to the network settings\.

1. In the submenu generated from the previous step, enter **2** to select **Set up a static IP**\. This will display a form to supply network settings:

   1. For each field, provide an appropriate value and press Enter\. You should see output similar to the following where *nnn\.nnn\.nnn\.nnn* is populated with the address numbers you entered for each field:

     ```
     Setting up static IP:
          1. Enter IP address: <nnn.nnn.nnn.nnn>
          2. Enter netmask: <nnn.nnn.nnn.nnn>
          3. Enter gateway: <nnn.nnn.nnn.nnn>
          4. Enter DNS 1: <nnn.nnn.nnn.nnn>
          5. Enter DNS 2: <nnn.nnn.nnn.nnn>
      
     Static IP address configured.
     ```

### Control the Scope of Data Collection<a name="data-collection-scope"></a>

The vCenter user requires read\-only permissions on each ESX host or VM to inventory using Application Discovery Service\. Using the permission settings, you can control which hosts and VMs are included in the data collection\. You can either allow all hosts and VMs under the current vCenter to be inventoried, or grant permissions on a case\-by\-case basis\.

**Note**  
As a security best practice, we recommend against granting additional, unneeded permissions to the vCenter user of the Discovery Connector\.

The following procedures describe configuration scenarios ordered from least granular to most granular\.

**To discover data about *all* ESX hosts and VMs under the current vCenter**

1. In your VMware vSphere client, choose **vCenter** and then choose either **Hosts and Clusters** or **VMs and Templates**\. 

1. Choose **Manage**, **Permissions**\.

1. Select the vCenter user, open the context \(right\-click\) menu, and choose **Change Role**\.

1. In the **Assigned Role** pane, choose **Read\-only**\.

1.  Choose **Propagate to children**, **OK**\.

**To discover data about a *specific* ESX host and *all* of its child objects**

1. In your VMware vSphere client, choose **vCenter** and then choose either **Hosts and Clusters** or **VMs and Templates**\. 

1. Choose **Related Objects**, **Hosts**\. 

1. Open the context \(right\-click\) menu for the host name and choose **All vCenter Actions**, **Add Permission**\.

1. Under **Add Permission**, add the vCenter user to the host\. For **Assigned Role**, choose **Read\-only**\. 

1. Choose **Propagate to children**, **OK**\.

**Discover data about a *specific* ESX host or child VM**

1. In your VMware vSphere client, choose **vCenter** and then choose either **Hosts and Clusters** or **VMs and Templates**\. 

1. Choose **Related Objects**\.

1. Choose **Hosts** \(showing a list of ESX hosts known to vCenter\) or **Virtual Machines** \(showing a list of VMs across all ESX hosts\)\. 

1. Open the context \(right\-click\) menu for the host or VM name and choose **All vCenter Actions**, **Add Permission**\.

1.  Under **Add Permission**, add the vCenter user to the host or VM\. For **Assigned Role**, choose **Read\-only**, \. 

1. Choose **OK**\. 

**Note**  
If you chose **Propagate to children**, you can still remove the read\-only permission from ESX hosts and VMs on a case\-by\-case basis\. This option has no effect on inherited permissions applying to other ESX hosts and VMs\. 

### Disabling Auto\-Upgrades on AWS Discovery Connector<a name="connector_auto_upgrade"></a>

To ensure that you are running the latest version of AWS Discovery Connector, the auto\-upgrade feature is enabled by default upon installation\. However, you may disable the auto\-upgrade feature as shown below\.

**To disable auto\-upgrades**

1. In a web browser, type the following URL in the address bar:  **https://***<ip\_address>***/**, where *ip\_address* is the IP address of the AWS Discovery Connector\.

1. In the Discovery Connector console, under **Actions**, choose **Disable Auto\-Upgrade**\.

**Warning**  
Disabling auto\-upgrades will prevent the latest security patches from being installed\.

### Troubleshooting the Discovery Connector<a name="agentless-troubleshooting"></a>
+ The Discovery Connector does not support a standalone ESX host\. The ESX host must be part of the vCenter Server instance\.
+ If you encounter problems and need help, contact [AWS Support](https://aws.amazon.com/contact-us/)\. You will be contacted and may be asked to send the connector logs\. To obtain the logs, do the following:
  + Log back in to the AWS Agentless Discovery Connector console \(as you did during [configuration](#configure-connector)\) and choose **Download log bundle**\.
  + Once the log bundle has finished downloading, send it as instructed by AWS Support\.

## Start Discovery Connector Data Collection<a name="start-connector-data-collection"></a>

Now that you have deployed and configured the Discovery Connector in your VMware environment, you must complete the final step of actually turning on its data collection process\. There are two ways to do this, through the console or by making API calls through the AWS CLI\. Instructions are provided below for both ways\.

### Start Data Collection Using the Migration Hub Console<a name="start-agentless-console"></a>

You start the Discovery Connector data collection process on the **Data Collectors** page of the Migration Hub console\.

**To start data collection**

1. In the navigation pane, choose **Data Collectors**\.

1. Choose the **Connectors** tab\.

1. Select the check box of the connector you want to start\.

1. Choose **Start data collection**\.

**Note**  
If you don’t see inventory information after starting data collection with the connector, confirm that you have registered the connector with your vCenter Server\.

### Start Data Collection Using the AWS CLI<a name="start-agentless-api"></a>

To start the Discovery Connector data collection process from the AWS CLI, the AWS CLI must first be installed in your environment\.

**To install the AWS CLI and start data collection**

1. Install the AWS CLI for your operating system \(Linux, macOS, or Windows\)\. See the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/) for instructions\.

1. Open the Command prompt \(Windows\) or Terminal \(Linux or macOS\)\.

   1. Type `aws configure` and press Enter\.

   1. Enter your AWS Access Key Id and AWS Secret Access Key\.

   1. Enter `us-west-2` for the Default Region Name\.

   1. Enter `text` for Default Output Format\.

1. Type the following command:

   ```
   aws discovery start-data-collection-by-agent-ids --agent-ids <connector ID>
   ```

   1. If you don't know the ID of the connector you want to start, enter the following command exactly as shown to see the connector's ID:

     ```
     aws discovery describe-agents --filters condition=EQUALS,name=hostName,values=connector
     ```

**Note**  
If you don’t see inventory information after starting data collection with the connector, confirm that you have registered the connector with your vCenter Server\.