## a. Scenarios based with anomaly detection and finding the warnings on three AOI KPIs.

The applicable time series functions are based on a robust well-known decomposition model, where each original time series is decomposed into seasonal, trend, and residual components. 
Anomalies are detected by outliers on the residual component, while forecasting is done by extrapolating the seasonal and trend components.

The applicable time series functions are based on a robust well-known decomposition model, where each original time series is decomposed into seasonal, trend, and residual components. 
Anomalies are detected by outliers on the residual component, 
while forecasting is done by extrapolating the seasonal and trend components.


This function is based on univariate analysis, i.e. analyzing each metric independently (of other metrics) for anomalies. 
Univariate analysis is simpler, faster, and easily scalable and is applicable to most real-life scenarios; 
however, there are some cases where it might miss anomalies that can only be detected by analyzing multiple metrics at the same time which we will discuss in the coming scenarios.


### a. User is interested to know total downlink packet loss % anomaly detection for the whole network.

Make_list()-Returns a dynamic array of all the values of expr in the group. If the input to the summarize operator isn't sorted, the order of 
elements in the resulting array is undefined. If the input to the summarize operator is sorted, the order of elements in the resulting array tracks that of the input.

#### (Flagged, scores, Baseline) = series_decompose_anomalies(['Downlink Packet Loss (%)'], 3, -1)

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

### b. Users are interested to know total downlink packet loss % anomaly detection for the all the set of locations by LGA.

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

### c. Users are interested to know total downlink packet loss % for the drillthrough cell from the list of cell of section b.

```
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
```

### Multivariate Anomaly Detection
Let’s see an example of missing anomalies when using univariate analysis. Suppose we monitor two car metrics: speed and engine rpm. Having 0 for specific metric is not anomalous – either the car doesn’t move, or its engine is off. But measuring speed of 40 km/hour with 0 rpm is definitely anomalous - the car might be sliding or pulled. To detect these types of anomalies we need to use multivariate analysis methods that jointly analyze time series of multiple metrics.

We are happy to introduce three new UDFs (User Defined Functions) that apply different multivariate models on ADX time series. These functions are based on models from scikit-learn (the most common Python ML library), taking advantage of ADX capability to run inline Python as part of the KQL query. These are the new functions:

series_mv_ee_anomalies_fl() – detects multivariate anomalies in series by applying elliptic envelope model.
series_mv_oc_anomalies_fl() - detects multivariate anomalies in series by applying the One Class SVM model.
series_mv_if_anomalies_fl() - detects multivariate anomalies in series by applying isolation forest model.

Canonical Example with Stock Prices 
````
TopStocksCleaned| where Date >= datetime(2021-01-01) 

| where Ticker in('SPY', 'MSFT') 

| order by Ticker asc, Date asc 

| extend Change = iff(Ticker == prev(Ticker), (AdjClose-prev(AdjClose))/prev(AdjClose)*100.0, 0.0) 

| project Date, Ticker, Change 

| evaluate pivot(Ticker, any(Change)) :(Date:datetime, MSFT:real, SPY:real) 

| summarize Date=make_list(Date), MSFT=make_list(MSFT), SPY=make_list(SPY) 

| extend anomalies=dynamic(null) 

| invoke series_mv_ee_anomalies_fl(pack_array('MSFT', 'SPY'), 'anomalies', anomalies_pct=2) 

| render anomalychart with(anomalycolumns=anomalies) 
```
 
```
// MULTI-VARIATE ANOMALY DETECTION 

// MULTI-VARIATE  

let _mint=ago(5h); 

let _maxt=now(); 

let _dt=2h; 

materialized_view('enriched_flow_cell_session_http_5m_agg') 

| where eventTimeFlow between (_mint .. _maxt) 

| where LGA contains "Glasgow City"  

| summarize  

    Count_Calls_by_Path=count(), 

    P50_Delay_Round_Trip=iff(isempty(percentile(httpRecord_httpTransactionRecord_tcpClientConnectionInformation_rtt_avg, 50)), 0.0, percentile(httpRecord_httpTransactionRecord_tcpClientConnectionInformation_rtt_avg, 50)), 

    P50_PkTput=iff(isempty(percentile(flowRecord_networkStatsInfo_downlinkFlowPeakThroughput_max, 50)),0.0, percentile(flowRecord_networkStatsInfo_downlinkFlowPeakThroughput_max, 50)*1.0),  

    AVG_packets=avg(flowRecord_dataStats_downLinkPackets), 

    P99_Retrans=percentile(flowRecord_tcpRetransInfo_downlinkRetransBytes, 99) 

    by bin_at(eventTimeFlow, 15m, now()), LGA 

| project 

    DATE=bin(eventTimeFlow, 15m), 

    Count_Calls_by_Path, 

    P50_Delay_Round_Trip, 

    P50_PkTput, 

    P99_Retrans, 

    AVG_packets 

| summarize  

    Date=make_list(DATE),  

    RTT=make_list(P50_Delay_Round_Trip),  

    DROP_PKTS=make_list(AVG_packets), 

    PKT_RETRANS=make_list(P99_Retrans),  

    PKT_TPUT=make_list(P50_PkTput), 

    PACKS=make_list(P50_PkTput) 

| extend anomalies=dynamic(null) 

| invoke series_mv_ee_anomalies_fl(pack_array('RTT', 'PKT_RETRANS', 'PKT_TPUT', 'DROP_PKTS'), 'anomalies', anomalies_pct=0.2) 

| render anomalychart with(anomalycolumns=anomalies, yaxis=log)//, ysplit=panels) 
````

## Summary
Time series-based anomaly detection is a very powerful method for health monitoring of cloud resources and IoT devices. Many metrics are collected and can be analyzed for anomalies. ADX has built in capabilities for applying univariate anomaly detection for thousands of time series in near real time. Further, anomalies that were found on different metrics can be time based correlated to increase their score/confidence. In most cases this method is effective to detect issues; however, for some scenarios there might be a need for a true multivariate model, jointly analyzing multiple metrics. This can be achieved now in ADX using the new Python based multivariate anomaly detection functions. You are welcome to try these functions and share your feedback with us, as we continue improving and adding more multivariate capabilities.

### b.	Scenarios based on forecasting(concept)

The function series_decompose_forecast() predicts future values of a set of time series. This function calls series_decompose() to build the decomposition model and then, for each time series, extrapolates the baseline component into the future.

```
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
let horizon=7d;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t+horizon step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend forecast = series_decompose_forecast(num, toint(horizon/dt))
| render timechart with(title='Web app. traffic of a month, forecasting the next week by Time Series Decomposition')
```

### Dashboard creation from all set of above queries:

<img width="854" alt="image" src="https://github.com/ibajaj3009/AIOPS-MCC-EDR-ADX-in-a-Day/assets/78459999/7f6bb74e-0fa4-496e-b5db-058b918d7bfb">

<img width="689" alt="image" src="https://github.com/ibajaj3009/AIOPS-MCC-EDR-ADX-in-a-Day/assets/78459999/153219f3-50ce-42f3-b45d-92189cb5d3eb">




