# SQL Saturday Atlanta AI BI Pre Con Session — Prerequisites

This guide uses the Contoso Auto 360 dataset pack across all three labs.

---

## Common Prerequisites for All Labs

### 1. Microsoft Fabric & Power BI Access

**Required Licenses:**

- **Microsoft Fabric** capacity (paid or trial):
  - **Trial License**: Microsoft Fabric free trial (60 days) - sufficient for workshop completion
  - **Paid Capacity**: F64 or higher SKU recommended for optimal performance
  - Supports Lakehouses, Notebooks, Eventhouses, KQL Databases, and Semantic Models
  
- **Power BI**:
  - Power BI Pro or Premium Per User (PPU) license
  - Required for semantic model creation and management
  - Must be in the same tenant as Microsoft Fabric

**Required Permissions:**

- Ability to create and manage Fabric workspaces
- Permission to create Lakehouses, Eventhouses, and semantic models
- Rights to publish and manage semantic models and reports
- Admin or Member role in the Fabric workspace

**Note:** The Microsoft Fabric free trial includes all necessary capabilities. To activate your trial, visit [Microsoft Fabric Trial](https://app.fabric.microsoft.com/) and select "Start Trial".

### 2. Development Environment

**Required Software:**

- **VS Code** (Visual Studio Code) - latest version
  - Download from: [https://code.visualstudio.com/](https://code.visualstudio.com/)
  
**Required Licenses:**

- **GitHub Copilot** subscription (required for MCP support):
  - **Individual**: $10/month or $100/year
  - **Business**: $19/user/month
  - **Enterprise**: $39/user/month
  - All tiers support MCP (Model Context Protocol) functionality
  - 30-day free trial available for first-time users
  - Activate at: [https://github.com/features/copilot](https://github.com/features/copilot)

- **Alternative MCP-Capable Clients** (if not using GitHub Copilot):
  - Claude Desktop (with Anthropic API access)
  - Other MCP-compatible AI assistants

**Authentication:**

- Active authentication to your Microsoft Fabric/Azure tenant
- GitHub account for Copilot activation

### 3. MCP Servers (to be installed during labs)

- **Microsoft Fabric MCP Server** (Lab 1)
- **Microsoft Fabric RTI MCP Server** (Lab 2)
- **Power BI Remote MCP Server** (Lab 3)
- **Power BI Modeling MCP Server** (Lab 3)

### 4. Dataset Files

Download all dataset files for Contoso Auto 360:

**Batch Files (for Lab 1):**

- `dealerships.csv`
- `inventory.csv`
- `customers.csv`
- `sales.csv`
- `service_orders.csv`
- `test_drives.csv`

**Telemetry File (for Lab 2):**

- `telemetry_events.jsonl`

### 5. Local Workspace Setup

1. Create a working folder locally: `contoso-auto-360-workshop`
2. Place all downloaded dataset files in that folder
3. Keep the folder accessible for uploading to Fabric during the labs

### 6. Fabric Workspace

Create one Fabric workspace for the workshop:

- Suggested name: `ContosoAuto360-MCP-Workshop`

---

## Suggested Fabric Workspace Item Names

Create or plan these items with consistent names:

- `lh_contoso_auto_360`
- `nb_load_contoso_auto_360`
- `eh_contoso_auto_360`
- `kql_contoso_auto_360`
- `sm_contoso_auto_360`
- `rti_dashboard_contoso_auto_360`
