# Install on Windows<a name="install_on_windows"></a>

Complete the following procedure to install an agent on Windows\. Be sure that your [Migration Hub home region](https://docs.aws.amazon.com/migrationhub/latest/ug/home-region.html) has been set before you begin this procedure\.<a name="windows_steps"></a>

**To install AWS Application Discovery Agent in your data center**

1. Download the [Windows agent installer](https://s3.us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/windows/latest/AWSDiscoveryAgentInstaller.exe) *but do not double\-click to run the installer within Windows*\.
**Important**  
Do not double\-click to run the installer within Windows as it will fail to install\. *Agent installation only works from the command prompt*\. \(If you already double\-clicked on the installer, you must go to **Add/Remove Programs** and uninstall the agent before continuing on with the remaining installation steps\.\)   
If the Windows agent installer doesn't detect any version of the Visual C\+\+ x86 runtime on the host, it automatically installs the Visual C\+\+ x86 2015–2019 runtime before installing the agent software\.

1. Open a command prompt as an administrator and navigate to the location where you saved the installation package\.

1. To install the agent, choose one of the following installation methods\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/application-discovery/latest/userguide/install_on_windows.html)

1. If outbound connections from your network are restricted, you must update your firewall settings\. Agents require access to `arsenal` over TCP port 443\. They don't require any inbound ports to be open\.

   For example, if your home region is `eu-central-1`, you'd use the following: `https://arsenal-discovery.eu-central-1.amazonaws.com:443`

## Package signing and automatic upgrades<a name="win2003"></a>

For Windows Server 2008 and later, Amazon cryptographically signs the Application Discovery Service agent installation package with an SHA256 certificate\. For SHA2\-signed autoupdates on Windows Server 2008 SP2, ensure that hosts have a hotfix installed to support SHA2 signature authentication\. Microsoft's latest support [hotfix](https://support.microsoft.com/en-us/topic/update-to-add-sha-2-code-signing-support-for-windows-server-2008-sp2-f120e4d0-da06-6860-3610-59c5cd0b7cd2) helps support SHA2 authentication on Windows Server 2008 SP2\. 



**Note**  
The hotfixes for SHA256 support for Windows 2003 are no longer publicly available from Microsoft\. If these fixes are not already installed in your Windows 2003 host, manual upgrades are necessary\.

**To perform upgrades manually**

1. Download the [Windows Agent Updater](https://s3.us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/windows/latest/AWSDiscoveryAgentUpdater.exe)\.

1. Open command prompt as an administrator\.

1. Navigate to the location where the updater was saved\.

1. Run the following command\.

   ```
   AWSDiscoveryAgentUpdater.exe /Q
   ```

## Manage the Discovery Agent process in Windows<a name="using_on_windows"></a>

You can manage the behavior of the Discovery Agent at the system level through the Windows Server Manager Services console\. The following table describes how\.


| Task | Service Name | Service Status/Action | 
| --- | --- | --- | 
| Verify that an agent is running |  AWS Discovery Agent AWS Discovery Updater  | Started | 
| Start an agent |  AWS Discovery Agent AWS Discovery Updater  | Choose Start | 
| Stop an agent |  AWS Discovery Agent AWS Discovery Updater  | Choose Stop | 
| Restart an agent |  AWS Discovery Agent AWS Discovery Updater  | Choose Restart | 

**To uninstall a discovery agent on Windows**

1. Open the Control Panel in Windows\.

1. Choose **Programs**\.

1. Choose **Programs and Features**\.

1. Select **AWS Discovery Agent**\.

1. Choose **Uninstall**\.

   
**Note**  
If you choose to reinstall the agent after uninstalling it, run the following command with the `/repair` and `/norestart` options\.  

   ```
   .\AWSDiscoveryAgentInstaller.exe REGION="your-home-region" KEY_ID="aws-access-key-id" KEY_SECRET="aws-secret-access-key" /quiet /repair /norestart
   ```

   

**To uninstall a discovery agent on Windows using the command line**

1. Right\-click **Start**\.

1. Choose **Command Prompt**\.

1. Use the following command to uninstall a discovery agent on Windows\. 

   ```
   wmic product where name='AWS Discovery Agent' call uninstall
   ```

## Troubleshooting Discovery Agent in Windows<a name="windows_troubleshooting"></a>

If you encounter problems while installing or using the AWS Application Discovery Agent on Windows, consult the following guidance about logging and configuration\. AWS Supportoften requests these files when helping to troubleshoot potential issues with the agent or its connection to the Application Discovery Service\.
+ **Installation logging** 

  In some cases, the agent install command appears to fail\. For example, a failure can appear with the Windows Services Manager showing that the discovery services are not being created\. In this case, add /log install\.log to the command to generate a verbose installation log\.
+  **Operational logging** 

  On Windows Server 2008 and later, agent log files can be found under the following directory\.

  ```
  C:\ProgramData\AWS\AWS Discovery\Logs
  ```

  On Windows Server 2003, agent log files can be found under the following directory\.

  ```
  C:\Documents and Settings\All Users\Application Data\AWS\AWS Discovery\Logs
  ```

  Log files are named to indicate whether generated by the main service, automatic upgrades, or the installer\.

   
+ **Configuration file**

  On Windows Server 2008 and later, the agent configuration file can be found at the following location\.

  ```
  C:\ProgramData\AWS\AWS Discovery\config
  ```

  On Windows Server 2003, the agent configuration file can be found at the following location\.

  ```
  C:\Documents and Settings\All Users\Application Data\AWS\AWS Discovery\config
  ```
+ For instructions on how to remove earlier versions of the Discovery Agent, see [Prerequisites for Discovery Agent](gen-prep-agents.md)\.