# The Agentless Collector dashboard<a name="agentless-collector-dashboard"></a>

On the Application Discovery Service Agentless Collector \(Agentless Collector\) dashboard page you can see the status of the collector and choose a method of data collection as described in the following topics\.

**Topics**
+ [Collector status](#using-collector-status)
+ [Data collection](#using-collector-data-collect)

## Collector status<a name="using-collector-status"></a>

**Collector status** gives you status information about the collector\. The collector name, the status of the collector's connection to AWS, the Migration Hub home Region, and the version\.

If you have AWS connection issues, you might need to edit Agentless Collector configuration settings\.

To edit the collector configuration settings, choose **Edit collector settings** and follow the instructions described in [Editing Agentless Collector settings](agentless-collector-edit-configure.md)\.

## Data collection<a name="using-collector-data-collect"></a>

Under **Data collection** you can choose a data collection method\. Application Discovery Service Agentless Collector \(Agentless Collector\) currently supports data collection from VMware VMs\. Future modules will support collection from additional virtualization platforms, and operating system level collection\.

### VMware vCenter data collection<a name="using-collector-data-collect-vcenter"></a>

To collect server inventory, profile, and utilization data from your VMware VMs, set up connections to your vCenter servers\. To set up the connections, choose **Set up** in the **VMware vCenter** section and follow the instructions described in [Step 6: Set up Agentless Collector data collection module](agentless-collector-gs-data-collection.md)\.

After you set up vCenter data collection, from the dashboard you can perform the following:
+ View data collection status
+ Start data collection
+ Stop data collection

**Note**  
On the dashboard page, after you set up vCenter data collection, the **Set up** button in the **VMware vCenter** section is replaced with data collection status information, a **Stop data collection** button, and a **View and edit** button\.