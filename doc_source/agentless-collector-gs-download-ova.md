# Step 2: Download the Agentless Collector<a name="agentless-collector-gs-download-ova"></a>

To set up the Application Discovery Service Agentless Collector \(Agentless Collector\), you must download and deploy the Agentless Collector Open Virtualization Archive \(OVA\) file\. The Agentless Collector is a virtual appliance that you install in your on\-premises VMware environment\. This step describes how to download the collector OVA file and the next step describes how to deploy it\.

**To download the collector OVA file and verify its checksum**

1. Sign in to vCenter as a VMware administrator and switch to the directory where you want to download the Agentless Collector OVA file\.

1. Download the OVA file from the following URL:

   [Agentless Collector OVA](https://s3.us-west-2.amazonaws.com/aws.agentless.discovery.collector.bundle/releases/latest/ApplicationDiscoveryServiceAgentlessCollector.ova) 

1. Depending on which hashing algorithm you use in your system environment, download either the [MD5](https://s3.us-west-2.amazonaws.com/aws.agentless.discovery.collector.bundle/releases/latest/ApplicationDiscoveryServiceAgentlessCollector.ova.md5) or [SHA256](https://s3.us-west-2.amazonaws.com/aws.agentless.discovery.collector.bundle/releases/latest/ApplicationDiscoveryServiceAgentlessCollector.ova.sha256) to get the file containing the checksum value\. Use the downloaded value to verify the `ApplicationDiscoveryServiceAgentlessCollector` file downloaded in the preceding step\.

1. Depending on your variation of Linux, run the version appropriate MD5 command or SHA256 command to verify that the cryptographic signature of the `ApplicationDiscoveryServiceAgentlessCollector.ova` file matches the value in the respective MD5/SHA256 file that you downloaded\. 

   ```
   $ md5sum ApplicationDiscoveryServiceAgentlessCollector.ova
   ```

   ```
   $ sha256sum ApplicationDiscoveryServiceAgentlessCollector.ova
   ```