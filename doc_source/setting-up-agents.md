# Setting up AWS Application Discovery Agent<a name="setting-up-agents"></a>

AWS Application Discovery Agent is installed on on\-premises servers and virtual machines \(VMs\) that you target for discovery and migration\. Agents can run on Linux and Windows servers and collect information including system configuration, system performance, running processes, and details of the network connections between systems\. Agents send this data to Application Discovery Service using Secure Sockets Layer \(SSL\) encryption\. 

We recommend installing Application Discovery Agent on a small number of servers to confirm installation procedures and connectivity to Application Discovery Service\. After it is running and connected, you can use the Application Discovery Service console to enable agent data collection\. This instructs the agent to collect capacity and utilization information, which should start appearing in the console several minutes later\.

**Note**  
Application Discovery Service has released a new 2\.0 version of the Application Discovery agent offering better OS support\. The 1\.0 version of the agent has been deprecated and is no longer recommended for new installations\. For additional questions on 1\.0 version, please reach out to [ADS Support](https://aws.amazon.com/support)\.


+ [Preparing for Agent Installation](#prep_linux)
+ [Agent Installation on Linux](#install_on_linux)
+ [Agent Installation on Windows](#install_on_windows)
+ [Frequently Asked Questions](#agent_faq)

## Preparing for Agent Installation<a name="prep_linux"></a>

Before starting the installation, complete the following tasks\.

+ **Create an IAM user with a policy providing agent access to Application Discovery Service\.** For information, see [Create an IAM User](before_you_install.md#appdisc-iam-user)\.

+ Check the time skew from your NTP servers and correct if necessary\. Incorrect time skew causes the agent registration call to fail\.

+ **Remove previous\-generation agents\.** If you previously installed Application Discovery Agent 1\.0 for either Windows or Linux, you must uninstall it before continuing with the installation of the current agent\.  
**Commands to uninstall a previous\-generation Application Discovery Agent 1\.0**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/application-discovery/latest/userguide/setting-up-agents.html)

## Agent Installation on Linux<a name="install_on_linux"></a>

Complete the following procedure on Linux\.

**Note**  
If you are using a noncurrent Linux version, see [Requirements on Older Linux Platforms](#old_linux)\.<a name="linux_steps"></a>

**To install AWS Application Discovery Agent in your data center**

1. Log in to your Linux\-based server or VM and download the installation script\.

   ```
   curl -o ./agent.tar.gz https://s3-us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/linux/latest/aws-discovery-agent.tar.gz
   ```

1. Verify the cryptographic signature of the installation package, and extract from the tarball:

   ```
   curl -o ./agent.sig https://s3-us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/linux/latest/aws-discovery-agent.tar.gz.sig
   curl -o ./discovery.gpg https://s3-us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/linux/latest/discovery.gpg 
   gpg --no-default-keyring --keyring ./discovery.gpg --verify agent.sig agent.tar.gz
   tar -xzf agent.tar.gz
   ```

   The ADS agent public key \(`discovery.gpg`\) fingerprint is `7638 F24C 6717 F97C 4F1B 3BC0 5133 255E 4DF4 2DA2`\.

1. Run the following command to install the agent in the `us-west-2` region:

   ```
   sudo bash install -r us-west-2 -k <aws key id> -s <aws key secret>
   ```
**Note**  
Agents automatically download and apply updates as they become available\. We recommend using this default configuration\. However, if you don't want agents to download and apply updates automatically, include the `-u false` parameter when running the installation script\.

1. Optionally, use the following command to remove the agent installation script after it completes:

   ```
   rm install
   ```

1. If outbound connections from your network are restricted, update your firewall settings\. Agents require access to `arsenal.<region>.amazonaws.com:443`\. They do not require any inbound ports to be open\. Agents also work with transparent web proxies\.

### Requirements on Older Linux Platforms<a name="old_linux"></a>

Some older Linux platforms such as SUSE 10, CentOS 5, and RHEL 5 are either at end\-of\-life or only minimally supported, underpaid, extended\-support agreements\. These platforms may suffer from out\-of\-date cipher suites that prevent the Application Discovery agent installation script from downloading installation packages, and they may have a limited ability to find and download platform libraries required by the agent from deprecated Linux repositories\. 

**32\-bit `libc`**  
One of the dependencies needed for the Application Discovery agent is 32\-bit `libc`\. This library must be installed on 64\-bit systems that run the agent\. If the installation script exits because it fails to find a suitable repository or otherwise fails to install 32\-bit `libc`, you must manually find and install 32\-bit `libc` before you can complete agent installation\. Because 32\-bit `libc` is a core Linux library, you must take great care in identifying a package that is compatible with your system\. We recommend contacting AWS Support for assistance\. After 32\-bit `libc` is installed, run the installation script with the `-p false` parameter to skip the automated search of Linux repositories for prerequisites\.

**Curl**  
The Application Discovery agent requires `curl` for secure communications with the AWS server\. Some old versions of `curl` are not able to communicate securely with a modern web service\. To use the version of `curl` included with the Application Discovery agent for all operations, run the installation script with the `-c true` parameter\. 

**Certificate Authority Bundle**  
Older Linux systems may have an out\-of\-date Certificate Authority \(CA\) bundle, which is critical to secure internet communication\. To use the CA bundle included with the Application Discovery agent for all operations, run the installation script with the `-b true` parameter\.

These three installation script options can be used in any combination\. In the following example command, all three have been passed to the installation script: 

```
sudo bash install -r us-west-2 -k <aws key id> -s <aws key secret> -p false -c true -b true
```

### Using Application Discovery Agent on Linux<a name="using_on_linux"></a>

After you install, you can use the Application Discovery Service API to programmatically manage agents, tag and query configuration items, and export data\. You can export data as a CSV file to an Amazon S3 bucket or an application that enables you to view and evaluate the data\. For more information, see the [Application Discovery Service API Reference](http://docs.aws.amazon.com/application-discovery/latest/APIReference/)\.

You can manage the behavior of Application Discovery Agent at the system level using the following commands\.


**Linux Commands for Application Discovery Agent**  

| Task | Command \(Depending on Linux distribution\) | 
| --- | --- | 
| Verify that an agent is running |  sudo systemctl status aws\-discovery\-daemon\.service sudo initctl status aws\-discovery\-daemon sudo /etc/init\.d/aws\-discovery\-daemon status  | 
| Start an agent |  sudo systemctl start aws\-discovery\-daemon\.service sudo initctl start aws\-discovery\-daemon sudo /etc/init\.d/aws\-discovery\-daemon start  | 
| Stop an agent |  sudo systemctl stop aws\-discovery\-daemon\.service sudo initctl stop aws\-discovery\-daemon sudo /etc/init\.d/aws\-discovery\-daemon stop  | 
| Restart an agent |  sudo systemctl restart aws\-discovery\-daemon\.service sudo initctl restart aws\-discovery\-daemon sudo /etc/init\.d/aws\-discovery\-daemon restart  | 
| Uninstall an agent from Amazon Linux, Red Hat Enterprise Linux, or CentOS | yum remove aws\-discovery\-agent | 
| Uninstall an agent from Ubuntu Server | apt\-get remove aws\-discovery\-agent | 
| Uninstall an agent from SUSE Server | zypper remove aws\-discovery\-agent | 

### Agent Troubleshooting on Linux<a name="linux_troubleshooting"></a>

If you encounter problems while installing or using the Application Discovery Agent on Linux, consult the following guidance about logging and configuration\. When helping to troubleshoot potential issues with the agent or its connection to the Application Discovery Service, AWS Support often requests these files\. 

+ **Log files**

  Agent log files can be found under: 

  ```
  /var/log/aws/discovery/
  ```

  Logs files are named to indicate whether they are generated by the main daemon, the automatic upgrader, or installer\.

+ **Configuration files**

  Agent configuration files can be found under:

  ```
  /var/opt/aws/discovery/
  ```

## Agent Installation on Windows<a name="install_on_windows"></a>

Complete the following procedure on Windows\.<a name="windows_steps"></a>

**To install AWS Application Discovery Agent in your data center**

1. Download and install [Microsoft Visual C\+\+ Runtime for x86 ](https://www.microsoft.com/en-us/download/details.aspx?id=48145)\. 

   Install the x86 version \(`vc_redist.x86.exe`, not `vc_redist.x64.exe`\) of the C\+\+ runtime regardless of the architecture of the machine you are installing on\. 

1. Download the [Windows agent installer](https://s3-us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/windows/latest/AWSDiscoveryAgentInstaller.msi)\. 

1. Open a command prompt as an administrator and navigate to the location where you saved the installation package\.

1. To install the agent, run the following command:

   ```
   msiexec.exe /i AWSDiscoveryAgentInstaller.msi REGION="us-west-2" KEY_ID="<aws key id>" KEY_SECRET="<aws key secret>" /q
   ```
**Note**  
Agents automatically download and apply updates as they become available\. We recommend this default configuration\. To avoid downloading agents and applying updates automatically, include the following parameter when running the installation:  
`AUTO_UPDATE=false`

1. If outbound connections from your network are restricted, update your firewall settings\. Agents require access to `arsenal.<region>.amazonaws.com:443`\. They do not require any inbound ports to be open\. Agents also work with transparent web proxies\.

### Package Signing on Windows 2003<a name="win2003"></a>

For Windows Server 2008 and later, Amazon cryptographically signs the Application Discovery Service agent installation package with an SHA256 certificate\. However, because the SHA2 certificate family is not supported by Windows Server 2003, the installation package for that platform is signed with an SHA1 certificate\. Microsoft has published [hotfixes](https://blogs.technet.microsoft.com/pki/2010/09/30/sha2-and-windows/) that *may* allow your Windows 2003 systems to read an SHA256 certificate\. If you require SHA256 in your Windows 2003 environment, contact AWS Support for assistance\. 

### Using Application Discovery Agent on Windows<a name="using_on_windows"></a>

To start or stop the Application Discovery Agent, use Windows Services Manager to start or stop the **AWS Discovery Agent** and **AWS Discovery Updater** services\. To uninstall, use **Add/Remove Programs** to remove these services\.

### Agent Troubleshooting on Windows<a name="windows_troubleshooting"></a>

If you encounter problems while installing or using the Application Discovery Agent on Windows, consult the following guidance about logging and configuration\. When helping to troubleshoot potential issues with the agent or its connection to the Application Discovery Service, AWS Support often requests these files\. 

+ **Installation logging** 

  If the msiexec command described above appears to fail—for example, with the Windows Services Manager showing that the discovery services are not being created—add /L\*V install\.log to the command to generate a verbose installation log\.

+  **Operational logging** 

  On Windows Server 2008 and later, agent log files can be found under:

  ```
  C:\ProgramData\AWS\AWS Discovery\Logs
  ```

  On Windows Server 2003, agent log files can be found under:

  ```
  C:\Documents and Settings\All Users\Application Data\AWS\AWSDiscovery\Logs
  ```

  Logs files are named to indicate whether generated by the main service, automatic upgrader, or installer\.

+ **Configuration file**

  On Windows Server 2008 and later, the agent configuration file can be found at:

  ```
  C:\ProgramData\AWS\AWS Discovery\config
  ```

  On Windows Server 2003, the agent configuration file can be found at:

  ```
  C:\Documents and Settings\All Users\Application Data\AWS\AWS Discovery\config
  ```

## Frequently Asked Questions<a name="agent_faq"></a>

This topic addresses common questions about the Application Discovery agent\.

**Why doesn't the Application Discovery agent have a 64\-bit version?**  
Providing a 32\-bit agent executable that works on both 32\- and 64\-bit operating systems reduces the number of installation packages that our customers need to qualify for deployment\. Additionally, on 64\-bit operating systems, memory use is reduced\.

**How does the Application Discovery agent ensure that communication with Application Discovery Service is secure?**  
All connections are outbound from the agent\. On all host platforms, the agent enforces secure communications over TLS using up\-to\-date encryption libraries\.

**How does the agent's automatic update process work, and is it secure?**  
Updates are initiated by the agent using TLS\. The agent retrieves information about the latest available agent release from the Application Discovery agent repository, which is securely hosted on Amazon S3\. Before the agent installs a new package, it verifies that the package has a valid digital signature from Amazon\.