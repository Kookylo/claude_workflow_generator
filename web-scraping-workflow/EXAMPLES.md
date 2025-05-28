# Web Scraping Workflow Examples

This document provides practical examples of how to use and customize the web scraping workflow for different scenarios.

## Example 1: Scraping Blog Articles

This example demonstrates how to scrape blog posts from a website and extract the title, author, date, and content.

### Initial Configuration

```javascript
// Input data for the workflow
{
  "url": "https://example-blog.com/posts",
  "baseUrl": "https://example-blog.com/posts",
  "currentPage": 1,
  "maxPages": 5
}
```

### CSS Selectors

```javascript
// HTML Extract node configuration
{
  "extractionValues": {
    "values": [
      {
        "key": "postTitle",
        "cssSelector": "h2.post-title",
        "returnArray": false
      },
      {
        "key": "postAuthor",
        "cssSelector": "span.author-name",
        "returnArray": false
      },
      {
        "key": "postDate",
        "cssSelector": "time.published-date",
        "returnArray": false
      },
      {
        "key": "postContent",
        "cssSelector": "div.post-content",
        "returnArray": false
      }
    ]
  }
}
```

### Pagination Detection

```javascript
// Function node to detect the last page
{
  "functionCode": `
    // Check if there's a "Next" button
    const hasNextPage = $json.hasNextButton !== null && $json.hasNextButton !== undefined;
    
    // Check if we've reached the maximum pages
    const reachedMaxPages = $json.currentPage >= $json.maxPages;
    
    // Determine if we're done
    $json.areWeDone = !hasNextPage || reachedMaxPages;
    
    return $json;
  `
}
```

## Example 2: E-commerce Product Scraping

This example shows how to scrape product information from an e-commerce website.

### Initial Configuration

```javascript
// Input data for the workflow
{
  "url": "https://example-store.com/products",
  "baseUrl": "https://example-store.com/products",
  "currentPage": 1,
  "maxPages": 10,
  "category": "electronics"
}
```

### CSS Selectors

```javascript
// HTML Extract node configuration
{
  "extractionValues": {
    "values": [
      {
        "key": "productName",
        "cssSelector": "h3.product-title",
        "returnArray": true
      },
      {
        "key": "productPrice",
        "cssSelector": "span.price",
        "returnArray": true
      },
      {
        "key": "productRating",
        "cssSelector": "div.rating",
        "returnArray": true
      },
      {
        "key": "productImageUrl",
        "cssSelector": "img.product-image@src",
        "returnArray": true
      }
    ]
  }
}
```

### Data Processing

```javascript
// Function node to structure the product data
{
  "functionCode": `
    // Get all the arrays
    const names = $json.productName || [];
    const prices = $json.productPrice || [];
    const ratings = $json.productRating || [];
    const images = $json.productImageUrl || [];
    
    // Create structured product objects
    const products = [];
    for (let i = 0; i < names.length; i++) {
      products.push({
        name: names[i],
        price: prices[i],
        rating: ratings[i],
        image: images[i],
        category: $json.category,
        scrapedAt: new Date().toISOString()
      });
    }
    
    return { products };
  `
}
```

## Example 3: News Aggregation

This example demonstrates how to scrape news articles from multiple sources and aggregate them.

### Multi-Source Configuration

```javascript
// Input data with multiple sources
{
  "sources": [
    {
      "name": "News Source 1",
      "url": "https://news-source-1.com/latest",
      "titleSelector": "h2.headline",
      "summarySelector": "p.article-summary",
      "dateSelector": "time.publish-date",
      "linkSelector": "a.read-more@href"
    },
    {
      "name": "News Source 2",
      "url": "https://news-source-2.com/news",
      "titleSelector": "h3.news-title",
      "summarySelector": "div.excerpt",
      "dateSelector": "span.date",
      "linkSelector": "a.article-link@href"
    }
  ]
}
```

### Dynamic Selector Configuration

```javascript
// Function node to configure selectors dynamically
{
  "functionCode": `
    // Get the current source
    const source = $json.sources[$json.currentSourceIndex];
    
    // Configure the extraction for this source
    return {
      url: source.url,
      sourceName: source.name,
      extractionConfig: {
        title: source.titleSelector,
        summary: source.summarySelector,
        date: source.dateSelector,
        link: source.linkSelector
      }
    };
  `
}
```

## Example 4: API Integration with Scraped Data

This example shows how to enrich scraped data with additional information from an API.

### Scraping Configuration

```javascript
// HTML Extract node configuration
{
  "extractionValues": {
    "values": [
      {
        "key": "movieTitle",
        "cssSelector": "h1.movie-title",
        "returnArray": false
      },
      {
        "key": "releaseYear",
        "cssSelector": "span.year",
        "returnArray": false
      }
    ]
  }
}
```

### API Integration

```javascript
// HTTP Request node for API enrichment
{
  "parameters": {
    "url": "https://api.moviedb.example/search",
    "method": "GET",
    "sendQuery": true,
    "queryParameters": {
      "query": "={{$json.movieTitle}}",
      "year": "={{$json.releaseYear}}"
    },
    "options": {
      "timeout": 5000
    }
  }
}
```

### Data Combination

```javascript
// Function node to combine scraped and API data
{
  "functionCode": `
    // Combine the data
    return {
      title: $json.movieTitle,
      year: $json.releaseYear,
      director: $json.apiData?.director || 'Unknown',
      rating: $json.apiData?.rating || 'N/A',
      plot: $json.apiData?.plot || 'No plot available',
      poster: $json.apiData?.poster || null
    };
  `
}
```

## Example 5: Scheduled Monitoring

This example demonstrates how to set up a scheduled monitoring workflow that checks for changes on a website.

### Trigger Configuration

```javascript
// Schedule Trigger node
{
  "parameters": {
    "rule": {
      "interval": [
        {
          "field": "hours",
          "minuteInterval": 12
        }
      ]
    }
  }
}
```

### Change Detection

```javascript
// Function node to detect changes
{
  "functionCode": `
    // Get the current data
    const currentData = $json.extractedContent;
    
    // Get the previous data from a database or file
    const previousData = $json.previousData;
    
    // Compare and find differences
    const changes = [];
    for (const key in currentData) {
      if (previousData[key] !== currentData[key]) {
        changes.push({
          field: key,
          oldValue: previousData[key],
          newValue: currentData[key],
          timestamp: new Date().toISOString()
        });
      }
    }
    
    return {
      hasChanges: changes.length > 0,
      changes: changes,
      currentData: currentData
    };
  `
}
```

## Best Practices for Adapting These Examples

1. **Start Small**: Begin with a simplified version of these examples and add complexity as needed
2. **Test Thoroughly**: Test your selectors and pagination logic with a small dataset first
3. **Add Error Handling**: Incorporate error handling for each critical step
4. **Respect Rate Limits**: Implement appropriate delays between requests
5. **Document Customizations**: Keep track of your customizations for future reference

These examples provide a starting point for common web scraping scenarios. Adapt them to your specific needs by modifying the selectors, logic, and data processing steps as required.