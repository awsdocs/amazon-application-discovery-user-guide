# Setting up Agentless Discovery<a name="setting-up-agentless"></a>

To set up agentless discovery, you must deploy the AWS Agentless Discovery Connector virtual appliance on a VMware vCenter Server host in your on\-premises environment\. Download the [Agentless Discovery Appliance OVA](https://s3-us-west-2.amazonaws.com/aws.agentless.discovery.connector.bundle/latest/AWSDiscoveryConnector.ova) \(and [MD5](https://s3-us-west-2.amazonaws.com/aws.agentless.discovery.connector.bundle/latest/AWSDiscoveryConnector.ova.md5) and [SHA256](https://s3-us-west-2.amazonaws.com/aws.agentless.discovery.connector.bundle/latest/AWSDiscoveryConnector.ova.sha256) checksums for verification\)\. The connector appliance is an Open Virtualization Archive \(OVA\) file that you must install in your on\-premises VMware environment\. Deploy and configure the connector as described in the following sections\.


+ [Deploying the AWS Agentless Discovery Connector Virtual Appliance](#deploy-connector-appliance)
+ [Configuring the AWS Agentless Discovery Connector](#configure-connector)
+ [Controlling the Scope of Data Collection](#data-collection-scope)
+ [Collecting and Exporting Data](#collecting-exporting-data)

## Deploying the AWS Agentless Discovery Connector Virtual Appliance<a name="deploy-connector-appliance"></a>

Deploy the downloaded OVA file in your VMware environment\.

**To deploy the connector virtual appliance**

1. Sign in to vCenter as a VMware administrator\.

1. Choose **File**, **Deploy OVF Template**\. Type the URL that was sent to you after you completed the registration and complete the wizard\.

1. On the **Disk Format** page, select one of the thick provision disk types\. We recommend that you choose **Thick Provision Eager Zeroed**, because it has the best performance and reliability\. However, it requires several hours to zero out the disk\. Do not choose **Thin Provision**\. This option makes deployment faster but significantly reduces disk performance\. For more information, see [Types of supported virtual disks](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1022242) in the VMware documentation\.

1. Locate and open the context \(right\-click\) menu for the newly deployed template in the vSphere client inventory tree and choose **Power**, **Power On**\. Open the context \(right\-click\) menu for the template again and choose **Open Console**\. The console displays the IP address of the connector console\. Save the IP address in a secure location\. You need it to complete the connector setup process\.

## Configuring the AWS Agentless Discovery Connector<a name="configure-connector"></a>

To finish the setup process, open a web browser and complete the following procedure\.

**To configure the connector using the console**

1. In a web browser, type the following URL in the address bar: https://*ip\_address*/, where *ip\_address* is the IP address of the connector console that you saved earlier\. 

1. Choose **Get started now** and follow the wizard steps\.

1. In **Step 5: Discovery Connector Set Up**, choose **Configure vCenter credentials**\.

   1. For **vCenter Host**, type the hostname or IP address of your VMware vCenter Server host\.

   1. For **vCenter Username**, type the name of a local or domain user that the connector uses to communicate with vCenter\. For domain users, use the form *domain*\\*username* or *username*@*domain*\.

   1. For **vCenter Password**, type the local or domain user password\.

   1. Choose **Ignore security certificate** to bypass SSL certificate validation with vCenter\.

1. Choose **Configure AWS credentials** and type the credentials for the IAM user who is assigned the `AWSAgentlessDiscoveryService` IAM policy that you created in [Attach Required IAM User Policies](before_you_install.md#appdisc-user-policy)\. Choose **Next**\.

1. Choose **Configure where to publish data** and select suitable publishing options\. Choose **Next**\. You should see the AWS Agentless Discovery Connector console\.

**Note**  
After you complete this initial setup, you can access connector settings by using SSH and the connector IP address: root@*Connector\_IP\_address*\. The default user name is ec2\-user and the default password is ec2pass\. We strongly encourage you to change the value of the default user name and password\.

### Enabling Auto\-Upgrades on AWS Agentless Discovery Connector<a name="connector_auto_upgrade"></a>

To ensure that you are running the latest version of AWS Agentless Discovery Connector, we recommend that you enable auto\-upgrades\. 

**To enable auto\-upgrades**

1. In a web browser, type the following URL in the address bar: https://*ip\_address*/, where *ip\_address* is the IP address of the AWS Agentless Discovery Connector\.

1. In the Application Discovery Service console, under **Actions**, choose **Enable Auto\-Upgrade**\.

### Troubleshooting the Agentless Discovery Connector<a name="agentless-troubleshooting"></a>

If you don’t see inventory information after starting data collection with the connector, confirm that you have registered the connector with your vCenter Server instance\. Agentless discovery does not support a stand\-alone ESX host that is not part of the vCenter Server instance\.

## Controlling the Scope of Data Collection<a name="data-collection-scope"></a>

The vCenter user requires read\-only permissions on each ESX host or virtual machine \(VM\) to inventory using Application Discovery Service\. Using the permission settings, you can control which hosts and VMs are included in the data collection\. You can either allow all hosts and VMs under the current vCenter to be inventoried, or grant permissions on a case\-by\-case basis\.

**Note**  
As a security best practice, we recommend against granting additional, unneeded permissions to the vCenter user\.

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

## Collecting and Exporting Data<a name="collecting-exporting-data"></a>

After agentless\-discovery setup is complete, you can use the console or API to start collecting data; managing service, tag, and query configuration items; and exporting data\. You can export data as a CSV file to an Amazon S3 bucket or an application that enables you to view and evaluate the data\. For more information, see [Tutorial: Using the AWS Application Discovery Service Console](http://docs.aws.amazon.com/application-discovery/latest/userguide/console_walkthrough.html) or the [Application Discovery Service API Reference](http://docs.aws.amazon.com/application-discovery/latest/APIReference/)\.