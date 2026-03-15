# Lab 2 Architecture — Real-Time Intelligence with RTI MCP

## Overview

Lab 2 implements real-time telemetry analytics using Microsoft Fabric's Real-Time Intelligence (Eventhouse + KQL) with AI-assisted query generation via RTI MCP Server.

## 📊 Architecture Diagram

```mermaid
flowchart TB
    subgraph SOURCE["📁 Data Source"]
        JSONL["📄 telemetry_events.jsonl<br/>Real-time Telemetry Data"]
    end
    
    subgraph INGESTION["📥 Data Ingestion Layer"]
        INGEST["⬇️ Data Ingestion Process"]
        STREAM["🌊 Eventstream (Optional)<br/>Real-time Streaming"]
    end
    
    subgraph RTI["⚡ Real-Time Intelligence"]
        TABLE["🗄️ telemetry_events Table<br/>KQL Database"]
    end
    
    subgraph AI["🤖 AI-Assisted Analytics"]
        USER["👤 User Questions<br/>Natural Language"]
        MCP["⚡ RTI MCP Server<br/>AI Query Generator"]
        KQL["📝 Generated KQL Queries"]
    end
    
    subgraph OUTPUT["📊 Results & Visualizations"]
        RESULTS["✅ Query Results<br/>Insights & Analytics"]
        DASH["📈 Real-Time Dashboard<br/>(Optional)"]
    end

    JSONL --> INGEST
    INGEST --> TABLE
    STREAM -.->|streams to| TABLE
    
    USER --> MCP
    MCP -->|generates| KQL
    KQL -->|queries| TABLE
    TABLE -->|returns| RESULTS
    RESULTS --> USER
    TABLE -.->|feeds| DASH

    style SOURCE fill:#E8EAF6,stroke:#3F51B5,stroke-width:3px,color:#000
    style INGESTION fill:#FFF3E0,stroke:#FF6F00,stroke-width:3px,color:#000
    style RTI fill:#FFEBEE,stroke:#C62828,stroke-width:4px,color:#000
    style AI fill:#E1F5FE,stroke:#0277BD,stroke-width:3px,color:#000
    style OUTPUT fill:#E8F5E9,stroke:#388E3C,stroke-width:3px,color:#000
    
    style MCP fill:#0277BD,stroke:#01579B,stroke-width:3px,color:#fff
    style USER fill:#FF6F00,stroke:#E65100,stroke-width:3px,color:#fff
    style TABLE fill:#C62828,stroke:#B71C1C,stroke-width:3px,color:#fff
    style RESULTS fill:#388E3C,stroke:#1B5E20,stroke-width:3px,color:#fff
    style KQL fill:#0288D1,stroke:#01579B,stroke-width:2px,color:#fff
    style DASH fill:#FFA726,stroke:#EF6C00,stroke-width:2px,color:#000
    style INGEST fill:#FFA726,stroke:#EF6C00,stroke-width:2px,color:#000
    style STREAM fill:#42A5F5,stroke:#1565C0,stroke-width:2px,color:#000
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

## 🔄 AI-Assisted Analytics Workflow

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryTextColor':'#000000', 'primaryColor':'#fff', 'primaryBorderColor':'#000', 'lineColor':'#000', 'secondaryColor':'#f4f4f4', 'tertiaryColor':'#fff', 'noteTextColor':'#000000', 'noteBkgColor':'#fff5ad', 'actorTextColor':'#000000', 'labelTextColor':'#000000'}}}%%
sequenceDiagram
    participant User as 👤 Business User
    participant MCP as ⚡ RTI MCP Server
    participant VSCode as 💻 VS Code
    participant KQL as 🗄️ KQL Database
    participant Results as 📊 Query Results

    rect rgb(225, 245, 254)
        Note over User,Results: Example 1: Wait Time Analysis
        User->>MCP: "Which dealerships have highest wait times?"
        MCP->>VSCode: Generate KQL query
        Note over VSCode: telemetry_events<br/>| summarize AvgWait = avg(WaitTimeSeconds)<br/>  by DealershipName<br/>| order by AvgWait desc
        User->>KQL: Execute generated query
        KQL->>Results: Return aggregated data
        Results->>User: Display insights
    end

    rect rgb(232, 245, 233)
        Note over User,Results: Example 2: Sentiment Analysis
        User->>MCP: "Show sentiment by region"
        MCP->>VSCode: Generate KQL query
        Note over VSCode: telemetry_events<br/>| summarize AvgSentiment = avg(SentimentScore)<br/>  by Region<br/>| order by AvgSentiment
        User->>KQL: Execute query
        KQL->>Results: Return regional sentiment
        Results->>User: Display trends
    end
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

## 📈 Real-Time Analytics Capabilities

```mermaid
graph LR
    KQL["🗄️ KQL Database<br/>Real-Time Engine"]
    
    subgraph PATTERNS["🔍 Analytics Patterns"]
        A["➕ Aggregations<br/>sum, avg, count, max"]
        B["📅 Time Series<br/>Trends & Patterns"]
        C["⚠️ Anomaly Detection<br/>Unusual Events"]
        D["🔎 Filtering<br/>Targeted Analysis"]
        E["🔗 Correlation<br/>Cross-Metrics"]
    end

    INSIGHTS["💡 Business Insights<br/>Actionable Intelligence"]

    KQL --> A & B & C & D & E
    A & B & C & D & E --> INSIGHTS

    style KQL fill:#C62828,stroke:#B71C1C,stroke-width:4px,color:#fff
    style INSIGHTS fill:#388E3C,stroke:#1B5E20,stroke-width:4px,color:#fff
    style PATTERNS fill:#E1F5FE,stroke:#0277BD,stroke-width:3px,color:#000
    style A fill:#42A5F5,stroke:#1565C0,stroke-width:2px,color:#000
    style B fill:#42A5F5,stroke:#1565C0,stroke-width:2px,color:#000
    style C fill:#FFA726,stroke:#EF6C00,stroke-width:2px,color:#000
    style D fill:#42A5F5,stroke:#1565C0,stroke-width:2px,color:#000
    style E fill:#66BB6A,stroke:#2E7D32,stroke-width:2px,color:#000
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

## 🌊 Optional: Real-Time Streaming Architecture

```mermaid
graph LR
    SOURCE["📡 Data Source<br/>IoT Devices<br/>Sensors"]
    STREAM["🌊 Eventstream<br/>Real-Time Ingestion"]
    TRANSFORM["⚙️ Stream<br/>Transformations<br/>Processing"]
    KQL["🗄️ KQL Database<br/>Analytics Engine"]
    DASH["📊 Real-Time<br/>Dashboard<br/>Visualizations"]
    ALERTS["🔔 Activator<br/>Alerts & Actions<br/>Automation"]
    
    SOURCE --> STREAM
    STREAM --> TRANSFORM
    TRANSFORM --> KQL
    KQL --> DASH
    KQL --> ALERTS

    style SOURCE fill:#E8EAF6,stroke:#3F51B5,stroke-width:3px,color:#000
    style STREAM fill:#0277BD,stroke:#01579B,stroke-width:3px,color:#fff
    style TRANSFORM fill:#FFA726,stroke:#EF6C00,stroke-width:3px,color:#000
    style KQL fill:#C62828,stroke:#B71C1C,stroke-width:4px,color:#fff
    style DASH fill:#388E3C,stroke:#1B5E20,stroke-width:3px,color:#fff
    style ALERTS fill:#D32F2F,stroke:#B71C1C,stroke-width:3px,color:#fff
```

## Key Benefits

- ✅ **Real-Time Analytics:** Query streaming data instantly
- ✅ **High Performance:** KQL optimized for time-series data
- ✅ **Natural Language:** Use business questions, not query syntax
- ✅ **Scalability:** Handle millions of events per second
- ✅ **Integration:** Connect to Power BI, Azure, and Fabric services
- ✅ **AI-Assisted:** RTI MCP generates complex queries from simple prompts

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
