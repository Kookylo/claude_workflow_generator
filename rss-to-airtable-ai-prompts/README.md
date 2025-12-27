# RSS to Airtable with AI Prompts

**Version 1.0** | n8n Workflow for Content Repurposing Automation

## Overview

Turn RSS feeds into LinkedIn content ideas automatically. This workflow fetches articles, extracts content, generates ChatGPT-ready prompts, and stores everything in Airtable for easy content repurposing.

**What it does:**

1. **RSS monitoring** – Watches your feed for new articles
2. **Content extraction** – Pulls full article text via HTTP request
3. **Prompt generation** – Creates structured ChatGPT prompts for LinkedIn posts
4. **Airtable storage** – Logs articles and prompts for batch processing

**Note:** This workflow generates prompts only — you copy them from Airtable and paste into ChatGPT to create the final LinkedIn posts.

## Authentication Requirements

This workflow requires two authentication credentials:

1. **No authentication required** for the RSS Feed Reader and HTTP Request nodes
2. **Airtable API Key** for the Airtable node:
   - Go to your [Airtable account](https://airtable.com/account)
   - Under API section, generate a Personal Access Token
   - In n8n, add a new Airtable credential using this token

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
3. Get your Airtable Base ID and Table Name:
   - Base ID: Found in your Airtable URL: `https://airtable.com/[BASE_ID]/...`
   - Table Name: The exact name of your table as it appears in Airtable

### Configuration Steps

1. Import the workflow JSON file into your n8n instance:
   - Go to Workflows > New > Import from File
   - Select the `rss-to-airtable-ai-prompts.json` file
   
2. Configure the RSS Feed Reader node:
   - Click on the node to edit it
   - Update the URL to your preferred RSS feed (default is TechCrunch)
   - Adjust polling intervals if needed (default is every 5 minutes)
   
3. Update the Airtable node:
   - Click on the node to edit it
   - Click "Add Credential" and enter your Airtable API token
   - Replace "YOUR_AIRTABLE_BASE_ID" with your actual Base ID
   - Replace "YOUR_TABLE_NAME" with your actual Table name
   
4. Save and activate the workflow

## How It Works

### RSS Feed Reader

The workflow begins by monitoring your specified RSS feed. When new content is published, the RSS Feed Reader node detects it and passes the article metadata to the next step.

### Extract Article Content

This node makes an HTTP request to the article's original URL to fetch the full content. This ensures you have the complete article text for AI prompt generation, not just the summary provided in the RSS feed.

### Generate LinkedIn Prompt

The JavaScript code in this node:
- Extracts the article title, content, URL, and publication date
- Creates a structured prompt specifically designed for ChatGPT
- Includes detailed instructions for creating engaging LinkedIn content
- All data is formatted into a structured object for Airtable storage

The prompts are designed with best practices for ChatGPT, including:
- Clear context about the source material
- Specific instructions for tone and style
- Character count limitations for LinkedIn
- Requirements for hashtags and engagement questions

### Save to Airtable

The final node saves all the collected information to your Airtable base:
- Article title and content
- Original publication date and source URL
- The generated ChatGPT prompt
- Processing timestamp

## Using the Generated Prompts

After the workflow runs:

1. Open your Airtable base
2. Find the latest entries with their ChatGPT prompts
3. Copy the prompt from the "LinkedIn Prompt" field
4. Paste it into ChatGPT (or Claude, or other AI tools)
5. Review and refine the generated LinkedIn post
6. Post the content to LinkedIn

## Customization Options

- **Change RSS Sources**: Modify the URL in the RSS Feed Reader node to monitor different feeds
- **Adjust Prompt Format**: Edit the JavaScript in the Generate LinkedIn Prompt node to change the instructions given to ChatGPT
- **Add More Social Platforms**: Extend the code to generate prompts for other platforms like Twitter or Facebook
- **Content Filtering**: Add Filter nodes to only process articles with specific keywords

## Troubleshooting

- If articles aren't being fetched correctly, check if the RSS feed structure has changed
- For Airtable connection issues, verify your API credentials and base/table IDs
- If content extraction fails, the website might be blocking automated requests