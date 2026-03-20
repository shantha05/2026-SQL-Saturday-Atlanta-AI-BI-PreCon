# SQL Saturday Atlanta AI BI Pre Con Session — Lab 1

## Provisioning and Exploring Fabric MCP Server

## Objective

Use Fabric MCP in an MCP-capable client to explore and scaffold a complete medallion architecture (bronze, silver, gold) Fabric solution for Contoso Auto 360.

**Estimated Time:** 60-75 minutes

## Lab 1 Steps

### Part A — Prepare the Fabric workspace

1. Sign in to Microsoft Fabric.
2. Create a new workspace named `ContosoAuto360-MCP-Workshop`.
3. Create a Lakehouse named `lh_contoso_auto_360`.
4. In the Lakehouse, create folders if you want structure such as:
   - `Files/raw`
   - `Files/reference`
   - `Tables`

### Part B — Upload batch files

1. Upload these files from the `01-Datasets/` folder to the Lakehouse `Files/raw` area:
   - `dealerships.csv`
   - `inventory.csv`
   - `customers.csv`
   - `sales.csv`
   - `service_orders.csv`
   - `test_drives.csv`

### Part C — Set up the MCP-capable client

1. Open VS Code.
2. Enable GitHub Copilot chat / agent mode, or another MCP-capable client.
3. Install the **Microsoft Fabric MCP Server**:
   - Follow the official Microsoft Fabric MCP Server documentation
   - Authenticate with your Azure/Fabric credentials
   - Configure the server to connect to your Fabric workspace
4. Verify the Fabric MCP server is visible in the client tools list.
5. Test the connection by asking a simple question like "List my Fabric workspaces."

### Part D — Explore Fabric with prompts

1. Start a new agent chat and use these prompts one by one:
    - “Create a Microsoft Fabric solution plan for Contoso Auto 360 using a Lakehouse, notebook, semantic model, and RTI components.”
    - “Suggest naming conventions and workspace item structure for Contoso Auto 360.”
    - “Explain the ingestion flow for dealerships, inventory, customers, sales, service orders, and test drives.”
    - “Generate notebook steps to load all six CSV files into delta tables.”

### Part E — Create the notebook manually or with assistance

1. Create a notebook called `nb_load_contoso_auto_360` in your Fabric workspace.
2. Use Fabric MCP guidance to produce PySpark code by asking:
    - "Generate PySpark code to load CSV files from Files/raw into delta tables"
    - "Show me the best practices for loading batch data into Fabric Lakehouse"
3. For each CSV file, implement code to:
    - Read from `Files/raw/...`
    - Infer or define schema
    - Write to a managed delta table

    Example pattern:

    ```python
    df = spark.read.option("header", True).csv("Files/raw/dealerships.csv")
    df.write.mode("overwrite").format("delta").saveAsTable("bronze_dealerships")
    ```

    **Note on table naming:** Use the `bronze_` prefix for all bronze layer tables to make the medallion architecture layer explicit and follow best practices.

4. Repeat the pattern for all six batch files, creating these bronze tables:
   - bronze_dealerships
   - bronze_inventory
   - bronze_customers
   - bronze_sales
   - bronze_service_orders
   - bronze_test_drives
5. Run the notebook to load all data.
6. Validate the load by checking:
    - Row counts match source files
    - All six delta tables appear in the Lakehouse with bronze_ prefix
    - Data types are correctly inferred

### Part F — Create Silver Layer using Fabric MCP

1. Ask the Fabric MCP Server to design your silver layer transformation:

   **Prompt:**

   ```text
   I have bronze tables in my Fabric Lakehouse: bronze_dealerships, bronze_inventory, bronze_customers, bronze_sales, bronze_service_orders, and bronze_test_drives. 
   Create a notebook that transforms these into silver layer tables with the following:
   - Remove duplicate records
   - Handle null values appropriately
   - Standardize data types and formats
   - Add audit columns (load_timestamp, source_system)
   - Apply data quality validations
   Generate the complete PySpark notebook code for all six silver tables following medallion architecture best practices.
   ```

2. Review the generated notebook code from Fabric MCP.

3. Create a new notebook in Fabric named `nb_bronze_to_silver` and copy the MCP-generated code.

4. If you need specific transformations, ask follow-up prompts such as:
   
   **Prompt for specific business rules:**

   ```text
   For the silver_sales table, add these specific transformations:
   - Filter out any sales with negative prices
   - Standardize currency to USD with 2 decimal places
   - Flag any records where discount exceeds sale price
   - Add a data quality score column
   ```

5. Run the silver layer notebook and validate:
    - All six silver tables are created
    - Data quality improvements are applied
    - Record counts are reasonable (after deduplication)

### Part G — Create Gold Layer using Fabric MCP

1. Ask the Fabric MCP Server to design your gold layer for analytics:

   **Prompt:**

   ```text
   Based on my silver layer tables (silver_dealerships, silver_inventory, silver_customers, silver_sales, silver_service_orders, silver_test_drives), 
   create a notebook that builds gold layer tables optimized for Power BI analytics:
   - gold_sales_fact: Enriched sales data joined with dimensions (dealerships, customers, vehicles)
   - gold_service_fact: Enriched service order data with dealership and customer details
   - gold_test_drive_fact: Test drive analysis with conversion metrics
   - gold_dealership_dim: Clean dealership dimension
   - gold_customer_dim: Clean customer dimension  
   - gold_vehicle_dim: Vehicle dimension with full specifications
   - gold_sales_summary: Daily aggregated sales metrics by dealership and region
   Generate the complete PySpark notebook code following star schema design patterns.
   ```

2. Review the generated gold layer notebook code.

3. Create a new notebook in Fabric named `nb_silver_to_gold` and copy the MCP-generated code.

4. For additional analytics tables, ask follow-up prompts:

   **Prompt for additional metrics:**

   ```text
   Add a gold_kpi_summary table that includes these business metrics:
   - Total sales revenue by dealership and month
   - Average sale price by vehicle type and region
   - Service revenue and customer satisfaction scores
   - Test drive conversion rates by dealership
   Include year-over-year comparisons and rolling 3-month averages.
   ```

5. Run the gold layer notebook and validate:
    - All fact and dimension tables are created
    - Joins are working correctly
    - Aggregations match expected business logic

### Part H — Orchestration and Documentation

1. Ask Fabric MCP for orchestration guidance:

   **Prompt:**

   ```text
   I have three notebooks: nb_load_contoso_auto_360 (bronze), nb_bronze_to_silver, and nb_silver_to_gold.
   How should I orchestrate these notebooks in Microsoft Fabric to create a production-ready data pipeline?
   Include recommendations for:
   - Scheduling and dependencies
   - Error handling and retries
   - Incremental loading strategies
   - Data quality monitoring
   ```

2. Ask for documentation:

   **Prompt:**

   ```text
   Generate documentation for my Contoso Auto 360 medallion architecture including:
   - Data lineage from bronze to silver to gold
   - Table schemas and relationships
   - Data quality rules applied at each layer
   - Recommended refresh schedules
   - Best practices for maintaining this architecture
   ```

3. Review the MCP-generated orchestration and documentation recommendations.

### Part I — Review and discuss

1. Ask the MCP client:
    - "What are the key benefits of implementing a medallion architecture for this solution?"
    - "How does the separation of bronze, silver, and gold layers improve data governance?"
    - "What monitoring and alerting should I set up for this data pipeline?"
    - "What Power BI semantic model design would best leverage these gold layer tables?"

## Lab 1 Expected Outcome

At the end of Lab 1 you should have:

- A Fabric workspace named `ContosoAuto360-MCP-Workshop`
- One Lakehouse: `lh_contoso_auto_360`
- Six CSV files uploaded to `Files/raw`
- **Bronze Layer:** Six raw delta tables with source data
  - bronze_dealerships
  - bronze_inventory
  - bronze_customers
  - bronze_sales
  - bronze_service_orders
  - bronze_test_drives
- **Silver Layer:** Six cleansed and standardized tables
  - silver_dealerships
  - silver_inventory
  - silver_customers
  - silver_sales
  - silver_service_orders
  - silver_test_drives
- **Gold Layer:** Analytics-ready tables
  - gold_sales_fact
  - gold_service_fact
  - gold_test_drive_fact
  - gold_dealership_dim
  - gold_customer_dim
  - gold_vehicle_dim
  - gold_sales_summary
- Three notebooks implementing the complete medallion architecture:
  - `nb_load_contoso_auto_360` (bronze layer)
  - `nb_bronze_to_silver` (silver layer)
  - `nb_silver_to_gold` (gold layer)
- Experience using Fabric MCP Server to generate production-ready data pipeline code
- Understanding of orchestration and data governance best practices

## Troubleshooting Tips

- **MCP Server not connecting:** Verify authentication and workspace permissions
- **Files not uploading:** Check file paths and Lakehouse folder structure
- **Delta table errors:** Ensure schema compatibility and valid data types
- **Notebook failures:** Run cells individually to isolate issues
- **MCP not generating complete code:** Be specific in prompts about table names and requirements
- **Silver layer errors:** Check that bronze tables exist and contain data before transformation
- **Gold layer join failures:** Verify that relationship keys match between silver tables
- **Performance issues:** Ask MCP for optimization suggestions specific to your data volume

## Key Prompts Summary

**Bronze to Silver:**

```text
Create silver layer transformations with deduplication, null handling, standardization, and audit columns
```

**Silver to Gold:**

```text
Create gold layer fact and dimension tables optimized for Power BI analytics with star schema design
```

**Orchestration:**

```text
Recommend orchestration strategy for bronze → silver → gold pipeline with scheduling and error handling
```

**Custom Requirements:**

```text
Add specific business rules to [table name]: [list your requirements]
```
