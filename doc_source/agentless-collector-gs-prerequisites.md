# Prerequisites for Agentless Collector<a name="agentless-collector-gs-prerequisites"></a>

The following are the prerequisites for using Application Discovery Service Agentless Collector \(Agentless Collector\):
+ One or more AWS accounts\.
+ An AWS account with the AWS Migration Hub home Region set, see [Sign in to the Migration Hub console and choose a home Region](setting-up-choose-home-region.md)\. Your Migration Hub data is stored in your home Region for purposes of discovery, planning, and migration tracking\.
+ An AWS account IAM user that is set up to use the IAM managed policy `AWSApplicationDiscoveryAgentlessCollectorAccess`, see [Step 1: Create an IAM user for Agentless Collector](agentless-collector-gs-iam-user.md)\. The IAM user must be created in an AWS account with Migration Hub home Region set\. 
+ VMware vCenter Server V5\.5, V6, V6\.5, 6\.7 or 7\.0\.
**Note**  
The Agentless Collector supports all of these versions of VMware, but we currently test against version 6\.7 and 7\.0\. 
+ For VMware vCenter Server setup, make sure that you can provide vCenter credentials with Read and View permissions set for the System group\.
+ Agentless Collector requires outbound access over TCP port 443 to several AWS domains\. For a list of these domains, see [Configure firewall for outbound access to AWS domains](#agentless-collector-gs-prerequisites-firewall)\.

## Configure firewall for outbound access to AWS domains<a name="agentless-collector-gs-prerequisites-firewall"></a>

If outbound connections from your network are restricted, you must update your firewall settings to allow outbound access to the AWS domains that Agentless Collector requires\. Which AWS domains require outbound access depend on if your Migration Hub home Region is US West \(Oregon\) Region, us\-west\-2, or some other Region\.

**The following domains require outbound access if your AWS account home Region is us\-west\-2:**
+ `arsenal-discovery.us-west-2.amazonaws.com` – The collector uses this domain to validate that it is configured with the required IAM user credentials\. The collector also uses it for sending and storing collected data since the home Region is us\-west\-2\.
+ `migrationhub-config.us-west-2.amazonaws.com` – The collector uses this domain to determine which home Region the collector sends data to based on the provided IAM user credentials\.
+ `api.ecr-public.us-east-1.amazonaws.com` – The collector uses this domain to discover available updates\.
+ `public.ecr.aws` – The collector uses this domain for downloading the updates\.

**The following domains require outbound access if your AWS account home Region is not `us-west-2`:**
+ `arsenal-discovery.us-west-2.amazonaws.com` – The collector uses this domain to validate that it is configured with the required IAM user credentials\.
+ `arsenal-discovery.your-migrationhub-home-region.amazonaws.com` – The collector uses this domain for sending and storing collected data\.
+ `migrationhub-config.us-west-2.amazonaws.com` – The collector uses this domain to determine which home Region the collector should send data to based on the provided IAM user credentials\.
+ `api.ecr-public.us-east-1.amazonaws.com` – The collector uses this domain to discover available updates\.
+ `public.ecr.aws` – The collector uses this domain for downloading the updates\.

When setting up Agentless Collector, you might receive errors such as **Setup failed – Check your credentials and try again** or **AWS cannot be reached \(connection reset\)\. Please verify network settings**\. These errors can be caused by a failed attempt by the Agentless Collector to establish an HTTPS connection to one of the AWS domains that it needs outbound access to\.



If a connection to AWS cannot be established, Agentless Collector cannot collect data from your on\-premises environment\. For information about how to fix the connection to AWS, see [Fixing Agentless Collector cannot reach AWS during setup](agentless-collector-troubleshooting.md#agentless-collector-fix-connector-cannot-reach-aws)\.