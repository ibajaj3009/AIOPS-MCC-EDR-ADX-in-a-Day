# AIOPS MCC EDR Azure Data Explorer in a day

## Challenge 1, Task 3: Ingest data from Azure Storage Account

Data ingestion to ADX is the process used to load data records from one or more sources into a table in your ADX cluster. Once ingested, the data becomes available for query.

ADX supports several ingestion methods, including ingestion tools, connectors and plugins, Azure managed pipelines, programmatic ingestion using SDKs, and direct access to ingestion.


Before starting this task, 

## What is a Kusto query?
Azure Data Explorer provides a web experience that enables you to connect to your Azure Data Explorer clusters and write and run Kusto Query Language queries. 

1. The web experience is available in the Azure portal and as a stand-alone web application, the Azure Data Explorer Web UI, which we will use later.

2. A Kusto query is a read-only request to process data and return results. The request is stated in plain text that's easy to read. A Kusto query has one or more query statements and returns data in a tabular or graph format.

Expected Learning Outcomes:

Ingest data using one-click ingestion from Azure Blob Storage to your ADX cluster.

## Challenge 1, Task 3.1: Create the raw table -logsRaw

Run the following command to create table:

.create table logsRaw(Timestamp:datetime, Source:string, Node:string, Level:string, Component:string, ClientRequestId:string, Message:string, Properties:dynamic) 

## Challenge 1, Task 3.2: Use the “One-click” UI (User Interface) to ingest data from Azure blob storage
You need to analyze the logs for Contoso, which are stored in Azure blob storage.
Go back to the My Cluster page, click the Ingest button
<img width="506" alt="image" src="https://user-images.githubusercontent.com/78459999/220137856-248fb10f-7289-4c04-9e6d-47a43f7507b2.png">

Make sure the cluster and the Database fields are correct. Select Existing table or create new table.
<img width="284" alt="image" src="https://user-images.githubusercontent.com/78459999/220138467-faa62871-ff9a-480c-9772-c0938b54b3d7.png">

Ingest from Storage:
Select "Blob container" as the Source type in the Source tab.
In the Link to source, paste the following SAS (Shared Access Signature) URL of the blob storage. SAS URL is a way to provide limited, time-bound access to Azure storage resources such as Blobs.

https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/?sp=rl&st=2022-08-18T00:00:00Z&se=2030-

Select one of the Schema defining file (one is autoselected unless you want to change that) and click Next
<img width="415" alt="image" src="https://user-images.githubusercontent.com/78459999/220139142-43690310-b232-4e98-9fee-ccf8ea6641d9.png">

Under Data format, make sure you select 'Keep current table schema' and deselect 'Ignore the first record'
<img width="693" alt="image" src="https://user-images.githubusercontent.com/78459999/220139397-919e7f21-483f-4e3c-8cd4-9b67e7f9f5b8.png">

Wait for the ingestion to be completed, and click Close.

Go to the Query page. Run the following KQL query to verify that data was ingested to the table. The logsRaw table should have 3834012 records
logsRaw
  | count

## Challenge 2 : Basics of KQL and Advanced KQL( run on the customer EDR Subscription resource for ADX)

Learn how to write KQL queries to explore and gain insights from your data

Expected Learning Outcomes:

1. Learn how to write queries with KQL
2. Use KQL to explore data by using the most common operators

## What is a Kusto query? 
A Kusto query is a read-only request to process data and return results. The request is stated in plain text that's easy to read, author, and automate. A Kusto query has one or more query statements and returns data in a tabular or graph format.

## What is a tabular statement? 
1. The most common kind of query statement is a tabular expression statement. Both its input and its output consist of tables or tabular datasets.

2. Tabular statements contain zero or more operators. Each operator starts with a tabular input and returns a tabular output. Operators are sequenced by a pipe (|). Data flows, or is piped, from one operator to the next. The data is filtered or manipulated at each step and then fed into the following step.

3. It's like a funnel, where you start out with an entire data table. Each time the data passes through another operator, it's filtered, rearranged, or summarized. Because the piping of information from one operator to another is sequential, the query's operator order is important. At the end of the funnel, you're left with a refined output. Let's look at an example query:

all_flow_events
| take 10 
This query has a single tabular expression statement. The statement begins with a reference to the table logsRaw and contains the operators take. Each operator is separated by a pipe.

### References:

KQL cheat sheets


## Challenge 2: Task 0 : Journey from SQL to KQL!

The primary language to interact with Kusto is KQL (Kusto Query Language). To make the transition and learning experience easier, you can use 'explain' operator to translate SQL queries to KQL.

explain select top 10 * from enriched_flow_agg_1_min 

Output of the above query will be a corresponsing KQL query
"Query": 
enriched_flow_agg_1_min
| project eventTimeWindowStart, eventTimeWindowEnd, flowRecord_keys_sessionId, flowRecord_subscriberInfo_imsi, sessionRecord_subscriberInfo_msisdn, sessionRecord_servingNetworkInfo_apnId, sessionRecord_servingNetworkInfo_nodeAddress, flowRecord_dpiStringInfo_application, recordCount, flowRecord_networkStatsInfo_downlinkFlowPeakThroughput, flowRecord_networkStatsInfo_downlinkFlowPeakThroughput_max, flowRecord_dataStats_downLinkOctets, flowRecord_networkStatsInfo_downlinkFlowActivityDuration, flowRecord_downlinkFlowCalculatedThroughput, flowRecord_networkStatsInfo_uplinkFlowPeakThroughput, flowRecord_networkStatsInfo_uplinkFlowPeakThroughput_max, flowRecord_dataStats_upLinkOctets, flowRecord_networkStatsInfo_uplinkFlowActivityDuration, flowRecord_uplinkFlowCalculatedThroughput, flowRecord_dataStats_downLinkDropOctets, flowRecord_dataStats_upLinkDropOctets, flowRecord_dataStats_downLinkPackets, flowRecord_dataStats_downLinkDropPackets, flowRecord_dataStats_downLinkTotalPackets, flowRecord_downLinkCalculatedPacketLoss_pct_max, flowRecord_dataStats_upLinkPackets, flowRecord_dataStats_upLinkDropPackets, flowRecord_dataStats_upLinkTotalPackets, flowRecord_upLinkCalculatedPacketLoss_pct_max, flowRecord_tcpRetransInfo_downlinkRetransBytes, flowRecord_downlinkCalculatedRetrans_pct_max, flowRecord_tcpRetransInfo_uplinkRetransBytes, flowRecord_uplinkCalculatedRetrans_pct_max, flowRecord_flowEdrRttInfo_downlinkMinRTT, flowRecord_flowEdrRttInfo_uplinkMinRTT, flowRecord_flowEdrRttInfo_downlinkMaxRTT, flowRecord_flowEdrRttInfo_uplinkMaxRTT, flowRecord_flowEdrRttInfo_downlinkAvgRTT_percentile_95, flowRecord_flowEdrRttInfo_uplinkAvgRTT_percentile_95, flowRecord_networkPerfInfo_initialRTTCalculatedTimeSecs, flowRecord_networkPerfInfo_downlinkInitialRTTCalculatedTimeSecs, flowRecord_networkPerfInfo_uplinkInitialRTTCalculatedTimeSecs, flowRecord_networkPerfInfo_initialRTTCalculatedTimeSecsRecordCount, flowRecord_networkPerfInfo_downlinkInitialRTTCalculatedTimeSecsRecordCount, flowRecord_networkPerfInfo_uplinkInitialRTTCalculatedTimeSecsRecordCount, delta_start_time, adx_start_time
| take int(10)

### References:

SQL to KQL cheat sheets - aka.ms/SQL2KQL

## Challenge 2: Task 1 : Basic KQL queries-explore the data

1 min aggregrated view is present from session and flow tables.

## Challenge 2: Query 1.1 : User is interested to view the records in 1min agregrated enriched flow table where application is not empty and able to view only session id, uplinkOctets, downlink octets value with event time window start and end details

KQL queries can be used to filter data and return specific information. Now, you'll learn how to choose specific rows of data.
The where operator filters results that satisfy a certain condition- 

To get desired columns project operator is used: project operator - https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator
       
enriched_flow_agg_1_min        
        | where flowRecord_dpiStringInfo_application != ''
        | project eventTimeWindowStart,eventTimeWindowEnd, flowRecord_keys_sessionId,flowRecord_dataStats_upLinkOctets, flowRecord_dataStats_downLinkOctets,  flowRecord_dpiStringInfo_application
      

The 'take' operator samples any number of records from our table without any order. In the above example, we asked to provide 10 random records.

enriched_flow_agg_1_min        
        | where flowRecord_dpiStringInfo_application != ''
        | project flowRecord_keys_sessionId,flowRecord_dataStats_upLinkOctets, flowRecord_dataStats_downLinkOctets, flowRecord_dpiStringInfo_application
        |take 10
