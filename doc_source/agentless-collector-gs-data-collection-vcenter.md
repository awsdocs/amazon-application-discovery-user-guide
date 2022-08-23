# VMware vCenter Agentless Collector data collection module<a name="agentless-collector-gs-data-collection-vcenter"></a>

This section describes the Application Discovery Service Agentless Collector \(Agentless Collector\) VMware vCenter data collection module, which is used to collect server inventory, profile, and utilization data from your VMware VMs\.

**Topics**
+ [How to set up the Agentless Collector data collection module for VMware vCenter](#agentless-collector-gs-vcenter)
+ [VMware data collection details](#agentless-collector-gs-vcenter-details)
+ [Control the scope of vCenter data collection](#control-data-collection-scope)

## How to set up the Agentless Collector data collection module for VMware vCenter<a name="agentless-collector-gs-vcenter"></a>

This section describes how to set up the Agentless Collector VMware vCenter data collection module to collect server inventory, profile, and utilization data from your VMware VMs\.

**Note**  
Before starting vCenter setup, make sure you can provide vCenter credentials with Read and View permissions set for the System group\.

**To set up the VMware vCenter data collection module**

1. On the **Agentless Collector** dashboard page, under **Data collection**, choose **Set up** in the **VMware vCenter** section\.

1. On the **Set up VMware vCenter data collection** page, perform the following:

   1. Under **vCenter credentials**:

      1. For **vCenter URL/IP**, enter the IP address of your VMware vCenter Server host\.

      1. For **vCenter Username**, enter the name of a local or domain user that the collector uses to communicate with vCenter\. For domain users, use the form *domain*\\*username* or *username*@*domain*\.

      1. For **vCenter Password**, enter the local or domain user password\.

   1. Under **Data collection preferences**:

      1. To automatically start collecting data immediately following a successful setup, select **Start data collection automatically**\.

   1. Choose **Set up**\.

Next, you'll see the **VMware data collection details** page, which is described in the next topic\.

## VMware data collection details<a name="agentless-collector-gs-vcenter-details"></a>

The **VMware data collection details** page shows details about the vCenter you set up in [How to set up the Agentless Collector data collection module for VMware vCenter](#agentless-collector-gs-vcenter)\.

Under **Discovered vCenter servers**, the vCenter you set up is listed with the following information about the vCenter:
+ The IP address of the vCenter server\.
+ The number of servers in the vCenter\.
+ The status of the data collection\.
+ How long since the last update\.

Choose **Remove vCenter server** to remove the displayed vCenter server and return you to the **Set up VMware vCenter data collection** page\.

If you did not choose to start data collection automatically, you can start data collection by using the **Start data collection** button on this page\. After data collection starts, the start button changes to **Stop data collection**\.

If the **Collection status** column shows **Collecting**, data collection has started\.

You view the collected data in the AWS Migration Hub console\. If you’re collecting data for a VMware vCenter server inventory, you can access data that appears in the console approximately 15 minutes after turning on data collection\.

You can choose **View servers in Migration Hub** on this page to open the Migration Hub console, if your access to the internet is not blocked\. Whether you choose this button or not, for information about how to access the Migration Hub console, see [Step 7: View collected data in the Migration Hub console](agentless-collector-gs-view-collected-data.md)\.

The following are the guidelines for recommended length of data collection according to migration planning activities: 
+ TCO \(total cost of ownership\) \- 2 to 4 weeks
+ Migration planning \- 2 to 6 weeks

## Control the scope of vCenter data collection<a name="control-data-collection-scope"></a>

The vCenter user requires read\-only permissions on each ESX host or VM to inventory using Application Discovery Service\. Using the permission settings, you can control which hosts and VMs are included in the data collection\. You can either allow all hosts and VMs under the current vCenter to be inventoried, or grant permissions on a case\-by\-case basis\.

**Note**  
As a security best practice, we recommend against granting additional, unneeded permissions to the vCenter user of the Application Discovery Service\.

The following procedures describe configuration scenarios ordered from least granular to most granular\. These procedures are for VSphere Client v6\.7\.0\.2\. The procedures for other versions of the client might be different, depending on which version of the VSphere client you are using\.

**To discover data about *all* ESX hosts and VMs under the current vCenter**

1. In your VMware vSphere client, choose **vCenter** and then choose either **Hosts and Clusters** or **VMs and Templates**\. 

1. Choose a datacenter resource and then choose **Permissions**\.

1. Choose the vCenter user and then choose the symbol to add, edit, or remove a user role\.

1. Choose **Read\-only** from the **Role** menu\.

1.  Choose **Propagate to children** and then choose **OK**\.

**To discover data about a *specific* ESX host and *all* of its child objects**

1. In your VMware vSphere client, choose **vCenter** and then choose either **Hosts and Clusters** or **VMs and Templates**\. 

1. Choose **Related Objects**, **Hosts**\. 

1. Open the context \(right\-click\) menu for the host name and choose **All vCenter Actions**, **Add Permission**\.

1. Under **Add Permission**, add the vCenter user to the host\. For **Assigned Role**, choose **Read\-only**\. 

1. Choose **Propagate to children**, **OK**\.

**To discover data about a *specific* ESX host or child VM**

1. In your VMware vSphere client, choose **vCenter** and then choose either **Hosts and Clusters** or **VMs and Templates**\. 

1. Choose **Related Objects**\.

1. Choose **Hosts** \(showing a list of ESX hosts known to vCenter\) or **Virtual Machines** \(showing a list of VMs across all ESX hosts\)\. 

1. Open the context \(right\-click\) menu for the host or VM name and choose **All vCenter Actions**, **Add Permission**\.

1.  Under **Add Permission**, add the vCenter user to the host or VM\. For **Assigned Role**, choose **Read\-only**, \. 

1. Choose **OK**\. 

**Note**  
If you chose **Propagate to children**, you can still remove the read\-only permission from ESX hosts and VMs on a case\-by\-case basis\. This option has no effect on inherited permissions applying to other ESX hosts and VMs\. 