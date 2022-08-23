# Step 3: Deploy Agentless Collector<a name="agentless-collector-gs-deploy"></a>

Application Discovery Service Agentless Collector\(Agentless Collector\) is a virtual appliance that you install in your on\-premises VMware environment\. This section describes how to deploy the Open Virtualization Archive \(OVA\) file that you downloaded in the previous step, in your VMware environment\.

**Agentless Collector virtual machine specifications**
+ **Operating System** –Amazon Linux 2
+ **RAM** –8 GB

The following procedure steps you through deploying the Agentless Collector OVA file in your VMware environment\.

**To deploy Agentless Collector**

1. Sign in to vCenter as a VMware administrator\.

1. Use one of the following ways to install the OVA file:
   + Use the UI: Choose **File**, choose **Deploy OVF Template**, select the collector OVA file you downloaded in the previous section, and then complete the wizard\.
   + Use the command line: To install the collector OVA file from the command line, download and use the VMwware Open Virtualization Format Tool \(ovftool\)\. To download ovftool, select a release from the [OVF Tool Documentation](https://www.vmware.com/support/developer/ovf/) page\.

     The following is an example of using the ovftool command line tool to install the collector OVA file\.

     ```
     ovftool --acceptAllEulas --name=AgentlessCollector --datastore=datastore1 -dm=thin ApplicationDiscoveryServiceAgentlessCollector.ova 'vi://username:password@vcenterurl/Datacenter/host/esxi/'
     ```

**The following describe the *replaceable* values in the example**
     + The name is the name that you want to use for your Agentless Collector VM\.
     + The datastore is the name of the datastore in your vCenter\.
     + The OVA file name is the name of the downloaded collector OVA file\.
     + The username/password are your vCenter credentials\.
     + The vcenterurl is the URL of your vCenter\.
     + The vi path is the path to your VMware ESXi host\.

1. Locate the deployed Agentless Collector in your vCenter\. Right\-click the VM, and then choose **Power**, **Power On**\.

1. After a few minutes, the IP address of the collector displays in vCenter\. You use this IP address to connect to the collector\. 