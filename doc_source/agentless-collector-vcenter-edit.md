# Editing VMware vCenter credentials<a name="agentless-collector-vcenter-edit"></a>

To collect server inventory, profile, and utilization data from your VMware VMs, set up connections to your vCenter servers\. For information about setting up VMware vCenter connections, see [Step 6: Set up Agentless Collector data collection module](agentless-collector-gs-data-collection.md)\.

This section describes how to edit the vCenter credentials\.

**Note**  
Before editing vCenter credentials, make sure you can provide vCenter credentials with Read and View permissions set for the System group\.

**To edit the VMware vCenter credentials**

On the [VMware data collection details](agentless-collector-gs-data-collection-vcenter.md#agentless-collector-gs-vcenter-details) page, choose **Edit vCenter servers**\.
+ On the **Edit vCenter** page, perform the following:

  1. Under **vCenter credentials**:

     1. For **vCenter URL/IP**, enter the IP address of your VMware vCenter Server host\.

     1. For **vCenter Username**, enter the name of a local or domain user that the connector uses to communicate with vCenter\. For domain users, use the form *domain*\\*username* or *username*@*domain*\.

     1. For **vCenter Password**, enter the local or domain user password\.

  1. Choose **Save**\.