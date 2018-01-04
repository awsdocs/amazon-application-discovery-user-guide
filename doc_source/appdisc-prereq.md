# Prerequisites<a name="appdisc-prereq"></a>

Before using using the Application Discovery Service, confirm that your on\-premises servers and your network environment meet the following requirements\. 

## Supported Operating Systems<a name="supported-os"></a>

The Application Discovery agent supports the following operating systems and system versions\.

## Linux<a name="linux_beta"></a>

+ Amazon Linux 2012\.03, 2015\.03

+ Ubuntu 12\.04, 14\.04, 16\.04

+ Red Hat Enterprise Linux 5\.11, 6\.9, 7\.3

+ CentOS 5\.11, 6\.9, 7\.3

+ SUSE 11 SP4, 12 SP2

## Windows<a name="windows_beta"></a>

+ Windows Server 2003 R2 SP2

+ Windows Server 2008 R1 SP2

+ Windows Server 2008 R2 SP1

+ Windows Server 2012 R1

+ Windows Server 2012 R2

+ Windows Server 2016

## Firewall Configuration<a name="ads-firewalling"></a>

The AWS Application Discovery Agent requires outbound access to arsenal\.us\-west\-2\.amazonaws\.com:443\. It does not require any inbound ports to be open\. Agents also work with transparent web proxies\.