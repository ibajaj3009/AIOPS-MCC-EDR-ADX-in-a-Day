# AIOPS-MCC-EDR-ADX-in-a-Day

Challenge 1, Task 3: Ingest data from Azure Storage Account

Data ingestion to ADX is the process used to load data records from one or more sources into a table in your ADX cluster. Once ingested, the data becomes available for query.

ADX supports several ingestion methods, including ingestion tools, connectors and plugins, Azure managed pipelines, programmatic ingestion using SDKs, and direct access to ingestion.


Before starting this task, 
What is a Kusto query?
Azure Data Explorer provides a web experience that enables you to connect to your Azure Data Explorer clusters and write and run Kusto Query Language queries. 

1. The web experience is available in the Azure portal and as a stand-alone web application, the Azure Data Explorer Web UI, which we will use later.

2. A Kusto query is a read-only request to process data and return results. The request is stated in plain text that's easy to read. A Kusto query has one or more query statements and returns data in a tabular or graph format.

Expected Learning Outcomes:

Ingest data using one-click ingestion from Azure Blob Storage to your ADX cluster.

Challenge 3, Task 3.1: Create the raw table -logsRaw

Run the following command to create table:

.create table logsRaw(Timestamp:datetime, Source:string, Node:string, Level:string, Component:string, ClientRequestId:string, Message:string, Properties:dynamic) 

Challenge 1, Task 3.2: Use the “One-click” UI (User Interface) to ingest data from Azure blob storage
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



