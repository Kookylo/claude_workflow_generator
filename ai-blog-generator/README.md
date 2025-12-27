# AI Blog Generator

**Version 1.0** | n8n Workflow for Automated WordPress Blog Publishing

## Overview

Publish SEO-optimized blog posts on autopilot. This workflow generates topics, writes long-form articles, creates featured images, and publishes directly to WordPress — all triggered by schedule or Telegram command.

**What it does:**

1. **Topic generation** – Gemini 2.0 Flash picks a category and creates SEO metadata
2. **Article writing** – GPT-4.1-mini writes 1,500–2,500 word articles
3. **Image generation** – DALL-E creates realistic featured images
4. **WordPress publishing** – Posts article with featured image
5. **Notifications** – Alerts via Discord and Telegram

## Workflow Logic

```
Schedule Trigger (every 3 hours) OR Telegram ("generate")
    ↓
Generate Topic, Title, Slug, Meta (Gemini 2.0 Flash via OpenRouter)
    ↓
Parse Structured Output
    ↓
Write Full Article (GPT-4.1-mini)
    ↓
Publish Draft to WordPress
    ↓
Generate Featured Image (DALL-E)
    ↓
Upload Image to WordPress Media
    ↓
Set Featured Image on Post
    ↓
Notify Discord + Telegram
```

## Categories

The workflow randomly selects from these categories:

| Category | WordPress Category ID |
|----------|----------------------|
| Technology | 3 |
| Artificial Intelligence (AI) | 4 |
| Tech Fact | 7 |
| Tech History | 8 |
| Tech Tips | 9 |

Update the category IDs in the **Wordpress Post Draft** node to match your site.

## Setup Instructions

### 1. Import the Workflow

Import `ai-blog-generator.json` into your n8n instance.

### 2. Configure Credentials

Set up these credentials in n8n:

- **WordPress API**: URL, username, and application password
- **OpenAI API**: For article writing and image generation
- **OpenRouter API**: For Gemini 2.0 Flash topic generation
- **Discord Webhook**: For publish notifications
- **Telegram API**: For trigger and notifications

### 3. Update WordPress URLs

Replace `YOUR_WORDPRESS_SITE` in these nodes:

- **Upload Image to WP**: `https://YOUR_WORDPRESS_SITE/wp-json/wp/v2/media`
- **Wordpress - Set Featured Image**: `https://YOUR_WORDPRESS_SITE/wp-json/wp/v2/posts/`

### 4. Configure Category IDs

Update the category mapping in **Wordpress Post Draft** to match your WordPress categories.

### 5. Set Up Telegram Bot (Optional)

Create a Telegram bot via @BotFather and configure the **Telegram Trigger** node. Send "generate" to trigger a new post.

## AI Models Used

| Purpose | Model | Provider |
|---------|-------|----------|
| Topic/Meta Generation | Gemini 2.0 Flash | OpenRouter |
| Article Writing | GPT-4.1-mini | OpenAI |
| Image Generation | DALL-E 3 | OpenAI |

This configuration optimizes for **cost efficiency** per AGENTS.md ROI mandate.

## Customization

### Change Schedule

Edit the **Schedule Trigger** node. Default: every 3 hours.

### Modify Content Style

Edit the system prompts in:

- **Topic Chooser and Title Maker** – Category selection and SEO metadata
- **Basic LLM Chain** – Article structure, tone, and formatting

### Add More Categories

1. Add category to the prompt in **Topic Chooser and Title Maker**
2. Update the category mapping expression in **Wordpress Post Draft**

### Change Image Style

Edit the prompt in **OpenAI - Generate Image**. Current: realistic, no text.

### Disable Notifications

Delete or disconnect the **Send to Discord Using Webhook** and **Telegram** nodes.

## Requirements

- n8n instance (self-hosted or cloud)
- WordPress site with REST API enabled
- WordPress application password (not regular password)
- OpenAI API key
- OpenRouter API key
- Discord webhook URL (optional)
- Telegram bot token (optional)

## Node Types Used

| Node | Purpose |
|------|---------|
| Schedule Trigger | Automated runs every 3 hours |
| Telegram Trigger | Manual trigger via chat |
| LLM Chain | Topic generation and article writing |
| OpenRouter Chat Model | Gemini 2.0 Flash for topics |
| OpenAI Chat Model | GPT-4.1-mini for articles |
| Output Parser | Structure topic metadata |
| WordPress | Publish posts |
| HTTP Request | Upload images, set featured |
| OpenAI | Generate DALL-E images |
| Discord | Webhook notifications |
| Telegram | Chat notifications |

## License

MIT License - Feel free to modify and share!
