# Lab 2 Architecture — Real-Time Intelligence with RTI MCP

## Overview

Lab 2 implements real-time telemetry analytics using Microsoft Fabric's Real-Time Intelligence (Eventhouse + KQL) with AI-assisted query generation via RTI MCP Server.

## Architecture Diagram

```mermaid
flowchart TB
    JSONL[telemetry_events.jsonl]
    INGEST[Data Ingestion]
    TABLE[telemetry_events Table]
    MCP[RTI MCP Server]
    KQL[Generated KQL Queries]
    USER[User Questions]
    RESULTS[Query Results]
    STREAM[Eventstream - Optional]
    DASH[Real-Time Dashboard - Optional]

    JSONL --> INGEST
    INGEST --> TABLE
    
    USER --> MCP
    MCP --> KQL
    KQL --> TABLE
    TABLE --> RESULTS
    RESULTS --> USER

    STREAM -.-> TABLE
    TABLE -.-> DASH

    style MCP fill:#00bcf2
    style USER fill:#ffb900
    style TABLE fill:#e81123
    style RESULTS fill:#00aa00
```

## Data Flow Details

### 1. Data Ingestion
- **Source:** telemetry_events.jsonl file with real-time dealership operational data
- **Method:** Direct upload to KQL database or streaming via Eventstream
- **Target:** telemetry_events table in KQL database

### 2. KQL Table Schema

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

### 3. AI-Assisted Query Generation
- **Tool:** RTI MCP Server in VS Code
- **Process:** Convert natural language questions to KQL queries
- **Benefit:** No need to know KQL syntax

## AI-Assisted Analytics Workflow

```mermaid
sequenceDiagram
    participant User
    participant MCP as RTI MCP Server
    participant VSCode as VS Code
    participant KQL as KQL Database
    participant Results as Query Results

    User->>MCP: "Which dealerships have highest wait times?"
    MCP->>VSCode: Generate KQL query
    Note over VSCode: telemetry_events<br/>| summarize AvgWait = avg(WaitTimeSeconds)<br/>  by DealershipName<br/>| order by AvgWait desc
    User->>KQL: Execute generated query
    KQL->>Results: Return aggregated data
    Results->>User: Display insights

    User->>MCP: "Show sentiment by region"
    MCP->>VSCode: Generate KQL query
    Note over VSCode: telemetry_events<br/>| summarize AvgSentiment = avg(SentimentScore)<br/>  by Region<br/>| order by AvgSentiment
    User->>KQL: Execute query
    KQL->>Results: Return regional sentiment
    Results->>User: Display trends
```

## Sample Business Questions & Generated Queries

### Average Wait Time by Dealership
**Question:**
```text
Which dealerships have the highest average wait time?
```

**Generated KQL:**
```kusto
telemetry_events
| summarize AvgWaitTimeSec = avg(WaitTimeSeconds) by DealershipName, City, Region
| order by AvgWaitTimeSec desc
| take 10
```

### Sentiment Analysis by Region
**Question:**
```text
What is the average sentiment score by region?
```

**Generated KQL:**
```kusto
telemetry_events
| summarize AvgSentiment = avg(SentimentScore) by Region
| order by AvgSentiment asc
```

### Queue Spike Detection
**Question:**
```text
Which dealerships experience the most queue spikes?
```

**Generated KQL:**
```kusto
telemetry_events
| where QueueLength >= 10
| summarize SpikeEvents = count() by DealershipName, City
| order by SpikeEvents desc
```

### Alert Analysis
**Question:**
```text
Which events trigger alerts most frequently?
```

**Generated KQL:**
```kusto
telemetry_events
| where AlertFlag == true
| summarize AlertCount = count() by EventType, Zone
| order by AlertCount desc
```

## Architecture Components

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Eventhouse** | Fabric RTI | Container for real-time data |
| **KQL Database** | Azure Data Explorer | High-performance analytics engine |
| **telemetry_events** | KQL Table | Stores operational telemetry |
| **RTI MCP Server** | AI Assistant | Generates KQL from natural language |
| **VS Code** | IDE | Development environment |
| **Eventstream** (Optional) | Fabric Streaming | Real-time data ingestion |

## Real-Time Analytics Capabilities

```mermaid
graph LR
    subgraph "Analytics Patterns"
        A[Aggregations<br/>sum, avg, count]
        B[Time Series<br/>Trends & Patterns]
        C[Anomaly Detection<br/>Unusual Events]
        D[Filtering<br/>Targeted Analysis]
        E[Correlation<br/>Cross-Metrics]
    end

    KQL[KQL Database] --> A & B & C & D & E
    A & B & C & D & E --> INSIGHTS[Business Insights]

    style KQL fill:#e81123,stroke:#c50f1f,stroke-width:2px,color:#fff
    style INSIGHTS fill:#00aa00,stroke:#008800,stroke-width:2px
```

## Sample Use Cases

### Operational Monitoring
- Monitor wait times across dealerships in real-time
- Identify locations needing immediate attention
- Track customer satisfaction trends

### Performance Analysis
- Compare dealership operational efficiency
- Identify peak traffic times and patterns
- Analyze queue management effectiveness

### Alert Management
- Detect and respond to operational issues
- Track alert patterns by zone and device type
- Establish baseline for normal operations

### Regional Insights
- Compare performance across geographic regions
- Identify regional operational best practices
- Allocate resources based on regional demand

## Optional: Real-Time Streaming Architecture

```mermaid
graph LR
    SOURCE[Data Source<br/>IoT Devices] --> STREAM[Eventstream]
    STREAM --> TRANSFORM[Stream<br/>Transformations]
    TRANSFORM --> KQL[KQL Database]
    KQL --> DASH[Real-Time<br/>Dashboard]
    KQL --> ALERTS[Activator<br/>Alerts]

    style STREAM fill:#00bcf2,stroke:#0078d4,stroke-width:2px
    style KQL fill:#e81123,stroke:#c50f1f,stroke-width:2px,color:#fff
    style DASH fill:#f8b900,stroke:#e09500,stroke-width:2px
```

## Key Benefits

✅ **Real-Time Analytics:** Query streaming data instantly
✅ **High Performance:** KQL optimized for time-series data
✅ **Natural Language:** Use business questions, not query syntax
✅ **Scalability:** Handle millions of events per second
✅ **Integration:** Connect to Power BI, Azure, and Fabric services
✅ **AI-Assisted:** RTI MCP generates complex queries from simple prompts

## Technical Specifications

- **Workspace:** ContosoAuto360-MCP-Workshop
- **Eventhouse:** eh_contoso_auto_360
- **KQL Database:** kql_contoso_auto_360
- **Query Language:** Kusto Query Language (KQL)
- **AI Tool:** RTI MCP Server in VS Code
- **Data Format:** JSONL (JSON Lines)

## Integration with Other Labs

- **From Lab 1:** Can join KQL data with Lakehouse gold tables for combined analysis
- **To Lab 3:** Real-time data can supplement historical semantic model queries

---

**Key Skill:** Using natural language to generate KQL queries without knowing query syntax
