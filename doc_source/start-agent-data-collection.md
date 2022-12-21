# Start or stop Discovery Agent data collection<a name="start-agent-data-collection"></a>

After the Discovery Agent is deployed and configured, if data collections stops you can restart it\. You can start or stop data collection through the console or by making API calls through the AWS CLI\. Both of these methods are described in the following procedures\. 

------
#### [ Using the Migration Hub console ]

The following procedure shows how to start or stop the Discovery Agent data collection process, on the **Data Collectors** page of the Migration Hub console\. 

**To start or stop data collection**

1. In the navigation pane, choose **Data Collectors**\.

1. Choose the **Agents** tab\.

1. Select the check box of the agent you want to start or stop\.
**Tip**  
If you installed multiple agents but only want to start or stop data collection on certain hosts, the **Hostname** column in the agent's row identifies the host the agent is installed on\.

1. Choose **Start data collection** or **Stop data collection**\.

------
#### [ Using the AWS CLI ]

To start or stop the Discovery Agent data collection process from the AWS CLI, you must first install the AWS CLI in your environment, and then you must set the CLI to use your selected [Migration Hub home region](https://docs.aws.amazon.com/migrationhub/latest/ug/home-region.html)\.

**To install the AWS CLI and start or stop data collection**

1. If you have not already done so, install the AWS CLI appropriate to your OS type \(Windows or Mac/Linux\)\. See the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/) for instructions\.

1. Open the Command prompt \(Windows\) or Terminal \(MAC/Linux\)\.

   1. Type `aws configure` and press Enter\.

   1. Enter your AWS Access Key ID and AWS Secret Access Key\.

   1. Enter your home region for the Default Region Name, for example *`us-west-2`*\. \(We are assuming that `us-west-2` is your home region in this example\.\)

   1. Enter `text` for Default Output Format\.

1. To find the ID of the agent you want to stop or start data collection for, type the following command:

   ```
   aws discovery describe-agents
   ```

1. To start data collection by the agent, type the following command:

   ```
   aws discovery start-data-collection-by-agent-ids --agent-ids <agent ID>
   ```

   To stop data collection by the agent, type the following command:

   ```
   aws discovery stop-data-collection-by-agent-ids --agent-ids <agent ID>
   ```

   

------