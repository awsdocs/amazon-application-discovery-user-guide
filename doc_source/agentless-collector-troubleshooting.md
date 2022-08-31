# Troubleshooting Agentless Collector<a name="agentless-collector-troubleshooting"></a>

This section contains topics that can help you troubleshoot known issues with Application Discovery Service Agentless Collector \(Agentless Collector\)\.

**Topics**
+ [Fixing Agentless Collector cannot reach AWS during setup](#agentless-collector-fix-connector-cannot-reach-aws)
+ [Finding unhealthy collectors](#agentless-collector-fixing-unhealthy-connectors)
+ [Fixing IP address issues](#agentless-collector-vcenter-ip-issues)
+ [Fixing vCenter credentials issues](#agentless-collector-vcenter-credentials-issues)
+ [Standalone ESX host support](#agentless-collector-standalone-esx-host)
+ [Contacting AWS Support for Agentless Collector issues](#agentless-collector-support)

## Fixing Agentless Collector cannot reach AWS during setup<a name="agentless-collector-fix-connector-cannot-reach-aws"></a>

Agentless Collector requires outbound access over TCP port 443 to several AWS domains\. When configuring Agentless Collector in the console you can get the following error message\.

**Could Not Reach AWS**  
AWS cannot be reached \(connection reset\)\. Please verify network settings\.

This error occurs because of a failed attempt by Agentless Collector to establish an HTTPS connection to an AWS domain that the collector needs to communicate with during the setup process\. The Agentless Collector configuration fails if a connection can't be established\. 

**To fix the connection to AWS**

1. Check with your IT admin to see if your company firewall is blocking outbound traffic on port 443 to any of the AWS domains that require outbound access\. Which AWS domains require outbound access depend on if your home Region is US West \(Oregon\) Region, us\-west\-2, or some other Region\.

**The following domains require outbound access if your AWS account home Region is us\-west\-2:**
   + `arsenal-discovery.us-west-2.amazonaws.com`
   + `migrationhub-config.us-west-2.amazonaws.com`
   + `api.ecr-public.us-east-1.amazonaws.com`
   + `public.ecr.aws`

**The following domains require outbound access if your AWS account home Region is not `us-west-2`:**
   + `arsenal-discovery.us-west-2.amazonaws.com`
   + `arsenal-discovery.your-home-region.amazonaws.com`
   + `migrationhub-config.us-west-2.amazonaws.com`
   + `api.ecr-public.us-east-1.amazonaws.com`
   + `public.ecr.aws`

   If your firewall is blocking outbound access to the AWS domains that Agentless Collector needs to communicate with, unblock it\. After you update the firewall, reconfigure Agentless Collector\.

1. If updating the firewall does not resolve the connection issue, check to make sure that the collector virtual machine has outbound network connectivity to the domains listed in the previous step\. If the virtual machine has outbound connectivity, test the connection to the listed domains by running **telnet** on ports 443 as shown in the following example\.

   ```
   telnet migrationhub-config.us-west-2.amazonaws.com 443
   ```

1. If outbound connectivity from the virtual machine is enabled, for further support, see [Contacting AWS Support for Agentless Collector issues](#agentless-collector-support)\.

## Finding unhealthy collectors<a name="agentless-collector-fixing-unhealthy-connectors"></a>

Status information for every collector is found on the [Data collectors](https://console.aws.amazon.com/migrationhub/discover/datacollectors?type=connector) page of the AWS Migration Hub \(Migration Hub\) console\. You can identify collectors with problems by finding any collectors with a **Status** of **Requires attention**\. 

The following procedure describes how to access the Agentless Collector console to identify health issues\.

**To access the Agentless Collector console**

1. Using your AWS account, sign in to the AWS Management Console and open the Migration Hub console at [https://console\.aws\.amazon\.com/migrationhub/](https://console.aws.amazon.com/migrationhub/)\.

1. In the Migration Hub console navigation pane under **Discover**, choose **Data collectors**\.

1. From the **Agentless collectors** tab, make a note of the **IP address** for each connector that has a status of **Requires attention**\.

1. To open the Agentless Collector console, open a web browser\. Then type the following URL in the address bar:  **https://***<ip\_address>***/**, where *ip\_address* is the IP address of an unhealthy collector\.

1. Choose **Log in**, and then enter the Agentless Collector password, which was set up when the collector was configured in [Step 5: Configure Agentless Collector](agentless-collector-gs-configure.md)\.

1. On the **Agentless Collector** dashboard page, under **Data collection**, choose **View and edit** in the **VMware vCenter** section\.

1. Follow the instructions in [Editing VMware vCenter credentials](agentless-collector-vcenter-edit.md) to correct the URL and credentials\.

After correcting the health issues, the collector will re\-establish connectivity with vCenter server, and the collector's status will change to the **Collecting** state\. If the issues persist, see [Contacting AWS Support for Agentless Collector issues](#agentless-collector-support)\.

The most common causes for unhealthy collectors are IP address and credentials issues\. [Fixing IP address issues](#agentless-collector-vcenter-ip-issues) and [Fixing vCenter credentials issues](#agentless-collector-vcenter-credentials-issues) can help you resolve these issues and return a collector to a healthy state\.

## Fixing IP address issues<a name="agentless-collector-vcenter-ip-issues"></a>

A collector can go into an unhealthy state if the vCenter endpoint provided during collector setup is malformed, invalid, or if the vCenter server is currently down and not reachable\. In this case, you'll receive a **Connection error** message \. 

The following procedure can help you resolve IP address issues\.

**To fix collector IP address issues**

1. Get the IP address of the Agentless Collector from VMware vCenter\.

1. Open the Agentless Collector console by opening a web browser, and then type the following URL in the address bar:  **https://***<ip\_address>***/**, where *ip\_address* is the IP address of the collector from [Step 3: Deploy Agentless Collector](agentless-collector-gs-deploy.md)\.

1. Choose **Log in**, and then enter the Agentless Collector password, which was set up when the collector was configured in [Step 5: Configure Agentless Collector](agentless-collector-gs-configure.md)\.

1. On the **Agentless Collector** dashboard page, under **Data collection**, choose **View and edit** in the **VMware vCenter** section\.

1. On the **VMware data collection details** page, under **Discovered vCenter servers**, make a note of the IP address in the **vCenter** column\.

1. Using a separate command line tool like `ping` or `traceroute`, validate that the associated vCenter server is active and the IP is reachable from the collector VM\.
   + If the IP address is incorrect and the vCenter service is active, then update the IP address in the collector console, and choose **Next**\.
   + If the IP address is correct but the vCenter server is inactive, activate it\.
   + If the IP address is correct and the vCenter server is active, check if it is blocking ingress network connections due to firewall issues\. If yes, update your firewall settings to allow incoming connections from the collector VM\.

## Fixing vCenter credentials issues<a name="agentless-collector-vcenter-credentials-issues"></a>

Collectors can go into an unhealthy state if the vCenter user credentials provided when configuring a collector are invalid, or do not have vCenter Read and View account privileges\.

If you experience issues related to vCenter credentials, check to make sure that you have vCenter Read and View permissions set for the System group\. 

For information about editing vCenter credentials, see [Editing VMware vCenter credentials](agentless-collector-vcenter-edit.md)\. 

## Standalone ESX host support<a name="agentless-collector-standalone-esx-host"></a>

The Agentless Collector does not support a standalone ESX host\. The ESX host must be part of the vCenter Server instance\.

## Contacting AWS Support for Agentless Collector issues<a name="agentless-collector-support"></a>

If you encounter issues with Application Discovery Service Agentless Collector \(Agentless Collector\) and need help, contact [AWS Support](http://aws.amazon.com/contact-us/) You'll be contacted and might be asked to send the collector logs\. 

**To obtain Agentless Collector logs**

1. Get the IP address of the Agentless Collector from VMware vCenter\.

1. Open the collector’s VM console and sign in as **ec2\-user** using the password **collector** as shown in the following example\.

   ```
   username: ec2-user
   password: collector
   ```

1. Use the following command to navigate to the log folder\. 

   ```
   cd /var/log/aws/collector
   ```

1. Zip the log files using the following command\.

   ```
   sudo tar cvzf logs_$(date '+%d-%m-%Y_%H.%M.%S').tar.gz *
   ```

1. Copy the log file from the Agentless Collector VM\.

   ```
   scp logs*.tar.gz 
             targetuser@targetaddress
   ```

1. Give the `tar.gz` file to AWS Enterprise Support\.