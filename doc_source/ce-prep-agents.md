# Enabling Data Exploration in Amazon Athena<a name="ce-prep-agents"></a>

Before you can actually see and start exploring your discovered data in Amazon Athena, Data Exploration in Amazon Athena must first be enabled by Continuous Export implicitly being turned on when you choose "Start data collection", or click the toggle labeled, "Data exploration in Amazon Athena" on the **Data Collectors** page of the Migration Hub console\. Data Exploration in Amazon Athena can also be enabled by Continuous Export explicitly being turned on through an API call from the AWS CLI\. Instructions are provided below for both ways by expanding your method of choice:

------
#### [ Enable with the console ]

Data Exploration in Amazon Athena is enabled by Continuous Export implicitly being turned on when you choose "Start data collection", or click the toggle labeled, "Data exploration in Amazon Athena" on the **Data Collectors** page of the Migration Hub console\.

**To enable Data Exploration in Amazon Athena from the console**

1. In the navigation pane, choose **Data Collectors**\.

1. Choose the **Agents** tab\.

1. Choose **Start data collection**, or if you already have data collection turned on, click the **Data exploration in Amazon Athena** toggle\.

1. In the dialog box generated from the previous step, click the checkbox agreeing to associated costs and choose **Continue** or **Enable**\.

**Note**  
Your agents are now running in "continuous export" mode which will enable you to see and work with your discovered data in Amazon Athena\. The first time this is enable it may take up to 30 minutes for your data to appear in Amazon Athena\.

------
#### [ Enable with the AWS CLI ]

Data Exploration in Amazon Athena is enabled by Continuous Export explicitly being turned on through an API call from the AWS CLI\. To do this, the AWS CLI must first be installed in your environment\.

**To install the AWS CLI and enable Data Exploration in Amazon Athena**

1. Install the AWS CLI for your operating system \(Linux, macOS, or Windows\)\. See the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/) for instructions\.

1. Open the Command prompt \(Windows\) or Terminal \(Linux or macOS\)\.

   1. Type `aws configure` and press Enter\.

   1. Enter your AWS Access Key Id and AWS Secret Access Key\.

   1. Enter `us-west-2` for the Default Region Name\.

   1. Enter `text` for Default Output Format\.

1. Type the following command:

   ```
   aws discovery start-continuous-export
   ```

**Note**  
Your agents are now running in "continuous export" mode which will enable you to see and work with your discovered data in Amazon Athena\. The first time this is enable it may take up to 30 minutes for your data to appear in Amazon Athena\.

------