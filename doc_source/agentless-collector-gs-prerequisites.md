# Prerequisites for Agentless Collector<a name="agentless-collector-gs-prerequisites"></a>

The following are the prerequisites for using Application Discovery Service Agentless Collector \(Agentless Collector\):
+ One or more AWS accounts\.
+ An AWS account with the AWS Migration Hub home Region set, see [Sign in to the Migration Hub console and choose a home Region](setting-up-choose-home-region.md)\. Your Migration Hub data is stored in your home Region for purposes of discovery, planning, and migration tracking\.
+ An AWS account IAM user that is set up to use the IAM managed policy `AWSApplicationDiscoveryAgentlessCollectorAccess`, see [Step 1: Create an IAM user for Agentless Collector](agentless-collector-gs-iam-user.md)\. The IAM user must be created in an AWS account with Migration Hub home Region set\. 
+ VMware vCenter Server V5\.5, V6, V6\.5, 6\.7 or 7\.0\.
**Note**  
The Agentless Collector supports all of these versions of VMware, but we currently test against version 6\.7 and 7\.0\. 
+ For VMware vCenter Server setup, make sure that you can provide vCenter credentials with Read and View permissions set for the System group\.
+ If outbound connections from your network are restricted, you'll need to update your firewall settings\. Agentless Collector requires only outbound \(egress\) access over TCP port 443\. Depending on your Migration Hub home Region, you will need to allow outbound access to one or two AWS endpoints\. 
  + If your home Region is us\-west\-2, you need to allow access to https://arsenal\-discovery\.us\-west\-2\.amazonaws\.com\. 
  + For other home Regions, you need to allow access to the regional arsenal\-discovery endpoints\. For example, https://arsenal\-discovery\.eu\-central\-1\.amazonaws\.com and the https://migrationhub\-config\.us\-west\-2\.amazonaws\.com\. 

    For more information, see [Network firewall configuration](#agentless-collector-gs-prerequisites-firewall)\.

## Network firewall configuration<a name="agentless-collector-gs-prerequisites-firewall"></a>

**Setup failed**  
Check your credentials and try again\.

In addition to bad credentials, this error can happen due to a failed attempt by the Agentless Collector to establish an HTTPS connection to the Application discovery Service endpoint\. If a connection to AWS cannot be established, the Agentless Collector fails to collect data from your on\-premises environment\. For this reason, the collector needs the following access:



For the AWS us\-west\-2 Region, the Agentless Collector requires an egress access to the arsenal\-discovery\.us\-west\-2\.amazonaws\.com endpoint on port 443\. The collector uses this endpoint to send collected data and validate it is configured with the IAM user credentials for the us\-west\-2 home Region\.



For other Regions, the Agentless Collector requires egress access to the following endpoints on port 443: 


+ The migrationhub\-config\.us\-west\-2\.amazonaws\.com is required to determine which home Region the collector should send data to based on the provided IAM user credentials\.
+ The regional arsenal\-discovery endpoint to send the collected data\. For example, if us\-east\-1 is your home region, then the endpoint will be arsenal\-discovery\.us\-east\-1\.amazonaws\.com

  

**To fix the connection to AWS**

1. Check with your IT admin to see if your company firewall is blocking egress traffic on port 443 to the endpoints listed earlier and request to unblock these ports\. After you update the firewall, reconfigure the Discovery Collector\.

1. If updating the firewall does not resolve the connection issue, check to make sure that the collector virtual machine has outbound network connectivity\. If the virtual machine has outbound connectivity, you can install the telnet utility and use it to check the outbound connections as in the following example:

   ```
   telnet arsenal-discovery.us-west-2.amazonaws.com 443
   ```

1. If the virtual machine has all required outbound connections, you must contact [AWS Support](https://aws.amazon.com/contact-us/) for further troubleshooting\.