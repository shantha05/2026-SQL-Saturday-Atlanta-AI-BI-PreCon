# Lab 3 Architecture — Power BI Semantic Model with MCP Servers

## Overview

Lab 3 builds a production-ready Power BI semantic model on top of Lab 1's gold layer tables, using both Power BI MCP Servers for AI-assisted development and natural language querying.

## 📊 Architecture Diagram

```mermaid
flowchart TB
    subgraph LAB1["🥇 Lab 1: Gold Layer Tables"]
        G1["📈 gold_sales_fact"]
        G2["🔧 gold_service_fact"]
        G3["🚗 gold_test_drive_fact"]
        G4["🏢 gold_dealership_dim"]
        G5["👤 gold_customer_dim"]
        G6["🚙 gold_vehicle_dim"]
    end
    
    subgraph MODEL["📊 Power BI Semantic Model"]
        direction TB
        subgraph FACTS["Fact Tables"]
            F1["📊 Sales Fact"]
            F2["🔧 Service Fact"]
            F3["🚗 Test Drive Fact"]
        end
        subgraph DIMS["Dimension Tables"]
            D1["🏢 Dealership Dim"]
            D2["👤 Customer Dim"]
            D3["🚙 Vehicle Dim"]
            D4["📅 Date Dim"]
        end
    end
    
    subgraph MEASURES["📐 DAX Measures & Analytics"]
        M1["💰 Sales Measures"]
        M2["⏰ Time Intelligence"]
        M3["👥 Customer Analytics"]
        M4["🔧 Service Metrics"]
        M5["🚗 Test Drive KPIs"]
    end
    
    subgraph AI_DEV["🤖 AI-Assisted Development"]
        DEV["👨‍💻 Developer"]
        MODELING["⚡ Power BI Modeling MCP<br/>DAX Generation"]
        CODE["📝 Generated DAX Code"]
    end
    
    subgraph AI_QUERY["🤖 AI-Assisted Querying"]
        BIZ["👤 Business User"]
        REMOTE["⚡ Power BI Remote MCP<br/>Natural Language"]
        INSIGHTS["💡 Query Results<br/>& Insights"]
    end

    LAB1 --> MODEL
    MODEL --> MEASURES

    DEV --> MODELING
    MODELING --> CODE
    CODE -.->|creates| MEASURES

    BIZ --> REMOTE
    REMOTE -.->|queries| MEASURES
    MEASURES --> INSIGHTS
    INSIGHTS --> BIZ

    style LAB1 fill:#E8F5E9,stroke:#388E3C,stroke-width:3px,color:#000
    style MODEL fill:#E3F2FD,stroke:#1976D2,stroke-width:3px,color:#000
    style FACTS fill:#BBDEFB,stroke:#1565C0,stroke-width:2px,color:#000
    style DIMS fill:#C5CAE9,stroke:#3F51B5,stroke-width:2px,color:#000
    style MEASURES fill:#FFF9C4,stroke:#F57F17,stroke-width:3px,color:#000
    style AI_DEV fill:#FCE4EC,stroke:#C2185B,stroke-width:3px,color:#000
    style AI_QUERY fill:#E1F5FE,stroke:#0277BD,stroke-width:3px,color:#000
    
    style MODELING fill:#C2185B,stroke:#880E4F,stroke-width:3px,color:#fff
    style REMOTE fill:#0277BD,stroke:#01579B,stroke-width:3px,color:#fff
    style DEV fill:#FF6F00,stroke:#E65100,stroke-width:2px,color:#fff
    style BIZ fill:#FF6F00,stroke:#E65100,stroke-width:2px,color:#fff
    style CODE fill:#F57F17,stroke:#F9A825,stroke-width:2px,color:#000
    style INSIGHTS fill:#388E3C,stroke:#1B5E20,stroke-width:3px,color:#fff
    
    style M1 fill:#FBC02D,stroke:#F57F17,stroke-width:2px,color:#000
    style M2 fill:#FBC02D,stroke:#F57F17,stroke-width:2px,color:#000
    style M3 fill:#FBC02D,stroke:#F57F17,stroke-width:2px,color:#000
    style M4 fill:#FBC02D,stroke:#F57F17,stroke-width:2px,color:#000
    style M5 fill:#FBC02D,stroke:#F57F17,stroke-width:2px,color:#000
```

## Semantic Model Structure

### ⭐ Star Schema Design

```mermaid
graph TB
    subgraph DIMS["🗂️ Dimension Tables"]
        DATE["📅 Date Dim<br/>Date, Year, Quarter<br/>Month, Day, Weekday"]
        DEALER["🏢 Dealership Dim<br/>DealershipID, Name<br/>City, Region, Country"]
        CUST["👤 Customer Dim<br/>CustomerID, Name<br/>City, State, Segment"]
        VEH["🚙 Vehicle Dim<br/>VehicleID, Make, Model<br/>Year, Type, Category"]
    end

    subgraph FACTS["📊 Fact Tables"]
        SALES["💰 Sales Fact<br/>SaleID, SaleDate<br/>SalePrice, Discount<br/>NetSalePrice, Margin"]
        SERVICE["🔧 Service Fact<br/>ServiceID, ServiceDate<br/>TotalCost, PartsCost<br/>LaborCost, Satisfaction"]
        TEST["🚗 Test Drive Fact<br/>TestDriveID, Date<br/>Outcome, Feedback<br/>Duration, Conversion"]
    end

    SALES -.->|Many:One| DATE
    SALES -.->|Many:One| DEALER
    SALES -.->|Many:One| CUST
    SALES -.->|Many:One| VEH
    
    SERVICE -.->|Many:One| DATE
    SERVICE -.->|Many:One| DEALER
    SERVICE -.->|Many:One| CUST
    
    TEST -.->|Many:One| DATE
    TEST -.->|Many:One| DEALER
    TEST -.->|Many:One| CUST
    TEST -.->|Many:One| VEH

    style DIMS fill:#E1F5FE,stroke:#0277BD,stroke-width:3px,color:#000
    style FACTS fill:#FFF9C4,stroke:#F57F17,stroke-width:3px,color:#000
    
    style DATE fill:#C5CAE9,stroke:#3F51B5,stroke-width:2px,color:#000
    style DEALER fill:#C5CAE9,stroke:#3F51B5,stroke-width:2px,color:#000
    style CUST fill:#C5CAE9,stroke:#3F51B5,stroke-width:2px,color:#000
    style VEH fill:#C5CAE9,stroke:#3F51B5,stroke-width:2px,color:#000
    
    style SALES fill:#FBC02D,stroke:#F57F17,stroke-width:3px,color:#000
    style SERVICE fill:#FBC02D,stroke:#F57F17,stroke-width:3px,color:#000
    style TEST fill:#FBC02D,stroke:#F57F17,stroke-width:3px,color:#000
```

## Two MCP Servers in Action

### 1️⃣ Power BI Modeling MCP Server
**Purpose:** Generate DAX measures and enhance model metadata

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryTextColor':'#000000', 'primaryColor':'#fff', 'primaryBorderColor':'#000', 'lineColor':'#000', 'secondaryColor':'#f4f4f4', 'tertiaryColor':'#fff', 'noteTextColor':'#000000', 'noteBkgColor':'#fff5ad', 'actorTextColor':'#000000', 'labelTextColor':'#000000'}}}%%
sequenceDiagram
    participant Dev as 👨‍💻 Developer
    participant MCP as ⚡ Modeling MCP
    participant Model as 📊 Semantic Model
    
    rect rgb(252, 228, 236)
        Note over Dev,Model: Step 1: Create Base Measures
        Dev->>MCP: "Create sales and margin measures"
        MCP->>Dev: Generate DAX code
        Dev->>Model: Add measures to model
    end
    
    rect rgb(225, 245, 254)
        Note over Dev,Model: Step 2: Add Time Intelligence
        Dev->>MCP: "Add time intelligence measures"
        MCP->>Dev: YoY, MoM, YTD, Rolling Avg
        Dev->>Model: Apply time calculations
    end
    
    rect rgb(232, 245, 233)
        Note over Dev,Model: Step 3: Enhance Metadata
        Dev->>MCP: "Add descriptions to all measures"
        MCP->>Dev: Business-friendly descriptions
        Dev->>Model: Update metadata
    end
    
    rect rgb(255, 249, 196)
        Note over Dev,Model: Step 4: Optimize Performance
        Dev->>MCP: "Optimize DAX performance"
        MCP->>Dev: Improved measure formulas
        Dev->>Model: Replace with optimized code
    end
```

### 2️⃣ Power BI Remote MCP Server
**Purpose:** Query semantic model using natural language

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryTextColor':'#000000', 'primaryColor':'#fff', 'primaryBorderColor':'#000', 'lineColor':'#000', 'secondaryColor':'#f4f4f4', 'tertiaryColor':'#fff', 'noteTextColor':'#000000', 'noteBkgColor':'#fff5ad', 'actorTextColor':'#000000', 'labelTextColor':'#000000'}}}%%
sequenceDiagram
    participant User as 👤 Business User
    participant MCP as ⚡ Remote MCP
    participant Model as 📊 Semantic Model
    participant Results as 💡 Query Results
    
    rect rgb(225, 245, 254)
        Note over User,Results: Query 1: Product Analysis
        User->>MCP: "What are top 10 vehicles by sales?"
        MCP->>Model: Generate & Execute DAX Query
        Model->>Results: Return aggregated data
        Results->>User: Display top vehicles
    end
    
    rect rgb(232, 245, 233)
        Note over User,Results: Query 2: Time Series Analysis
        User->>MCP: "Show monthly sales trends"
        MCP->>Model: Time-series DAX query
        Model->>Results: Monthly aggregations
        Results->>User: Display trends
    end
    
    rect rgb(255, 249, 196)
        Note over User,Results: Query 3: Customer Analytics
        User->>MCP: "Customer lifetime value top 20"
        MCP->>Model: Complex calculated query
        Model->>Results: Customer analytics
        Results->>User: Display rankings
    end
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

## 🔄 AI-Assisted Development Workflow

```mermaid
graph TB
    START["🥇 Start with Gold Tables<br/>from Lab 1"]
    
    subgraph SETUP["🛠️ Model Setup Phase"]
        BUILD["📊 Build Semantic Model<br/>Import Gold Tables"]
        REL["⭐ Configure Relationships<br/>Star Schema Design"]
    end
    
    subgraph DEV["👨‍💻 Development Phase (Modeling MCP)"]
        MODELING["⚡ Power BI Modeling MCP<br/>AI-Assisted DAX Generation"]
        GEN1["💰 Generate Base Measures<br/>Sales, Service, Test Drive"]
        GEN2["⏰ Generate Time Intelligence<br/>YoY, MoM, YTD, Rolling Avg"]
        GEN3["👥 Generate Customer Analytics<br/>Lifetime Value, Retention"]
        META["📝 Add Metadata<br/>Descriptions, Synonyms"]
        OPT["⚡ Optimize Performance<br/>Improve DAX Formulas"]
    end
    
    PUBLISH["🚀 Publish to<br/>Power BI Service"]
    
    subgraph CONSUME["👥 Consumption Phase (Remote MCP)"]
        REMOTE["⚡ Power BI Remote MCP<br/>Natural Language Queries"]
        QUERY1["❓ Top vehicles by sales"]
        QUERY2["📈 Monthly sales trends"]
        QUERY3["🏢 Dealership rankings"]
        QUERY4["👤 Customer insights"]
    end
    
    INSIGHTS["💡 Business Insights<br/>& Decision Making"]
    
    START --> BUILD
    BUILD --> REL
    REL --> MODELING
    
    MODELING --> GEN1
    GEN1 --> GEN2
    GEN2 --> GEN3
    GEN3 --> META
    META --> OPT
    
    OPT --> PUBLISH
    
    PUBLISH --> REMOTE
    REMOTE --> QUERY1 & QUERY2 & QUERY3 & QUERY4
    
    QUERY1 & QUERY2 & QUERY3 & QUERY4 --> INSIGHTS
    
    style START fill:#E8F5E9,stroke:#388E3C,stroke-width:3px,color:#000
    style SETUP fill:#E3F2FD,stroke:#1976D2,stroke-width:3px,color:#000
    style DEV fill:#FCE4EC,stroke:#C2185B,stroke-width:3px,color:#000
    style CONSUME fill:#E1F5FE,stroke:#0277BD,stroke-width:3px,color:#000
    
    style MODELING fill:#C2185B,stroke:#880E4F,stroke-width:4px,color:#fff
    style REMOTE fill:#0277BD,stroke:#01579B,stroke-width:4px,color:#fff
    style PUBLISH fill:#FF6F00,stroke:#E65100,stroke-width:3px,color:#fff
    style INSIGHTS fill:#388E3C,stroke:#1B5E20,stroke-width:4px,color:#fff
    
    style GEN1 fill:#F8BBD0,stroke:#C2185B,stroke-width:2px,color:#000
    style GEN2 fill:#F8BBD0,stroke:#C2185B,stroke-width:2px,color:#000
    style GEN3 fill:#F8BBD0,stroke:#C2185B,stroke-width:2px,color:#000
    style META fill:#F8BBD0,stroke:#C2185B,stroke-width:2px,color:#000
    style OPT fill:#F8BBD0,stroke:#C2185B,stroke-width:2px,color:#000
    
    style QUERY1 fill:#B3E5FC,stroke:#0277BD,stroke-width:2px,color:#000
    style QUERY2 fill:#B3E5FC,stroke:#0277BD,stroke-width:2px,color:#000
    style QUERY3 fill:#B3E5FC,stroke:#0277BD,stroke-width:2px,color:#000
    style QUERY4 fill:#B3E5FC,stroke:#0277BD,stroke-width:2px,color:#000
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

- ✅ **Built on Gold Layer:** Clean, analytics-ready data from Lab 1
- ✅ **Star Schema:** Optimized query performance
- ✅ **Reusable Measures:** Consistent calculations across all reports
- ✅ **AI-Generated DAX:** Rapid measure development
- ✅ **Natural Language Queries:** Business users query without DAX knowledge
- ✅ **Rich Metadata:** Descriptions improve AI query generation
- ✅ **Time Intelligence:** Sophisticated date calculations built-in
- ✅ **Customer Analytics:** Deep insights into customer behavior

## 🔗 Integration Points

```mermaid
graph LR
    subgraph SOURCES["📥 Data Sources"]
        LAB1["🥇 Lab 1<br/>Gold Tables<br/>Analytics-Ready"]
        LAB2["⚡ Lab 2<br/>KQL Data<br/>Real-Time (Optional)"]
    end
    
    MODEL["📊 Semantic Model<br/>Unified Business Logic<br/>DAX Measures"]
    
    subgraph CONSUMERS["📤 Consumption Layers"]
        REPORTS["📄 Power BI Reports<br/>Visual Analytics"]
        DASHBOARDS["📊 Dashboards<br/>Executive Views"]
        REMOTE["⚡ Remote MCP<br/>Natural Language<br/>Queries"]
        APPS["📱 Power BI Apps<br/>Packaged Solutions"]
    end
    
    LAB1 -->|Source Data| MODEL
    LAB2 -.->|Real-time<br/>Supplement| MODEL
    
    MODEL --> REPORTS
    MODEL --> DASHBOARDS
    MODEL --> REMOTE
    MODEL --> APPS
    
    style SOURCES fill:#E8F5E9,stroke:#388E3C,stroke-width:3px,color:#000
    style CONSUMERS fill:#E1F5FE,stroke:#0277BD,stroke-width:3px,color:#000
    
    style LAB1 fill:#66BB6A,stroke:#2E7D32,stroke-width:3px,color:#fff
    style LAB2 fill:#EF5350,stroke:#C62828,stroke-width:2px,color:#fff
    style MODEL fill:#FBC02D,stroke:#F57F17,stroke-width:4px,color:#000
    style REMOTE fill:#0277BD,stroke:#01579B,stroke-width:3px,color:#fff
    style REPORTS fill:#42A5F5,stroke:#1565C0,stroke-width:2px,color:#000
    style DASHBOARDS fill:#42A5F5,stroke:#1565C0,stroke-width:2px,color:#000
    style APPS fill:#42A5F5,stroke:#1565C0,stroke-width:2px,color:#000
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
