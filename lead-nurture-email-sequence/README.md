# Lead Nurture Email Sequence

This automated email follow-up system uses n8n workflow automation to nurture leads through a strategic email sequence over a 14-day period.

## Features

- **Webhook Integration**: Captures new leads via a webhook endpoint
- **Data Processing**: Structures and stores lead data
- **Google Sheets Integration**: Maintains lead records in a spreadsheet
- **Automated Email Sequence**:
  - Day 0: Welcome email
  - Day 3: Pain point email
  - Day 7: Case study email
  - Day 14: Final offer email with call-to-action
- **Status Tracking**: Updates lead status upon sequence completion

## Setup Requirements

- n8n instance
- SMTP email account
- Google Sheets API access

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