# Discovery Connector Data Collection<a name="start-connector-data-collection"></a>

After the Discovery Connector is deployed and configured in your VMware environment, if data collections stops you can restart it\. You can start or stop data collection through the console or by making API calls through the AWS CLI\. Both of these methods are described in the following procedures\.

------
#### [ Using the Migration Hub Console ]

The following procedure shows how to start or stop the Discovery Connector data collection process, on the **Data Collectors** page of the Migration Hub console\.

**To start or stop data collection**

1. In the navigation pane, choose **Data Collectors**\.

1. Choose the **Connectors** tab\.

1. Select the check box of the connector you want to start or stop\.

1. Choose **Start data collection** or **Stop data collection**\.

**Note**  
If you don’t see inventory information after starting data collection with the connector, confirm that you have registered the connector with your vCenter Server\.

------
#### [ Using the AWS CLI ]

To start the Discovery Connector data collection process from the AWS CLI, the AWS CLI must first be installed in your environment, and then you must set the CLI to use your selected [Migration Hub home Region](https://docs.aws.amazon.com/migrationhub/latest/ug/home-region.html)\.

**To install the AWS CLI and start data collection**

1. Install the AWS CLI for your operating system \(Linux, macOS, or Windows\)\. See the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/) for instructions\.

1. Open the Command prompt \(Windows\) or Terminal \(Linux or macOS\)\.

   1. Type `aws configure` and press Enter\.

   1. Enter your AWS Access Key ID and AWS Secret Access Key\.

   1. Enter your home Region for the Default Region Name\. For example, `us-west-2`\.

   1. Enter `text` for Default Output Format\.

1. To find the ID of the connector you want to start or stop data collection for, type the following command to see the connector's ID:

   ```
   aws discovery describe-agents --filters condition=EQUALS,name=hostName,values=connector
   ```

1. To start data collection by the connector, type the following command:

   ```
   aws discovery start-data-collection-by-agent-ids --agent-ids <connector ID>
   ```
**Note**  
If you don’t see inventory information after starting data collection with the connector, confirm that you have registered the connector with your vCenter Server\.

   To stop data collection by the connector, type the following command:

   ```
   aws discovery stop-data-collection-by-agent-ids --agent-ids <connector ID>
   ```

------