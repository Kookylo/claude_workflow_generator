# AI Email Response Handler

**Version 1.0** | n8n Workflow for Intelligent Lead Response Automation

## Overview

Stop manually responding to lead emails. This workflow monitors your inbox, uses GPT-4 to analyze replies, qualifies leads automatically, and sends personalized responses — all without human intervention.

**What it does:**

1. **IMAP monitoring** – Checks inbox every minute for new lead replies
2. **Context extraction** – Pulls email content and thread history
3. **AI qualification** – GPT-4 scores leads on budget, timeline, authority, pain points
4. **Response generation** – Creates personalized replies addressing specific objections
5. **CRM updates** – Logs qualification data to Google Sheets
6. **Team alerts** – Notifies Slack when qualified leads respond
7. **Email automation** – Sends AI response and marks original as read

## How It Works

1. The workflow monitors your inbox for new email replies from leads
2. When a new email is detected, it extracts the core content and context
3. The system looks up the sender in your Google Sheets database to retrieve lead history
4. GPT-4 analyzes the email content to determine:
   - Qualification status (qualified/nurturing/not-qualified)
   - Qualification score (0-100)
   - Budget indicators
   - Timeline requirements
   - Decision-making authority
   - Pain points
   - Objections
5. The AI generates a personalized response addressing specific points and objections
6. For qualified leads, the response includes a direct booking link
7. The system sends the AI-generated response and updates the lead's status in Google Sheets
8. If the lead is qualified, a notification is sent to your team via Slack
9. All interactions are logged for training and analytics purposes

## Setup Requirements

- n8n instance
- IMAP email account credentials
- SMTP email account credentials
- Google Sheets API access
- OpenAI API key
- Slack webhook (for notifications)
- Gmail API access (for marking emails as read)

## Integration with Lead Nurture Email Sequence

This workflow complements the [Lead Nurture Email Sequence](../lead-nurture-email-sequence/README.md) by handling responses to the automated email campaign. When a lead replies to any email in the nurture sequence, this workflow:

1. Analyzes their response
2. Updates their qualification status
3. Provides a personalized follow-up
4. Moves qualified leads toward booking a call

## Configuration

Before importing, you'll need to configure:

1. Your IMAP email credentials (for monitoring incoming emails)
2. Your SMTP email credentials (for sending responses)
3. Google Sheets document and sheet IDs
4. OpenAI API key
5. Slack webhook URL (if using team notifications)
6. Gmail API credentials (for marking emails as read)

## Import Instructions

1. In your n8n instance, go to Workflows
2. Click "Import from File"
3. Select the `ai-email-response-handler.json` file
4. Configure your credentials
5. Activate the workflow