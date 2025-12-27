# Personalized AI Tech Newsletter

**Version 1.0** | n8n Workflow for Automated RSS-to-Email Tech Digests

## Overview

Stop drowning in tech news. This workflow fetches articles from top tech RSS feeds daily, stores them in a vector database, and sends you a personalized weekly summary based on your interests. AI does the reading — you get the highlights.

**What it does:**

1. **Daily RSS ingestion** – Pulls articles from Engadget, Ars Technica, The Verge, Wired, MIT Tech Review, and TechCrunch
2. **Vector storage** – Embeds articles using OpenAI and stores in memory for semantic search
3. **AI-powered summarization** – Agent queries the vector store and curates relevant stories
4. **Email delivery** – Sends a clean, formatted newsletter to your inbox weekly

## How It Works

```
Daily Trigger (fetches articles)
    ↓
Set RSS Feed URLs
    ↓
Split Out → Read Each RSS Feed
    ↓
Normalize Fields (title, content, date, link)
    ↓
Embed with OpenAI → Store in Vector Memory
    ↓
[Weekly Trigger]
    ↓
Set Your Interests & Item Count
    ↓
AI Agent queries Vector Store
    ↓
Convert Markdown → HTML
    ↓
Send via Gmail
```

## Default RSS Sources

| Source | Feed URL |
|--------|----------|
| Engadget | `https://www.engadget.com/rss.xml` |
| Ars Technica | `https://feeds.arstechnica.com/arstechnica/index` |
| The Verge | `https://www.theverge.com/rss/index.xml` |
| Wired | `https://www.wired.com/feed/rss` |
| MIT Tech Review | `https://www.technologyreview.com/topnews.rss` |
| TechCrunch | `https://techcrunch.com/feed/` |

## Setup Instructions

### 1. Import the Workflow

Import `personalized-ai-newsletter.json` into your n8n instance.

### 2. Configure Credentials

Set up these credentials in n8n:

- **OpenAI API**: For embeddings and AI agent summarization
- **Gmail OAuth2**: For sending the newsletter (or swap for another email service)

### 3. Customize Your Interests

Edit the **Your topics of interest** node:

```json
{
  "Interests": "AI, games, gadgets",
  "Number of news items to include": "15"
}
```

### 4. Add Your Email

Update the **Send Newsletter** node with your email address.

### 5. Customize RSS Feeds (Optional)

Edit the **Set Tech News RSS Feeds** node to add/remove sources:

```json
[
  "https://www.engadget.com/rss.xml",
  "https://feeds.arstechnica.com/arstechnica/index",
  "https://your-favorite-site.com/rss"
]
```

## Schedule

| Trigger | Default Schedule | Purpose |
|---------|------------------|---------|
| Get Articles Daily | Every day | Fetches and stores new articles |
| Send Weekly Summary | Monday 5 AM | Sends personalized digest |

Adjust schedules in the **Schedule Trigger** nodes as needed.

## Customization Options

### Use a Persistent Vector Store

The default uses in-memory storage (resets on restart). For production:
- Replace with **Pinecone**, **Weaviate**, **Qdrant**, or **Supabase pgvector**
- Update the `memoryKey` to match your external store configuration

### Change Delivery Method

Swap Gmail for:
- **Telegram Bot** – Get updates in chat
- **Slack** – Post to a channel
- **Discord** – Webhook integration

### Adjust AI Model

The workflow uses GPT-4o. For cost savings, switch to:
- `gpt-4o-mini` for lighter summarization
- Or use **Gemini 2.0 Flash via OpenRouter** for better ROI

### Modify Summarization Style

Edit the system prompt in the **News reader AI** node to change:
- Tone (formal, casual, technical)
- Format (bullet points, paragraphs, numbered list)
- Focus areas (prioritize certain topics)

## Requirements

- n8n instance (self-hosted or cloud)
- OpenAI API key
- Gmail account with OAuth2 configured (or alternative email service)

## Node Types Used

| Node | Purpose |
|------|---------|
| Schedule Trigger | Daily/weekly automation |
| RSS Feed Read | Fetch articles from feeds |
| Set | Configure feeds and interests |
| Split Out | Process multiple RSS URLs |
| Embeddings OpenAI | Generate vector embeddings |
| Vector Store In Memory | Store/retrieve articles |
| AI Agent | Summarize with tool access |
| OpenAI Chat Model | Power the AI agent |
| Markdown | Convert to HTML for email |
| Gmail | Send the newsletter |

## License

MIT License - Feel free to modify and share!
