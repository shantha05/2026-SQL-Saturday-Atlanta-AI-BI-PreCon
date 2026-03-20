# SQL Saturday Atlanta AI BI Pre Con Session — Lab 0

## MCP Server Setup and Verification

## Objective

Configure all Model Context Protocol (MCP) servers required for the workshop and verify they're working correctly through test prompts. This lab ensures your development environment is properly configured before starting the main workshop labs.

**Estimated Time:** 20-30 minutes

---

## Prerequisites

Before starting, ensure you have:

- ✅ VS Code installed (latest version)
- ✅ GitHub Copilot subscription active
- ✅ Microsoft Fabric access (trial or paid capacity)
- ✅ Power BI Pro or PPU license
- ✅ Ability to create Fabric workspaces

---

## Lab 0 Steps

### Part A — Install and Configure MCP Servers

**Extensions Required:**

You'll install these VS Code extensions to enable MCP functionality:

1. **Microsoft Fabric** - Enables Fabric workspace, lakehouse, and notebook operations
2. **Power BI** - Provides Power BI Modeling and Remote MCP capabilities  
3. **Real-Time Intelligence (RTI)** - Enables Eventhouse and KQL database access

All extensions use Azure authentication and integrate with GitHub Copilot to provide AI-assisted development capabilities.

#### 1. Microsoft Fabric MCP Server

**Installation via Extension:**

1. Open VS Code
2. Click on the Extensions icon in the Activity Bar (or press `Ctrl+Shift+X` / `Cmd+Shift+X`)
3. Search for **"Microsoft Fabric"** in the Extensions Marketplace
4. Look for the official Microsoft Fabric extension
5. Click **Install**
6. After installation, you may be prompted to reload VS Code

**Authentication Setup:**

1. Open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`)
2. Type **"Azure: Sign In"** and select it
3. Follow the browser authentication flow
4. Sign in with your Microsoft account that has Fabric access
5. Grant VS Code the necessary permissions

**Verify Installation:**

1. Open GitHub Copilot Chat (`Ctrl+Shift+I` / `Cmd+Shift+I`)
2. Look for the MCP tools icon or "Attach Context" menu
3. You should see "Microsoft Fabric MCP" listed in available tools/servers
4. Alternatively, check the Extensions view - the Fabric extension should show as installed and enabled

---

#### 2. Power BI Modeling MCP Server

**Installation via Extension:**

1. Open VS Code Extensions view (`Ctrl+Shift+X` / `Cmd+Shift+X`)
2. Search for **"Power BI"** or **"Power BI Modeling MCP"**
3. Look for the official Power BI extension from Microsoft
4. Click **Install**
5. Reload VS Code if prompted

**Authentication Setup:**

1. Use the same Azure sign-in from Step 1 (Fabric MCP)
2. If not already signed in, open Command Palette and run **"Azure: Sign In"**
3. Ensure your account has Power BI Pro or PPU license
4. The extension will automatically connect to Power BI service

**Verify Installation:**

1. Check Extensions view - Power BI extension should be installed and enabled
2. Open GitHub Copilot Chat - the Power BI Modeling MCP server should appear in available tools
3. You may see Power BI-specific options in the VS Code Activity Bar

---

#### 3. Power BI Remote MCP Server

**Installation via MCP Link:**

1. Click this installation link: [Install Power BI Remote MCP](https://vscode.dev/redirect/mcp/install?name=powerbi-remote&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.fabric.microsoft.com%2Fv1%2Fmcp%2Fpowerbi%22%7D)
2. Your browser will prompt to open VS Code - click **Open**
3. VS Code will ask to configure the MCP server - click **Yes** or **Configure**
4. The Power BI Remote MCP server will be added to your GitHub Copilot configuration
5. You may be prompted to reload VS Code

**Configuration:**

1. Authentication is shared with the Power BI Modeling extension
2. This server enables querying published semantic models via natural language
3. No additional authentication needed if already signed into Azure/Power BI
4. Verify the MCP server appears in GitHub Copilot Chat's available tools

**Note:** Power BI Remote MCP requires published semantic models to query. This will be configured in Lab 3. The extension can be installed now, but full testing will occur after you publish a model.

---

#### 4. Real-Time Intelligence (RTI) MCP Server

**Installation via MCP Command:**

1. Open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`)
2. Type and select **"MCP: Add Server"**
3. When prompted, select **"Enable MCP Extensions"**
4. From the list of available MCP servers, select **"Fabric Real-Time"**
5. The RTI MCP server will be configured and added to GitHub Copilot
6. Reload VS Code if prompted

**Configuration:**

1. This server uses the same Fabric/Azure authentication from Step 1
2. It provides access to Eventhouses and KQL databases in Fabric
3. If prompted for additional permissions, grant access to Real-Time Analytics
4. Verify the server appears in GitHub Copilot Chat's MCP tools list

**Verify Installation:**

1. Open GitHub Copilot Chat - RTI MCP should appear in available tools
2. You should be able to query Eventhouses and run KQL queries through natural language
3. The server enables AI-assisted KQL query generation and analysis

---

### Part B — Verify MCP Server Connections

Use these test prompts to verify each MCP server is working correctly.

#### Test 1: Fabric MCP Server

**Prompt 1:**
```
List all my Microsoft Fabric workspaces.
```

**Expected Result:**
- Should return a list of your Fabric workspaces with names and IDs
- If you have no workspaces yet, it should return an empty list without errors

**Prompt 2:**
```
What Fabric items are in my workspace [your workspace name]?
```

**Expected Result:**
- Lists lakehouses, notebooks, warehouses, and other items in the specified workspace
- If workspace is empty, returns appropriate message

**Prompt 3:**
```
Explain the medallion architecture pattern for data engineering in Microsoft Fabric.
```

**Expected Result:**
- Should provide a detailed explanation of bronze, silver, and gold layers
- May reference Fabric-specific implementation details

---

#### Test 2: Power BI Modeling MCP Server

**Prompt 1:**
```
What are the best practices for creating DAX measures in Power BI?
```

**Expected Result:**
- Returns guidance on DAX measure creation
- May include examples and naming conventions

**Prompt 2:**
```
List the semantic models I have access to in Power BI.
```

**Expected Result:**
- Lists your Power BI semantic models across workspaces
- Shows model names, workspace locations, and access level

**Prompt 3:**
```
Generate a DAX measure for Year-over-Year sales growth.
```

**Expected Result:**
- Provides complete DAX measure code
- Includes proper time intelligence functions
- Follows best practices

---

#### Test 3: Power BI Remote MCP Server

**Note:** This server requires published semantic models. If you don't have any yet, this test will be completed in Lab 3.

**Prompt 1:**
```
What tables are in the [model name] semantic model?
```

**Expected Result:**
- Lists tables, columns, and relationships
- If no model exists yet, should provide guidance on setting one up

**Prompt 2 (If you have a published model):**
```
What is the total sales by region in [model name]?
```

**Expected Result:**
- Executes a query against the model
- Returns data results in a structured format

---

#### Test 4: Real-Time Intelligence (RTI) MCP Server

**Prompt 1:**
```
What is KQL and how is it used in Microsoft Fabric?
```

**Expected Result:**
- Explains Kusto Query Language
- Describes usage in Eventhouses and Real-Time Analytics

**Prompt 2:**
```
List my Eventhouses in Fabric.
```

**Expected Result:**
- Returns list of Eventhouses (may be empty if none created yet)
- Should not return authentication or connection errors

**Prompt 3:**
```
Generate a KQL query to summarize event counts by hour.
```

**Expected Result:**
- Provides a sample KQL query with summarize and bin operators
- Shows best practices for time-based aggregation

---

### Part C — Troubleshooting Common Issues

#### Issue 1: MCP Server Not Appearing

**Solution:**
1. Restart VS Code
2. Check GitHub Copilot extension is updated to latest version
3. Verify GitHub Copilot subscription is active
4. Check VS Code settings → Extensions → GitHub Copilot → MCP Servers

#### Issue 2: Authentication Failures

**Solution:**
1. Sign out and sign back into Azure/Power BI in VS Code
2. Run `az login` in terminal to refresh Azure CLI credentials
3. Check that your account has necessary permissions in Fabric
4. Verify tenant is correct if using multiple Azure tenants

#### Issue 3: "Unable to Connect to Workspace"

**Solution:**
1. Verify workspace name spelling (case-sensitive)
2. Check workspace permissions (must be Admin or Member)
3. Ensure Fabric capacity is running (not paused/suspended)
4. Verify you're using the correct tenant

#### Issue 4: MCP Tools Not Responding

**Solution:**
1. Check internet connection
2. Verify Fabric service status: [https://status.fabric.microsoft.com](https://status.fabric.microsoft.com)
3. Try a simple prompt first before complex queries
4. Check VS Code Output panel for error messages

---

### Part D — Validate Complete Setup

Once all four tests are successful, you're ready for the main workshop labs!

**Final Verification Checklist:**

- [ ] Fabric MCP Server authenticated and responding
- [ ] Power BI Modeling MCP Server responding to DAX queries
- [ ] Power BI Remote MCP Server configured (will test in Lab 3)
- [ ] RTI MCP Server responding to KQL queries
- [ ] Can list workspaces and items without errors
- [ ] No authentication or connection errors in any server

**You're Ready!**

Proceed to Lab 1 to start building the Contoso Auto 360 solution using Fabric MCP Server.

---

## Quick Reference: Test Prompts Summary

### Fabric MCP
```
- List all my Microsoft Fabric workspaces
- Show items in [workspace name]
- Create a lakehouse named [name] in [workspace]
```

### Power BI Modeling MCP
```
- Generate DAX for [measure description]
- What are best practices for [modeling topic]
- List my semantic models
```

### Power BI Remote MCP
```
- What tables are in [model name]?
- Query [model name]: [business question]
```

### RTI MCP
```
- List my Eventhouses
- Generate KQL to [query description]
- Explain [KQL concept]
```

---

## Additional Resources

- [Microsoft Fabric MCP Documentation](https://learn.microsoft.com/fabric/mcp)
- [Model Context Protocol Specification](https://modelcontextprotocol.io/)
- [GitHub Copilot MCP Support](https://docs.github.com/copilot)
- [Power BI Semantic Models](https://learn.microsoft.com/power-bi/connect-data/service-datasets-understand)

---

## What's Next?

**Lab 1:** Build a complete medallion architecture lakehouse solution using Fabric MCP prompts  
**Lab 2:** Analyze real-time telemetry using RTI MCP and KQL queries  
**Lab 3:** Create and query semantic models with Power BI MCP servers

---

*Last Updated: March 2026*
