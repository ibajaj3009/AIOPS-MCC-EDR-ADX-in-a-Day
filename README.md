# AIOPS MCC EDR Azure Data Explorer in a day

Welcome to Hands on Workshop on MCC-EDR Azure Data Explorer workshop.These challenges are discover by doing experience.

## After the workshop, you will better understand how to 

1. Set up Free Azure DataExplorer cluster and database
2. Ingest data into ADX Database
3. Run powerful kql queries to explore data
4. Visualize Data in ADX Dashboards

## Lab 1: Cluster Creation, Data Ingestion and Exploration 
This Lab will focus on enabling the participants to Create a free ADX cluster, and ingest data into the cluster - One click ingestion of historic data , write some KQL queries and maintain dashboard for visualization

## What is Data Explorer and when is it a good fit?

1. Data Explorer is a fully managed, high-performance, big data analytics platform that makes it easy to analyze high volumes of data in near real time. The Azure Data Explorer toolbox gives you an end-to-end solution for data ingestion, query, visualization, and management.
2. By analyzing structured, semi-structured, and unstructured data across time series, and by using Machine Learning, Azure Data Explorer makes it simple to extract key insights, spot patterns and trends, and create forecasting models.
3.  Azure Data Explorer is scalable, secure, robust, and enterprise-ready, and is useful for log analytics, time series analytics, IoT, and general-purpose exploratory analytics.

ADX capabilities are extended by other services built on its powerful query language, including Azure Monitor logs, Application Insights, Time Series Insights, and Microsoft Defender for Endpoint.

<img width="780" alt="image" src="https://user-images.githubusercontent.com/78459999/220305163-6f848629-0739-4514-b9b1-b44e824115b1.png">

## Scenario

Contoso is an telecommunication enterprise company that is looking to gain some valuable insights from MCC EDR dataset which are getting generated at on premise MCC server.They are looking for an option to stream the data on Azure Data Services and build the dashboard and reports. For that, they had deployed AIOps solution in their Azure environment for ingestion in delta lake format and streamed cleaned data in Azure Data Explorer Service with an enriched table having 1min aggregrated view of session and flow tables.

<img width="793" alt="image" src="https://user-images.githubusercontent.com/78459999/220313676-819aea26-8c3d-43a9-9b11-417b1b983bbe.png">

One of the Contoso's Business Analyst planning to explore data in ADX and check its ingestion features, explore streaming data in ADX for enriched flow table and looking for the below usecases to be build on dashboards:

1. Total active users per day per application.
2. Total volume computation at Application level.

This workshop walks through the steps in designing, creating, and configuring Azure Data Explorer clusters keeping in mind these requirements.Once the cluster is deployed, this workshop enlists the steps to ingest data into ADX databases and tables using One Click ingestion for sample run.

### Pre-requisites
Either a Microsoft account (MSA) or an Azure Active Directory (AAD) identity. This will be used to create free cluster.


## Challenge 1, Task 3: Ingest data from Azure Storage Account

Data ingestion to ADX is the process used to load data records from one or more sources into a table in your ADX cluster. Once ingested, the data becomes available for query.

ADX supports several ingestion methods, including ingestion tools, connectors and plugins, Azure managed pipelines, programmatic ingestion using SDKs, and direct access to ingestion.

## How to start with ADX
Generally, when starting with Azure Data Explorer, you will follow the following steps (ADX Workshop Labs will cover all these steps):

1. Create a free ADX cluster: To use Azure Data Explorer you first create a free cluster. An Azure Data Explorer cluster is the most basic unit.
2. Create database: Each cluster has one or more databases in that cluster. Each Azure Data Explorer cluster can hold up to 10,000 databases and each database up to 10,000 tables.
3. Ingest data: Load data into database tables so that you can run queries against it. Azure Data Explorer supports several ingestion methods.
4. Query data: Azure Data Explorer uses the Kusto Query Language, which is an expressive, intuitive, and highly productive query language. It offers a smooth transition from simple one-liners to complex data processing scripts, and supports querying structured, semi-structured, and unstructured (text search) data. Use the web application to run, review, and share queries and results. You can also send queries programmatically (using an SDK) or to a REST API endpoint.
5. Visualize results: Use different visual displays of your data in the native Azure Data Explorer Dashboards. You can also display your results using connectors to some of the leading visualization services, such as Power BI and Grafana.

Ready to go? Click on the below links to start the challenges

## Lab 1: Cluster Creation, Data Ingestion, Exploration and Visualization

This Lab is organized into the following 4 Challenges:

Challenge	Description	Est. Time
Challenge 1	Create a free ADX cluster	and load Data from Azure Storage	25 Min
Challenge 2	Starting with the basics of KQL	1 and Advance KQL Queries Data Exploration	1 hour
Challenge 3 Visualization through queries and dashboards 1hour

Each challenge has a set of tasks that need to be completed in order to move on to the next challenge

## Challenge 1: Create an ADX cluster and load Data from Azure Storage(the sample data to explore ADX features with one click ingestion.AIOps handles ingestion)
To use Azure Data Explorer (ADX), you first have to create a free ADX cluster, and create one or more databases in that cluster. Each database has tables. Then you can ingest data into a database so that you can run queries against it.

In this Challenge, you will create a Free cluster and a database. You will run simple KQL query in Kusto Web Explorer (KWE UI).

## Expected Learning Outcomes:

Create and work with Free ADX cluster.

### Challenge 1, Task 1: Create an ADX cluster and Database and review the free cluster home page and the Azure Data Explorer Web UI

Create your free cluster and database here: https://aka.ms/kustofree.

<img width="515" alt="image" src="https://user-images.githubusercontent.com/78459999/220317774-d3de5eba-1a6c-4c70-bdeb-06ea67280f9b.png">

On your My Cluster page, you'll see the following:

Your cluster's name, the option to upgrade to a full cluster, and the option to delete the cluster.
Cluster details like: cluster's location, and URI links for connecting to your cluster via APIs or other tools.
Quick actions you can take to get started with your cluster.
A list of databases in your cluster.
If you already have a free cluster and just want to create a new database for this lab, use the Create button in the Create database tile.




## Challenge 1, Task 2: Write your Kusto query to create table and ingest data from the storage

Before starting this task, 

## What is a Kusto query?
Azure Data Explorer provides a web experience that enables you to connect to your Azure Data Explorer clusters and write and run Kusto Query Language queries. 

1. The web experience is available in the Azure portal and as a stand-alone web application, the Azure Data Explorer Web UI, which we will use later.

2. A Kusto query is a read-only request to process data and return results. The request is stated in plain text that's easy to read. A Kusto query has one or more query statements and returns data in a tabular or graph format.

Expected Learning Outcomes:

Ingest data using one-click ingestion from Azure Blob Storage to your ADX cluster.

## Challenge 1, Task 2.1: Create the raw table -logsRaw

Run the following command to create table:

.create table logsRaw(Timestamp:datetime, Source:string, Node:string, Level:string, Component:string, ClientRequestId:string, Message:string, Properties:dynamic) 

## Challenge 1, Task 2.2: Use the “One-click” UI (User Interface) to ingest data from Azure blob storage
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

## Challenge 2: Query 1.0 : User is interested to view the schema of 1min aggregrated view of enriched flow table.

enriched_flow_agg_1_min
| getschema


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
        | take 10

## Challenge 2: Query 1.2 : User is interested to view the sum of total volume bytes or bites for maximum and minimum event time window start for all the flow records in enriched table on application level

Summarize-https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sum-aggfunction
Sum function-https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sum-aggfunction
Min function-https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/min-aggfunction
Max function-https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/max-aggfunction

enriched_flow_agg_1_min 
| summarize maxTapp= max(eventTimeWindowStart), minTapp= min(eventTimeWindowStart),
    total_volume_bytes=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)),
    total_volume_bits=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)) * 8 
| project maxTapp,minTapp, total_volume_bits,total_volume_bytes

## Challenge 2: Query 1.3 :  While exploring more on dataset, user is now interested to drill down above query and check what would be volume of bytes and bites for each application in every 5 mins

bin-The nearest multiple of roundTo below value. Null values, a null bin size, or a negative bin size will result in null.https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction
extend-Create calculated columns and append them to the result set.https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator
project- https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator

Expression	Result
bin(4.5,1)	4.0
bin(time(16d),7d)	14d
bin(datetime(1970-05-11 13:45:07),1d)	datetime(1970-05-11)

enriched_flow_agg_1_min 
| summarize maxTapp= max(eventTimeWindowStart),
    total_volume_bytes=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)),
    total_volume_bits=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)) * 8 
    by bin(eventTimeWindowStart, 5m), flowRecord_dpiStringInfo_application
| extend minTapp = maxTapp - 5m 
| where eventTimeWindowStart between (minTapp .. maxTapp)
| project minTapp,maxTapp,total_volume_bits, total_volume_bytes, flowRecord_dpiStringInfo_application
  
View should have below columns:
<img width="444" alt="image" src="https://user-images.githubusercontent.com/78459999/220405408-b6d28068-fda6-4d1b-ba82-3db323ca10c3.png">

## Challenge 2: Query 1.4 : User wanted to gain insights on the number of records which got aggregrated in 5 min view of above queries while getting a view on total volume on application level.(there is already column name in enriched view of flow which had record count value on 1 min view)

enriched_flow_agg_1_min 
| summarize maxTapp= max(eventTimeWindowStart),
    total_volume_bytes=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)),
    total_volume_bits=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)) * 8 ,
    recordcount_in5min= sum(recordCount)
    by bin(eventTimeWindowStart, 5m), flowRecord_dpiStringInfo_application
| extend minTapp = maxTapp - 5m 
| where eventTimeWindowStart between (minTapp .. maxTapp)
| project minTapp,maxTapp,total_volume_bits, total_volume_bytes, flowRecord_dpiStringInfo_application, recordcount_in5min

