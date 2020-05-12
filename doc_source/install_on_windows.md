# Agent Installation on Windows<a name="install_on_windows"></a>

Complete the following procedure to install an agent on Windows\. Be sure that your [Migration Hub home region](https://docs.aws.amazon.com/migrationhub/latest/ug/home-region.html) has been set before you begin this procedure\.<a name="windows_steps"></a>

**To install AWS Application Discovery Agent in your data center**

1. Navigate to the [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=48145) and choose **Download** to be taken to the download selection page, then on this page, select only **`vc_redist.x86.exe`** *\(do not select the "x64" version\)* regardless of the architecture of the machine you are installing on, then choose **Next**\. Your download begins immediately\.

1. Download the [Windows agent installer](https://s3.us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/windows/latest/AWSDiscoveryAgentInstaller.msi) *but do not double\-click and execute the installer within Windows*\.
**Important**  
Do not double\-click and execute the installer within Windows as it will fail to install\. *Agent installation only works from the command prompt*\. \(If you already double\-clicked on the installer, you must go to **Add/Remove Programs** and uninstall the agent before continuing on with the remaining installation steps\.\)

1. Open a command prompt as an administrator and navigate to the location where you saved the installation package\.

1. To install the agent, run the following example command\. The example uses the string `Your_Home_Region` to show where to insert the name of your home region\.

   ```
   msiexec.exe /i AWSDiscoveryAgentInstaller.msi REGION="Your_Home_Region" KEY_ID="<aws key id>" KEY_SECRET="<aws key secret>" /q
   ```
**Note**  
Agents automatically download and apply updates as they become available\. We recommend this default configuration\. To avoid downloading agents and applying updates automatically, include the following parameter when running the installation:  
`AUTO_UPDATE=false`
**Warning**  
Disabling auto\-upgrades will prevent the latest security patches from being installed\.

1. If outbound connections from your network are restricted, you'll need to update your firewall settings\. Agents require access to `arsenal` over TCP port 443\. They don't require any inbound ports to be open\.
   + For example, if your home region is `eu-central-1`, you'd use `https://arsenal-discovery.eu-central-1.amazonaws.com:443`
   + Or substitute your home region as needed for all other regions except us\-west\-2\.
   + If `us-west-2` is your home region, use `https://arsenal.us-west-2.amazonaws.com:443`
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

## Package Signing on Windows 2003<a name="win2003"></a>

For Windows Server 2008 and later, Amazon cryptographically signs the Application Discovery Service agent installation package with an SHA256 certificate\. However, because the SHA2 certificate family is not supported by Windows Server 2003, the installation package for that platform is signed with an SHA1 certificate\. Microsoft has published [hotfixes](https://blogs.technet.microsoft.com/pki/2010/09/30/sha2-and-windows/) that might allow your Windows 2003 systems to read an SHA256 certificate\. If you require SHA256 in your Windows 2003 environment, contact AWS Support for assistance\. 

## Manage the Discovery Agent Process on Windows<a name="using_on_windows"></a>

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

## Agent Troubleshooting on Windows<a name="windows_troubleshooting"></a>

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
+ For instructions on how to remove older versions of the Discovery Agent, see [Prerequisites for Agent Installation](gen-prep-agents.md)\.