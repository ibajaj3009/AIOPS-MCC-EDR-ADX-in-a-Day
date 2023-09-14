## Prerequisites

1. A Microsoft account or an Azure Active Directory user identity. An Azure subscription isn't required.
2. An Azure Data Explorer cluster and database. Create a cluster and database.
3. Power BI Desktop.

## Connectivity modes

Power BI supports Import and DirectQuery connectivity modes. When building Power BI reports or dashboards, choose your connectivity mode depending on your scenario, scale, and performance requirements. Using Import mode copies your data to Power BI. In contrast, using DirectQuery mode queries your data directly from your Azure Data Explorer cluster.

## Use Import mode when:

1. Your dataset is small and you don't need near real-time data.
    You perform aggregation in Kusto.
    Use DirectQuery mode when:

2. Your dataset is large or you need near real-time data.
   For more information on connectivity modes, see Import and Direct Query connectivity modes.

## Use data in Power BI

You can connect Azure Data Explorer as a data source to Power BI in the following ways:

1. Starting in Azure Data Explorer web UI and then pasting the data in Power BI Desktop.
2. Starting directly in Power BI Desktop and then adding the Azure Data Explorer connector.

