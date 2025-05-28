---
description: n8n Workflow Debugging Assistant
---

markdown# n8n Workflow Debugging Assistant

## Overview
This workflow helps Cascade understand when and how to debug n8n workflows, providing systematic approaches to identify and resolve issues.

## When to Trigger This Workflow

### Automatic Triggers
- User mentions "workflow not working", "error", "debug", or "fix"
- n8n JSON validation fails
- Node connection issues detected
- Execution errors reported

### Manual Triggers
- User explicitly asks for workflow debugging
- User shares error messages or screenshots
- User reports unexpected workflow behavior

## Debugging Process

### 1. Initial Analysis
When user reports an n8n issue:

Request the workflow JSON or screenshot
Identify the workflow type (webhook, scheduled, manual)
Check for obvious syntax errors
Verify node connections


### 2. Common Issues Checklist

#### Node Configuration
- [ ] All required fields populated
- [ ] Correct authentication/credentials
- [ ] Proper data format (JSON/string/number)
- [ ] Valid expressions in {{ }} brackets

#### Data Flow
- [ ] Input data structure matches expected format
- [ ] Node outputs properly connected
- [ ] No missing connections between nodes
- [ ] Proper error handling nodes

#### Expression Errors
- [ ] Check for typos in expression syntax
- [ ] Verify $json paths exist
- [ ] Confirm data types match operations
- [ ] Test expressions with sample data

### 3. Debugging Steps

#### Step 1: Validate JSON Structure
```javascript
// Check if the workflow JSON is valid
try {
  JSON.parse(workflowJSON);
  // Valid JSON - proceed to next step
} catch (error) {
  // Invalid JSON - identify syntax error location
}
Step 2: Node Analysis
For each node in workflow:
  - Verify node type exists in n8n
  - Check required parameters
  - Validate connection indices
  - Test with mock data
Step 3: Expression Testing
Common expression fixes:
- {{ $json.field }} → {{ $json["field"] }} (for fields with spaces)
- {{ $node.nodeName.json.field }} → {{ $('nodeName').item.json.field }}
- Missing quotes in string comparisons
- Incorrect array access methods
Step 4: Connection Verification
Verify connections:
- Source node exists
- Target node exists
- Connection indices are valid
- No circular dependencies
4. Debug Output Format
When debugging, provide:

Issue Summary

What's broken
Where it's breaking
Why it's breaking


Solution

Specific fix with corrected JSON
Step-by-step implementation
Alternative approaches if applicable


Prevention Tips

Best practices to avoid similar issues
Testing recommendations



5. Common Fixes Database
Webhook Issues
json// Fix: Path must start with /
"path": "/webhook-endpoint"  // ✓ Correct
"path": "webhook-endpoint"   // ✗ Incorrect
Expression Errors
javascript// Fix: Proper null checking
{{ $json.data?.field || 'default' }}  // Safe access
{{ $json.data.field }}  // May cause errors if data is undefined
Authentication Problems
json// Fix: Ensure credentials are properly referenced
"authentication": "predefinedCredentialType",
"credentials": {
  "apiKey": "={{$credentials.apiKey}}"
}
6. Interactive Debugging Mode
When complex issues arise:

Break down the workflow into smaller parts
Test each node individually
Use Set nodes to inspect data between steps
Add error handling nodes for better debugging

7. Response Templates
For Syntax Errors:
"I found a syntax error in your workflow at [location]. Here's the fix:
[Show corrected code]
The issue was [explanation]."
For Logic Errors:
"The workflow logic issue is in the [node name] node.
Current behavior: [what it does]
Expected behavior: [what it should do]
Fix: [corrected configuration]"
For Connection Issues:
"There's a connection problem between [node A] and [node B].
Issue: [missing/incorrect connection]
Solution: [how to fix the connection]"
Integration with Your Debugging Workflows
This workflow should:

Parse any n8n workflow shared by the user
Run validation checks automatically
Provide specific, actionable fixes
Suggest improvements for reliability
Offer to test the corrected workflow

Keywords for Activation

debug, fix, error, not working, broken
"workflow fails", "node error", "connection issue"
"expression error", "authentication failed"
Any n8n error message patterns

Output Preferences

Always provide corrected JSON when fixing
Highlight changes clearly
Explain the reasoning behind fixes
Suggest preventive measures