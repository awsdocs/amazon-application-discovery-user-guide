# Manually updating Agentless Collector<a name="agentless-collector-update"></a>

When you configure Application Discovery Service Agentless Collector \(Agentless Collector\), you can choose to enable automatic updates as described in [Step 5: Configure Agentless Collector](agentless-collector-gs-configure.md)\. If you do not enable automatic updates, you'll need to manually update Agentless Collector\.

The following procedure describes how to manually update Agentless Collector\.

**To manually update Agentless Collector**

1. Obtain the latest Agentless Collector Open Virtualization Archive \(OVA\) file\.

1. \(Optional\) We recommend that you delete the previous Agentless Collector OVA file, before you deploy the latest one\.

1. In the [Getting started with Agentless Collector](agentless-collector-gs.md) section, follow steps [Step 3: Deploy Agentless Collector](agentless-collector-gs-deploy.md) through [Step 6: Set up Agentless Collector data collection module](agentless-collector-gs-data-collection.md)\.

**Kernel Live Patching on Amazon Linux 2**

The Agentless Collector virtual machine uses Amazon Linux 2 as described in [Step 3: Deploy Agentless Collector](agentless-collector-gs-deploy.md)\. 

To enable and use live patching for Amazon Linux 2, see [ Kernel Live Patching on Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html) in the *Amazon EC2 User Guide for Linux Instances*\.