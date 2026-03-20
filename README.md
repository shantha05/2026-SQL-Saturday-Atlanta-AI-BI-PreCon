# SQL Saturday Atlanta AI BI Pre Con Session

## Deep Dive: Power BI and Fabric MCP Servers in Action

## What You'll Learn

This hands-on workshop demonstrates how Model Context Protocol (MCP) servers work with Microsoft Fabric and Power BI to build complete data solutions using AI-assisted development. Learn to use natural language prompts to generate production-ready code, create medallion architectures, and build semantic models—all without writing code manually.

**Key Skills:**

- Using Fabric MCP to generate notebooks for medallion architecture (bronze/silver/gold layers)
- Leveraging RTI MCP for real-time analytics with KQL
- Building semantic models with Power BI Modeling MCP
- Querying data with Power BI Remote MCP using natural language
- Prompt engineering for AI-assisted data development

**Workshop Flow:**

1. **Lab 1 (60-75 min):** Build complete medallion architecture using Fabric MCP prompts
2. **Lab 2 (30-45 min):** Analyze real-time telemetry using RTI MCP queries
3. **Lab 3 (45-60 min):** Create and query semantic models with Power BI MCP Servers

**Total Time:** Approximately 2.5-3 hours

### Lab 1: Provisioning and Exploring Fabric MCP Server

In this lab, you'll build a complete medallion architecture (bronze, silver, gold) lakehouse solution for Contoso Auto 360, an automotive dealership network. You'll create a Fabric workspace and lakehouse, upload batch data files (dealerships, inventory, customers, sales, service orders, and test drives), and then use the Fabric MCP Server with natural language prompts to generate production-ready notebooks.

**What you'll build:**

- **Bronze Layer:** Load raw CSV files into delta tables
- **Silver Layer:** Create cleansed and standardized tables with data quality validations
- **Gold Layer:** Build analytics-ready fact and dimension tables optimized for reporting

The Fabric MCP Server will generate all the PySpark notebook code through AI-assisted prompts, teaching you how to use natural language to scaffold complete data pipelines following medallion architecture best practices.

**Time:** 60-75 minutes

### Lab 2: Real-Time Intelligence MCP Server in Action

This lab focuses on real-time telemetry data from dealership operations. You'll create an Eventhouse and KQL database to work with live event data from dealership locations, including customer counts, wait times, sentiment scores, queue lengths, and alerts. Using the RTI (Real-Time Intelligence) MCP Server, you'll query telemetry data using natural language prompts to analyze operational patterns.

**What you'll learn:**

- Create and configure Real-Time Intelligence resources in Fabric
- Load telemetry data into KQL tables
- Use natural language to generate KQL queries for business insights
- Explore alerting patterns for real-time monitoring

Sample questions: Which dealerships have the highest wait times? What's the average sentiment score by region? Which events trigger alerts most frequently?

**Time:** 30-45 minutes

### Lab 3: Power BI MCP Server Integration

In the final lab, you'll build a semantic model on top of the gold layer tables from Lab 1 and use both Power BI MCP Servers to create and query a production-ready analytics solution. You'll leverage AI-assisted development to generate DAX measures, enhance metadata, and query the model using natural language.

**What you'll build:**

- Semantic model using gold layer fact and dimension tables from Lab 1
- 30+ business measures including sales, service, and test drive analytics
- Time intelligence measures (YoY, MoM, YTD, rolling averages)
- Customer lifetime value and dealership performance KPIs
- Complete metadata and descriptions for AI-powered queries

**Two MCP Servers in action:**

- **Power BI Modeling MCP Server:** Generate DAX measures, optimize performance, add descriptions
- **Power BI Remote MCP Server:** Query the model using natural language business questions

Experience the complete workflow: from medallion architecture in Lab 1, through real-time analytics in Lab 2, to AI-powered semantic modeling in Lab 3.

**Time:** 45-60 minutes  
**Prerequisites:** Complete Lab 1 with gold layer tables created

Power BI MCP Server

{
  "servers": {
    "powerbi-remote": {
      "type": "http",
      "url": "https://api.fabric.microsoft.com/v1/mcp/powerbi"
    }
  }
}