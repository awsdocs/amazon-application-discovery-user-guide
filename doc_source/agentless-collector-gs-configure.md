# Step 5: Configure Agentless Collector<a name="agentless-collector-gs-configure"></a>

Application Discovery Service Agentless Collector \(Agentless Collector\) is an Amazon Linux 2 based virtual machine \(VM\)\. The following section describes how to configure a collector VM on the Agentless Collector console's **Configure Agentless Collector** page\.

**To configure a collector VM on the **Configure Agentless Collector** page**

1. For **Collector name**, enter a name for the collector to identify it\. The name can contain spaces but it cannot contain special characters\.

1. Under **Destination AWS account for discovery data**, enter the AWS access key and secret key for the AWS account IAM user to specify as the destination account to receive the data discovered by the collector\. For information about the requirements for the IAM user, see [Step 1: Create an IAM user for Agentless Collector](agentless-collector-gs-iam-user.md)\.

   1. For **AWS access\-key**, enter the access key of the AWS account IAM user that you're specifying as the destination account\.

   1. For **AWS secret\-key**, enter the secret key of the AWS account IAM user that you are you're specifying as the destination account\.

1. Under **Agentless Collector password**, set up a password to use to authenticate access to Agentless Collector\.

   1. For **Agentless Collector password**, enter a password to use to authenticate access to the collector\.

   1. For **Re\-enter Agentless Collector password**, for verification, enter the password again\.

1. Under **Other settings**, read the **License Agreement**\. If you agree to accept it, select the check box\.

1. To enable automatic updates for Agentless Collector, under **Other settings**, select **Automatically update Agentless Collector**\. If you do not select this checkbox, you'll need to manually update Agentless Collector as described in [Manually updating Agentless Collector](agentless-collector-update.md)\. 

1. Choose **Save configurations**\.

The following topics describe optional collector configuration tasks\.

**Topics**
+ [\(Optional\) Configure a static IP address for the Agentless Collector VM](#agentless-collector-gs-configure-ip)
+ [\(Optional\) Reset the Agentless Collector VM back to using DHCP](#agentless-collector-gs-configure-dhcp)

## \(Optional\) Configure a static IP address for the Agentless Collector VM<a name="agentless-collector-gs-configure-ip"></a>

The following steps describe how to configure a static IP address for the Application Discovery Service Agentless Collector \(Agentless Collector\) VM\. When first installed, the collector VM is configured to use the Dynamic Host Configuration Protocol \(DHCP\)\.

**To configure a static IP address for the collector VM**

1. Collect the following network information from VMware vCenter:
   + **Static IP address**–An unsigned IP address in the subnet\. For example, 192\.168\.1\.138\.
   + **Network mask**–This can be obtained by checking IP address setting of the VMware vCenter host that hosts the collector VM\. For example, 255\.255\.255\.0\.
   + **Default Gateway**–This can be obtained by checking IP address setting of the VMware vCenter host that hosts the collector VM\. For example, 192\.168\.1\.1\.
   + **Primary DNS**–This can be obtained by checking IP address setting of the VMware vCenter host that hosts the collector VM\. For example, 192\.168\.1\.1\.
   + \(Optional\) **Secondary DNS**
   + \(Optional\) **Local domain name**–This allows the collector to reach the vCenter host URL without the domain name\.

1. Open the collector’s VM console and sign in as **ec2\-user** using the password **collector** as shown in the following example\.

   ```
   username: ec2-user
   password: collector
   ```

1. Disable the network interface, by entering the following command in the remote terminal\.

   ```
   sudo /sbin/ifdown eth0
   ```

1. 

****

   Update the interface eth0 configuration using the following steps\.

   1. Open ifcfg\-eth0 in the vi editor using the following command\.

      ```
      sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0
      ```

   1. Update the interface values, as shown in the following example, with the information that you collect in the **Collect network information** step\.

      ```
      DEVICE=eth0
      BOOTPROTO=static
      ONBOOT=yes
      IPADDR=static-ip-value
      NETMASK=netmask-value
      GATEWAY=gateway-value
      TYPE=Ethernet
      USERCTL=yes
      PEERDNS=no
      RES_OPTIONS="timeout:2 attempts:5"
      ```

1. Update the Domain Name System \(DNS\) using the following steps\.

   1. Open the `resolv.conf` file in vi using the following command\. 

      ```
      sudo vi /etc/resolv.conf
      ```

   1. Update the `resolv.conf` file in vi using the following command\. 

      ```
      search localdomain-name
      options timeout:2 attempts:5
      nameserver dnsserver-value
      ```

      The following example shows an edited `resolv.conf` file\. 

      ```
      search vsphere.local
      options timeout:2 attempts:5
      nameserver 192.168.1.1
      ```

1. Enable the network interface, by entering the following command\.

   ```
   sudo /sbin/ifup eth0
   ```

1. Reboot the VM as shown in the following example\.

   ```
   sudo reboot
   ```

1. Verify your network settings using the following steps\.

   1. Check if the IP address is configured correctly, by entering the following commands\. 

      ```
      ifconfig 
      
      ip addr show
      ```

   1. Check that the gateway was added correctly, by entering the following command\.

      ```
      route -n
      ```

      The output should be similar to the following example\.

      ```
      Kernel IP routing table
      Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
      0.0.0.0         192.168.1.1     0.0.0.0         UG    0      0        0 eth0
      172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
      192.168.1.0     0.0.0.0         255.255.255.0   U     0      0
      ```

   1. Verify that you can ping a public URL, by entering the following command\.

      ```
      ping www.google.com
      ```

   1. Verify that you can ping the vCenter IP address or host name as shown in the following example\.

      ```
      ping vcenter-host-url
      ```

## \(Optional\) Reset the Agentless Collector VM back to using DHCP<a name="agentless-collector-gs-configure-dhcp"></a>

The following steps describe how to reconfigure the Agentless Collector VM to use DHCP\.

**To configure the collector VM to use DHCP**

1. Disable the network interface, by entering the following command in the remote terminal\.

   ```
   sudo /sbin/ifdown eth0
   ```

1. Update the network configuration using the following steps\.

   1. Open the `ifcfg-eth0 ` file in the vi editor using the following command\.

      ```
      sudo /sbin/ifdown eth0
      ```

   1. Update the values in the `ifcfg-eth0 ` file as shown in the following example\.

      ```
      DEVICE=eth0
      BOOTPROTO=dhcp
      ONBOOT=yes
      TYPE=Ethernet
      USERCTL=yes
      PEERDNS=yes
      DHCPV6C=yes
      DHCPV6C_OPTIONS=-nw
      PERSISTENT_DHCLIENT=yes
      RES_OPTIONS="timeout:2 attempts:5"
      ```

1. Reset the DNS setting, by entering the following command\.

   ```
   echo "" | sudo tee /etc/resolv.conf
   ```

1. Enable the network interface, by entering the following command\.

   ```
   sudo /sbin/ifup eth0
   ```

1. Reboot the collector VM as shown in the following example\.

   ```
   sudo reboot
   ```