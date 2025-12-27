# Article Knowledgebase Generator

**Version 1.0** | n8n Workflow for AI-Powered Knowledge Base Content Creation

## Overview

Build a comprehensive knowledge base without the manual grind. This workflow uses a multi-agent AI system to research topics, write articles, edit for quality, and publish to Contentful — all from a simple chat interface.

**What it does:**

1. **Chat-triggered creation** – Start with a topic via n8n chat interface
2. **AI Writer Agent** – GPT-3.5 drafts structured article with SEO metadata
3. **Deep research** – Perplexity Sonar researches and expands content with citations
4. **AI Editor Agent** – GPT-4.5 reviews quality and requests improvements
5. **Iterative refinement** – Writer/Editor loop until quality threshold met
6. **Auto-publish** – Sends to Contentful via sub-workflow

## Workflow Architecture

```
Chat Trigger (topic input)
    ↓
Initialize Iteration Counter
    ↓
AI Writer Agent (GPT-3.5)
    ↓
├── Create Perplexity Content (deep research)
│       ↓
│   Format & Add Citations
│       ↓
└── Merge: Article + Research + Counter
    ↓
AI Editor Agent (GPT-4.5)
    ↓
Check Iteration Limit (max 3)
    ↓
┌─ Limit Reached? ─────────────────────┐
│  YES → Force publish                  │
│  NO  → Check if "submit" or "rewrite" │
└───────────────────────────────────────┘
    ↓
Submit → Publish to Contentful
Rewrite → Loop back to Writer Agent
```

## Article Categories

| Category ID | Description |
|-------------|-------------|
| `getting-started` | Introductory guides |
| `learning-paths` | Structured learning journeys |
| `certifications` | Certification prep content |
| `programming` | Code tutorials and guides |
| `applications` | Tool and software guides |
| `career` | Career development content |

## Output Schema

Articles are generated with this structure:

```json
{
  "title": "Article Title",
  "slug": "article-slug",
  "category": { "id": "programming" },
  "description": "Brief description",
  "keywords": ["keyword1", "keyword2"],
  "content": "Full article content with markdown",
  "metaTitle": "SEO title",
  "metaDescription": "SEO description under 160 chars",
  "readingTime": "5 min",
  "difficulty": "beginner"
}
```

## Setup Instructions

### 1. Import the Workflow

Import `article-knowledgebase-generator.json` into your n8n instance.

### 2. Configure Credentials

Set up these credentials in n8n:

- **OpenAI API**: For GPT-3.5 (Writer) and GPT-4.5 (Editor) agents

### 3. Configure Perplexity API

Update the **Create Perplexity Content** node headers:

```json
{
  "Authorization": "Bearer YOUR_PERPLEXITY_API_KEY",
  "Content-Type": "application/json"
}
```

### 4. Set Up Contentful Workflow (Optional)

Create a sub-workflow for Contentful publishing and update the workflow IDs in:

- **Accept and Publish** tool node
- **Execute Workflow** node

Or remove these nodes if publishing elsewhere.

### 5. Configure Error Handling (Optional)

Set up an error workflow and update the `errorWorkflow` setting.

## AI Models Used

| Agent | Model | Purpose |
|-------|-------|---------|
| Writer | GPT-3.5-turbo | Fast initial drafts |
| Editor | GPT-4.5-preview | Quality review and decisions |
| Research | Perplexity Sonar Deep Research | In-depth topic research with citations |

## Customization

### Change Iteration Limit

Edit the **Check Limit** node. Default: 3 iterations max.

### Modify Article Categories

Update the category list in the **AI Writer Agent** system prompt.

### Adjust Research Depth

Edit the **Create Perplexity Content** node:

- `max_tokens`: Increase for longer research (default: 60000)
- `temperature`: Lower for more focused output (default: 0.7)

### Change Publishing Destination

Replace the **Execute Workflow** node with:

- WordPress node
- Notion API
- Airtable
- Any CMS with API access

### Disable Editor Loop

For faster (lower quality) output:

1. Remove the **AI Editor Agent** and related nodes
2. Connect **Format** directly to publishing

## Requirements

- n8n instance (self-hosted or cloud)
- OpenAI API key (GPT-3.5 and GPT-4.5 access)
- Perplexity API key
- Contentful account (or alternative CMS)

## Node Types Used

| Node | Purpose |
|------|---------|
| Chat Trigger | Interactive topic input |
| AI Agent | Writer and Editor agents |
| OpenAI Chat Model | Power the agents |
| HTTP Request | Perplexity API calls |
| Code | JSON parsing and formatting |
| Merge | Combine article + research |
| If | Iteration and action checks |
| Execute Workflow | Publish to Contentful |
| Tool Workflow | Editor's publish tool |

## Multi-Agent Pattern

This workflow demonstrates a **Writer-Editor loop pattern**:

1. **Writer** creates content based on input
2. **Researcher** (Perplexity) adds depth and citations
3. **Editor** evaluates and decides: rewrite or publish
4. Loop continues until quality threshold or max iterations

This pattern ensures higher quality output than single-pass generation.

## License

MIT License - Feel free to modify and share!
