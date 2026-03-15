# SQL Saturday Atlanta AI BI Pre Con Session — Lab 3

## Power BI MCP Server Integration

## Objective

Use Power BI Remote MCP Server for querying a semantic model and Power BI Modeling MCP Server for model enhancement. Build on the gold layer tables created in Lab 1 to create a production-ready semantic model.

**Estimated Time:** 45-60 minutes

**Prerequisites:** Complete Lab 1 with gold layer tables created

## Lab 3 Steps

### Part A — Build the semantic model from gold tables

1. In your Fabric workspace, navigate to the Lakehouse (`lh_contoso_auto_360`) from Lab 1.
2. Create a new semantic model named `sm_contoso_auto_360`.
3. Add the **gold layer fact tables** created in Lab 1:
   - gold_sales_fact
   - gold_service_fact
   - gold_test_drive_fact
4. Add the **gold layer dimension tables** from Lab 1:
   - gold_dealership_dim
   - gold_customer_dim
   - gold_vehicle_dim
5. If you created a Date table in Lab 1, add it; otherwise create one:
   - Include columns: Date, Year, Quarter, Month, Day
   - Mark as a date table in the model
6. Verify and configure relationships (these should be pre-established in gold tables):
   - gold_sales_fact[DealershipID] -> gold_dealership_dim[DealershipID]
   - gold_sales_fact[CustomerID] -> gold_customer_dim[CustomerID]
   - gold_sales_fact[VehicleID] -> gold_vehicle_dim[VehicleID]
   - gold_sales_fact[SaleDate] -> Date[Date]
   - Apply similar relationships for gold_service_fact and gold_test_drive_fact
7. Hide any technical columns that shouldn't be visible to report creators (IDs, load timestamps, etc.)

### Part B — Create core measures using Power BI Modeling MCP

1. Install the **Power BI Modeling MCP Server** in VS Code:
   - Follow the official documentation
   - Connect to your semantic model project

2. Use the Modeling MCP Server to generate initial measures with this prompt:

   **Prompt:**

   ```text
   I have a Power BI semantic model with these tables:
   - gold_sales_fact (SalePrice, Discount, NetSalePrice, SaleDate, DealershipID, CustomerID, VehicleID)
   - gold_service_fact (TotalCost, PartsCost, LaborCost, SatisfactionScore, ServiceDate, DealershipID)
   - gold_test_drive_fact (TestDriveDate, Outcome, DealershipID, CustomerID)
   - gold_dealership_dim (DealershipName, City, Region)
   - gold_customer_dim (CustomerName, City, State)
   - gold_vehicle_dim (Make, Model, VehicleYear, Type)
   
   Create essential business measures for:
   - Total sales revenue and discounts
   - Gross margin calculations
   - Service revenue and margins
   - Customer satisfaction metrics
   - Test drive conversion tracking
   - Average sale prices
   Include proper error handling with DIVIDE function.
   ```

3. Review the generated DAX measures and add them to your semantic model.

4. Ask for additional time intelligence measures:

   **Prompt:**

   ```text
   Add time intelligence measures for my sales and service facts:
   - Year-over-year growth for sales and service revenue
   - Month-over-month comparison
   - Year-to-date totals
   - Rolling 3-month and 12-month averages
   - Same period last year comparisons
   ```

5. Validate all measures work correctly by creating test visuals.

### Part C — Enhance model metadata using Power BI Modeling MCP

1. Use the Modeling MCP Server to add descriptions to all model elements:

   **Prompt:**

   ```text
   Review my semantic model and add business-friendly descriptions to:
   - All tables: Explain what data each table contains and its purpose
   - All columns: Describe what each field represents in business terms
   - All measures: Explain the calculation and business meaning
   - All relationships: Document how tables are connected
   
   Make descriptions clear for non-technical business users who will use this model for reporting.
   ```

2. Ask for Q&A optimization:

   **Prompt:**

   ```text
   Suggest synonyms and Q&A optimizations for my Contoso Auto 360 semantic model to improve natural language query understanding.
   Include common business terms like "revenue", "profit", "cars", "vehicles", "stores", "locations", etc.
   ```

3. Request data quality and performance recommendations:

   **Prompt:**

   ```text
   Analyze my semantic model and suggest:
   - Performance optimizations for DAX measures
   - Recommended aggregations for large fact tables
   - Columns that should be hidden from report view
   - Potential data quality issues or missing validations
   - Best practices for relationship configurations
   ```

4. Apply the suggested improvements to your model.
5. Save and publish the enhanced semantic model to the Power BI service.

### Part D — Query the model using Power BI Remote MCP Server

1. Verify the Power BI Remote MCP tenant setting is enabled (if required by your environment).
2. In VS Code, install the **Power BI Remote MCP Server**:
   - Follow the official Power BI Remote MCP Server documentation
   - Authenticate with your Power BI credentials
   - Configure access to your workspace and semantic model
3. Locate your semantic model ID:
   - Open the semantic model in Power BI Service
   - Copy the model ID from the URL

4. Start a new chat with Remote MCP and ask these business questions:

   **Sales Analysis Prompts:**
   
   ```text
   What are the top 10 vehicle makes by total sales revenue?
   ```
   
   ```text
   Show me monthly sales trends for the last 12 months
   ```
   
   ```text
   Which dealerships have the highest gross margin percentage?
   ```
   
   ```text
   What is the average sale price by vehicle type and region?
   ```

   **Service Analysis Prompts:**
   
   ```text
   Which dealerships have the highest service revenue?
   ```
   
   ```text
   What is the average customer satisfaction score by region?
   ```
   
   ```text
   Show me service margin trends over time
   ```

   **Test Drive Analysis Prompts:**
   
   ```text
   What is the test drive conversion rate by dealership?
   ```
   
   ```text
   Which vehicle models have the highest test drive to sale conversion?
   ```
   
   ```text
   Compare test drive volumes and conversion rates by region
   ```

   **Cross-Functional Analysis Prompts:**
   
   ```text
   Show me the relationship between test drives and actual sales by dealership
   ```
   
   ```text
   Which customers have the highest lifetime value (sales + service)?
   ```

5. Review the DAX queries generated by the Remote MCP server for each question.
6. Observe how the model descriptions and metadata influence query generation quality.
7. Note which questions worked well and which need model improvements.

### Part E — Enhance measures using Power BI Modeling MCP Server

1. Based on the queries from Part D, ask Modeling MCP to create additional measures:

   **Prompt for customer analytics:**

   ```text
   Create measures for customer lifetime value analysis:
   - Total customer sales (sum of all purchases)
   - Total customer service spending
   - Customer lifetime value (sales + service)
   - Average days between purchases
   - Customer retention rate
   - New vs returning customer metrics
   ```

   **Prompt for dealership performance:**

   ```text
   Create dealership performance KPIs:
   - Sales per dealership (average and total)
   - Service revenue per dealership
   - Combined dealership performance score
   - Market share by dealership within region
   - Dealership efficiency metrics (sales per employee if data available)
   ```

   **Prompt for vehicle analytics:**

   ```text
   Create vehicle performance measures:
   - Top selling vehicles by units and revenue
   - Average days in inventory
   - Vehicle profitability (margin by make/model)
   - Warranty attachment rate by vehicle type
   - Vehicle popularity index
   ```

2. Ask for measure optimization:

   **Prompt:**

   ```text
   Review all my DAX measures and suggest performance optimizations:
   - Identify measures that could be simplified
   - Suggest variables to reduce formula complexity
   - Recommend which measures should use CALCULATE vs direct aggregations
   - Identify opportunities to use measure branching
   ```

3. Request measure documentation:

   **Prompt:**

   ```text
   For each measure in my model, ensure it has:
   - A clear business description
   - The calculation logic explained
   - Example use cases
   - Any assumptions or limitations
   - Related measures that work together
   ```

4. Apply the generated measures and optimizations to your model.
5. Republish the enhanced semantic model.

### Part F — Validate and compare MCP capabilities

1. Test Remote MCP with the enhanced model:

   **Re-run previous questions and compare:**
   - Do the same queries from Part D return better results?
   - Are the generated DAX queries more efficient?
   - Does the improved metadata lead to more accurate answers?

2. Test new measures with Remote MCP:

   **Prompt:**

   ```text
   What is the customer lifetime value for the top 20 customers?
   ```
   
   ```text
   Show me dealership performance rankings by combined sales and service revenue
   ```
   
   ```text
   Which vehicle models have the best profitability?
   ```

3. Compare AI-assisted vs manual development:
   - Time saved using MCP for measure creation
   - Quality and consistency of generated DAX
   - Completeness of documentation and descriptions
   - Ease of implementing complex calculations

4. Document your findings:
   - Which MCP server features were most valuable?
   - What types of prompts worked best?
   - Where did you still need manual intervention?
   - How would you use these tools in production projects?

## Lab 3 Expected Outcome

At the end of Lab 3 you should have:

- A complete semantic model: `sm_contoso_auto_360` built on gold layer tables from Lab 1
- **Gold layer tables integrated:**
  - gold_sales_fact
  - gold_service_fact  
  - gold_test_drive_fact
  - gold_dealership_dim
  - gold_customer_dim
  - gold_vehicle_dim
- **Comprehensive measures created via Modeling MCP:**
  - Sales metrics (revenue, discounts, margins)
  - Service metrics (revenue, satisfaction, margins)
  - Test drive conversion analytics
  - Time intelligence measures (YoY, MoM, YTD, rolling averages)
  - Customer lifetime value metrics
  - Dealership performance KPIs
  - Vehicle profitability measures
- **Enhanced metadata:**
  - Business-friendly descriptions for all tables, columns, and measures
  - Q&A synonyms for improved natural language queries
  - Performance optimizations applied
- **Power BI Remote MCP Server** configured and tested with 15+ business questions
- **Power BI Modeling MCP Server** used to generate and optimize 30+ DAX measures
- Experience with prompt engineering for both query generation and model development
- Understanding of how gold layer architecture simplifies semantic model creation
- Documentation of MCP capabilities and best practices

## Troubleshooting Tips

- **Gold tables not available:** Ensure Lab 1 is completed and gold layer tables were created successfully
- **Semantic model creation fails:** Check Lakehouse permissions and gold table availability
- **Relationship errors:** Gold tables should have pre-established relationships; verify keys match
- **DAX measure errors:** Validate column references match gold table schema
- **Remote MCP connection issues:** Confirm Power BI Service permissions and correct model ID
- **Remote MCP returns incorrect results:** Improve model descriptions and metadata
- **Modeling MCP not generating measures:** Be specific in prompts about table and column names
- **Modeling MCP not working:** Ensure PBIP format is supported; may need to use web-based model editor
- **Generated DAX is too complex:** Ask MCP to simplify or break into multiple measures
- **Poor query results:** Add more detailed descriptions and Q&A synonyms to improve AI understanding
- **Measures not calculating correctly:** Verify relationships are active and filter context is correct

## Key Prompts Summary for Power BI MCP Servers

**Modeling MCP - Create Measures:**

```text
Create [category] measures for my model with tables [list tables] including [specific requirements]
```

**Modeling MCP - Add Metadata:**

```text
Add business-friendly descriptions to all tables, columns, and measures for non-technical users
```

**Modeling MCP - Optimize:**

```text
Review and optimize all DAX measures for performance and simplify complex calculations
```

**Remote MCP - Query Model:**

```text
[Ask any business question about your data - be specific and natural]
```

**Remote MCP - Complex Analysis:**

```text
Show me [metric] by [dimension] filtered by [criteria] with [time period]
```
