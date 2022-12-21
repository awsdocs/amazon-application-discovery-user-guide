# Appendix: Transition from Discovery Connector to Agentless Collector<a name="discovery-collector-transition"></a>

This section describes how to transition from AWS Agentless Discovery Connector \(Discovery Connector\) to Application Discovery Service Agentless Collector \(Agentless Collector\)\.

 

We recommended that all customers currently using Discovery Connector transition to the new Agentless Collector\. Customer's currently using Discovery Connector can continue to do so until Aug 31, 2023\. After this date, data sent to AWS Application Discovery Service by Discovery Connector will not be processed\. Going forward, Application Discovery Service Agentless Collector is the supported discovery tool for agentless data collection by AWS Application Discovery Service\.

To learn how to start using Agentless Collector, see [Getting started with Agentless Collector](agentless-collector-gs.md)\. 

After the Agentless Collector is deployed, you can delete the Discovery Connector virtual machine\. All data previously collected will continue to be available in AWS Migration Hub \(Migration Hub\)\.