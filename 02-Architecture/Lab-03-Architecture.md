# Lab 3 Architecture — Power BI Semantic Model with MCP Servers

## Overview

Lab 3 builds a production-ready Power BI semantic model on top of Lab 1's gold layer tables, using both Power BI MCP Servers for AI-assisted development and natural language querying.

## Architecture Diagram

```mermaid
flowchart TB
    G1[gold_sales_fact]
    G2[gold_service_fact]
    G3[gold_test_drive_fact]
    G4[gold_dealership_dim]
    G5[gold_customer_dim]
    G6[gold_vehicle_dim]
    
    F1[Sales Fact]
    F2[Service Fact]
    F3[Test Drive Fact]
    D1[Dealership Dim]
    D2[Customer Dim]
    D3[Vehicle Dim]
    D4[Date Dim]
    
    M1[Sales Measures]
    M2[Time Intelligence]
    M3[Customer Analytics]
    M4[Service Metrics]
    M5[Test Drive KPIs]
    
    MODELING[Power BI Modeling MCP]
    REMOTE[Power BI Remote MCP]
    DEV[Developer]
    BIZ[Business User]
    CODE[Generated DAX]
    INSIGHTS[Query Results]

    G1 & G2 & G3 & G4 & G5 & G6 --> F1 & F2 & F3 & D1 & D2 & D3
    F1 & F2 & F3 & D1 & D2 & D3 & D4 --> M1 & M2 & M3 & M4 & M5

    DEV --> MODELING
    MODELING --> CODE
    CODE --> M1 & M2 & M3 & M4 & M5

    BIZ --> REMOTE
    REMOTE --> M1 & M2 & M3 & M4 & M5
    M1 & M2 & M3 & M4 & M5 --> INSIGHTS
    INSIGHTS --> BIZ

    style MODELING fill:#00bcf2
    style REMOTE fill:#00bcf2
    style DEV fill:#ffb900
    style BIZ fill:#ffb900
    style M1 fill:#00aa00
    style M2 fill:#00aa00
    style M3 fill:#00aa00
    style M4 fill:#00aa00
    style M5 fill:#00aa00
```

## Semantic Model Structure

### Star Schema Design

```mermaid
graph LR
    subgraph "Dimension Tables"
        DATE[Date Dim<br/>Date, Year, Quarter<br/>Month, Day]
        DEALER[Dealership Dim<br/>DealershipID, Name<br/>City, Region]
        CUST[Customer Dim<br/>CustomerID, Name<br/>City, State]
        VEH[Vehicle Dim<br/>VehicleID, Make<br/>Model, Year, Type]
    end

    subgraph "Fact Tables"
        SALES[Sales Fact<br/>SaleID, SaleDate<br/>SalePrice, Discount<br/>NetSalePrice]
        SERVICE[Service Fact<br/>ServiceID, ServiceDate<br/>TotalCost, PartsCost<br/>Satisfaction]
        TEST[Test Drive Fact<br/>TestDriveID, Date<br/>Outcome, Feedback]
    end

    SALES --> DATE
    SALES --> DEALER
    SALES --> CUST
    SALES --> VEH
    
    SERVICE --> DATE
    SERVICE --> DEALER
    SERVICE --> CUST
    
    TEST --> DATE
    TEST --> DEALER
    TEST --> CUST
    TEST --> VEH

    style SALES fill:#f8b900,stroke:#e09500,stroke-width:2px
    style SERVICE fill:#f8b900,stroke:#e09500,stroke-width:2px
    style TEST fill:#f8b900,stroke:#e09500,stroke-width:2px
```

## Two MCP Servers in Action

### 1. Power BI Modeling MCP Server
**Purpose:** Generate DAX measures and enhance model metadata

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant MCP as Modeling MCP
    participant Model as Semantic Model
    
    Dev->>MCP: "Create sales and margin measures"
    MCP->>Dev: Generate DAX code
    Dev->>Model: Add measures to model
    
    Dev->>MCP: "Add time intelligence measures"
    MCP->>Dev: YoY, MoM, YTD, Rolling Avg
    Dev->>Model: Apply time calculations
    
    Dev->>MCP: "Add descriptions to all measures"
    MCP->>Dev: Business-friendly descriptions
    Dev->>Model: Update metadata
    
    Dev->>MCP: "Optimize DAX performance"
    MCP->>Dev: Improved measure formulas
    Dev->>Model: Replace with optimized code
```

### 2. Power BI Remote MCP Server
**Purpose:** Query semantic model using natural language

```mermaid
sequenceDiagram
    participant User as Business User
    participant MCP as Remote MCP
    participant Model as Semantic Model
    participant Results as Query Results
    
    User->>MCP: "What are top 10 vehicles by sales?"
    MCP->>Model: Generate & Execute DAX Query
    Model->>Results: Return aggregated data
    Results->>User: Display top vehicles
    
    User->>MCP: "Show monthly sales trends"
    MCP->>Model: Time-series DAX query
    Model->>Results: Monthly aggregations
    Results->>User: Display trends
    
    User->>MCP: "Customer lifetime value top 20"
    MCP->>Model: Complex calculated query
    Model->>Results: Customer analytics
    Results->>User: Display rankings
```

## Measure Categories Created

### 1. Sales Metrics
```dax
Total Sales = SUM(gold_sales_fact[NetSalePrice])
Total Discount = SUM(gold_sales_fact[Discount])
Gross Margin = SUM(gold_sales_fact[NetSalePrice]) - SUM(gold_sales_fact[Cost])
Vehicle Units Sold = COUNTROWS(gold_sales_fact)
Average Sale Price = AVERAGE(gold_sales_fact[NetSalePrice])
```

### 2. Time Intelligence
```dax
Sales YoY = [Total Sales] - CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date'[Date]))
Sales YoY % = DIVIDE([Sales YoY], CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date'[Date])))
Sales YTD = TOTALYTD([Total Sales], 'Date'[Date])
Sales Rolling 3M = CALCULATE([Total Sales], DATESINPERIOD('Date'[Date], LASTDATE('Date'[Date]), -3, MONTH))
```

### 3. Customer Analytics
```dax
Customer Lifetime Value = 
    SUMX(
        VALUES(gold_customer_dim[CustomerID]),
        CALCULATE([Total Sales]) + CALCULATE([Service Revenue])
    )

New Customers = CALCULATE(DISTINCTCOUNT(gold_sales_fact[CustomerID]), ...)
Customer Retention Rate = DIVIDE([Returning Customers], [Total Customers])
```

### 4. Service Metrics
```dax
Service Revenue = SUM(gold_service_fact[TotalCost])
Service Margin = [Service Revenue] - SUM(gold_service_fact[PartsCost]) - SUM(gold_service_fact[LaborCost])
Avg Customer Satisfaction = AVERAGE(gold_service_fact[SatisfactionScore])
```

### 5. Test Drive KPIs
```dax
Test Drive Count = COUNTROWS(gold_test_drive_fact)
Converted Test Drives = CALCULATE(COUNTROWS(gold_test_drive_fact), gold_test_drive_fact[Outcome] = "Converted")
Test Drive Conversion Rate = DIVIDE([Converted Test Drives], [Test Drive Count])
```

## AI-Assisted Development Workflow

```mermaid
graph TB
    START[Start with Gold Tables<br/>from Lab 1]
    
    START --> BUILD[Build Semantic Model<br/>Import Gold Tables]
    BUILD --> REL[Configure Relationships<br/>Star Schema]
    
    REL --> MODELING[Power BI Modeling MCP]
    
    MODELING --> GEN1[Generate Base Measures<br/>Sales, Service, Test Drive]
    GEN1 --> GEN2[Generate Time Intelligence<br/>YoY, MoM, YTD]
    GEN2 --> GEN3[Generate Customer Analytics<br/>Lifetime Value, Retention]
    GEN3 --> META[Add Metadata<br/>Descriptions, Synonyms]
    META --> OPT[Optimize Performance<br/>Improve DAX]
    
    OPT --> PUBLISH[Publish to Power BI Service]
    
    PUBLISH --> REMOTE[Power BI Remote MCP]
    REMOTE --> QUERY1[Ask: Top vehicles by sales]
    REMOTE --> QUERY2[Ask: Monthly trends]
    REMOTE --> QUERY3[Ask: Dealership rankings]
    REMOTE --> QUERY4[Ask: Customer insights]
    
    QUERY1 & QUERY2 & QUERY3 & QUERY4 --> INSIGHTS[Business Insights]
    
    style MODELING fill:#00bcf2,stroke:#0078d4,stroke-width:3px,color:#fff
    style REMOTE fill:#00bcf2,stroke:#0078d4,stroke-width:3px,color:#fff
    style INSIGHTS fill:#00aa00,stroke:#008800,stroke-width:2px
```

## Sample Prompts Used

### Modeling MCP - Create Measures
**Prompt:**
```text
I have a Power BI semantic model with these tables:
- gold_sales_fact (SalePrice, Discount, NetSalePrice, SaleDate, DealershipID, CustomerID, VehicleID)
- gold_service_fact (TotalCost, PartsCost, LaborCost, SatisfactionScore, ServiceDate)
- gold_test_drive_fact (TestDriveDate, Outcome, DealershipID)

Create essential business measures for:
- Total sales revenue and discounts
- Gross margin calculations
- Service revenue and margins
- Test drive conversion tracking
```

### Modeling MCP - Time Intelligence
**Prompt:**
```text
Add time intelligence measures for my sales and service facts:
- Year-over-year growth
- Month-over-month comparison
- Year-to-date totals
- Rolling 3-month and 12-month averages
```

### Modeling MCP - Metadata
**Prompt:**
```text
Add business-friendly descriptions to all tables, columns, and measures for non-technical users
```

### Remote MCP - Sales Analysis
**Prompt:**
```text
What are the top 10 vehicle makes by total sales revenue?
```

### Remote MCP - Performance Comparison
**Prompt:**
```text
Which dealerships have the highest gross margin percentage?
```

### Remote MCP - Customer Insights
**Prompt:**
```text
What is the customer lifetime value for the top 20 customers?
```

## Key Components

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Semantic Model** | Power BI Dataset | Unified business logic layer |
| **Gold Tables** | Delta Lake | Source data from Lab 1 |
| **Star Schema** | Dimensional Model | Optimized for analytics |
| **DAX Measures** | Data Analysis Expressions | Business calculations |
| **Modeling MCP** | AI Assistant | Generate measures & metadata |
| **Remote MCP** | AI Assistant | Query model with natural language |
| **VS Code** | IDE | Development environment |
| **Power BI Service** | Cloud Platform | Host and share semantic model |

## Architecture Benefits

✅ **Built on Gold Layer:** Clean, analytics-ready data from Lab 1
✅ **Star Schema:** Optimized query performance
✅ **Reusable Measures:** Consistent calculations across all reports
✅ **AI-Generated DAX:** Rapid measure development
✅ **Natural Language Queries:** Business users query without DAX knowledge
✅ **Rich Metadata:** Descriptions improve AI query generation
✅ **Time Intelligence:** Sophisticated date calculations built-in
✅ **Customer Analytics:** Deep insights into customer behavior

## Integration Points

```mermaid
graph LR
    LAB1[Lab 1<br/>Gold Tables] -->|Source Data| MODEL[Semantic Model]
    LAB2[Lab 2<br/>KQL Data] -.->|Optional<br/>Real-time| MODEL
    
    MODEL --> REPORTS[Power BI Reports]
    MODEL --> DASHBOARDS[Dashboards]
    MODEL --> REMOTE[Remote MCP<br/>NL Queries]
    MODEL --> APPS[Power BI Apps]
    
    style LAB1 fill:#00aa00,stroke:#008800,stroke-width:2px
    style MODEL fill:#f8b900,stroke:#e09500,stroke-width:2px
    style REMOTE fill:#00bcf2,stroke:#0078d4,stroke-width:2px
```

## Technical Specifications

- **Workspace:** ContosoAuto360-MCP-Workshop
- **Semantic Model:** sm_contoso_auto_360
- **Storage Mode:** Import (from gold tables)
- **Measure Count:** 30+ measures
- **Relationships:** Star schema with proper cardinality
- **AI Tools:** 
  - Power BI Modeling MCP Server (measure generation)
  - Power BI Remote MCP Server (natural language queries)
- **Development:** VS Code + Power BI Desktop
- **Deployment:** Power BI Service

---

**Key Innovation:** Using two specialized MCP servers for both development (Modeling) and consumption (Remote querying)
