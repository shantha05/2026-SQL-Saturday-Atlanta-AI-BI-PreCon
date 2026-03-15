# SQL Saturday Atlanta AI BI Pre Con Session — Lab 2

## Real-Time Intelligence MCP Server in Action

## Objective

Use the RTI MCP Server to work with live telemetry for Contoso Auto 360.

**Estimated Time:** 30-45 minutes

## Lab 2 Steps

### Part A — Prepare Real-Time Intelligence items

1. In the same workspace (`ContosoAuto360-MCP-Workshop`), create an Eventhouse named `eh_contoso_auto_360`.
2. Inside the Eventhouse, create a KQL database named `kql_contoso_auto_360`.
3. Create a target table named `telemetry_events` for telemetry data.
4. Define the table schema with these columns:
   - EventID: long
   - EventTime: datetime
   - DealershipID: long
   - DealershipName: string
   - City: string
   - Region: string
   - Country: string
   - DeviceType: string
   - Zone: string
   - EventType: string
   - CustomerCount: long
   - WaitTimeSeconds: long
   - SentimentScore: real
   - QueueLength: long
   - AlertFlag: bool

   Example KQL to create the table:

   ```kusto
   .create table telemetry_events (
       EventID: long,
       EventTime: datetime,
       DealershipID: long,
       DealershipName: string,
       City: string,
       Region: string,
       Country: string,
       DeviceType: string,
       Zone: string,
       EventType: string,
       CustomerCount: long,
       WaitTimeSeconds: long,
       SentimentScore: real,
       QueueLength: long,
       AlertFlag: bool
   )
   ```

### Part B — Load the telemetry file

1. Use one of these approaches:
   - **Option A:** Upload and ingest the JSONL file directly into the KQL table using the "Get Data" feature
   - **Option B:** Create an Eventstream to stream data for a more live demo experience
2. Confirm data loaded successfully by running this KQL query:

   ```kusto
   telemetry_events
   | take 10
   ```

3. Verify row count and data types:

   ```kusto
   telemetry_events
   | count
   ```

### Part C — Configure the RTI MCP Server

1. In VS Code, install the **Microsoft Fabric RTI MCP Server**:
   - Follow the official Microsoft Fabric RTI MCP Server documentation
   - Authenticate with your Azure/Fabric credentials
   - Configure the server to connect to your Eventhouse
2. Verify authentication is working by testing a simple query.
3. Confirm RTI MCP tools are visible in the client tools list.

### Part D — Use natural-language prompts

1. Start a new chat and ask the RTI MCP client:
   - “List the KQL tables in my Contoso Auto 360 Eventhouse.”
   - “Show me 20 sample rows from telemetry_events.”
   - “Which dealerships have the highest average wait time?”
   - “Which cities had the lowest sentiment score in the last hour?”
   - “Summarize queue conditions by region.”

### Part E — Inspect generated KQL

1. For at least two prompts from Part D, review the generated KQL code.
2. Analyze the queries and discuss:
   - Did the AI choose the right table?
   - Did the filters and aggregations make sense?
   - Would you change anything before production use?

### Part F — Add a simple alert concept

1. Use MCP prompts to explore alerting patterns:
   - “Create a concept for alerting when average wait time exceeds 900 seconds.”
   - “How would Activator be used for queue spikes in Contoso Auto 360?”

## Sample KQL Queries for the lab

### Average wait time by dealership

```kusto
telemetry_events
| summarize AvgWaitTimeSec = avg(WaitTimeSeconds) by DealershipName, City, Region
| order by AvgWaitTimeSec desc
```

### Sentiment by region

```kusto
telemetry_events
| summarize AvgSentiment = avg(SentimentScore) by Region
| order by AvgSentiment asc
```

### Queue spikes

```kusto
telemetry_events
| where QueueLength >= 10
| summarize SpikeEvents = count() by DealershipName, City
| order by SpikeEvents desc
```

## Lab 2 Expected Outcome

At the end of Lab 2 you should have:

- An Eventhouse: `eh_contoso_auto_360`
- A KQL database: `kql_contoso_auto_360`
- A `telemetry_events` table with loaded data
- RTI MCP Server connected to your VS Code client
- Experience querying real-time data using natural language
- Understanding of how KQL queries are generated from prompts
- Knowledge of alerting patterns for real-time scenarios

## Troubleshooting Tips

- **Eventhouse creation fails:** Verify Fabric capacity and workspace permissions
- **Data ingestion issues:** Check JSONL file format and column mappings
- **RTI MCP not connecting:** Confirm authentication and Eventhouse access rights
- **Query errors:** Validate table name and column names match exactly
- **No data returned:** Check if data was successfully loaded using `| count`
