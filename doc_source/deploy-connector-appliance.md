# Deploy the Discovery Connector<a name="deploy-connector-appliance"></a>



In the previous section you downloaded the AWS Agentless Discovery Connector in the Open Virtualization Archive \(OVA\) file\. This section lists the specifications for the Discovery Connector that you downloaded and shows you how to deploy it in your VMware environment\.

**Discovery Connector virtual machine specifications**
+ **Operating System** –FreeBSD 11 \(64 bit\)
+ **RAM** –2 GB
+ **Disk storage**

  When the you deploy the OVA as virtual machine in vCenter, the vCenter client gives you the following two options to provision:
  + **Thin Provisioned** –approximately 7\.8 GB
  + **Thick Provisioned** –approximately 299\.0 GB \(recommended option\)

The following procedure steps you through deploying the Discovery Connector OVA file in your VMware environment\.

**To deploy the Discovery Connector**

1. Sign in to vCenter as a VMware administrator\.

1. Choose **File**, **Deploy OVF Template**, select the OVA file you downloaded in the previous section, and complete the wizard\.

1. On the **Disk Format** page, select one of the thick provision disk types\. We recommend that you choose **Thick Provision Eager Zeroed**, because it has the best performance and reliability\. However, it requires several hours to zero out the disk\. Do not choose **Thin Provision**\. This option makes deployment faster but significantly reduces disk performance\. For more information, see [Types of supported virtual disks](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1022242) in the VMware documentation\.

1. Locate and open the context \(right\-click\) menu for the newly deployed template in the vSphere client inventory tree and choose **Power**, **Power On**\.

1. Open the context \(right\-click\) menu for the template again and choose **Open Console**\. The console displays the IP address of the connector console\. Make note of the IP address as you'll need it in order to complete the connector setup process\.