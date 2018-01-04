# Service Limitations<a name="ads_service_limits"></a>

Application Discovery Service has the following limitations for agentless and agent\-based discovery\.

**Agentless discovery**  
The service limits you to 10 GB of data per day\. If you reach this limit, the service won't process any more data for that day\. If you frequently reach this limit, contact [AWS Support](https://aws.amazon.com/premiumsupport/) about extending the limit\. 

**Agent\-based discovery**  
Agent\-based discovery currently has the following limitations\.

+ The service enforces the following maximum limits:

  + 1000 active agents \(agents that are collecting and sending data to Application Discovery Service in the cloud\)\.

  + 10,000 inactive agents \(agents that are responsive but not collecting data\)\.

  + 10 GB of data per day \(collected by all agents associated with a given AWS account\)\.

  + 90 days of data storage \(after which the data is purged\)\.