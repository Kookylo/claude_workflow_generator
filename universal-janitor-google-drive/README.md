# Universal Janitor – Google Drive

**Version 1.3** | n8n Workflow for Automated Google Drive Organization

## Overview

Universal Janitor is an n8n workflow that automatically organizes files in your Google Drive root folder. It runs on a schedule (default: 4 AM daily) and:

1. **Detects & removes duplicate files** (by MD5 checksum or name+size)
2. **Classifies files** into category folders using MIME type detection
3. **AI-powered categorization** for ambiguous files (via OpenRouter/Gemini)
4. **Renames generic filenames** (IMG_1234, Screenshot, etc.) with AI-suggested names
5. **Moves files** to appropriate destination folders using HTTP PATCH requests

## Folder Categories

| Category | Folder | File Types |
|----------|--------|------------|
| Ideas | `_IDEAS` | Strategy docs, plans, brainstorming files |
| Data | `_DATA` | CSV, spreadsheets, JSON, Excel files |
| Docs | `_DOCS` | PDFs, Google Docs, Word docs, text files, presentations |
| Archives | `_ARCHIVES` | ZIP, RAR, 7z, TAR files |
| Media | `_MEDIA` | Images, videos, audio files |

## Setup Instructions

### 1. Import the Workflow
Import `Universal Janitor – Google Drive.json` into your n8n instance.

### 2. Create Destination Folders
Create these folders in your Google Drive root:
- `_IDEAS`
- `_DATA`
- `_DOCS`
- `_ARCHIVES`
- `_MEDIA`

### 3. Get Folder IDs
For each folder, right-click → "Get link" → extract the ID from the URL:
```
https://drive.google.com/drive/folders/FOLDER_ID_HERE
```

### 4. Configure Folder IDs
Replace the placeholder values in these nodes:
- **Move → _IDEAS**: `REPLACE_WITH_YOUR_IDEAS_FOLDER_ID`
- **Move → _DATA**: `REPLACE_WITH_YOUR_DATA_FOLDER_ID`
- **Move → _DOCS**: `REPLACE_WITH_YOUR_DOCS_FOLDER_ID`
- **Move → _ARCHIVES**: `REPLACE_WITH_YOUR_ARCHIVES_FOLDER_ID`
- **Move → _MEDIA**: `REPLACE_WITH_YOUR_MEDIA_FOLDER_ID`

### 5. Configure Credentials
Set up these credentials in n8n:
- **Google Drive OAuth2**: For file listing, deletion, and renaming
- **OpenRouter API** (optional): For AI-powered categorization and renaming

## HTTP PATCH Fix

This workflow uses **HTTP Request nodes with PATCH method** instead of the native Google Drive "Move" operation. This approach:

- **Why**: The native n8n Google Drive node's move operation can be unreliable for bulk operations
- **How**: Uses the Google Drive API v3 directly with `addParents` and `removeParents` query parameters
- **Benefits**: 
  - More reliable file moves
  - Better error handling with retry logic (3 attempts, 1s delay)
  - Continues on error to process remaining files

### PATCH Request Structure
```
PATCH https://www.googleapis.com/drive/v3/files/{fileId}
?addParents={destinationFolderId}
&removeParents=root
```

## Workflow Logic

```
Schedule Trigger (4 AM)
    ↓
List Root Files (up to 500)
    ↓
Detect Duplicates → Delete if duplicate
    ↓
Filter (exclude folders & _prefixed items)
    ↓
Detect Generic Filenames (IMG_, Screenshot, etc.)
    ↓
┌─ Generic? ─────────────────────────────┐
│  YES → AI Agent → Rename → Route       │
│  NO  → Classify by MIME type           │
└────────────────────────────────────────┘
    ↓
Move to destination folder via HTTP PATCH
```

## Customization

### Adjust Schedule
Edit the **Schedule Trigger** node to change when the workflow runs.

### Add Categories
1. Add conditions to the **Classify** switch node
2. Create a new **Move →** HTTP Request node
3. Update the **Route AI Categorized** switch node

### Modify Generic Filename Patterns
Edit the regex patterns in the **Code in JavaScript** node:
```javascript
const genericPatterns = [
  /^IMG_\d+/i,
  /^DSC_\d+/i,
  /^screenshot/i,
  // Add your patterns here
];
```

## Requirements

- n8n instance (self-hosted or cloud)
- Google Cloud project with Drive API enabled
- Google OAuth2 credentials configured in n8n
- (Optional) OpenRouter API key for AI features

## License

MIT License - Feel free to modify and share!
