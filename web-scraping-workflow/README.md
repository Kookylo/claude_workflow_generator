# Web Scraping Workflow for n8n

This workflow provides a comprehensive web scraping solution with three main components, designed to extract data from websites efficiently while following modern web standards and best practices.

## Table of Contents
- [Overview](#overview)
- [Workflow Components](#workflow-components)
  - [1. Split into Items](#1-split-into-items)
  - [2. Data Scraping](#2-data-scraping)
  - [3. Handle Pagination](#3-handle-pagination)
- [Technical Implementation](#technical-implementation)
- [Installation Guide](#installation-guide)
- [Configuration Options](#configuration-options)
- [Usage Examples](#usage-examples)
- [Performance Optimization](#performance-optimization)
- [Security Considerations](#security-considerations)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

## Overview

This workflow demonstrates a modular approach to web scraping, handling the three most common scenarios:

1. **Split into Items**: Extract and process data from HTTP responses
2. **Data Scraping**: Extract specific elements from web pages using CSS selectors
3. **Handle Pagination**: Navigate through multiple pages to collect complete datasets

The workflow is built using n8n's latest node versions and follows best practices for web scraping, including rate limiting, error handling, and proper data extraction techniques.

## Workflow Components

### 1. Split into Items

This component demonstrates how to handle the common case of splitting an HTTP response into multiple items for processing:

- Uses an HTTP Request node to fetch data from a source URL
- Splits the response body into individual items using the Set node
- Creates separate items for each element in the response using the Item Lists node

**Technical Flow:**
1. HTTP Request fetches the body from an API endpoint
2. Split Body node extracts and formats the response
3. Split Medium Links node creates individual items from the structured data

### 2. Data Scraping

This component shows how to extract specific elements from web pages:

- Fetches content from a specified URL (e.g., Wikipedia page)
- Uses HTML Extract node to pull specific elements using CSS selectors
- Processes the extracted data for further use

**Technical Flow:**
1. HTTP Request node fetches the HTML content from the target website
2. HTML Extract node uses CSS selectors to extract specific elements
3. The extracted data is structured for downstream processing

### 3. Handle Pagination

This component implements a robust pagination system to handle multi-page data sources:

- Implements a pagination process to handle multi-page data sources
- Loops through pages of results using a counter
- Checks for completion condition and stops when all data is collected

**Technical Flow:**
1. Set node initializes the page counter
2. HTTP Request node fetches data from the current page
3. Fetch Body node extracts the items from the page
4. IF node checks if we've reached the end of available pages
5. Set Increment node increases the page counter for the next iteration
6. The workflow loops back to step 2 until all pages are processed

## Technical Implementation

The workflow uses the following n8n nodes:

- **HTTP Request**: To fetch data from web sources
- **Set**: For data manipulation and variable setting
- **Item Lists**: To split and process collections of items
- **HTML Extract**: To extract specific elements from HTML using CSS selectors
- **IF**: For conditional logic to control the workflow

All nodes are configured with the latest typeVersions to ensure compatibility with current n8n releases.

## Installation Guide

To install and use this workflow:

1. Download the `web-scraping-workflow.json` file from this repository
2. Open your n8n instance
3. Navigate to Workflows
4. Click on "Import from File"
5. Select the downloaded JSON file
6. Save the workflow

## Configuration Options

Before running the workflow, you'll need to configure:

1. **HTTP Request URLs**: 
   - Update the URL parameters in all HTTP Request nodes to point to your target websites
   - Set appropriate headers if needed (e.g., User-Agent)

2. **CSS Selectors**:
   - Modify the CSS selectors in the HTML Extract nodes to match the structure of your target websites
   - Use browser developer tools to identify the correct selectors

3. **Pagination Logic**:
   - Adjust the pagination parameters based on your target website's structure
   - Update the completion condition in the IF node

## Usage Examples

### Example 1: Scraping a Blog

```javascript
// Initial data
{
  "url": "https://example-blog.com/posts",
  "baseUrl": "https://example-blog.com/posts",
  "currentPage": 1,
  "maxPages": 5
}
```

### Example 2: Extracting Product Data

```javascript
// Configure the HTML Extract node with these selectors
{
  "productName": ".product-title",
  "price": ".product-price",
  "description": ".product-description",
  "imageUrl": "img.product-image@src"
}
```

## Performance Optimization

To optimize the performance of this workflow:

1. **Rate Limiting**: Add delay nodes between requests to avoid overloading the target server
2. **Selective Extraction**: Only extract the data you need to minimize processing time
3. **Batch Processing**: Use the SplitInBatches mode for large datasets
4. **Error Handling**: Add error handling nodes to manage failed requests

## Security Considerations

When using this workflow, consider the following security best practices:

1. Respect robots.txt files and website terms of service
2. Implement proper rate limiting to avoid IP bans
3. Store any authentication credentials securely using n8n credentials
4. Be mindful of data privacy regulations when storing scraped data

## Troubleshooting

Common issues and solutions:

1. **Selector Not Working**: 
   - Check if the website uses dynamic content loading (may require waiting)
   - Verify selectors using browser developer tools

2. **Rate Limiting Issues**:
   - Add random delays between requests
   - Use a rotating proxy service for high-volume scraping

3. **Incomplete Data**:
   - Check if the pagination logic correctly identifies the last page
   - Verify that all items are being properly extracted

## Contributing

Contributions to improve this workflow are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

When contributing, please ensure your code follows the established patterns and includes appropriate documentation.