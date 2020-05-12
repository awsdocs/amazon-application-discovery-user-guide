# Prerequisites for Agent Installation<a name="gen-prep-agents"></a>

 These are the pre\-installation tasks that should be performed to prevent errors from occurring during the actual installation of the agent\. Be sure you have set a [Migration Hub home region](https://docs.aws.amazon.com/migrationhub/latest/ug/home-region.html) before you begin installing an agent\. If you have a *1\.x* version of the agent installed, it must be removed before installing the latest version\. Instructions for removing older versions are provided in the tasks that follow\.
+ Verify that your operating system \(OS\) environment is supported:
  + **Linux**
    + Amazon Linux 2012\.03, 2015\.03
    + Amazon Linux 2 \(9/25/2018 update and later\)
    + Ubuntu 12\.04, 14\.04, 16\.04
    + Red Hat Enterprise Linux 5\.11, 6\.9, 7\.3
    + CentOS 5\.11, 6\.9, 7\.3
    + SUSE 11 SP4, 12 SP2
  + **Windows**
    + Windows Server 2003 R2 SP2
    + Windows Server 2008 R1 SP2, 2008 R2 SP1
    + Windows Server 2012 R1, 2012 R2
    + Windows Server 2016
    + Windows Server 2019
+ If outbound connections from your network are restricted, you'll need to update your firewall settings\. Agents require access to `arsenal` over TCP port 443\. They don't require any inbound ports to be open\.
  + For example, if your home region is `eu-central-1`, you'd use `https://arsenal-discovery.eu-central-1.amazonaws.com:443`
  + Or substitute your home region as needed for all other regions except us\-west\-2\.
  + If `us-west-2` is your home region, use `https://arsenal.us-west-2.amazonaws.com:443`
+ Access to AWS S3 in your home region is required for auto\-upgrade to function\.
+ Create an IAM user in the IAM console and attach the existing `AWSApplicationDiscoveryAgentAccess` permissions policy\. This policy allows the user to perform necessary agent actions on your behalf\. See [Step 3: Provide Application Discovery Service Access to Non\-Administrator Users by Attaching Policies](setting-up.md#setting-up-user-policy) for Discovery Agent installation prerequisites\.
+ Check the time skew from your Network Time Protocol \(NTP\) servers and correct if necessary\. Incorrect time synchronization causes the agent registration call to fail\.
+ Remove any previous\-generation agents\. If you previously installed Application Discovery Agent 1\.0 for either Windows or Linux, you must uninstall it before continuing with the installation of the current agent\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/application-discovery/latest/userguide/gen-prep-agents.html)

**Note**  
The Discovery Agent has a 32\-bit agent executable, which works on 32\-bit and 64\-bit operating systems\. The number of installation packages needed for deployment is reduced by having a single executable\. This executable agent works for Linux and for Windows OS\. It is addressed in their respective installation sections that follow\.