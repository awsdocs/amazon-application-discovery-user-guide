# Agent Installation on Linux<a name="install_on_linux"></a>

Complete the following procedure on Linux\. Be sure that your [Migration Hub home region](https://docs.aws.amazon.com/migrationhub/latest/ug/home-region.html) has been set before you begin this procedure\.

**Note**  
If you are using a non\-current Linux version, see [Requirements on Older Linux Platforms](#old_linux)\.<a name="linux_steps"></a>

**To install AWS Application Discovery Agent in your data center**

1. Sign in to your Linux\-based server or VM and create a new directory to contain your agent components\.

1. Switch to the new directory and download the installation script from either the command line or the console\.

   1. To download from the command line, run the following command\.

      ```
      curl -o ./aws-discovery-agent.tar.gz https://s3-us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/linux/latest/aws-discovery-agent.tar.gz
      ```

   1. To download from the Migration Hub console, do the following: 

      1. Open the console and go to the [Discovery Tools](https://us-west-2.console.aws.amazon.com/migrationhub/discover/tools/options) page\.

      1. In the **Discovery Agent** box, choose **Download agent**, then choose **Linux** in the resultant list box\. Your download begins immediately\.

1. Verify the cryptographic signature of the installation package with the following three commands:

   ```
   curl -o ./agent.sig https://s3.us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/linux/latest/aws-discovery-agent.tar.gz.sig
   ```

   ```
   curl -o ./discovery.gpg https://s3.us-west-2.amazonaws.com/aws-discovery-agent.us-west-2/linux/latest/discovery.gpg
   ```

   ```
   gpg --no-default-keyring --keyring ./discovery.gpg --verify agent.sig aws-discovery-agent.tar.gz
   ```

   The agent public key \(`discovery.gpg`\) fingerprint is `7638 F24C 6717 F97C 4F1B 3BC0 5133 255E 4DF4 2DA2`\.

1. Extract from the tarball as shown following\.

   ```
   tar -xzf aws-discovery-agent.tar.gz
   ```

1. To install the agent, choose one of the following installation methods\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/application-discovery/latest/userguide/install_on_linux.html)

1. If outbound connections from your network are restricted, you'll need to update your firewall settings\. Agents require access to `arsenal` over TCP port 443\. They don't require any inbound ports to be open\.

   For example, if your home region is `eu-central-1`, you'd use `https://arsenal-discovery.eu-central-1.amazonaws.com:443`

**Topics**
+ [Requirements on Older Linux Platforms](#old_linux)
+ [Manage the Discovery Agent Process on Linux](#using_on_linux)
+ [Uninstall Discovery Agent on Linux](#using_on_linux-uninstall)
+ [Agent Troubleshooting on Linux](#linux_troubleshooting)

## Requirements on Older Linux Platforms<a name="old_linux"></a>

Some older Linux platforms such as SUSE 10, CentOS 5, and RHEL 5 are either at end of life or only minimally supported\. These platforms can suffer from out\-of\-date cipher suites that prevent the agent update script from downloading installation packages\. 

**Curl**  
The Application Discovery agent requires `curl` for secure communications with the AWS server\. Some old versions of `curl` are not able to communicate securely with a modern web service\.   
To use the version of `curl` included with the Application Discovery agent for all operations, run the installation script with the `-c true` parameter\. 

**Certificate Authority Bundle**  
Older Linux systems might have an out\-of\-date Certificate Authority \(CA\) bundle, which is critical to secure internet communication\.   
To use the CA bundle included with the Application Discovery agent for all operations, run the installation script with the `-b true` parameter\.

These installation script options can be used together\. In the following example command, both of the script parameters are passed to the installation script: 

```
sudo bash install -r your-home_region -k aws-access-key-id -s aws-secret-access-key -c true -b true
```

 

## Manage the Discovery Agent Process on Linux<a name="using_on_linux"></a>

You can manage the behavior of the Discovery Agent at the system level using the `systemd`, `Upstart`, or `System V init` tools\. The following tabs outline the commands for the supported tasks in each of the respective tools\.

------
#### [ systemd ]


**Management Commands for the Application Discovery Agent**  

| Task | Command | 
| --- | --- | 
| Verify that an agent is running |  `sudo systemctl status aws-discovery-daemon.service`   | 
| Start an agent |  `sudo systemctl start aws-discovery-daemon.service`   | 
| Stop an agent |  `sudo systemctl stop aws-discovery-daemon.service`   | 
| Restart an agent |  `sudo systemctl restart aws-discovery-daemon.service`   | 

------
#### [ Upstart ]


**Management Commands for the Application Discovery Agent**  

| Task | Command | 
| --- | --- | 
| Verify that an agent is running |  `sudo initctl status aws-discovery-daemon`   | 
| Start an agent |  `sudo initctl start aws-discovery-daemon`   | 
| Stop an agent |  `sudo initctl stop aws-discovery-daemon`   | 
| Restart an agent |  `sudo initctl restart aws-discovery-daemon`   | 

------
#### [ System V init ]


**Management Commands for the Application Discovery Agent**  

| Task | Command | 
| --- | --- | 
| Verify that an agent is running |  `sudo /etc/init.d/aws-discovery-daemon status`   | 
| Start an agent |  `sudo /etc/init.d/aws-discovery-daemon start`   | 
| Stop an agent |  `sudo /etc/init.d/aws-discovery-daemon stop`   | 
| Restart an agent |  `sudo /etc/init.d/aws-discovery-daemon restart`   | 

------

## Uninstall Discovery Agent on Linux<a name="using_on_linux-uninstall"></a>

This section describes how to uninstall Discovery Agent on Linux\.

**To uninstall an agent if you're using the yum package manager**
+ Use the following command to uninstall an agent if using yum\.

  ```
  rpm -e --nodeps aws-discovery-agent
  ```

**To uninstall an agent if you're using the apt\-get package manager**
+ Use the following command to uninstall an agent if using apt\-get\.

  ```
  apt-get remove aws-discovery-agent:i386
  ```

**To uninstall an agent if you're using the zypper package manager**
+ Use the following command to uninstall an agent if using zypper\.

  ```
  zypper remove aws-discovery-agent
  ```

## Agent Troubleshooting on Linux<a name="linux_troubleshooting"></a>

If you encounter problems while installing or using the Discovery Agent on Linux, consult the following guidance about logging and configuration\. When helping to troubleshoot potential issues with the agent or its connection to the Application Discovery Service, AWS Support often requests these files\. 
+ **Log files**

  Log files for Discovery Agent are located in the following directory\. 

  ```
  /var/log/aws/discovery/
  ```

  Log files are named to indicate whether they are generated by the main daemon, the automatic upgrader, or the installer\.

   
+ **Configuration files**

  Configuration files for Discovery Agent version 2\.0\.1617\.0 or newer are located in the following directory\.

  ```
  /etc/opt/aws/discovery/
  ```

  Configuration files for versions of Discovery Agent before 2\.0\.1617\.0 are located in the following directory\.

  ```
  /var/opt/aws/discovery/
  ```
+ For instructions on how to remove older versions of the Discovery Agent, see [Installation Prerequisites for Discovery Agent](gen-prep-agents.md)\.