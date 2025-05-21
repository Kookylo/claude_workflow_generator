# MCP Integration Workflow for n8n

This project provides a workflow configuration for integrating n8n with the Model Context Protocol (MCP) server.

## What is this workflow?

This workflow demonstrates how to:
1. Connect to an MCP server using the MCP Server Trigger
2. Process MCP requests
3. Create n8n workflows dynamically
4. Use the Workflow Coder node to generate workflow code

## How to use

### Importing the workflow

1. Open your n8n instance
2. Go to Workflows
3. Click on "Import from File"
4. Select the `mcp-workflow.json` file

### Configuration

You will need to update the following:

1. In the MCP Server Trigger node:
   - Verify the URL points to your MCP server
   - Check the path is correct

2. In the Workflow Coder node:
   - Ensure the workflow input is properly mapped

## Integration with external AI

This workflow is designed to interact with Claude or other AI assistants through the MCP protocol. When a request comes in through the MCP Server Trigger, it will:

1. Process the request data
2. Create a new workflow based on the request
3. Use the Workflow Coder to generate dynamic workflow code

## Troubleshooting

If the MCP tools aren't appearing:
1. Verify your MCP server is running
2. Check the MCP configuration in your Codeium config file
3. Make sure the n8n node has connectivity to the MCP server

Remember that the Workflow Coder node needs a valid JSON workflow object to work properly.