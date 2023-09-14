### a. Scenarios based with anomaly detection and finding the warnings on three AOI KPIs.

The applicable time series functions are based on a robust well-known decomposition model, where each original time series is decomposed into seasonal, trend, and residual components. 
Anomalies are detected by outliers on the residual component, while forecasting is done by extrapolating the seasonal and trend components.

The applicable time series functions are based on a robust well-known decomposition model, where each original time series is decomposed into seasonal, trend, and residual components. 
Anomalies are detected by outliers on the residual component, 
while forecasting is done by extrapolating the seasonal and trend components.


This function is based on univariate analysis, i.e. analyzing each metric independently (of other metrics) for anomalies. 
Univariate analysis is simpler, faster, and easily scalable and is applicable to most real-life scenarios; 
however, there are some cases where it might miss anomalies that can only be detected by analyzing multiple metrics at the same time which we will discuss in the coming scenarios.


## a. User is interested to know total downlink packet loss % anomaly detection for the whole network.

Make_list()-Returns a dynamic array of all the values of expr in the group. If the input to the summarize operator isn't sorted, the order of 
elements in the resulting array is undefined. If the input to the summarize operator is sorted, the order of elements in the resulting array tracks that of the input.

# (Flagged, scores, Baseline) = series_decompose_anomalies(['Downlink Packet Loss (%)'], 3, -1)

make_list() (aggregation function) - Azure Data Explorer | Microsoft Learn

```
materialized_view('enriched_flow_cell_session_http_5m_agg') | where eventTimeFlow between (_start_time_parameter..(_end_time_parameter-totimespan(_granularity_parameter))) | where flowRecord_flowRecordType == "EDR_STOP_RECORD" 
and (isnotnull(flowRecord_dataStats_downLinkPackets) or isnotnull(flowRecord_dataStats_downLinkDropPackets)) 
and (flowRecord_dataStats_downLinkPackets + flowRecord_dataStats_downLinkDropPackets) > 0 
| summarize flowRecord_dataStats_downLinkPackets = sum(flowRecord_dataStats_downLinkPackets), 
flowRecord_dataStats_downLinkDropPackets = sum(flowRecord_dataStats_downLinkDropPackets) by bin(eventTimeFlow, totimespan(_granularity_parameter))  
| extend downLinkTotalPackets = flowRecord_dataStats_downLinkPackets + flowRecord_dataStats_downLinkDropPackets 
| extend ['Downlink Packet Loss (%)'] = round((toreal(flowRecord_dataStats_downLinkDropPackets) / toreal(downLinkTotalPackets)) * 100, 6) 
| order by ['Downlink Packet Loss (%)'] desc 
| summarize eventTimeFlow=make_list(eventTimeFlow), ['Downlink Packet Loss (%)'] = make_list(['Downlink Packet Loss (%)']) 
| extend (Flagged, scores, Baseline) = series_decompose_anomalies(['Downlink Packet Loss (%)'], 3, -1) 
| project-away scores
```
## b. Users are interested to know total downlink packet loss % anomaly detection for the all the set of locations by LGA.

```
materialized_view('enriched_flow_cell_session_http_5m_agg') | where eventTimeFlow between (_start_time_parameter..(_end_time_parameter-totimespan(_granularity_parameter))) | where flowRecord_flowRecordType == "EDR_STOP_RECORD" 
and (isnotnull(flowRecord_dataStats_downLinkPackets) or isnotnull(flowRecord_dataStats_downLinkDropPackets)) 
and (flowRecord_dataStats_downLinkPackets + flowRecord_dataStats_downLinkDropPackets) > 0 
| summarize flowRecord_dataStats_downLinkPackets = sum(flowRecord_dataStats_downLinkPackets), 
flowRecord_dataStats_downLinkDropPackets = sum(flowRecord_dataStats_downLinkDropPackets) by bin(eventTimeFlow, totimespan(_granularity_parameter)),LGA  
| extend downLinkTotalPackets = flowRecord_dataStats_downLinkPackets + flowRecord_dataStats_downLinkDropPackets 
| extend ['Downlink Packet Loss (%)'] = round((toreal(flowRecord_dataStats_downLinkDropPackets) / toreal(downLinkTotalPackets)) * 100, 6) 
| order by ['Downlink Packet Loss (%)'] desc 
| summarize eventTimeFlow=make_list(eventTimeFlow), ['Downlink Packet Loss (%)'] = make_list(['Downlink Packet Loss (%)']) by LGA
| extend (Flagged, scores, Baseline) = series_decompose_anomalies(['Downlink Packet Loss (%)'], 3, -1) 
| extend ['Max Anomaly Score']= array_sum(array_slice(array_sort_desc(series_abs(scores)), 0, 0)) 
| extend ['Anomaly Count'] = array_sum(series_abs(Flagged)) 
| project ['LGA']= LGA, ['Anomaly Count'], ['Max Anomaly Score'] 
| order by ['Anomaly Count'] desc
```

## c. Users are interested to know total downlink packet loss % for the drillthrough cell from the list of cell of section b.

materialized_view('enriched_flow_cell_session_http_5m_agg') 
|where eventTimeFlow between (_start_time_parameter..(_end_time_parameter-totimespan(_granularity_parameter))) 
| where flowRecord_flowRecordType == "EDR_STOP_RECORD" 
and (isnotnull(flowRecord_dataStats_downLinkPackets) or isnotnull(flowRecord_dataStats_downLinkDropPackets)) 
and (flowRecord_dataStats_downLinkPackets + flowRecord_dataStats_downLinkDropPackets) > 0
| summarize flowRecord_dataStats_downLinkPackets = sum(flowRecord_dataStats_downLinkPackets), flowRecord_dataStats_downLinkDropPackets = sum(flowRecord_dataStats_downLinkDropPackets) by bin(eventTimeFlow, totimespan(_granularity_parameter)), start_cell 
| extend downLinkTotalPackets = flowRecord_dataStats_downLinkPackets + flowRecord_dataStats_downLinkDropPackets 
| extend ['Downlink Packet Loss (%)'] = round((toreal(flowRecord_dataStats_downLinkDropPackets) / toreal(downLinkTotalPackets)) * 100, 6) 
| order by ['Downlink Packet Loss (%)'] desc 
| summarize eventTimeFlow=make_list(eventTimeFlow), ['Downlink Packet Loss (%)'] = make_list(['Downlink Packet Loss (%)']) by start_cell 
| extend (Flagged, scores, Baseline) = series_decompose_anomalies(['Downlink Packet Loss (%)'], 3, -1) 
| extend ['Max Anomaly Score']= array_sum(array_slice(array_sort_desc(series_abs(scores)), 0, 0)) 
| extend ['Anomaly Count'] = array_sum(series_abs(Flagged)) 
| project ['Cell Tower']=start_cell, ['Anomaly Count'], ['Max Anomaly Score'] | order by ['Anomaly Count'] desc

## d.	Scenarios based on forecasting.





