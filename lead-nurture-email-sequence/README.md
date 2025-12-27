# Lead Nurture Email Sequence

**Version 1.0** | n8n Workflow for Automated Lead Follow-Up

## Overview

Stop manually following up with leads. This workflow captures new leads via webhook, stores them in Google Sheets, and automatically sends a 4-email nurture sequence over 14 days.

**What it does:**

1. **Webhook capture** – Receives lead data (name, email, phone, source)
2. **Google Sheets storage** – Logs all leads with timestamps and status
3. **Automated email sequence** – Sends 4 emails at strategic intervals
4. **Status tracking** – Updates lead status after sequence completion

## Email Sequence

| Day | Email Type | Purpose |
|-----|------------|---------|
| Day 0 | Welcome | Immediate acknowledgment and value proposition |
| Day 3 | Pain Point | Address common challenges and position solution |
| Day 7 | Case Study | Social proof and success stories |
| Day 14 | Final Offer | Clear call-to-action with urgency |

## Requirements

- n8n instance (self-hosted or cloud)
- SMTP email account
- Google Sheets with OAuth2 access

## Implementation

The workflow is defined in the `lead-nurture-email-sequence.json` file, which can be imported into n8n.

## How It Works

1. A new lead submits their information through the webhook endpoint
2. Lead data is processed, formatted, and stored in Google Sheets
3. The automated email sequence begins immediately with the welcome email
4. Subsequent emails are sent at predetermined intervals (3, 7, and 14 days)
5. Lead status is updated to "sequence_completed" after the final email

## Customization

Email templates can be customized to match your brand voice and specific offerings. The timing between emails can also be adjusted based on your sales cycle.

## Example Webhook Payload

```json
{
  "name": "John Smith",
  "email": "john.smith@example.com",
  "phone": "555-123-4567",
  "source": "website_contact_form"
}
```

## Import Instructions

1. In your n8n instance, go to Workflows
2. Click "Import from File"
3. Select the `lead-nurture-email-sequence.json` file
4. Configure your Google Sheets and SMTP credentials
5. Activate the workflow

## Webhook URL

Once activated, your webhook will be available at:
`https://your-n8n-domain/webhook/lead-nurture-webhook`