# Viral TikToks – Multi-Platform Auto-Poster

**Version 1.0** | n8n Workflow for Cloning & Repurposing Viral TikTok Content

## Overview

Stop manually recreating content. This workflow takes a viral TikTok URL, reverse-engineers its structure, generates a fresh spin using AI, creates a new video with your avatar, and blasts it to 9 platforms automatically.

**What it does:**

1. **Clones viral TikTok content** – Downloads video, extracts thumbnail, transcribes audio
2. **AI-powered content ideation** – Uses Perplexity to suggest similar but unique angles
3. **Script rewriting** – GPT-4o rewrites script, caption, and overlay text
4. **Avatar video generation** – Creates new video using Captions.ai
5. **Multi-platform publishing** – Posts to 9 platforms via Blotato API

## Platforms Supported

| Platform | API | Status |
|----------|-----|--------|
| Instagram | Blotato | ✅ |
| YouTube | Blotato | ✅ |
| TikTok | Blotato | ✅ |
| Facebook | Blotato | ✅ |
| Threads | Blotato | ✅ |
| Twitter/X | Blotato | ✅ |
| LinkedIn | Blotato | ✅ |
| Bluesky | Blotato | ✅ |
| Pinterest | Blotato | ✅ |

## Setup Instructions

### 1. Import the Workflow

Import `viral-tiktoks-workflow.json` into your n8n instance.

### 2. Configure API Keys

Replace `YOUR_API_KEY` placeholders in these nodes:

- **Download TikTok Video (RapidAPI)**: `x-rapidapi-key`
- **Suggest Similar Idea (Perplexity)**: `Authorization` header
- **Fetch Available Avatars / Generate Video with Avatar**: `x-api-key` (Captions.ai)
- **All platform nodes (INSTAGRAM, YOUTUBE, etc.)**: `blotato-api-key`

### 3. Configure Credentials

Set up these credentials in n8n:

- **Telegram API**: For receiving TikTok URLs and sending results
- **OpenAI API**: For GPT-4o vision analysis, text extraction, transcription, and rewriting
- **Google Sheets OAuth2**: For tracking templates and generated videos
- **HTTP Basic Auth**: For Cloudinary image uploads
- **HTTP Custom Auth**: For JSON2Video API

### 4. Configure Cloudinary

Update the Cloudinary URL in the **Upload Thumbnail to Cloudinary** node:
```
https://api.cloudinary.com/v1_1/YOUR_CLOUD_NAME/image/upload
```

### 5. Configure Social Media IDs

Update the **Assign Social Media IDs** node with your Blotato account IDs:
```json
{
  "instagram_id": "YOUR_ID",
  "youtube_id": "YOUR_ID",
  "threads_id": "YOUR_ID",
  "tiktok_id": "YOUR_ID",
  "facebook_id": "YOUR_ID",
  "facebook_page_id": "YOUR_PAGE_ID",
  "twitter_id": "YOUR_ID",
  "linkedin_id": "YOUR_ID",
  "pinterest_id": "YOUR_ID",
  "pinterest_board_id": "YOUR_BOARD_ID",
  "bluesky_id": "YOUR_ID"
}
```

### 6. Set Up Google Sheets

Create a Google Sheet with two tabs:

**Tab 1: Template**
| ID du modèle | Lien de la vidéo | Modèle de texte superposé | Modèle de script vidéo | Caption |

**Tab 2: MA VIDEO**
| ID du modèle | ID de la vidéo | Sujet | Texte superposé | Script | Caption | URL de la vidéo |

## Workflow Logic

```
Telegram Trigger (receive TikTok URL)
    ↓
Download TikTok Video (RapidAPI)
    ↓
Extract Thumbnail → Upload to Cloudinary
    ↓
Analyze Thumbnail (GPT-4o Vision)
    ↓
Extract Overlay Text (GPT-4o)
    ↓
Download & Transcribe Audio (GPT Whisper)
    ↓
Save Original to Google Sheets
    ↓
Suggest Similar Idea (Perplexity)
    ↓
Rewrite Script, Caption, Overlay (GPT-4o)
    ↓
Save Rewritten Content to Sheets
    ↓
Generate Avatar Video (Captions.ai)
    ↓
Add Overlay Text (JSON2Video)
    ↓
Update Sheet with Final URL
    ↓
Send via Telegram → Publish to 9 Platforms
```

## External Services Required

| Service | Purpose | Pricing |
|---------|---------|---------|
| RapidAPI (TikTok Downloader) | Download TikTok videos | Pay-per-use |
| OpenAI | Vision, transcription, text generation | Pay-per-use |
| Perplexity | Content ideation with web search | Subscription |
| Captions.ai | AI avatar video generation | Subscription |
| JSON2Video | Video overlay/subtitles | Pay-per-use |
| Blotato | Multi-platform posting | Subscription |
| Cloudinary | Image hosting | Free tier available |

## Customization

### Change AI Model

The workflow uses GPT-4o. To switch to a more cost-effective model, update the `modelId` in these nodes:
- Analyze Thumbnail (GPT-4o Vision)
- Extract Overlay Text (GPT-4o)
- Rewrite Script, Caption, Overlay (GPT-4o)

### Adjust Wait Times

Avatar rendering takes ~3 minutes, caption rendering ~2 minutes. Adjust the **Wait** nodes if your videos are longer or shorter.

### Add/Remove Platforms

Each platform has its own HTTP Request node. Delete nodes you don't need, or duplicate and modify for additional platforms.

## Requirements

- n8n instance (self-hosted or cloud)
- Telegram bot for triggering
- All API keys listed above
- Google Cloud project with Sheets API enabled

## License

MIT License - Feel free to modify and share!
