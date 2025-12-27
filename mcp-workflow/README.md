# MCP Integration Workflow

**Version 1.0** | n8n Workflow for Model Context Protocol Integration

## Overview

Connect n8n to the Model Context Protocol (MCP) ecosystem. This workflow lets AI assistants like Claude trigger n8n workflows, process requests, and dynamically generate new workflows on demand.

**What it does:**

1. **MCP Server Trigger** – Listens for incoming MCP requests via SSE
2. **Request processing** – Extracts and structures MCP payload data
3. **Dynamic workflow creation** – Generates n8n workflows from AI instructions
4. **Workflow Coder** – Converts natural language prompts into executable workflow code

## Workflow Logic

```
MCP Server Trigger (SSE endpoint)
    ↓
Process MCP Request (extract payload)
    ↓
Create n8n Workflow (dynamic generation)
    ↓
Workflow Coder (AI-powered code generation)
```

## Setup Instructions

### 1. Import the Workflow

Import `mcp-workflow.json` into your n8n instance.

### 2. Configure MCP Server URL

Update the **MCP Server Trigger** node:

```
url: YOUR_MCP_SERVER_URL/sse
path: workflowmaster
```

Replace `YOUR_MCP_SERVER_URL` with your actual MCP server endpoint.

### 3. Configure Workflow Coder

The **Workflow Coder** node uses AI to generate workflow code. Ensure:
- Workflow input is properly mapped from the payload
- AI service (Claude) is configured in your MCP setup

### 4. Test the Connection

1. Start your MCP server
2. Activate the workflow in n8n
3. Send a test request from your AI assistant
4. Verify the workflow processes and responds

## Use Cases

### AI-Driven Workflow Generation

Ask Claude: "Create a workflow that fetches GitHub issues and posts to Slack"

The MCP workflow will:
1. Receive the request via MCP trigger
2. Parse the natural language prompt
3. Generate the workflow structure
4. Return the executable n8n workflow JSON

### Dynamic Automation

Use MCP to:
- Generate workflows on-the-fly based on user requests
- Modify existing workflows programmatically
- Create custom integrations without manual configuration

## Requirements

- n8n instance (self-hosted or cloud)
- MCP server running and accessible
- AI assistant with MCP support (Claude, etc.)
- Network connectivity between n8n and MCP server

## Troubleshooting

### MCP Tools Not Appearing

1. Verify MCP server is running: `curl YOUR_MCP_SERVER_URL/health`
2. Check MCP configuration in your AI assistant settings
3. Ensure n8n can reach the MCP server (firewall/network rules)

### Workflow Coder Errors

- Verify the payload contains valid workflow JSON
- Check AI service credentials in MCP config
- Review n8n logs for detailed error messages

## Node Types Used

| Node | Purpose |
|------|---------|
| MCP Server Trigger | SSE endpoint for MCP requests |
| Function | Process and structure MCP payload |
| n8n | Create workflows dynamically |
| Workflow Coder | AI-powered code generation |

## License

MIT License - Feel free to modify and share!