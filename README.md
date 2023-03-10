# AIOPS MCC EDR Azure Data Explorer in a day

Welcome to Hands-On Workshop on MCC-EDR Azure Data Explorer. Azure Data Explorer and Kusto queries might be overwhelming in the beginning, but this challenge can be overcome with practice!


# Lab 1: Cluster Creation, Data Ingestion, Exploration and Visualization
This Lab will focus on enabling the participants to create a free ADX cluster and ingest data into the cluster (one click ingestion of historic data), write some KQL queries and maintain dashboard for visualization.

| Challenge Description | Estimated Time|
|-----------------------|----------------|
|**Challenge 1.** Create a free ADX cluster and load data from Azure Storage | 25 Min |
|**Challenge 2.** Starting with the basics of KQL 1 and Advance KQL Queries Data Exploration|1 hour |
|**Challenge 3.** Visualization through queries and dashboards | 1hour|

Each challenge has a set of tasks that needs to be completed in order to move on to the next challenge.

## What is Data Explorer and when is it a good fit?

1. Data Explorer is a fully managed, high-performance, big data analytics platform that makes it easy to analyze high volumes of data in near real time. The Azure Data Explorer toolbox gives you an end-to-end solution for data ingestion, query, visualization, and management.

2. By analyzing structured, semi-structured, and unstructured data across time series, and by using Machine Learning, Azure Data Explorer makes it simple to extract key insights, spot patterns and trends, and create forecasting models.

3.  Azure Data Explorer is scalable, secure, robust, and enterprise-ready, and is useful for log analytics, time series analytics, IoT, and general-purpose exploratory analytics.

ADX capabilities are extended by other services built on its powerful query language, including Azure Monitor logs, Application Insights, Time Series Insights, and Microsoft Defender for Endpoint.



<img width="780" alt="image" src="https://user-images.githubusercontent.com/78459999/220305163-6f848629-0739-4514-b9b1-b44e824115b1.png">



# Scenario

Contoso is a telecommunication enterprise company that is looking to gain some valuable insights from MCC EDR dataset which are getting generated at on premise MCCs. They are looking for an option to stream the data to Azure Data Services and build dashboards and reports. For that, they had deployed AIOps solution in their Azure environment for ingestion in delta lake format and streamed cleaned data to Azure Data Explorer Service with an enriched table having 1min aggregated view of session and flow tables.


<img width="818" alt="image" src="https://user-images.githubusercontent.com/78459999/222466274-9abf51e7-4e36-4cf5-86d9-638475224679.png">



One of the Contoso's Business Analyst plans to explore data in ADX and to check its ingestion features, explore streaming data to ADX for enriched flow table and looks for the below usecases to be built on dashboards:
 
1. Total volume computation at application level.
2. Total active users per day for top 10 applications.

This workshop walks through the steps of designing, creating, and configuring Azure Data Explorer clusters keeping these requirements in mind.
Once the cluster is deployed, this workshop lists the steps to ingest data into ADX databases and tables using One Click ingestion for a sample run.


**Pre-requisites**
Either a Microsoft account (MSA) or an Azure Active Directory (AAD) identity. This will be used to create free cluster.


## How to start with ADX
Generally, when starting with Azure Data Explorer, you will follow the following steps (ADX Workshop Labs will cover all these steps):

1. Create a free ADX cluster: To use Azure Data Explorer you first create a free cluster. An Azure Data Explorer cluster is the most basic unit.

2. Create database: Each cluster has one or more databases in that cluster. Each Azure Data Explorer cluster can hold up to 10,000 databases and each database up to  10,000 tables.

3. Ingest data: Load data into database tables so that you can run queries against it. Azure Data Explorer supports several ingestion methods.

4. Query data: Azure Data Explorer uses the Kusto Query Language, which is an expressive, intuitive, and highly productive query language. It offers a smooth transition from simple one-liners to complex data processing scripts, and supports querying structured, semi-structured, and unstructured (text search) data. Use the web application to run, review, and share queries and results. You can also send queries programmatically (using an SDK) or to a REST API endpoint.

5. Visualize results: Use different visual displays of your data in the native Azure Data Explorer Dashboards. You can also display your results using connectors to some of the leading visualization services, such as Power BI and Grafana.


Ready to go for Lab 1?

## Challenge 1: Create an ADX cluster and load data from Azure Storage
To use Azure Data Explorer (ADX), you first have to create a free ADX cluster, and create one or more databases in that cluster. Each database has tables. Then you can ingest data into a database so that you can run queries against it.

In this Challenge, you will create a Free cluster and a database. You will run simple KQL query in Kusto Web Explorer (KWE UI).

**Expected Learning Outcomes:**

Create and work with Free ADX cluster.


### Challenge 1, Task 1: Create an ADX cluster and Database
**Create an ADX cluster and Database and review the free cluster home page and the Azure Data Explorer Web UI**
Create your free cluster and database here: https://aka.ms/kustofree.

<img width="515" alt="image" src="https://user-images.githubusercontent.com/78459999/220317774-d3de5eba-1a6c-4c70-bdeb-06ea67280f9b.png">

On your My Cluster page, you'll see the following:

* Your cluster's name, the option to upgrade to a full cluster, and the option to delete the cluster.
* Cluster details like: cluster's location, and URI links for connecting to your cluster via APIs or other tools.
* Quick actions you can take to get started with your cluster.
* A list of databases in your cluster.

If you already have a free cluster and just want to create a new database for this lab, use the Create button in the Create database tile.


### Challenge 1, Task 2: Write your first Kusto query
**Write your first Kusto query to create table and ingest data from the storage**

Before starting this task, 

#### What is a Kusto query?
Azure Data Explorer provides a web experience that enables you to connect to your Azure Data Explorer clusters and write and run Kusto Query Language queries. 

1. The web experience is available in the Azure portal and as a stand-alone web application, the Azure Data Explorer Web UI, which we will use later.

2. A Kusto query is a read-only request to process data and return results. The request is stated in plain text that's easy to read. A Kusto query has one or more query statements and returns data in a tabular or graph format.

**Expected Learning Outcomes:**

Ingest data using one-click ingestion from Azure Blob Storage to your ADX cluster.


#### Challenge 1, Task 2.1: Create the raw table `-logsRaw`

Run the following command to create table:

```
.create table logsRaw(Timestamp:datetime, Source:string, Node:string, Level:string, Component:string, ClientRequestId:string, Message:string, Properties:dynamic) 
```


#### Challenge 1, Task 2.2: Use the ???One-click??? ingestion
**Use the ???One-click??? ingestion UI (User Interface) to ingest data from Azure blob storage**

You need to analyze the logs for Contoso, which are stored in Azure blob storage (Dummy log files, EDR data is already uploaded in the real environment).
Go back to the My Cluster page, click the Ingest button

<img width="506" alt="image" src="https://user-images.githubusercontent.com/78459999/220137856-248fb10f-7289-4c04-9e6d-47a43f7507b2.png">

Make sure the cluster and the database fields are correct. Select Existing table or create new table.

<img width="284" alt="image" src="https://user-images.githubusercontent.com/78459999/220138467-faa62871-ff9a-480c-9772-c0938b54b3d7.png">

Ingest from Storage: Select "Blob container" as the Source type in the Source tab.
In the Link to source, paste the following SAS (Shared Access Signature) URL of the blob storage. SAS URL is a way to provide limited, time-bound access to Azure storage resources such as Blobs.

```
https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D
```

Select one of the Schema defining file (one is autoselected unless you want to change that) and click Next
<img width="415" alt="image" src="https://user-images.githubusercontent.com/78459999/220139142-43690310-b232-4e98-9fee-ccf8ea6641d9.png">

Under Data format, make sure you select 'Keep current table schema' and deselect 'Ignore the first record'
<img width="693" alt="image" src="https://user-images.githubusercontent.com/78459999/220139397-919e7f21-483f-4e3c-8cd4-9b67e7f9f5b8.png">

Wait for the ingestion to be completed, and click Close.

Go to the Query page. Run the following KQL query to verify that data was ingested to the table. The logsRaw table should have 3834012 records

```
logsRaw
| count
```

## Challenge 2: Basics of KQL and Advanced KQL
**Basics of KQL and Advanced KQL (run on the customer EDR Subscription resource for ADX)**
Learn how to write KQL queries to explore and gain insights from your data

**Expected Learning Outcomes:**

1. Learn how to write queries with KQL
2. Use KQL to explore data by using the most common operators

### What is a Kusto query? 
A Kusto query is a read-only request to process data and return results. The request is stated in plain text that's easy to read, author, and automate. A Kusto query has one or more query statements and returns data in a tabular or graph format.

### What is a tabular statement? 
1. The most common kind of query statement is a tabular expression statement. Both its input and its output consist of tables or tabular datasets.

2. Tabular statements contain zero or more operators. Each operator starts with a tabular input and returns a tabular output. Operators are sequenced by a pipe (|). Data flows, or is piped, from one operator to the next. The data is filtered or manipulated at each step and then fed into the following step.

3. It's like a funnel, where you start out with an entire data table. Each time the data passes through another operator, it's filtered, rearranged, or summarized. Because the piping of information from one operator to another is sequential, the query's operator order is important. At the end of the funnel, you're left with a refined output. Let's look at an example query:

```
all_flow_events
| take 10 
```
This query has a single tabular expression statement. The statement begins with a reference to the table logsRaw and contains the operators take. Each operator is separated by a pipe.

### References:

[KQL cheat sheets](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sqlcheatsheet)


### Challenge 2, Task 0: Journey from SQL to KQL!

The primary language to interact with Kusto is KQL (Kusto Query Language). To make the transition and learning experience easier, you can use 'explain' operator to translate SQL queries to KQL.

```
explain select top 10 * from enriched_flow_agg_1_min 
```

Output of the above query will be a corresponding KQL query:
```
enriched_flow_agg_1_min
| project eventTimeWindowStart, eventTimeWindowEnd, flowRecord_keys_sessionId, flowRecord_subscriberInfo_imsi, sessionRecord_subscriberInfo_msisdn, sessionRecord_servingNetworkInfo_apnId, sessionRecord_servingNetworkInfo_nodeAddress, flowRecord_dpiStringInfo_application, recordCount, flowRecord_networkStatsInfo_downlinkFlowPeakThroughput, flowRecord_networkStatsInfo_downlinkFlowPeakThroughput_max, flowRecord_dataStats_downLinkOctets, flowRecord_networkStatsInfo_downlinkFlowActivityDuration, flowRecord_downlinkFlowCalculatedThroughput, flowRecord_networkStatsInfo_uplinkFlowPeakThroughput, flowRecord_networkStatsInfo_uplinkFlowPeakThroughput_max, flowRecord_dataStats_upLinkOctets, flowRecord_networkStatsInfo_uplinkFlowActivityDuration, flowRecord_uplinkFlowCalculatedThroughput, flowRecord_dataStats_downLinkDropOctets, flowRecord_dataStats_upLinkDropOctets, flowRecord_dataStats_downLinkPackets, flowRecord_dataStats_downLinkDropPackets, flowRecord_dataStats_downLinkTotalPackets, flowRecord_downLinkCalculatedPacketLoss_pct_max, flowRecord_dataStats_upLinkPackets, flowRecord_dataStats_upLinkDropPackets, flowRecord_dataStats_upLinkTotalPackets, flowRecord_upLinkCalculatedPacketLoss_pct_max, flowRecord_tcpRetransInfo_downlinkRetransBytes, flowRecord_downlinkCalculatedRetrans_pct_max, flowRecord_tcpRetransInfo_uplinkRetransBytes, flowRecord_uplinkCalculatedRetrans_pct_max, flowRecord_flowEdrRttInfo_downlinkMinRTT, flowRecord_flowEdrRttInfo_uplinkMinRTT, flowRecord_flowEdrRttInfo_downlinkMaxRTT, flowRecord_flowEdrRttInfo_uplinkMaxRTT, flowRecord_flowEdrRttInfo_downlinkAvgRTT_percentile_95, flowRecord_flowEdrRttInfo_uplinkAvgRTT_percentile_95, flowRecord_networkPerfInfo_initialRTTCalculatedTimeSecs, flowRecord_networkPerfInfo_downlinkInitialRTTCalculatedTimeSecs, flowRecord_networkPerfInfo_uplinkInitialRTTCalculatedTimeSecs, flowRecord_networkPerfInfo_initialRTTCalculatedTimeSecsRecordCount, flowRecord_networkPerfInfo_downlinkInitialRTTCalculatedTimeSecsRecordCount, flowRecord_networkPerfInfo_uplinkInitialRTTCalculatedTimeSecsRecordCount, delta_start_time, adx_start_time
| take int(10)
```


### References:

[SQL to KQL cheat sheets](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sqlcheatsheet)



### Challenge 2, Task 1: Basic KQL queries-explore the data

1 min aggregated view is formed from session and flow tables.



### Challenge 2, Query 1.0: Schema

**User is interested to view the schema of 1min aggregated view of enriched flow table.**

```
enriched_flow_agg_1_min
| getschema
```


### Challenge 2, Query 1.1: Records selection

**User is interested to view the records in 1min aggregated enriched flow table, where application is not empty and able to view only session id, uplinkOctets, downlink octets, application column values along with the event time window start and end details**

<img width="471" alt="image" src="https://user-images.githubusercontent.com/78459999/221900494-15eb4acb-68b1-4cdd-80aa-231219082ff6.png">

KQL queries can be used to filter data and return specific information. Now, you'll learn how to choose specific rows of data.

[where](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/whereoperator)- The where operator filters results that satisfy a certain condition. 

[project operator](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)-Project operator is used to get desired columns.

```       
enriched_flow_agg_1_min        
        | where flowRecord_dpiStringInfo_application != ''
        | project eventTimeWindowStart,eventTimeWindowEnd, flowRecord_keys_sessionId,flowRecord_dataStats_upLinkOctets, flowRecord_dataStats_downLinkOctets,  flowRecord_dpiStringInfo_application
```      

The [take](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/takeoperator) operator samples any number of records from our table without any order. In the above example, also provide 10 random records with 'take'operator.
```
enriched_flow_agg_1_min        
        | where flowRecord_dpiStringInfo_application != ''
        | project flowRecord_keys_sessionId,flowRecord_dataStats_upLinkOctets, flowRecord_dataStats_downLinkOctets, flowRecord_dpiStringInfo_application
        | take 10
```


### Challenge 2, Query 1.2: Records aggregation

**User is interested to view the sum of total volume bytes and bits for maximum and minimum event time window start for all the flow records in an enriched table in a single record**


<img width="318" alt="image" src="https://user-images.githubusercontent.com/78459999/221898009-3d56ec12-1c86-46a8-982e-548f0df8fcd0.png">


[Summarize](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator)-The input rows are arranged into groups having the same values of the *by* expressions. Then the specified aggregation functions are computed over each group, producing a row for each group. The result contains the *by* columns and also at least one column for each computed aggregate. (Some aggregation functions return multiple columns.)

The result has as many rows as there are distinct combinations of *by* values (which may be zero). If there are no group keys provided, the result has a single record.

To summarize over ranges of numeric values, use bin() to reduce ranges to discrete values.
* [Sum function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sum-aggfunction)
* [Min function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/min-aggfunction)
* [Max function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/max-aggfunction)
* [project](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)


```
enriched_flow_agg_1_min 
| summarize maxTapp= max(eventTimeWindowStart), minTapp= min(eventTimeWindowStart),
  total_volume_bytes=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)),
  total_volume_bits=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)) * 8 
| project maxTapp, minTapp, total_volume_bits,total_volume_bytes
```


### Challenge 2, Query 1.3: Time bins

**While exploring more on dataset, user is now interested to drill down above query and check what would be volume of bytes and bits for each application in every 5 mins for above selected columns view** 

* [bin](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction) - The nearest multiple of roundTo below value. Null values, a null bin size, or a negative bin size will result in null.

|Expression	| Result|
|----|----|
|`bin(4.5,1)`	| 4.0|
|`bin(datetime(1970-05-11 13:45:07),1d)`|	datetime(1970-05-11)|

* [extend](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator) - Create calculated columns and append them to the result set.

* [project](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)


```
enriched_flow_agg_1_min 
| summarize maxTapp= max(eventTimeWindowStart),
    total_volume_bytes=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)),
    total_volume_bits=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)) * 8 
    by bin(eventTimeWindowStart, 5m), flowRecord_dpiStringInfo_application
| extend minTapp = maxTapp - 5m 
| where eventTimeWindowStart between (minTapp .. maxTapp)
| project minTapp,maxTapp,total_volume_bits, total_volume_bytes, flowRecord_dpiStringInfo_application
```
  
View should have below columns:

<img width="444" alt="image" src="https://user-images.githubusercontent.com/78459999/220405408-b6d28068-fda6-4d1b-ba82-3db323ca10c3.png">




### Challenge 2, Query 1.4: Selecting top applications by volume after sorting volume in descending order

**User wanted to gain insights on the number of records which got aggregated in 5 min view of above queries while getting a view on total volume on application level and sort total volume in descending order while viewing data (there is already column name in enriched view of flow which have record count value on 1 min view) for top 10 applications according to the total volume in bytes.**

<img width="772" alt="image" src="https://user-images.githubusercontent.com/78459999/221907200-8604cb77-3767-42a7-8b54-93dccdb46019.png">

* [sort](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sort-operator) - Sorts the rows of the input table into order by one or more columns.
* [top](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/topoperator)-Returns the first N records sorted by the specified columns. 
* [sum](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/topoperator) 


```
enriched_flow_agg_1_min 
| summarize maxTapp= max(eventTimeWindowStart),
  total_volume_bytes=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)),
  total_volume_bits=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)) * 8 ,
  recordcount_in5min= sum(recordCount)
  by bin(eventTimeWindowStart, 5m), flowRecord_dpiStringInfo_application
| extend minTapp = maxTapp - 5m 
| where eventTimeWindowStart between (minTapp .. maxTapp)
| sort by total_volume_bytes desc
| top 5 by total_volume_bytes 
| project minTapp,maxTapp,total_volume_bits, total_volume_bytes, flowRecord_dpiStringInfo_application, recordcount_in5min
```


### Challenge 2, Query 1.5: Count distinct values

**One of an other user wanted to view the active distinct users count in a day for the last 7 days(eventTimeWindowStart) at application level**

<img width="348" alt="image" src="https://user-images.githubusercontent.com/78459999/221916047-d300e401-7c31-4878-ac65-79a5e3a5a47c.png">

* [count_distinct](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/count-distinct-aggfunction) - Counts unique values specified by the scalar expression per summary group, or the total number of unique values if the summary group is omitted.
* [startofday](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/startofdayfunction) - Returns the start of the day containing the date, shifted by an offset, if provided.
* [format_datetime](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/format-datetimefunction) - Formats a datetime according to the provided format.
* [order by or sort by](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sort-operator) -
* [project](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)


```
enriched_flow_agg_1_min 
| summarize maxTapp= max(eventTimeWindowStart), 
  distinct_flowRecord_subscriberInfo_imsi= count_distinct(flowRecord_subscriberInfo_imsi)
  by startofday(eventTimeWindowStart),flowRecord_dpiStringInfo_application
| extend minTapp=maxTapp-7d
| where eventTimeWindowStart between (minTapp .. maxTapp)
| order by eventTimeWindowStart
| project
    format_datetime(eventTimeWindowStart, "yyyy-MM-dd"),
    distinct_flowRecord_subscriberInfo_imsi,
    flowRecord_dpiStringInfo_application
```


### Challenge 2, Query 1.6: Optimisation with let operator
**Try to write the above query by using variables with 'let' to optimize the main query**

Declaring variables and using 'let' statements 

You can use the 'let' statement to set a variable name equal to an expression or a function, or to create views (virtual, temporary, tables based on the result-set of another KQL query).

[let](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/letstatement) statements are useful for:

* Breaking up a complex expression into multiple parts, each represented by a variable.
* Defining constants outside of the query body for readability.
* Defining a variable once and using it multiple times within a query.

For example, you can use 2 'let' statements to create "LogType" and "TimeBucket" variables with the following values:
```
let lookbackduration=7d; // Specify lookback duration
let maxTapp =  toscalar(enriched_flow_agg_1_min | summarize max(eventTimeWindowStart)); 
let minTapp = maxTapp - lookbackduration;
```
Remember to include a ";" at the end of your let statement.


*[toscalar]() - Returns a scalar constant value of the evaluated expression.This function is useful for queries that require staged calculations. For example, calculate a total count of events, and then use the result to filter groups that exceed a certain percent of all events.

```
let lookbackduration=7d; // Specify lookback duration
let maxTapp =  toscalar(enriched_flow_agg_1_min | summarize max(eventTimeWindowStart));
let minTapp = maxTapp - lookbackduration;
enriched_flow_agg_1_min
| where eventTimeWindowStart between (minTapp .. maxTapp)
| summarize distinct_flowRecord_subscriberInfo_imsi=count_distinct(flowRecord_subscriberInfo_imsi) by startofday(eventTimeWindowStart), flowRecord_dpiStringInfo_application
| order by eventTimeWindowStart
| project
    format_datetime(eventTimeWindowStart, "yyyy-MM-dd"),
    distinct_flowRecord_subscriberInfo_imsi,
    flowRecord_dpiStringInfo_application;
 ```
 
 
 ## Challenge 2, Query 1.7:  Visualization with render operator
 **Visualize the above query with render operator(piechart) for top 10 applications with the distinct count of users in the descending order and for empty application column, it should be marked as "UNDETECTED".**
 
 
 <img width="280" alt="image" src="https://user-images.githubusercontent.com/78459999/221929538-41d72ad2-905d-4491-bcfc-c2797d103791.png">

 
* [render](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/renderoperator?pivots=azuredataexplorer): Instructs the user agent to render a visualization of the query results.

The render operator must be the last operator in the query, and can only be used with queries that produce a single tabular data stream result. 
The render operator does not modify data. It injects an annotation ("Visualization") into the result's extended properties. 

* [isempty](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/isemptyfunction)-Returns true if the argument is an empty string or is null.

* [case](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/casefunction)-Evaluates a list of predicates and returns the first result expression whose predicate is satisfied.
If none of the predicates return true, the result of the else expression is returned. All predicate arguments must be expressions that evaluate to a boolean value. All then arguments and the else argument must be of the same type.

* [top](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/topoperator)

https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer#displaychartortable

```
let lookbackduration=7d; // Specify lookback duration
let maxTapp =  toscalar(enriched_flow_agg_1_min | summarize max(eventTimeWindowStart));
let minTapp = maxTapp - lookbackduration;
enriched_flow_agg_1_min
| where eventTimeWindowStart between (minTapp .. maxTapp)
| summarize distinct_flowRecord_subscriberInfo_imsi=count_distinct(flowRecord_subscriberInfo_imsi) by startofday(eventTimeWindowStart), 
  flowRecord_dpiStringInfo_application=case(isempty(flowRecord_dpiStringInfo_application),"UNDETECTED", flowRecord_dpiStringInfo_application)
| order by distinct_flowRecord_subscriberInfo_imsi desc 
| top 10 by distinct_flowRecord_subscriberInfo_imsi
| render piechart
```
  
 
## Challenge 2, Query 1.8: Timecharts

**User wanted to view flow records timechart for every 10 sec to get total volume in bytes and where the application column is empty '' then in those cases it should be marked as "UNDETECTED".**

Looking for slicing an enriched table (containing session and flow records joined aggregated view in 1 min) for total_volume_bytes in 1sec.
* [bin](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction)
* [toint](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/tointfunction)
* [render](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/renderoperator?pivots=azuredataexplorer)
* [isempty](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/isemptyfunction)-Returns true if the argument is an empty string or is null.
* [case](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/casefunction)-Evaluates a list of predicates and returns the first result expression whose 

```
all_flow_events
| summarize total_volume_bytes=(sum (toint(flowRecord_dataStats_downLinkOctets)) + sum(toint(flowRecord_dataStats_upLinkOctets))) by bin(eventTime,10s)
 , flowRecord_dpiStringInfo_application=case(isempty(flowRecord_dpiStringInfo_application),"UNDETECTED", flowRecord_dpiStringInfo_application)
| top 10 by total_volume_bytes
| render timechart
```

Such kind of view:

<img width="597" alt="image" src="https://user-images.githubusercontent.com/78459999/221234303-357aea00-05a2-41af-b225-4b827108c6ce.png">


# Challenge 3: Visualize with ADX dashboards

## Challenge 3, Task 1: Prepare interactive dashboards with ADX Dashboard 

Azure Data Explorer is a fast and highly scalable data exploration service for log and telemetry data. Explore your data from end-to-end in the Azure Data Explorer web application, starting with data ingestion, running queries, and ultimately building dashboards.

Pin a query from the query tab of the web UI.

<img width="132" alt="image" src="https://user-images.githubusercontent.com/78459999/221937579-a924597f-47ba-48df-a00e-959d9e9f3b01.png">



To pin a query:

Create and run the query whose output you want to visualize in the dashboard.

Select Share > Pin to dashboard.

In the Pin to dashboard pane:

1. Provide a Tile name.
2. The Data source name is auto populated from the query data source.
3. Select Use existing data source if possible.
4. Select Create new.
5. Enter Dashboard name.
6. Select the View dashboard after creation checkbox (if it's a new dashboard).
7. Select Pin

**User wanted to view above created queries on dashboard pages.**

Screenshot of the Pin to dashboard pane.

 <img width="815" alt="image" src="https://user-images.githubusercontent.com/78459999/221936404-f361c2c1-dbec-476a-b7ca-7cf828ca5588.png">

Reference:

https://docs.microsoft.com/en-us/azure/data-explorer/azure-data-explorer-dashboards

