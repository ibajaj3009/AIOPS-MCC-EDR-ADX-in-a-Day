# AIOPS AOI MCC EDR Azure Data Explorer in a day-Advcance AOI Insight usecases

Welcome to Hands-On Workshop on MCC-EDR Azure Data Explorer. Azure Data Explorer and Kusto queries might be overwhelming in the beginning, but this challenge can be overcome with practice!


# Lab 2: AOI Advance Exploration and Visualization
This Lab will focus on enabling the participants to write some advance KQL queries and maintain dashboard for visualization and manage accesses

| Challenge Description | Estimated Time|
|-----------------------|----------------|
|**Challenge 1.** Create KQLs for advance AOI Insights | 90 Min |
|**Challenge 2.** Visualization through queries and dashboards|15min |
|**Challenge 3.** Data Access mangement and security|15min|

Each challenge has a set of tasks that needs to be completed in order to move on to the next challenge.

## What is Data Explorer and when is it a good fit?

1. Data Explorer is a fully managed, high-performance, big data analytics platform that makes it easy to analyze high volumes of data in near real time. The Azure Data Explorer toolbox gives you an end-to-end solution for data ingestion, query, visualization, and management.

2. By analyzing structured, semi-structured, and unstructured data across time series, and by using Machine Learning, Azure Data Explorer makes it simple to extract key insights, spot patterns and trends, and create forecasting models.

3.  Azure Data Explorer is scalable, secure, robust, and enterprise-ready, and is useful for log analytics, time series analytics, IoT, and general-purpose exploratory analytics.

ADX capabilities are extended by other services built on its powerful query language, including Azure Monitor logs, Application Insights, Time Series Insights, and Microsoft Defender for Endpoint.



<img width="780" alt="image" src="https://user-images.githubusercontent.com/78459999/220305163-6f848629-0739-4514-b9b1-b44e824115b1.png">



# Scenario

Contoso is a telecommunication enterprise company that is looking to gain some valuable insights from MCC EDR dataset which are getting generated at on premise MCCs. They are looking for an option to stream the data to Azure Data Services and build dashboards and reports. For that, they had deployed AIOps solution in their Azure environment for ingestion in delta lake format and streamed cleaned data to Azure Data Explorer Service with an enriched table having 1min aggregated view of session and flow tables.

![image](https://github.com/microsoft/aoi-demos/assets/78459999/aa6a89a3-dced-4efe-94a5-9708a0781753)




**Pre-requisites**
Either a Microsoft account (MSA) or an Azure Active Directory (AAD) identity. This will be used to create free cluster.




Ready to go for Lab 2?


### Challenge 1, Task 1: Journey from SQL to KQL!
Before starting this task, 

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



### Challenge 1, Task 1:Users are interested in viewing which are the top 10 Gaming applications according to their volume in the last 3 days.

KQL queries can be used to filter data and return specific information. Now, you'll learn how to choose specific rows of data.

[where](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/whereoperator)- The where operator filters results that satisfy a certain condition. 

[project operator](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)-Project operator is used to get desired columns.

[top](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/topoperator)-Returns the first N records sorted by the specified columns. 

[sum](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/topoperator) 

[Summarize](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator)-The input rows are arranged into groups having the same values of the *by* expressions. Then the specified aggregation functions are computed over each group, producing a row for each group. The result contains the *by* columns and also at least one column for each computed aggregate. (Some aggregation functions return multiple columns.)

The result has as many rows as there are distinct combinations of *by* values (which may be zero). If there are no group keys provided, the result has a single record.

To summarize over ranges of numeric values, use bin() to reduce ranges to discrete values.
* [Sum function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sum-aggfunction)
* [Min function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/min-aggfunction)
* [Max function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/max-aggfunction)
* [project](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)


[extend](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator) - Create calculated columns and append them to the result set.

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
let lookbackduration= 3d; // Specify lookback duration                                                                  
let maxTapp = toscalar( materialized_view('enriched_flow_cell_session_http_5m_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
materialized_view('enriched_flow_cell_session_http_5m_agg')
| where eventTimeFlow  between (minTapp .. maxTapp)  
  and flowRecord_flowRecordType == 'EDR_STOP_RECORD'
  and dpi_group like 'Gaming'
| summarize flowRecord_dataStats_upLinkOctets = sum(flowRecord_dataStats_upLinkOctets),
flowRecord_dataStats_downLinkOctets = sum(flowRecord_dataStats_downLinkOctets)
by flowRecord_dpiStringInfo_application
| extend totalvolume = flowRecord_dataStats_upLinkOctets + flowRecord_dataStats_downLinkOctets 
| top 10 by totalvolume
```



### Challenge 1, Task 2: Users are interested in viewing the Gaming applications distinct users per day in the last 3 days on each. 
### a.	APP
### b.	MCC
### c.	SITE
### d.	CELL

[bin]-(https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction)

[count_distinct]-(https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/count-distinct-aggfunction)


```
let lookbackduration= 3d; // Specify lookback duration                                                                  
let maxTapp = toscalar( materialized_view('enriched_flow_cell_session_http_5m_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
materialized_view('enriched_flow_cell_session_http_5m_agg')
| where eventTimeFlow  between (minTapp .. maxTapp)  
  and flowRecord_flowRecordType == 'EDR_STOP_RECORD'
  and dpi_group like 'Gaming'
| summarize distinct_users_count= count_distinct(flowRecord_subscriberInfo_imsi)
by startofday(eventTimeFlow),flowRecord_dpiStringInfo_application
```

```
let lookbackduration= 3d; // Specify lookback duration                                                                  
let maxTapp = toscalar( materialized_view('enriched_flow_cell_session_http_5m_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
materialized_view('enriched_flow_cell_session_http_5m_agg')
| where eventTimeFlow  between (minTapp .. maxTapp)  
  and flowRecord_flowRecordType == 'EDR_STOP_RECORD'
  and dpi_group like 'Gaming'
| summarize distinct_users_count= count_distinct(flowRecord_subscriberInfo_imsi)
by startofday(eventTimeFlow), flowRecord_gatewayInfo_gwNodeID
```

```
let lookbackduration= 3d; // Specify lookback duration                                                                  
let maxTapp = toscalar( materialized_view('enriched_flow_cell_session_http_5m_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
materialized_view('enriched_flow_cell_session_http_5m_agg')
| where eventTimeFlow  between (minTapp .. maxTapp)  
  and flowRecord_flowRecordType == 'EDR_STOP_RECORD'
  and dpi_group like 'Gaming'
| summarize distinct_users_count= count_distinct(flowRecord_subscriberInfo_imsi)
by startofday(eventTimeFlow), LGA
```

```
let lookbackduration= 3d; // Specify lookback duration                                                                  
let maxTapp = toscalar( materialized_view('enriched_flow_cell_session_http_5m_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
materialized_view('enriched_flow_cell_session_http_5m_agg')
| where eventTimeFlow  between (minTapp .. maxTapp)  
  and flowRecord_flowRecordType == 'EDR_STOP_RECORD'
  and dpi_group like 'Gaming'
| summarize distinct_users_count= count_distinct(flowRecord_subscriberInfo_imsi)
by startofday(eventTimeFlow), start_cell
```

### Challenge 1, Task 3:Users are interested in viewing the top 10 cells every day where gaming volume is getting generated in a large number and top 5 applications volume in these cell SITES along with unique users count per day details in the last 3 days.

[top-nested]-https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/topnestedoperator


[project-away]-https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/projectawayoperator


```       
let lookbackduration= 2d; // Specify lookback duration                                                                  
let maxTapp = toscalar( materialized_view('enriched_flow_cell_session_http_5m_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
materialized_view('enriched_flow_cell_session_http_5m_agg')
| where eventTimeFlow  between (minTapp .. maxTapp)  
  and flowRecord_flowRecordType == 'EDR_STOP_RECORD'
  and isnotempty(flowRecord_dpiStringInfo_application) and isnotempty(start_cell)
  and isnotempty(LGA) and dpi_group like 'Gaming'
| summarize flowRecord_dataStats_upLinkOctets = sum(flowRecord_dataStats_upLinkOctets),
flowRecord_dataStats_downLinkOctets = sum(flowRecord_dataStats_downLinkOctets),
distinct_flowRecord_subscriberInfo_imsi=count_distinct(flowRecord_subscriberInfo_imsi)
by startofday(eventTimeFlow),start_cell,LGA, flowRecord_dpiStringInfo_application
| extend totalvolume = flowRecord_dataStats_upLinkOctets + flowRecord_dataStats_downLinkOctets 
| top-nested of eventTimeFlow by max(1), 
  top-nested 10 of start_cell by cellvolume=sum(totalvolume),
  top-nested 5 of flowRecord_dpiStringInfo_application by dpivolume=sum(totalvolume), //with others = "Others" 
  top-nested of distinct_flowRecord_subscriberInfo_imsi by distinctimsi=max(1)
| project-away aggregated_eventTimeFlow, distinctimsi
```


### Challenge 2, Task 1: Total volume and distinct users by device TAC per hour for HRV Cells with Make and Model details.

[make_set]-(https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/makeset-aggfunction)
Creates a dynamic array of the set of distinct values that expr takes in the group.

[make_list]-(https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/makelist-aggfunction)
Creates a dynamic array of all the values of expr in the group.

[join]-(https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/joinoperator?pivots=azuredataexplorer)

```
let selected_mbnl = dynamic([
    'INV010', 'GLC106', 'NLA122', 'NLA034', 'ERE009', 'EDC077', 'INV006', 'MLO015', 'GLC146', 'NLA186',
    'GLC140', 'NLA123', 'GLC155', 'GLC235', 'GLC021', 'GLC104', 'NLA136', 'NLA109', 'GLC100', 'NLA035',
    'NLA066', 'GLC075', 'RFW069', 'RFW045', 'EAY016', 'INV004', 'RFW059', 'GLC022', 'EDC298', 'NLA075',
    'NLA135', 'SAY011', 'NAY011', 'NLA086', 'GLC179', 'RFW043', 'NAY015', 'GLC183', 'INV008', 'RFW002',
    'SLN080', 'HLD090', 'EAY009', 'NLA060', 'RFW076', 'NAY026', 'SAY012', 'NAY032', 'GLC118', 'GLC182',
    'RFW026' 
]);
let selected_ecgi = toscalar(
    table('cell_reference')
    | where MBNL_ID in (selected_mbnl)
    | summarize make_set(ecgi)
);
let lookbackduration= 2d; // Specify lookback duration                                                                  
let maxTapp = toscalar( materialized_view('enriched_flow_cell_session_http_5m_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
let TAC = toscalar(
table('inprogress_device_reference')
| summarize make_list(TAC));
let hrvcelltacvolume=materialized_view('enriched_flow_cell_session_http_5m_agg')
| where eventTimeFlow  between (minTapp .. maxTapp)  
  and flowRecord_flowRecordType == 'EDR_STOP_RECORD'
  and sessionRecord_subscriberInfo_imeisv_tac in (TAC) 
  and start_cell in (selected_ecgi)
  and isnotempty(LGA) 
| summarize flowRecord_dataStats_upLinkOctets = sum(flowRecord_dataStats_upLinkOctets),
flowRecord_dataStats_downLinkOctets = sum(flowRecord_dataStats_downLinkOctets),
distinct_flowRecord_subscriberInfo_imsi=count_distinct(flowRecord_subscriberInfo_imsi)
by startofday(eventTimeFlow), sessionRecord_subscriberInfo_imeisv_tac
| extend totalvolume = flowRecord_dataStats_upLinkOctets + flowRecord_dataStats_downLinkOctets; 
hrvcelltacvolume
| join kind=inner hint.strategy=shuffle inprogress_device_reference
on $left.sessionRecord_subscriberInfo_imeisv_tac == $right.TAC
```

### Challenge 2, Task 2: A geographical map view (cell latitude and longitude) to represent top 10 HRV cells SITES(LGA) what is a large volume generated by tac and distinct users in every hr in last 2 days and user is also interested to know their make and model.

[dynamics]- https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/scalar-data-types/dynamic

```
let selected_mbnl = dynamic([
    'INV010', 'GLC106', 'NLA122', 'NLA034', 'ERE009', 'EDC077', 'INV006', 'MLO015', 'GLC146', 'NLA186',
    'GLC140', 'NLA123', 'GLC155', 'GLC235', 'GLC021', 'GLC104', 'NLA136', 'NLA109', 'GLC100', 'NLA035',
    'NLA066', 'GLC075', 'RFW069', 'RFW045', 'EAY016', 'INV004', 'RFW059', 'GLC022', 'EDC298', 'NLA075',
    'NLA135', 'SAY011', 'NAY011', 'NLA086', 'GLC179', 'RFW043', 'NAY015', 'GLC183', 'INV008', 'RFW002',
    'SLN080', 'HLD090', 'EAY009', 'NLA060', 'RFW076', 'NAY026', 'SAY012', 'NAY032', 'GLC118', 'GLC182',
    'RFW026' 
]);
let selected_ecgi = toscalar(
    cell_reference
    | where MBNL_ID in (selected_mbnl)
    | summarize make_set(ecgi)
);
let lookbackduration= 7d; // Specify lookback duration                                                                  
let maxTapp = toscalar( materialized_view('enriched_flow_cell_session_http_5m_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
let hrvcell10toptac=
materialized_view('enriched_flow_cell_session_http_5m_agg')
| where eventTimeFlow  between (minTapp .. maxTapp)  
  and flowRecord_flowRecordType == 'EDR_STOP_RECORD'
  and  start_cell in (selected_ecgi) and isnotempty(sessionRecord_subscriberInfo_imeisv_tac)
| summarize flowRecord_dataStats_upLinkOctets = sum(flowRecord_dataStats_upLinkOctets),
flowRecord_dataStats_downLinkOctets = sum(flowRecord_dataStats_downLinkOctets),
distinct_flowRecord_subscriberInfo_imsi=count_distinct(flowRecord_subscriberInfo_imsi)
by bin(eventTimeFlow,1h),sessionRecord_subscriberInfo_imeisv_tac,start_cell
| extend totalvolume = flowRecord_dataStats_upLinkOctets + flowRecord_dataStats_downLinkOctets 
| top-nested of eventTimeFlow by dummy=max(1), 
  top-nested 10 of start_cell by count=sum(totalvolume),
  top-nested of sessionRecord_subscriberInfo_imeisv_tac by _count=sum(totalvolume),
  top-nested of distinct_flowRecord_subscriberInfo_imsi by totaldistinctusers=max(1);
hrvcell10toptac
| join kind=inner hint.strategy=shuffle cell_reference
on $left.start_cell == $right.ecgi
| join kind=inner hint.strategy=shuffle inprogress_device_reference
on $left.sessionRecord_subscriberInfo_imeisv_tac == $right.TAC
```



### Challenge 2, Task 3: The total volume and distinct users generated by each HRV SITES in an hour and detect during nighttime (2 to 4am) which applications are producing low volume and how many distinct users are active on each HRV cell.

```
let selected_mbnl = dynamic([
    'INV010', 'GLC106', 'NLA122', 'NLA034', 'ERE009', 'EDC077', 'INV006', 'MLO015', 'GLC146', 'NLA186',
    'GLC140', 'NLA123', 'GLC155', 'GLC235', 'GLC021', 'GLC104', 'NLA136', 'NLA109', 'GLC100', 'NLA035',
    'NLA066', 'GLC075', 'RFW069', 'RFW045', 'EAY016', 'INV004', 'RFW059', 'GLC022', 'EDC298', 'NLA075',
    'NLA135', 'SAY011', 'NAY011', 'NLA086', 'GLC179', 'RFW043', 'NAY015', 'GLC183', 'INV008', 'RFW002',
    'SLN080', 'HLD090', 'EAY009', 'NLA060', 'RFW076', 'NAY026', 'SAY012', 'NAY032', 'GLC118', 'GLC182',
    'RFW026' 
]);
let selected_ecgi = toscalar(
    cell_reference
    | where MBNL_ID in (selected_mbnl)
    | summarize make_set(ecgi)
);
let lookbackduration= 2d; // Specify lookback duration                         
let maxTapp = toscalar( materialized_view('enriched_flow_cell_session_http_5m_agg') | summarize max(eventTimeFlow));
let minTapp = maxTapp - lookbackduration;
let hrvcell10topapp=
materialized_view('enriched_flow_cell_session_http_5m_agg')
| where eventTimeFlow  between (minTapp .. maxTapp)  
  and flowRecord_flowRecordType == 'EDR_STOP_RECORD'
  and  start_cell in (selected_ecgi) and isnotempty(flowRecord_dpiStringInfo_application) 
| summarize hint.strategy=shuffle flowRecord_dataStats_upLinkOctets = sum(flowRecord_dataStats_upLinkOctets),
flowRecord_dataStats_downLinkOctets = sum(flowRecord_dataStats_downLinkOctets),
distinct_flowRecord_subscriberInfo_imsi=count_distinct(flowRecord_subscriberInfo_imsi)
by bin(eventTimeFlow,1h)
,flowRecord_dpiStringInfo_application,dpi_group
,start_cell
| extend totalvolume = flowRecord_dataStats_upLinkOctets + flowRecord_dataStats_downLinkOctets ;
hrvcell10topapp
| join kind=inner hint.strategy=shuffle cell_reference
on $left.start_cell == $right.ecgi
````


### Performance Optimization Best Practices: - (https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/best-practices)

summarize operator	Use the hint.shufflekey=<key> when the group by keys of the summarize operator are with high cardinality.		High cardinality is ideally above 1 million.

join operator-Select the table with the fewer rows to be the first one (left-most in query).		
	
 Use in instead of left semi join for filtering by a single column.		

Join when left side is small and right side is large	Use hint.strategy=broadcast Small refers to up to 100MB of data.

Join when right side is small and left side is large	Use the lookup operator instead of the join operator	
If the right side of the lookup is larger than several tens of MBs, the query will fail.

Join when both sides are too large	Use hint.shufflekey=<key>
	Use when the join key has high cardinality.

Reduce the amount of data being queried



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

## Challenge 3, Task 2:  Visualization with render operator
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
  
