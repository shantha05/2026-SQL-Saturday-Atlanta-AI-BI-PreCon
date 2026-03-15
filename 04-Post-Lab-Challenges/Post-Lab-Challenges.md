# SQL Saturday Atlanta AI BI Pre Con Session — Post-Lab Challenges

These optional challenges are designed for participants who complete the labs early or want to extend their learning. Each challenge builds on the skills learned in the labs and introduces more advanced concepts.

---

## Lab 1 Challenges - Advanced Medallion Architecture

### Challenge 1.1: Incremental Loading ⭐⭐

**Objective:** Convert full-load notebooks to incremental loading for production efficiency.

**Tasks:**

- Modify the bronze layer notebook to implement incremental loading instead of full refresh
- Add watermark columns to track last load timestamp
- Use Delta Lake time travel to handle late-arriving data
- Implement change data capture (CDC) patterns

**Fabric MCP Prompt:**

```text
Convert my full-load bronze notebook to use incremental loading with watermarks. 
Add logic to track the last processed timestamp and only load new or changed records.
Include error handling for cases where watermark is missing.
```

**Success Criteria:**

- Notebook only processes new/changed records
- Properly handles first-time runs (no watermark exists)
- Execution time reduced for subsequent runs

---

### Challenge 1.2: Data Quality Framework ⭐⭐⭐

**Objective:** Build a comprehensive data quality monitoring system.

**Tasks:**

- Create a quarantine layer for rejected records
- Add data quality scorecards for each silver table
- Implement automated validation rules (nulls, formats, ranges, referential integrity)
- Generate data quality reports showing trends over time

**Fabric MCP Prompt:**

```text
Create a data quality framework for my silver layer tables including:
- Validation rules for required fields, data types, and business rules
- A quarantine table for records that fail validation
- Data quality metrics (completeness, validity, consistency scores)
- A quality summary table showing pass/fail rates by rule and table
```

**Success Criteria:**

- Invalid records are captured in quarantine with reason codes
- Quality scores calculated for each table
- Dashboard showing data quality trends

---

### Challenge 1.3: Delta Lake Optimization ⭐⭐

**Objective:** Optimize Delta tables for query performance.

**Tasks:**

- Implement OPTIMIZE and VACUUM operations on large tables
- Add Z-ORDERING on frequently filtered columns
- Partition large fact tables by date
- Enable Delta Lake features (liquid clustering, deletion vectors)

**Fabric MCP Prompt:**

```text
Suggest Delta Lake optimization strategies for my gold_sales_fact table with 10M+ rows:
- Partitioning strategy for optimal query performance
- Z-ORDER columns based on common filter patterns
- OPTIMIZE and VACUUM schedule recommendations
- Performance monitoring queries
```

**Success Criteria:**

- Reduced query execution times (measure before/after)
- Optimized file sizes (fewer small files)
- Documented maintenance schedule

---

### Challenge 1.4: Pipeline Orchestration ⭐⭐

**Objective:** Create production-ready pipeline orchestration.

**Tasks:**

- Build a Fabric Data Pipeline to orchestrate bronze → silver → gold notebooks
- Add error handling and retry logic
- Implement parallel processing where dependencies allow
- Configure success/failure notifications
- Add logging and monitoring

**Success Criteria:**

- Notebooks execute in correct order with dependencies
- Failed steps trigger notifications
- Pipeline can be scheduled for automated runs
- Execution history is tracked

---

## Lab 2 Challenges - Advanced Real-Time Analytics

### Challenge 2.1: Real-Time Alerting ⭐⭐

**Objective:** Implement proactive monitoring with automated alerts.

**Tasks:**

- Configure Fabric Activator (formerly Data Activator) for real-time alerts
- Set up notifications when wait times exceed thresholds
- Create escalation rules for critical events (e.g., multiple alerts from same dealership)
- Design alert message templates with actionable information

**RTI MCP Prompt:**

```text
Create alert rules for my telemetry_events table:
- Trigger when average wait time exceeds 15 minutes
- Alert when sentiment score drops below 3.0
- Detect queue length spikes (>10) sustained for 30+ minutes
Include recommended KQL queries for the alert definitions.
```

**Success Criteria:**

- Alerts trigger based on real-time conditions
- Notifications sent via Teams/Email
- Alert history tracked for analysis

---

### Challenge 2.2: Streaming with Eventstream ⭐⭐⭐

**Objective:** Convert batch loading to real-time streaming.

**Tasks:**

- Create an Eventstream to ingest telemetry data
- Simulate real-time data flow (use Power Automate or custom script)
- Add stream transformations (filtering, enrichment)
- Create Real-Time Dashboard in Power BI

**Success Criteria:**

- Data flows continuously to KQL database
- Transformations applied in real-time
- Dashboard updates without refresh

---

### Challenge 2.3: Advanced KQL Analytics ⭐⭐⭐

**Objective:** Leverage advanced KQL features for deeper insights.

**Tasks:**

- Create materialized views for frequently-used aggregations
- Build time-series analysis with anomaly detection using `series_decompose_anomalies()`
- Implement user-defined functions for complex calculations
- Use machine learning functions for pattern detection

**RTI MCP Prompt:**

```text
Create advanced KQL analytics for my telemetry data:
- Materialized view for hourly dealership performance metrics
- User-defined function for calculating customer experience scores
- Anomaly detection query for unusual wait time patterns
- Time-series forecasting for queue lengths
```

**Success Criteria:**

- Materialized views improve query performance
- Anomalies automatically detected
- Custom functions reusable across queries

---

### Challenge 2.4: Cross-Database Queries ⭐⭐

**Objective:** Unify batch and real-time analytics.

**Tasks:**

- Query both Lakehouse delta tables and KQL database together
- Create shortcuts between Eventhouse and Lakehouse
- Build reports combining historical trends with real-time status
- Compare same-day sales (Lakehouse) with current wait times (KQL)

**Success Criteria:**

- Single query joins Lakehouse and KQL data
- Report shows both historical context and real-time state
- Performance is acceptable for interactive queries

---

## Lab 3 Challenges - Advanced Semantic Modeling

### Challenge 3.1: Advanced DAX Patterns ⭐⭐⭐

**Objective:** Implement sophisticated calculation patterns.

**Tasks:**

- Create calculation groups for time intelligence (instead of individual measures)
- Implement field parameters for dynamic measure selection
- Build what-if parameters for scenario analysis
- Use measure branching for complex logic

**Power BI Modeling MCP Prompt:**

```text
Create calculation groups for my semantic model:
- Time intelligence group with MTD, QTD, YTD, YoY, PY, and rolling averages
- Variance group with absolute, percentage, and index calculations
- Format strings for each calculation item
Show how to use these with my existing base measures.
```

**Success Criteria:**

- One calculation group replaces 20+ time intelligence measures
- Users can dynamically switch between calculations
- Report size reduced, maintenance simplified

---

### Challenge 3.2: Row-Level Security ⭐⭐

**Objective:** Implement data security at the row level.

**Tasks:**

- Create RLS roles to filter dealership data by region
- Test RLS with different user contexts
- Implement dynamic RLS using USERPRINCIPALNAME()
- Document security model and testing approach

**Power BI Modeling MCP Prompt:**

```text
Implement row-level security for my semantic model:
- Role for each region (East, West, North, South)
- Admin role with access to all regions
- Dynamic role using user email to filter their assigned region
- Test DAX expressions to validate security
```

**Success Criteria:**

- Users only see their authorized data
- Performance remains acceptable with RLS filters
- Security documented and tested

---

### Challenge 3.3: Composite Model ⭐⭐⭐

**Objective:** Build hybrid model combining Import and DirectQuery.

**Tasks:**

- Create composite model with Import for gold tables, DirectQuery for KQL
- Set up aggregations to improve DirectQuery performance
- Build reports showing both historical (Import) and real-time (DirectQuery) data
- Optimize refresh strategies for each storage mode

**Success Criteria:**

- Model uses appropriate storage mode for each table
- Reports load quickly despite DirectQuery connections
- Real-time data updates without full refresh

---

### Challenge 3.4: Executive Dashboard ⭐⭐

**Objective:** Build a complete, polished business dashboard.

**Tasks:**

- Create multi-page report with navigation
- Implement drill-through for detailed analysis
- Add bookmarks for different user personas (CEO, Regional Manager, Dealership Manager)
- Use conditional formatting to highlight KPIs
- Add custom visuals (if appropriate)
- Design for mobile viewing

**Success Criteria:**

- Professional, business-ready dashboard
- Intuitive navigation and interaction
- Meets accessibility standards
- Works on mobile devices

---

## Cross-Lab Integration Challenges

### Challenge 4.1: End-to-End Unified Solution ⭐⭐⭐

**Objective:** Create a single integrated analytics solution.

**Tasks:**

- Build one Power BI report combining:
  - Gold tables from Lab 1 (historical analysis)
  - KQL data from Lab 2 (real-time operations)
  - Semantic model from Lab 3 (business metrics)
- Implement unified navigation with clear context switching
- Show correlations between historical patterns and real-time events
- Create "Today's Story" view combining all data sources

**Success Criteria:**

- Seamless user experience across data sources
- Performance acceptable despite complexity
- Clear value demonstrated from unified view

---

### Challenge 4.2: Data Platform Monitoring Dashboard ⭐⭐

**Objective:** Monitor the health of your entire data platform.

**Tasks:**

- Track notebook execution times, success rates, and failures
- Monitor data quality metrics over time
- Display pipeline lineage and dependencies
- Show refresh schedules and completion status
- Alert on pipeline anomalies

**Success Criteria:**

- Single dashboard shows entire platform health
- Proactive identification of issues
- Historical trending for capacity planning

---

### Challenge 4.3: Automated Documentation ⭐⭐

**Objective:** Generate comprehensive technical documentation.

**Tasks:**

- Use MCP servers to generate complete data catalog
- Create data dictionary for all tables, columns, and measures
- Document business logic and transformation rules
- Generate lineage diagrams
- Create user guides for semantic model

**Fabric MCP Prompt:**

```text
Generate comprehensive documentation for my Contoso Auto 360 solution:
- Data catalog listing all bronze, silver, and gold tables
- Data dictionary with column descriptions and data types
- Transformation logic from bronze to silver to gold
- Business rules applied in each layer
- Data lineage showing source to consumption flow
Format as markdown suitable for a wiki.
```

**Success Criteria:**

- Complete, accurate documentation
- Easy to maintain and update
- Accessible to both technical and business users

---

### Challenge 4.4: CI/CD Implementation ⭐⭐⭐

**Objective:** Implement DevOps practices for Fabric solutions.

**Tasks:**

- Set up Git integration for notebooks and semantic models
- Create workspace for Dev, Test, and Prod environments
- Build deployment pipeline using Fabric APIs or Azure DevOps
- Implement automated testing (data validation, DAX tests)
- Use PBIP format for Power BI projects

**Success Criteria:**

- Changes deployed through automated pipeline
- Each environment properly isolated
- Rollback capability in case of issues
- Audit trail of all changes

---

### Challenge 4.5: Multi-Source Data Integration ⭐⭐⭐

**Objective:** Enrich your solution with external data sources.

**Tasks:**

- Add external data (weather, economic indicators, demographics)
- Enrich dealership locations with geographic data
- Integrate social media sentiment for dealerships
- Correlate external factors with sales performance

**Fabric MCP Prompt:**

```text
How should I integrate external weather data to correlate with sales patterns?
- Suggest weather APIs or data sources
- Recommend schema for weather dimension table
- Propose DAX measures to analyze weather impact on sales
- Identify potential correlations to investigate
```

**Success Criteria:**

- External data successfully integrated
- New insights discovered from enriched data
- Documented relationships and correlations

---

## Learning Extension Challenges

### Challenge 5.1: Prompt Engineering Mastery ⭐

**Objective:** Become an expert at prompting MCP servers.

**Tasks:**

- Document your 10 most effective prompts used during labs
- Create a prompt template library for future projects
- Compare results from different prompt variations
- Share findings with group (what worked, what didn't)
- Create a "prompt patterns" guide

**Deliverable:** Markdown document with prompt templates and best practices

---

### Challenge 5.2: Performance Benchmarking ⭐⭐

**Objective:** Quantify the value of optimizations and MCP assistance.

**Tasks:**

- Measure query performance before and after optimizations
- Compare development time: manual coding vs MCP-generated
- Document time savings and quality improvements
- Create cost/benefit analysis of MCP servers
- Present findings to stakeholders

**Deliverable:** Performance report with metrics and recommendations

---

### Challenge 5.3: Industry Adaptation ⭐⭐

**Objective:** Apply learnings to your own domain.

**Tasks:**

- Adapt the Contoso Auto 360 solution for a different industry (retail, healthcare, manufacturing)
- Modify data model for your organization's specific needs
- Identify your organization's equivalent of bronze/silver/gold
- Present implementation plan to your team

**Deliverable:** Presentation showing adapted solution design

---

### Challenge 5.4: Security and Governance ⭐⭐⭐

**Objective:** Implement enterprise-grade security and governance.

**Tasks:**

- Apply sensitivity labels to sensitive data columns
- Set up data lineage tracking end-to-end
- Create governance policies for the workspace
- Document compliance considerations (GDPR, HIPAA, etc.)
- Implement data retention policies
- Set up audit logging and monitoring

**Success Criteria:**

- Security controls properly configured
- Compliance requirements documented
- Governance framework established

---

## Challenge Difficulty Legend

- ⭐ **Beginner:** 30-60 minutes, uses skills directly from labs
- ⭐⭐ **Intermediate:** 1-2 hours, requires some additional research
- ⭐⭐⭐ **Advanced:** 2+ hours, requires significant additional learning

## Submission Ideas

If you complete challenges and want to share:

1. **Blog Post:** Write about your experience and learnings
2. **GitHub Repo:** Share your code and documentation
3. **Lightning Talk:** Present at your local user group
4. **LinkedIn Post:** Share your accomplishments with your network
5. **Workshop Feedback:** Email instructor with your solutions for review

---

## Additional Resources

- [Microsoft Fabric Documentation](https://learn.microsoft.com/fabric/)
- [Power BI Documentation](https://learn.microsoft.com/power-bi/)
- [DAX Guide](https://dax.guide/)
- [KQL Quick Reference](https://learn.microsoft.com/azure/data-explorer/kql-quick-reference)
- [Model Context Protocol (MCP) Documentation](https://modelcontextprotocol.io/)

---

# Happy Learning! 🚀
