# RSS to Airtable with AI Prompts

This workflow automatically fetches articles from an RSS feed, extracts their content, generates AI-ready prompts for LinkedIn posts, and stores everything in Airtable for easy content repurposing.

## Workflow Overview

1. **RSS Feed Monitoring**: Monitors a specified RSS feed for new content
2. **Content Extraction**: Extracts full article content from the source URL
3. **AI Prompt Generation**: Creates structured prompts for LinkedIn content creation
4. **Airtable Integration**: Stores all data in a structured Airtable base

## Use Cases

- **Content Curation**: Automate the collection of industry news and articles
- **Social Media Management**: Create a database of content ready for AI-powered social posts
- **Content Marketing**: Streamline the process of repurposing content across platforms
- **Research Collection**: Gather and organize articles on specific topics automatically

## Setup Instructions

### Prerequisites

- An n8n instance
- Airtable account with API access
- The RSS feed URL you want to monitor

### Airtable Setup

1. Create a new base in Airtable
2. Create a table with the following fields:
   - Article Title (Single line text)
   - Article Content (Long text)
   - Publication Date (Date)
   - Source URL (URL)
   - LinkedIn Prompt (Long text)
   - Processed Date (Date)
3. Get your Airtable Base ID and Table Name

### Configuration Steps

1. Import the workflow JSON file into your n8n instance
2. Configure the RSS Feed Reader node:
   - Update the URL to your preferred RSS feed
   - Adjust polling intervals as needed
3. Update the Airtable node:
   - Replace "YOUR_AIRTABLE_BASE_ID" with your actual Base ID
   - Replace "YOUR_TABLE_NAME" with your actual Table name
   - Configure your Airtable API credentials
4. Activate the workflow

## How It Works

### RSS Feed Reader

The workflow begins by monitoring your specified RSS feed. When new content is published, the RSS Feed Reader node detects it and passes the article metadata to the next step.

### Extract Article Content

This node makes an HTTP request to the article's original URL to fetch the full content. This ensures you have the complete article text for AI prompt generation, not just the summary provided in the RSS feed.

### Generate LinkedIn Prompt

The JavaScript code in this node:
- Extracts the article title, content, URL, and publication date
- Creates a structured prompt for AI tools like ChatGPT or Claude
- The prompt includes instructions for creating engaging LinkedIn content
- All data is formatted into a structured object for Airtable storage

### Save to Airtable

The final node saves all the collected information to your Airtable base:
- Article title and content
- Original publication date and source URL
- The generated LinkedIn prompt
- Processing timestamp

## Customization Options

- **Change RSS Sources**: Modify the URL in the RSS Feed Reader node to monitor different feeds
- **Adjust Prompt Format**: Edit the JavaScript in the Generate LinkedIn Prompt node to create different types of prompts
- **Add More Social Platforms**: Extend the code to generate prompts for other platforms like Twitter or Facebook
- **Content Filtering**: Add Filter nodes to only process articles with specific keywords

## Integration Ideas

- Connect to OpenAI/Claude nodes to automatically generate the LinkedIn posts
- Add a Slack notification when new content is processed
- Integrate with a scheduling tool to automatically post content
- Add a Text Summarization API to create brief article summaries

## Troubleshooting

- If articles aren't being fetched correctly, check if the RSS feed structure has changed
- For Airtable connection issues, verify your API credentials and base/table IDs
- If content extraction fails, the website might be blocking automated requests