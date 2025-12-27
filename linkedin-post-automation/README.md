# LinkedIn Post Automation

**Version 1.0** | n8n Workflow for AI-Generated LinkedIn Posts with Approval Flow

## Overview

Stop manually crafting LinkedIn posts. This workflow pulls content ideas from a Google Sheet, generates polished posts using GPT-4o-mini, sends them for email approval, and publishes directly to LinkedIn — with or without images.

**What it does:**

1. **Scheduled content retrieval** – Pulls pending posts from Google Sheets
2. **AI post generation** – GPT-4o-mini writes professional LinkedIn content
3. **Email approval workflow** – Review, approve, request changes, or cancel
4. **Smart publishing** – Posts with or without images based on sheet data
5. **Status tracking** – Updates sheet with completion status and post links

## Workflow Logic

```
Schedule Trigger
    ↓
Get Pending Row from Google Sheets
    ↓
Generate Post Content (GPT-4o-mini)
    ↓
Format Data
    ↓
Send Approval Email (Gmail)
    ↓
┌─ Approval Response ─────────────────┐
│  YES → Check for Image → Post       │
│  NO  → Regenerate with feedback     │
│  CANCEL → Update sheet as Cancelled │
└─────────────────────────────────────┘
    ↓
Post to LinkedIn (with/without image)
    ↓
Update Google Sheet (Completed + Link)
```

## Google Sheet Structure

Create a sheet with these columns:

| Column | Description |
|--------|-------------|
| Post Description | Brief idea or topic for the post |
| Instructions | Specific requirements (tone, hashtags, CTA) |
| Image | Optional URL to an image |
| Status | `Pending`, `Completed`, or `Cancelled` |
| Output | LinkedIn URN after posting |
| Post Link | Link to published post |
| row_number | Auto-populated for updates |

## Setup Instructions

### 1. Import the Workflow

Import `linkedin-post-automation.json` into your n8n instance.

### 2. Configure Credentials

Set up these credentials in n8n:

- **OpenAI API**: For GPT-4o-mini post generation
- **Google Sheets OAuth2**: For reading/updating the content sheet
- **Gmail OAuth2**: For sending approval emails
- **LinkedIn OAuth2**: For publishing posts

### 3. Update LinkedIn Person ID

In the **Post With Image** and **Post Without Image** nodes, replace:

```json
"person": "YOUR_LINKEDIN_PERSON_ID"
```

Get your LinkedIn Person ID from your LinkedIn profile URL or API.

### 4. Configure Google Sheet

Update the **Get Data from Sheets** and **Update Google Sheet** nodes with your sheet's document ID.

### 5. Set Approval Email

Update the **Send Content Confirmation** node with your email address.

## Approval Options

When you receive the approval email, respond with:

| Response | Action |
|----------|--------|
| **Yes** | Approve and publish immediately |
| **No** | Regenerate with your feedback |
| **Cancel** | Mark as cancelled, don't post |

The "Any Changes?" field lets you provide specific feedback for regeneration.

## Customization

### Change AI Model

The workflow uses `gpt-4o-mini` for cost efficiency. To change:
- Edit the **OpenAI Chat Model** node
- Consider `gpt-4o` for higher quality or **Gemini 2.0 Flash via OpenRouter** for better ROI

### Modify Post Style

Edit the system prompt in **Generate Post Content** to change:
- Tone (professional, casual, thought-leader)
- Format (bullet points, storytelling, listicle)
- Character limit (default: 1300)
- Hashtag style

### Add More Approval Options

Extend the **Content Confirmation Logic** switch node to add options like:
- "Schedule for later"
- "Save as draft"
- "Post to multiple platforms"

### Remove Approval Step

For automated posting without approval:
1. Delete the Gmail and Switch nodes
2. Connect **Data Formatting 1** directly to **If Image Provided**

## Requirements

- n8n instance (self-hosted or cloud)
- OpenAI API key
- Google Cloud project with Sheets API enabled
- Gmail account with OAuth2
- LinkedIn Developer App with OAuth2 credentials

## Node Types Used

| Node | Purpose |
|------|---------|
| Schedule Trigger | Automated daily/hourly runs |
| Google Sheets | Read pending posts, update status |
| LLM Chain | Generate and regenerate content |
| OpenAI Chat Model | Power the content generation |
| Gmail | Send approval emails with form |
| Switch | Route based on approval response |
| If | Check for image presence |
| HTTP Request | Download images for posting |
| LinkedIn | Publish posts with/without images |

## License

MIT License - Feel free to modify and share!
