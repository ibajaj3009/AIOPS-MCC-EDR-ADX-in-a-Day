# Azure Operator Insights MCC EDR Azure Data Explorer in a day

Welcome to Hands-On Workshop on MCC-EDR Azure Data Explorer. Azure Data Explorer and Kusto queries might be overwhelming in the beginning, but this challenge can be overcome with practice!

# Lab 1: Data Exploration and Visualization
This Lab will focus on enabling the participants to create a free ADX cluster and ingest data into the cluster (one click ingestion of historic data), write some KQL queries and maintain dashboard for visualization.

| Challenge Description | Estimated Time|
|-----------------------|----------------|
|**Challenge 1.**  What is KQL? Starting with the basics of KQL with creation of AOI queries | 1hr 30 min |
|**Challenge 2.** KQL Visualization and Advance KQL Queries Data Exploration| 30 min |
|**Challenge 3.** Visualization through queries and dashboards | 30 min |


Each challenge has a set of tasks that needs to be completed in order to move on to the next challenge.

## What is Azure Operator Insights KQL Service?
1.	AOI KQL Service is a fully managed, high-performance, big data analytics platform that makes it easy to analyse high volumes of data in near real time. The KQL toolbox gives you an end-to-end solution for data ingestion, query, visualization, and management.
2.	By analysing structured, semi-structured, and unstructured data across time series, and by using Machine Learning, KQL service makes it simple to extract key insights, spot patterns and trends, and create forecasting models.
3.	KQL Service is scalable, secure, robust, and enterprise-ready, and is useful for log analytics, time series analytics, IoT, and general-purpose exploratory analytics.

## Scenario
Contoso is a telecommunication enterprise company that is looking to gain some valuable insights from MCC EDR dataset which are getting generated at on premise MCCs. They are looking for an option to stream the data to Azure Data Services and build dashboards and reports. For that, they had deployed Azure Operator Insights solution for ingestion, digestion and streaming cleaned data through KQL Service with an enriched table having 1hr aggregated view of session, flow, cell, and http tables.
 

One of the Contoso's Business Analyst plans to explore data in Azure Operator Insight KQL Service, he used enriched flow table and looks for the below usecases to be built on dashboards:
1.	Total volume computation at application level.
2.	Total active users per day for top 10 applications.
3.	Top devices used by distinct users

**Pre-requisites**: Either a Microsoft account (MSA) or an Azure Active Directory (AAD) identity and permission on KQL Service as data reader.

## Challenge 1: Basics of KQL and Advanced KQL

Learn how to write KQL queries to explore and gain insights from your data

Expected Learning Outcomes:
1.	Learn how to write queries with KQL.
2.	Use KQL to explore data by using the most common operators.

## What is a Kusto query?
A Kusto query is a read-only request to process data and return results. The request is stated in plain text that's easy to read, author, and automate. A Kusto query has one or more query statements and returns data in a tabular or graph format.
AOI service uses the Kusto Query Language service, which is an expressive, intuitive, and highly productive query language. It offers a smooth transition from simple one-liners to complex data processing scripts, and supports querying structured, semi-structured, and unstructured (text search) data. Use the web application to run, review, and share queries and results. You can also send queries programmatically to a REST API endpoint.
KQL Service Uses different visual displays of your data in the native Azure Data Explorer Dashboards. You can also display your results using connectors to some of the leading visualization services, such as Power BI and Grafana.


## What is a tabular statement?
1.	The most common kind of query statement is a tabular expression statement. Both its input and its output consist of tables or tabular datasets.
2.	Tabular statements contain zero or more operators. Each operator starts with a tabular input and returns a tabular output. Operators are sequenced by a pipe (|). Data flows, or is piped, from one operator to the next. The data is filtered or manipulated at each step and then fed into the following step.
3.	It's like a funnel, where you start out with an entire data table. Each time the data passes through another operator, it's filtered, rearranged, or summarized. Because the piping of information from one operator to another is sequential, the query's operator order is important. At the end of the funnel, you're left with a refined output. Let's look at an example query:

enriched_flow_cell_session_http
| take 10 

This query has a single tabular expression statement. The statement begins with a reference to the table and contains the operators take. Each operator is separated by a pipe.

### References:

[KQL cheat sheets](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sqlcheatsheet)


## Challenge 1, Task 0: Journey from SQL to KQL!
The primary language to interact with Kusto is KQL (Kusto Query Language). To make the transition and learning experience easier, you can use 'explain' operator to translate SQL queries to KQL.

```
explain select top 10 * from enriched_flow_cell_session_http
```

Output of the above query will be a corresponding KQL query:


### References:

[SQL to KQL cheat sheets](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sqlcheatsheet)


## Challenge 1, Task 1: Basic KQL queries-explore the data

### Challenge 1, Query 1.0: Schema
User is interested to view the schema of enriched flow table.

enriched_flow_cell_session_http
| getschema

### Challenge 1, Query 1.1: Records selection
User is interested to view the 10 records from enriched 1hr Materialized View, 
1.	where Application and application category are not empty, flow record type is equal to 'EDR_STOP_RECORD' 
2.	And able to view only uplinkOctets, downlink octets, application and application group column values along with the event time flow start details.

 ![image](https://github.com/ibajaj3009/AIOPS-MCC-EDR-ADX-in-a-Day/assets/78459999/1d9c10e8-fd04-44f6-acef-10a86bbf65b6)

KQL queries can be used to filter data and return specific information. Now, you'll learn how to choose specific rows of data.
[where](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/whereoperator)- The where operator filters results that satisfy a certain condition. 

[project operator](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)-Project operator is used to get desired columns.

The [take](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/takeoperator) operator samples any number of records from our table without any order. In the below example, also provide 10 random records with 'take'operator.

```
enriched_flow_cell_session_http_1h_agg      
        | where isnotempty(flowRecord_dpiStringInfo_application) and isnotempty(dpi_group)
        and flowRecord_flowRecordType == 'EDR_STOP_RECORD' 
        | project eventTimeFlow, flowRecord_dataStats_downLinkOctets, flowRecord_dataStats_upLinkOctets,
          dpi_group
        | take 10
```

* [top](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/topoperator)-Returns the first N records sorted by the specified columns. 


````
enriched_flow_cell_session_http_1h_agg      
        | where isnotempty(flowRecord_dpiStringInfo_application) and isnotempty(dpi_group)
        and flowRecord_flowRecordType == 'EDR_STOP_RECORD' 
        | project eventTimeFlow, flowRecord_dataStats_downLinkOctets, flowRecord_dataStats_upLinkOctets,
          dpi_group
        | top 10 by eventTimeFlow desc 
````

## Challenge 1, Query 1.2: Records aggregation
User is interested to view the sum of total volume bytes for maximum and minimum event time window start for all the records in Materialized View of 1h in a single record with the same where conditions which are mentioned above
1.	where Application and application category are not empty, flow record type is equal to 'EDR_STOP_RECORD' 

![image](https://github.com/ibajaj3009/AIOPS-MCC-EDR-ADX-in-a-Day/assets/78459999/3fcce72b-3b56-4ce2-829d-04661f2d80e6)

 [Summarize](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator)-The input rows are arranged into groups having the same values of the *by* expressions. Then the specified aggregation functions are computed over each group, producing a row for each group. The result contains the *by* columns and also at least one column for each computed aggregate. (Some aggregation functions return multiple columns.)

The result has as many rows as there are distinct combinations of *by* values (which may be zero). If there are no group keys provided, the result has a single record.

To summarize over ranges of numeric values, use bin() to reduce ranges to discrete values.
* [Sum function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sum-aggfunction)
* [Min function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/min-aggfunction)
* [Max function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/max-aggfunction)
* [project](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)


````
enriched_flow_cell_session_http_1h_agg  
| where isnotempty(flowRecord_dpiStringInfo_application) and isnotempty(dpi_group) and flowRecord_flowRecordType == 'EDR_STOP_RECORD' 
| summarize maxTapp= max(eventTimeFlow), minTapp= min(eventTimeFlow),
  total_volume_bytes=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)) 
| project maxTapp, minTapp,total_volume_bytes
````

## Challenge 1, Query 1.3: Time bins and let

**While exploring more on dataset, user is now interested to drill down above query and check what would be volume of bytes and bits for each application in every 5 mins for above selected columns view** 

* [bin](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction) - The nearest multiple of roundTo below value. Null values, a null bin size, or a negative bin size will result in null.

|Expression	| Result|
|----|----|
|`bin(4.5,1)`	| 4.0|
|`bin(datetime(1970-05-11 13:45:07),1d)`|	datetime(1970-05-11)|

* [extend](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator) - Create calculated columns and append them to the result set.

* [project](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)
  
*[toscalar]() - Returns a scalar constant value of the evaluated expression.This function is useful for queries that require staged calculations. For example, calculate a total count of events, and then use the result to filter groups that exceed a certain percent of all events.


Try to write the above query by using variables with 'let' to optimize the main query
Declaring variables and using 'let' statements
You can use the 'let' statement to set a variable name equal to an expression or a function, or to create views (virtual, temporary, tables based on the result-set of another KQL query).
let statements are useful for:
•	Breaking up a complex expression into multiple parts, each represented by a variable.
•	Defining constants outside of the query body for readability.
•	Defining a variable once and using it multiple times within a query.
For example, you can use 2 'let' statements to create "LogType" and "TimeBucket" variables with the following values:

````
let lookbackduration=7d; // Specify lookback duration
let maxTapp =  toscalar(enriched_flow_agg_1_min | summarize max(eventTimeWindowStart)); 
let minTapp = maxTapp - lookbackduration;
Remember to include a ";" at the end of your let statement.
*toscalar - Returns a scalar constant value of the evaluated expression. This function is useful for queries that require staged calculations. For example, calculate a total count of events, and then use the result to filter groups that exceed a certain percent of all events.
let lookbackduration=7d; // Specify lookback duration
let maxTapp =  toscalar(enriched_flow_cell_session_http_1h_agg | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
enriched_flow_cell_session_http_1h_agg
| where eventTimeFlow  between (minTapp .. maxTapp) 
    and isnotempty(flowRecord_dpiStringInfo_application) 
    and isnotempty(dpi_group)
    and flowRecord_flowRecordType == 'EDR_STOP_RECORD'  
| summarize total_volume_bytes=(sum(flowRecord_dataStats_downLinkOctets) + sum(flowRecord_dataStats_upLinkOctets)) by bin(eventTimeFlow,1d), flowRecord_dpiStringInfo_application,dpi_group
| order by eventTimeFlow
````

View should have below columns:
 ![image](https://github.com/ibajaj3009/AIOPS-MCC-EDR-ADX-in-a-Day/assets/78459999/dbe451a1-30c9-403e-b2ba-d8593e4d4d6b)


## Challenge 1, Query 1.4: Count distinct values.
One of another user wanted to view the active distinct users count in a day for the last 4 days(eventTimeWindowStart) at application level and group level.

* [count_distinct](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/count-distinct-aggfunction) - Counts unique values specified by the scalar expression per summary group, or the total number of unique values if the summary group is omitted.
* [startofday](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/startofdayfunction) - Returns the start of the day containing the date, shifted by an offset, if provided.
* [format_datetime](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/format-datetimefunction) - Formats a datetime according to the provided format.
* [order by or sort by](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sort-operator) -
* [project](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)

````
let lookbackduration=4d; // Specify lookback duration
let maxTapp =  toscalar(materialized_view('enriched_flow_cell_session_http_1h_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
materialized_view('enriched_flow_cell_session_http_1h_agg')
| where eventTimeFlow  between (minTapp .. maxTapp) and 
isnotempty(flowRecord_dpiStringInfo_application)
    and flowRecord_flowRecordType == 'EDR_STOP_RECORD'  
| summarize distinct_flowRecord_subscriberInfo_imsi= count_distinct(flowRecord_subscriberInfo_imsi)
  by startofday(eventTimeFlow), flowRecord_dpiStringInfo_application, dpi_group
````

## Challenge 2, Query 1.1: Visualization with render operator
Visualize the query with render operator(piechart) for top 10 applications with the distinct count of users in the descending order  in last 4 days and for empty application column, it should be marked as "UNDETECTED".

![image](https://github.com/ibajaj3009/AIOPS-MCC-EDR-ADX-in-a-Day/assets/78459999/c02ca26b-db57-41d5-a7ae-5b990a7fe122)

 
* [render](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/renderoperator?pivots=azuredataexplorer): Instructs the user agent to render a visualization of the query results.

The render operator must be the last operator in the query, and can only be used with queries that produce a single tabular data stream result. 
The render operator does not modify data. It injects an annotation ("Visualization") into the result's extended properties. 

* [isempty](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/isemptyfunction)-Returns true if the argument is an empty string or is null.
* [case](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/casefunction)-Evaluates a list of predicates and returns the first result expression whose predicate is satisfied.
If none of the predicates return true, the result of the else expression is returned. All predicate arguments must be expressions that evaluate to a boolean value. All then arguments and the else argument must be of the same type.
* [top](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/topoperator)
* Looking for slicing an enriched table (containing session and flow records joined aggregated view in 1 min) for total_volume_bytes in 1sec.
* [bin](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction)
* [toint](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/tointfunction)
* [render](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/renderoperator?pivots=azuredataexplorer)
* [isempty](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/isemptyfunction)-Returns true if the argument is an empty string or is null.
* [case](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/casefunction)-Evaluates a list of predicates and returns the first result expression whose 


https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer#displaychartortable

````
let lookbackduration=4d; // Specify lookback duration
let maxTapp =  toscalar(materialized_view('enriched_flow_cell_session_http_1h_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
materialized_view('enriched_flow_cell_session_http_1h_agg')
| where eventTimeFlow  between (minTapp .. maxTapp) 
        and flowRecord_flowRecordType == 'EDR_STOP_RECORD'  
| summarize distinct_flowRecord_subscriberInfo_imsi=count_distinct(flowRecord_subscriberInfo_imsi) by  
  flowRecord_dpiStringInfo_application=case(isempty(flowRecord_dpiStringInfo_application),"UNDETECTED", flowRecord_dpiStringInfo_application)
| order by distinct_flowRecord_subscriberInfo_imsi desc 
| top 10 by distinct_flowRecord_subscriberInfo_imsi
| project  flowRecord_dpiStringInfo_application, distinct_flowRecord_subscriberInfo_imsi
| render piechart
````

https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer#displaychartortable


## Challenge 2, Query 1.2: Join Queries
User is interested to know top 5 devices in last 4 days.

***Performance Optimization Best Practices***: - (https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/best-practices)

summarize operator Use the hint.shufflekey= when the group by keys of the summarize operator are with high cardinality. High cardinality is ideally above 1 million.
join operator-Select the table with the fewer rows to be the first one (left-most in query).
Use in instead of left semi join for filtering by a single column.
Join when left side is small and right side is large Use hint.strategy=broadcast Small refers to up to 100MB of data.
Join when right side is small and left side is large Use the lookup operator instead of the join operator If the right side of the lookup is larger than several tens of MBs, the query will fail.
Join when both sides are too large Use hint.shufflekey= Use when the join key has high cardinality.
Reduce the amount of data being queried.

<img width="765" alt="image" src="https://github.com/ibajaj3009/AIOPS-MCC-EDR-ADX-in-a-Day/assets/78459999/8420ea48-8bce-4007-a388-13952eeea461">

````
let lookbackduration=7d; // Specify lookback duration
let maxTapp =  toscalar(materialized_view('enriched_flow_cell_session_http_1h_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
materialized_view('enriched_flow_cell_session_http_1h_agg')
| where eventTimeFlow  between (minTapp .. maxTapp) 
        and flowRecord_flowRecordType == 'EDR_STOP_RECORD' 
| summarize distinct_flowRecord_subscriberInfo_imsi=count_distinct(flowRecord_subscriberInfo_imsi) by  
sessionRecord_subscriberInfo_imeisv_tac
| order by distinct_flowRecord_subscriberInfo_imsi desc 
| top 10 by distinct_flowRecord_subscriberInfo_imsi
| project sessionRecord_subscriberInfo_imeisv_tac, distinct_flowRecord_subscriberInfo_imsi
| join kind=inner hint.strategy=shuffle inprogress_device_reference
on $left.sessionRecord_subscriberInfo_imeisv_tac == $right.TAC
````

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

