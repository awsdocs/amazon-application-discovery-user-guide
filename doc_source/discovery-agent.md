# AWS Application Discovery Agent<a name="discovery-agent"></a>

The AWS Discovery Agent is AWS software that you install on on\-premises servers and VMs targeted for discovery and migration\. Agents capture system configuration, system performance, running processes, and details of the network connections between systems\. Agents support most Linux and Windows operating systems, and you can deploy them on physical on\-premises servers, Amazon EC2 instances, and virtual machines\. 

The Discovery Agent runs in your local environment and requires root privileges\. When you start the Discovery Agent, it connects securely with `arsenal.us-west-2.amazonaws.com` and registers with Application Discovery Service\. Then it pings the service at 15 minute intervals for configuration information\. When you send a command that tells an agent to start data collection, it starts collecting data for the host or VM where it resides\. Collection includes system specifications, times series utilization or performance data, network connections, and process data\. You can use this information to map your IT assets and their network dependencies\. All of these data points can help you determine the cost of running these servers in AWS and also plan for migration\.

Data is transmitted securely by the Discovery Agents to Application Discovery Service using Transport Layer Security \(TLS\) encryption\. Agents are configured to upgrade automatically when new versions become available\. You can change this configuration setting if desired\.

**Tip**  
Before downloading and beginning Discovery Agent installation, be sure to read through all of the required prerequisites in [Prerequisites for Agent Installation](#gen-prep-agents)

**Topics**
+ [Data Collected by the Discovery Agent](#agent-data-collected)
+ [Prerequisites for Agent Installation](#gen-prep-agents)
+ [Agent Installation on Linux](#install_on_linux)
+ [Agent Installation on Windows](#install_on_windows)
+ [Start Discovery Agent Data Collection](#start-agent-data-collection)

## Data Collected by the Discovery Agent<a name="agent-data-collected"></a>

Following, you can find an inventory of the information collected by the Discovery Agent\.

**Table legend for Discovery Agent collected data:**
+ The term host refers to either a physical server or a VM\.
+ Collected data is in measurements of kilobytes \(KB\) unless stated otherwise\.
+ Equivalent data in the Migration Hub console is reported in megabytes \(MB\)\.
+ Data fields denoted with an asterisk \(\*\) are only available in the \.csv files produced from the agent's API export function\.
+ The polling period is in intervals of approximately 15 minutes\.


| Data field | Description | 
| --- | --- | 
| agentAssignedProcessId\* | Unique process ID of agent | 
| agentId | Unique ID of agent | 
| agentProvidedTimeStamp\* | Date and time of agent observation \(mm/dd/yyyy hh:mm:ss am/pm\) | 
| cmdLine\* | Process entered at the command line | 
| cpuType  | Type of CPU \(central processing unit\) used in host | 
| destinationIp\* | IP address of device to which packet is being sent | 
| destinationPort\* | Port number to which the data/request is to be sent | 
| family\* | Protocol of routing family | 
| freeRAM \(MB\)  | Free RAM and cached RAM that can be made immediately available to applications, measured in MB | 
| gateway\* | Node address of network | 
| hostName | Name of host data was collected on | 
| hypervisor | Type of hypervisor | 
| ipAddress | IP address of the host | 
| ipVersion\* | IP version number | 
| isSystem\* | Boolean attribute to indicate if a process is owned by the OS | 
| macAddress  | MAC address of the host | 
| name\* | Name of the host, network, metrics, etc\. data is being collected for | 
| netMask\* | IP address prefix that a network host belongs to | 
| osName  | Operating system name on host | 
| osVersion | Operating system version on host | 
| path | Path of the command sourced from the command line | 
| sourceIp\* | IP address of the device sending the IP packet  | 
| sourcePort\* | Port number from which the data/request originates from | 
| timestamp\* | Date and time of reported attribute logged by agent | 
| totalCpuUsagePct  | Percentage of CPU usage on host during polling period | 
| totalDiskBytesReadPerSecond \(Kbps\) | Total amount of disk free space on host | 
| totalDiskBytesWrittenPerSecond \(Kbps\) | Total size of disk on host  | 
| totalDiskFreeSize \(GB\) | Free disk space expressed in GB | 
| totalDiskReadOpsPerSecond | Total number of read I/O operations per second | 
| totalDiskSize \(GB\) | Total capacity of disk expressed in GB | 
| totalDiskWriteOpsPerSecond | Total number of write I/O operations per second | 
| totalNetworkBytesReadPerSecond \(Kbps\) | Total amount of throughput of bytes read per second | 
| totalNetworkBytesWrittenPerSecond \(Kbps\) | Total amount of throughput of bytes written per second | 
| totalNumCores | Total number of independent processing units within CPU | 
| totalNumCpus | Total number of central processing units | 
| totalNumDisks | The number of physical hard disks on a host | 
| totalNumLogicalProcessors\* | Total number of physical cores times the number of threads that can run on each core | 
| totalNumNetworkCards | Total count of network cards on server | 
| totalRAM \(MB\) | Total amount of RAM available on host | 
| transportProtocol\* | Type of transport protocol used | 

## Prerequisites for Agent Installation<a name="gen-prep-agents"></a>

 These are the pre\-installation tasks that should be performed to prevent errors from occurring during the actual installation of the agent\. If you have a *1\.x* version of the agent installed, it needs to be removed before installing the latest version\. Instructions for removing older versions are provided in the tasks below:
+ Verify your OS environment is supported:
  + **Linux**
    + Amazon Linux 2012\.03, 2015\.03
    + Ubuntu 12\.04, 14\.04, 16\.04
    + Red Hat Enterprise Linux 5\.11, 6\.9, 7\.3
    + CentOS 5\.11, 6\.9, 7\.3
    + SUSE 11 SP4, 12 SP2
  + **Windows**
    + Windows Server 2003 R2 SP2
    + Windows Server 2008 R1 SP2, 2008 R2 SP1
    + Windows Server 2012 R1, 2012 R2
    + Windows Server 2016
+ If outbound connections from your network are restricted, you'll need to update your firewall settings\. Agents require access to `arsenal` over TCP port 443 as in `https://arsenal.us-west-2.amazonaws.com:443`\. They don't require any inbound ports to be open\.
+ Access to AWS S3 in us\-west\-2 is required for auto\-upgrade to function\.
+ Create an IAM user in the IAM console and attach the existing `AWSApplicationDiscoveryAgentAccess` permissions policy\. This will allow the user to perform the necessary agent actions on your behalf\. See [Step 3: Provide Application Discovery Service Access to Non\-Administrator Users by Attaching Policies](setting-up.md#setting-up-user-policy) for Discovery Agent installation prerequisites\.
+ Check the time skew from your Network Time Protocol \(NTP\) servers and correct if necessary\. Incorrect time skew causes the agent registration call to fail\.
+ Remove any previous\-generation agents\. If you previously installed Application Discovery Agent 1\.0 for either Windows or Linux, you must uninstall it before continuing with the installation of the current agent\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/application-discovery/latest/userguide/discovery-agent.html)

**Note**  
The Discovery Agent has a 32\-bit agent executable, which works on both 32\-bit and 64\-bit operating systems\. Having a single executable reduces the number of installation packages needed for deployment\. This applies for both Linux and Windows OS and is addressed in their respective installation sections below\.

## Agent Installation on Linux<a name="install_on_linux"></a>

Complete the following procedure on Linux\.

**Note**  
If you are using a non\-current Linux version, see [Requirements on Older Linux Platforms](#old_linux)\.<a name="linux_steps"></a>

**To install AWS Application Discovery Agent in your data center**

1. Log in to your Linux\-based server or VM and create a new directory to contain your agent components\.

1. Switch to the new directory and download the installation script from either the command line or the console\.

   1. To download from the command line, run the following command\.

      ```
      curl -o ./aws-discovery-agent.tar.gz https://s3-us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/linux/latest/aws-discovery-agent.tar.gz
      ```

   1. To download from the Migration Hub console, do the following: 

      1. Open the console and go to the [Discovery Tools](https://us-west-2.console.aws.amazon.com/migrationhub/discover/tools/options) page\.

      1. In the **Discovery Agent** box, choose **Download agent**, then choose **Linux** in the resultant list box\. Your download begins immediately\.

1. Verify the cryptographic signature of the installation package with the following three commands:

   ```
   curl -o ./agent.sig https://s3-us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/linux/latest/aws-discovery-agent.tar.gz.sig
   ```

   ```
   curl -o ./discovery.gpg https://s3-us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/linux/latest/discovery.gpg
   ```

   ```
   gpg --no-default-keyring --keyring ./discovery.gpg --verify agent.sig aws-discovery-agent.tar.gz
   ```

   The agent public key \(`discovery.gpg`\) fingerprint is `7638 F24C 6717 F97C 4F1B 3BC0 5133 255E 4DF4 2DA2`\.

1. Extract from the tarball as shown following\.

   ```
   tar -xzf aws-discovery-agent.tar.gz
   ```

1. Run the following command to install the agent in the `us-west-2` Region\.

   ```
   sudo bash install -r us-west-2 -k <aws key id> -s <aws key secret>
   ```
**Note**  
Agents automatically download and apply updates as they become available\. We recommend using this default configuration\. However, if you don't want agents to download and apply updates automatically, include the `-u false` parameter when running the installation script\.

1. If outbound connections from your network are restricted, update your firewall settings\. Agents require access to `arsenal` over TCP port 443 as in `us-west-2.amazonaws.com:443` They don't require any inbound ports to be open\.
**Note**  
Agents also work with transparent web proxies\. However, if you need to **configure a non\-transparent proxy**, proceed to the next step\.

1. Optional: To Configure a Non\-Transparent Proxy:

   1. Find the configuration file as described in [Agent Troubleshooting on Linux](#linux_troubleshooting) and edit the file by adding the required configuration data as follows:

      ```
      "proxyHost" : "<myproxy.mycompany.com>",
      "proxyPort" : <1234>,
      "proxyUser" : "<myusername>",
      "proxyPassword" : "<mypassword>",
      ```

   1. Save the edited configuration file ensuring that you still have valid json \(taking care with the quotes and the commas\)\. If your proxy doesn't require authentication, then leave out *proxyUser* and *proxyPassword*\. While most proxies use http, if yours uses https, specify the following in the configuration file:

      ```
      "proxyScheme" : "https"
      ```

   1. Restart the agent\. 
**Note**  
If you encounter problems, add the following to the configuration file:  

      ```
      "enableAWSSDKLogging" : true
      ```
Then, restart the agent again, let it run for at least 15 minutes, and contact [AWS Support](https://aws.amazon.com/contact-us/)\. They will help you troubleshoot and may ask you to send them the generated log files which can be found as described in [Agent Troubleshooting on Linux](#linux_troubleshooting)\.

**Topics**
+ [Requirements on Older Linux Platforms](#old_linux)
+ [Manage the Discovery Agent Process on Linux](#using_on_linux)
+ [Agent Troubleshooting on Linux](#linux_troubleshooting)

### Requirements on Older Linux Platforms<a name="old_linux"></a>

Some older Linux platforms such as SUSE 10, CentOS 5, and RHEL 5 are either at end of life or only minimally supported\. These platforms can suffer from out\-of\-date cipher suites that prevent the agent installation script from downloading installation packages\. They might also have a limited ability to find and download the platform libraries required by the agent from deprecated Linux repositories\. 

**32\-bit `libc`**  
One of the dependencies needed for the Application Discovery agent is 32\-bit `libc`\. This library must be installed on 64\-bit systems that run the agent\. If the installation script exits because it fails to find a suitable repository or otherwise fails to install 32\-bit `libc`, you must manually find and install 32\-bit `libc` before you can complete agent installation\. Because 32\-bit `libc` is a core Linux library, you must take great care in identifying a package that is compatible with your system\. We recommend contacting AWS Support for assistance\. After 32\-bit `libc` is installed, run the installation script with the `-p false` parameter to skip the automated search of Linux repositories for prerequisites\.

**Curl**  
The Application Discovery agent requires `curl` for secure communications with the AWS server\. Some old versions of `curl` are not able to communicate securely with a modern web service\. To use the version of `curl` included with the Application Discovery agent for all operations, run the installation script with the `-c true` parameter\. 

**Certificate Authority Bundle**  
Older Linux systems might have an out\-of\-date Certificate Authority \(CA\) bundle, which is critical to secure internet communication\. To use the CA bundle included with the Application Discovery agent for all operations, run the installation script with the `-b true` parameter\.

These three installation script options can be used in any combination\. In the following example command, all three have been passed to the installation script: 

```
sudo bash install -r us-west-2 -k <aws key id> -s <aws key secret> -p false -c true -b true
```

 

### Manage the Discovery Agent Process on Linux<a name="using_on_linux"></a>

You can manage the behavior of the Discovery Agent at the system level using the `systemd`, `Upstart`, or `System V init` tools\. The following tabs outline the commands for the supported tasks in each of the respective tools\.

------
#### [ systemd ]


**Management Commands for the Application Discovery Agent**  

| Task | Command | 
| --- | --- | 
| Verify that an agent is running |  `sudo systemctl status aws-discovery-daemon.service`   | 
| Start an agent |  `sudo systemctl start aws-discovery-daemon.service`   | 
| Stop an agent |  `sudo systemctl stop aws-discovery-daemon.service`   | 
| Restart an agent |  `sudo systemctl restart aws-discovery-daemon.service`   | 
| Uninstall an agent |  `yum remove aws-discovery-agent`   | 

------
#### [ Upstart ]


**Management Commands for the Application Discovery Agent**  

| Task | Command | 
| --- | --- | 
| Verify that an agent is running |  `sudo initctl status aws-discovery-daemon`   | 
| Start an agent |  `sudo initctl start aws-discovery-daemon`   | 
| Stop an agent |  `sudo initctl stop aws-discovery-daemon`   | 
| Restart an agent |  `sudo initctl restart aws-discovery-daemon`   | 
| Uninstall an agent |  `apt-get remove aws-discovery-agent`   | 

------
#### [ System V init ]


**Management Commands for the Application Discovery Agent**  

| Task | Command | 
| --- | --- | 
| Verify that an agent is running |  `sudo /etc/init.d/aws-discovery-daemon status`   | 
| Start an agent |  `sudo /etc/init.d/aws-discovery-daemon start`   | 
| Stop an agent |  `sudo /etc/init.d/aws-discovery-daemon stop`   | 
| Restart an agent |  `sudo /etc/init.d/aws-discovery-daemon restart`   | 
| Uninstall an agent |  `zypper remove aws-discovery-agent`   | 

------

### Agent Troubleshooting on Linux<a name="linux_troubleshooting"></a>

If you encounter problems while installing or using the Application Discovery Agent on Linux, consult the following guidance about logging and configuration\. When helping to troubleshoot potential issues with the agent or its connection to the Application Discovery Service, AWS Support often requests these files\. 
+ **Log files**

  Agent log files can be found under the following directory\. 

  ```
  /var/log/aws/discovery/
  ```

  Log files are named to indicate whether they are generated by the main daemon, the automatic upgrader, or installer\.

   
+ **Configuration files**

  Agent configuration files can be found under the following directory\.

  ```
  /var/opt/aws/discovery/
  ```
+ For instructions on how to remove older versions of the Discovery Agent, see [Prerequisites for Agent Installation](#gen-prep-agents)\.

## Agent Installation on Windows<a name="install_on_windows"></a>

Complete the following procedure on Windows\.<a name="windows_steps"></a>

**To install AWS Application Discovery Agent in your data center**

1. Navigate to the [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=48145) and choose **Download** to be taken to the download selection page, then on this page, select only **`vc_redist.x86.exe`** *\(do not select the "x64" version\)* regardless of the architecture of the machine you are installing on, then choose **Next**\. Your download begins immediately\.

1. Download the [Windows agent installer](https://s3-us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/windows/latest/AWSDiscoveryAgentInstaller.msi) *but do not double\-click and execute the installer within Windows*\.
**Important**  
Do not double\-click and execute the installer within Windows as it will fail to install\. *Agent installation only works from the command prompt*\. \(If you already double\-clicked on the installer, you must go to **Add/Remove Programs** and uninstall the agent before continuing on with the remaining installation steps\.\)

1. Open a command prompt as an administrator and navigate to the location where you saved the installation package\.

1. To install the agent, run the following command\.

   ```
   msiexec.exe /i AWSDiscoveryAgentInstaller.msi REGION="us-west-2" KEY_ID="<aws key id>" KEY_SECRET="<aws key secret>" /q
   ```
**Note**  
Agents automatically download and apply updates as they become available\. We recommend this default configuration\. To avoid downloading agents and applying updates automatically, include the following parameter when running the installation:  
`AUTO_UPDATE=false`
**Warning**  
Disabling auto\-upgrades will prevent the latest security patches from being installed\.

1. If outbound connections from your network are restricted, update your firewall settings\. Agents require access to `arsenal` over TCP port 443 as in `us-west-2.amazonaws.com:443`\. They do not require any inbound ports to be open\.
**Note**  
Agents also work with transparent web proxies\. However, if you need to **configure a non\-transparent proxy**, continue on with the following steps\.

1. Optional: To Configure a Non\-Transparent Proxy:

   1. Find the configuration file as described in [Agent Troubleshooting on Windows](#windows_troubleshooting), and edit the file by adding the required configuration data as follows:

      ```
      "proxyHost" : "<myproxy.mycompany.com>",
      "proxyPort" : <1234>,
      "proxyUser" : "<myusername>",
      "proxyPassword" : "<mypassword>",
      ```

   1. Save the edited configuration file ensuring that you still have valid json \(taking care with the quotes and the commas\)\. If your proxy doesn't require authentication, then leave out *proxyUser* and *proxyPassword*\. While most proxies use http, if yours uses https, specify the following in the configuration file:

      ```
      "proxyScheme" : "https"
      ```

   1. Restart the agent\. 
**Note**  
If you encounter problems, add the following to the configuration file:  

      ```
      "enableAWSSDKLogging" : true
      ```
Then, restart the agent again, let it run for 15 minutes, and troubleshoot what the issue may be by reading through the generated log files which can be found as described in [Agent Troubleshooting on Windows](#windows_troubleshooting)\.

**Topics**
+ [Package Signing on Windows 2003](#win2003)
+ [Manage the Discovery Agent Process on Windows](#using_on_windows)
+ [Agent Troubleshooting on Windows](#windows_troubleshooting)

### Package Signing on Windows 2003<a name="win2003"></a>

For Windows Server 2008 and later, Amazon cryptographically signs the Application Discovery Service agent installation package with an SHA256 certificate\. However, because the SHA2 certificate family is not supported by Windows Server 2003, the installation package for that platform is signed with an SHA1 certificate\. Microsoft has published [hotfixes](https://blogs.technet.microsoft.com/pki/2010/09/30/sha2-and-windows/) that might allow your Windows 2003 systems to read an SHA256 certificate\. If you require SHA256 in your Windows 2003 environment, contact AWS Support for assistance\. 

### Manage the Discovery Agent Process on Windows<a name="using_on_windows"></a>

You can manage the behavior of the Discovery Agent at the system level through the Windows Server Manager Services console\. The following table describes how\.


| Task | Service Name | Service Status/Action | 
| --- | --- | --- | 
| Verify that an agent is running | AWS Discovery Agent AWS Discovery Updater | Started | 
| Start an agent | AWS Discovery Agent AWS Discovery Updater | Choose Start | 
| Stop an agent | AWS Discovery Agent AWS Discovery Updater | Choose Stop | 
| Restart an agent | AWS Discovery Agent AWS Discovery Updater | Choose Restart | 

**To uninstall a discovery agent on Windows**

1. Open Control Panel in Windows\.

1. Choose **Programs**\.

1. Choose **Programs and Features**\.

1. Select **AWS Discovery Agent**\.

1. Choose **Uninstall**\.

### Agent Troubleshooting on Windows<a name="windows_troubleshooting"></a>

If you encounter problems while installing or using the Application Discovery Agent on Windows, consult the following guidance about logging and configuration\. When helping to troubleshoot potential issues with the agent or its connection to the Application Discovery Service, AWS Support often requests these files\. 
+ **Installation logging** 

  In some cases, the msiexec command described preceding appears to fail\. For example, a failure can appear with the Windows Services Manager showing that the discovery services are not being created\. In this case, add /L\*V install\.log to the command to generate a verbose installation log\.

   
+  **Operational logging** 

  On Windows Server 2008 and later, agent log files can be found under the following directory\.

  ```
  C:\ProgramData\AWS\AWS Discovery\Logs
  ```

  On Windows Server 2003, agent log files can be found under the following directory\.

  ```
  C:\Documents and Settings\All Users\Application Data\AWS\AWSDiscovery\Logs
  ```

  Logs files are named to indicate whether generated by the main service, automatic upgrader, or installer\.

   
+ **Configuration file**

  On Windows Server 2008 and later, the agent configuration file can be found at the following location\.

  ```
  C:\ProgramData\AWS\AWS Discovery\config
  ```

  On Windows Server 2003, the agent configuration file can be found at the following location\.

  ```
  C:\Documents and Settings\All Users\Application Data\AWS\AWS Discovery\config
  ```
+ For instructions on how to remove older versions of the Discovery Agent, see [Prerequisites for Agent Installation](#gen-prep-agents)\.

## Start Discovery Agent Data Collection<a name="start-agent-data-collection"></a>

Now that you have deployed and configured the Discovery Agent, you must complete the final step of actually turning on its data collection process\. There are two ways to do this, through the console or by making API calls through the AWS CLI\. Instructions are provided below for both ways by expanding your method of choice:

### Start Data Collection Using the Migration Hub Console<a name="start-agent-console"></a>

You start the Discovery Agent data collection process on the **Data Collectors** page of the Migration Hub console\.

**To start data collection**

1. In the navigation pane, choose **Data Collectors**\.

1. Choose the **Agents** tab\.

1. Select the check box of the agent you want to start\.
**Tip**  
If you installed multiple agents but only want to start data collection on certain hosts, the **Hostname** column in the agent's row identifies the host the agent is installed on\.

1. Choose **Start data collection**\.

### Start Data Collection Using the AWS CLI<a name="start-agent-api"></a>

To start the Discovery Agent data collection process from the AWS CLI the AWS CLI must first be installed in your environment\.

**To install the AWS CLI and start data collection**

1. If you have not already done so, install the AWS CLI appropriate to your OS type \(Windows or Mac/Linux\)\. See the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/) for instructions\.

1. Open the Command prompt \(Windows\) or Terminal \(MAC/Linux\)\.

   1. Type `aws configure` and press Enter\.

   1. Enter your AWS Access Key Id and AWS Secret Access Key\.

   1. Enter `us-west-2` for the Default Region Name\.

   1. Enter `text` for Default Output Format\.

1. Type the following command:

   ```
   aws discovery start-data-collection-by-agent-ids --agent-ids <agent ID>
   ```

   1. If you don't know the ID of the agent you want to start, enter the following command to see the agent's ID:

     ```
     aws discovery describe-agents
     ```