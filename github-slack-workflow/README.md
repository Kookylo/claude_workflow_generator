# GitHub Issues to Slack Notification

**Version 1.0** | n8n Workflow for Real-Time Issue Alerts

## Overview

Stop manually checking GitHub for new issues. This workflow sends instant Slack notifications whenever someone opens an issue in your repository — complete with all the details your team needs to respond fast.

**What it does:**

1. **GitHub webhook trigger** – Detects new issue events in real-time
2. **Smart filtering** – Only processes newly opened issues (ignores comments/updates)
3. **Message formatting** – Extracts title, number, creator, labels, and description
4. **Slack delivery** – Posts formatted alert to your designated channel
5. **Error handling** – Continues running even if individual notifications fail

## Workflow Logic

```
GitHub Webhook (new issue created)
    ↓
Filter: Only "opened" action
    ↓
Format Message (extract & structure data)
    ↓
Send to Slack (webhook)
    ↓
Error Handler (optional)
```

## Setup Instructions

### 1. Import the Workflow

Import `github_issues_slack_notification.json` into your n8n instance.

### 2. Configure GitHub Webhook

In your GitHub repository:
1. Go to **Settings** → **Webhooks** → **Add webhook**
2. Set Payload URL to your n8n webhook URL (from the GitHub Trigger node)
3. Content type: `application/json`
4. Select **Issues** event only
5. Save webhook

### 3. Configure Slack Webhook

In your Slack workspace:
1. Create an [Incoming Webhook](https://api.slack.com/messaging/webhooks)
2. Choose the target channel (e.g., `#github-issues`)
3. Copy the webhook URL

Update the **Send to Slack** node:
```
webhookUrl: YOUR_SLACK_WEBHOOK_URL_HERE
```

### 4. Customize (Optional)

Edit the **Format Message** node to adjust:
- Message structure
- Description length (default: 280 chars)
- Channel name
- Bot username and emoji

## Message Format

Slack notifications include:

- Issue title and number
- Creator username
- Creation timestamp
- Description excerpt (truncated at 280 chars)
- Labels (if any)
- Direct link to GitHub issue

## Customization

### Filter by Labels

Add a condition after **Only New Issues** to filter by specific labels:

```javascript
{{$json.issue.labels.some(l => l.name === 'bug')}}
```

### Send to Multiple Channels

Duplicate the **Send to Slack** node and configure different webhook URLs.

### Include Assignees

Add to the message format in **Format Message**:

```
{{$json.issue.assignees.length > 0 ? '👤 Assigned to: ' + $json.issue.assignees.map(a => a.login).join(', ') : ''}}
```

## Requirements

- n8n instance (self-hosted or cloud)
- GitHub repository with admin access
- Slack workspace with webhook permissions

## Node Types Used

| Node | Purpose |
|------|---------|
| GitHub Trigger | Webhook listener for issue events |
| If | Filter for "opened" action only |
| Set | Extract and format issue data |
| Slack | Send webhook notification |
| No Op | Error handling placeholder |

## License

MIT License - Feel free to modify and share!
