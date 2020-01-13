# Configure the AWS Discovery Connector<a name="configure-connector"></a>

To finish the setup process, open a web browser and complete the following procedure and optional tasks within this section\. Be sure you have first selected a home region in Migration Hub\. 

**To configure the connector using it's console**

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

## Configure a static IP address for the connector<a name="connector_static_ip"></a>

Follow this procedure if your environment requires that you use a static IP address\.

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

## Control the Scope of Data Collection<a name="data-collection-scope"></a>

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

## Disabling Auto\-Upgrades on AWS Discovery Connector<a name="connector_auto_upgrade"></a>

To ensure that you are running the latest version of AWS Discovery Connector, the auto\-upgrade feature is enabled by default upon installation\. However, you may disable the auto\-upgrade feature as shown below\.

**To disable auto\-upgrades**

1. In a web browser, type the following URL in the address bar:  **https://***<ip\_address>***/**, where *ip\_address* is the IP address of the AWS Discovery Connector\.

1. In the Discovery Connector console, under **Actions**, choose **Disable Auto\-Upgrade**\.

**Warning**  
Disabling auto\-upgrades will prevent the latest security patches from being installed\.