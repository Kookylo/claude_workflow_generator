# Technical Implementation Guide: Web Scraping Workflow

This document provides detailed technical information about the implementation of the web scraping workflow, including node configurations, data structures, and advanced customization options.

## Architecture Overview

The workflow follows a modular architecture with three distinct components that can be used independently or combined:

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│                 │     │                 │     │                 │
│  Split Items    │────▶│  Data Scraping  │────▶│   Pagination    │
│                 │     │                 │     │                 │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

## Node Configurations

### HTTP Request Node

```javascript
{
  "parameters": {
    "url": "={{$json.url}}",
    "authentication": "none",
    "method": "GET",
    "sendQuery": true,
    "sendBody": false,
    "options": {
      "timeout": 10000,
      "responseFormat": "json"
    }
  }
}
```

**Key Configuration Points:**
- Dynamic URL from workflow data
- 10-second timeout to handle slow responses
- JSON response format expected

### HTML Extract Node

```javascript
{
  "parameters": {
    "extractionValues": {
      "values": [
        {
          "key": "articleTitle",
          "cssSelector": "h1#firstHeading",
          "returnArray": false
        },
        {
          "key": "content",
          "cssSelector": "div#mw-content-text",
          "returnArray": false
        }
      ]
    }
  }
}
```

**Advanced Selector Techniques:**
- Use `@attr` to extract specific attributes (e.g., `img@src`)
- Use `:nth-child(n)` for selecting specific elements in a list
- Combine selectors for more precise targeting

### Pagination Implementation

The pagination logic uses a combination of:
1. Counter initialization
2. Dynamic URL construction
3. Conditional checking
4. Counter incrementing
5. Loop back mechanism

## Data Structures

### Input Data Structure

```javascript
{
  "url": "https://example.com/api/data",
  "baseUrl": "https://example.com/api/data",
  "currentPage": 1,
  "maxPages": 10,
  "areWeDone": false
}
```

### Output Data Structure (After Processing)

```javascript
{
  "items": [
    {
      "title": "Item 1",
      "description": "Description for item 1",
      "url": "https://example.com/item1"
    },
    {
      "title": "Item 2",
      "description": "Description for item 2",
      "url": "https://example.com/item2"
    }
  ],
  "metadata": {
    "totalPages": 10,
    "currentPage": 1,
    "totalItems": 237
  }
}
```

## Advanced Customization

### Adding Error Handling

To enhance the workflow with robust error handling, add these nodes:

1. **Error Catcher Node** after each HTTP Request:
```javascript
{
  "parameters": {
    "errorMessage": "Failed to fetch data from {{$json.url}}",
    "continueOnFail": true
  }
}
```

2. **Retry Logic**:
```javascript
{
  "parameters": {
    "maxRetries": 3,
    "retryDelay": 5000
  }
}
```

### Rate Limiting Implementation

To implement proper rate limiting:

1. Add a **Wait** node between HTTP requests:
```javascript
{
  "parameters": {
    "amount": "={{Math.floor(Math.random() * 5000) + 2000}}",
    "unit": "milliseconds"
  }
}
```

2. Configure a **Function** node to track request timing:
```javascript
{
  "parameters": {
    "functionCode": "
      // Track last request time
      const lastRequestTime = $workflow.variables.lastRequestTime || 0;
      const now = Date.now();
      const timeSinceLastRequest = now - lastRequestTime;
      
      // Ensure minimum 2 seconds between requests
      if (timeSinceLastRequest < 2000) {
        const waitTime = 2000 - timeSinceLastRequest;
        await new Promise(resolve => setTimeout(resolve, waitTime));
      }
      
      // Update last request time
      $workflow.variables.lastRequestTime = Date.now();
      
      return items;
    "
  }
}
```

## Handling Different Website Types

### Single Page Applications (SPAs)

For SPAs that load content dynamically with JavaScript:

1. Increase the HTTP Request timeout
2. Consider using the **Wait** node to allow content to load
3. Use more specific CSS selectors that target the final rendered state

### Authentication-Required Sites

For sites requiring authentication:

1. Configure the HTTP Request node with appropriate headers:
```javascript
{
  "headers": {
    "Authorization": "Bearer {{$credentials.apiToken}}",
    "Cookie": "{{$credentials.sessionCookie}}"
  }
}
```

2. Store credentials securely using n8n's credential store

## Performance Optimization Techniques

### Memory Usage Optimization

For handling large datasets:

1. Use the **Split In Batches** mode in the Set node
2. Process data in chunks of 100 items
3. Consider using the **Merge** node to recombine processed data

### Processing Speed Optimization

To improve processing speed:

1. Use parallel processing for independent data items
2. Minimize the use of complex Function nodes
3. Use the **Item Lists** node for efficient data manipulation

## Integration with Other Systems

### Exporting Scraped Data

Configure output nodes to export data to:

1. **Google Sheets**:
```javascript
{
  "parameters": {
    "operation": "append",
    "sheetId": "your-sheet-id",
    "range": "A:Z"
  }
}
```

2. **Database** (SQL):
```javascript
{
  "parameters": {
    "operation": "insert",
    "table": "scraped_data",
    "columns": {
      "title": "={{$json.title}}",
      "url": "={{$json.url}}",
      "content": "={{$json.content}}",
      "scraped_at": "={{$now.toISOString()}}"
    }
  }
}
```

## Debugging and Troubleshooting

### Common Issues and Solutions

1. **Selector Not Working**
   - Check if the website structure has changed
   - Use browser developer tools to verify selectors
   - Try more specific or alternative selectors

2. **Rate Limiting or IP Blocking**
   - Implement exponential backoff strategy
   - Rotate user agents
   - Consider using proxy services

3. **Incomplete Data**
   - Verify pagination detection logic
   - Check for dynamic content loading
   - Ensure all required fields are being extracted

### Debugging Techniques

1. Add **Debug** nodes throughout the workflow
2. Use `console.log()` in Function nodes
3. Enable execution data display in n8n

## Advanced Use Cases

### Content Monitoring

Adapt this workflow to monitor websites for changes:

1. Store previous scrape results
2. Compare new results with stored data
3. Trigger notifications for detected changes

### Data Enrichment

Extend the workflow to enrich scraped data:

1. Extract basic information from primary source
2. Use additional HTTP requests to gather supplementary data
3. Combine and structure the enriched data

## Compliance and Ethics

Always ensure your web scraping activities comply with:

1. Website Terms of Service
2. robots.txt directives
3. Legal regulations in your jurisdiction
4. Data privacy laws (GDPR, CCPA, etc.)

Implement these best practices:

1. Identify your scraper with an appropriate User-Agent
2. Cache results to minimize redundant requests
3. Implement proper rate limiting
4. Only collect publicly available data