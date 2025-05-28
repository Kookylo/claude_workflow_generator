# Setup Guide: GitHub Issues to Slack Notification Workflow

## 📋 Prerequisites

Before starting the setup process, ensure you have:

- **n8n instance**: Either [self-hosted](https://docs.n8n.io/hosting/) or [n8n.cloud](https://www.n8n.cloud/) account
- **GitHub repository**: Admin access to the repository you want to monitor
- **Slack workspace**: Permission to create apps and webhooks

## 🔧 Installation Steps

### Step 1: Set Up Your n8n Instance

1. If not already running, install and configure your n8n instance:
   - **Self-hosted**: Follow the [official installation guide](https://docs.n8n.io/hosting/installation/)
   - **n8n.cloud**: Sign up at [n8n.cloud](https://www.n8n.cloud/) and create a new workspace

2. Log in to your n8n dashboard

### Step 2: Import the Workflow

1. In the n8n dashboard, navigate to **Workflows** in the left sidebar
2. Click the **Import from File** button in the top-right corner
   ```
   [PLACEHOLDER: Screenshot of n8n import button]
   ```
3. Upload the `github_issues_slack_notification.json` file from this repository
4. Click **Import** to add the workflow to your n8n instance
5. Once imported, you'll see the complete workflow with all nodes ready for configuration
   ```
   [PLACEHOLDER: Screenshot of imported workflow]
   ```

### Step 3: Configure GitHub Webhook

1. In n8n, open the imported workflow by clicking on its name
2. Find and click on the **GitHub Trigger** node to select it
3. In the node settings panel, click **Add Credential** > **GitHub Webhook API**
   ```
   [PLACEHOLDER: Screenshot of GitHub node configuration]
   ```
4. Enter a name for this credential (e.g., "GitHub Issues Webhook")
5. Click **Create** to generate a webhook URL
6. Copy the generated webhook URL to your clipboard

Now, set up the webhook in GitHub:

1. Navigate to your GitHub repository in a new browser tab
2. Go to **Settings** > **Webhooks** (you need admin access)
3. Click **Add webhook**
   ```
   [PLACEHOLDER: Screenshot of GitHub webhook page]
   ```
4. Configure the webhook with these settings:
   - **Payload URL**: Paste the n8n webhook URL you copied
   - **Content type**: Select `application/json`
   - **Secret**: Leave empty (n8n handles authentication)
   - **Which events would you like to trigger this webhook?**: Select "Let me select individual events"
   - Check only the **Issues** box
   - Ensure **Active** is checked
5. Click **Add webhook** to save
6. GitHub will attempt to send a ping event to verify the webhook works

### Step 4: Configure Slack Integration

1. Go to your Slack workspace in a new browser tab
2. Visit the [Slack API applications page](https://api.slack.com/apps)
3. Click **Create New App** > **From scratch**
   ```
   [PLACEHOLDER: Screenshot of Slack create app page]
   ```
4. Enter a name (e.g., "GitHub Issue Notifier") and select your workspace
5. Click **Create App**
6. In the left sidebar, find and click **Incoming Webhooks**
7. Toggle the switch to **Activate Incoming Webhooks**
8. Click **Add New Webhook to Workspace**
9. Select the channel where notifications should appear
10. Click **Allow** to authorize the app
11. On the next screen, find and copy the webhook URL (begins with https://hooks.slack.com/)

Return to n8n to configure the Slack node:

1. In your workflow, click on the **Send to Slack** node
2. In the node settings panel, click **Add Credential** > **Slack Webhook API**
   ```
   [PLACEHOLDER: Screenshot of Slack node configuration]
   ```
3. Enter a name for this credential (e.g., "GitHub Issues Slack Webhook")
4. Paste the Slack webhook URL in the **Webhook URL** field
5. Click **Create** to save the credential
6. You can customize the channel, username and icon in the **Other Options** section
7. Click **Execute Node** to test (if you have received any GitHub events)

### Step 5: Customize Message Format (Optional)

You can customize how the notification appears in Slack:

1. Click on the **Format Message** node in the workflow
2. Find the **slackMessage** field in the **Values to Set** section
   ```
   [PLACEHOLDER: Screenshot of Format Message node]
   ```
3. Modify the message template using [Slack's message formatting syntax](https://api.slack.com/reference/surfaces/formatting)
4. Some formatting options you can adjust:
   - Add or remove emoji icons
   - Change how issue details are displayed
   - Adjust what information is included/excluded
   - Modify the preview length for the issue description
5. Click **Save** to apply your changes

### Step 6: Activate and Test the Workflow

1. Click the **Activate** toggle switch in the top-right corner of the workflow editor
   ```
   [PLACEHOLDER: Screenshot of activate toggle]
   ```
2. The workflow is now live and listening for GitHub issue events
3. Create a test issue in your GitHub repository to verify the setup:
   - Go to your repository
   - Click **Issues** > **New issue**
   - Enter a title and description
   - Click **Submit new issue**
4. Check your Slack channel - you should receive a notification with the issue details

## ⚙️ Additional Configuration

### Error Handling

This workflow includes an error handler node that you can customize:

1. Click on the **Error Handler** node
2. You can modify this node to send error notifications or take other actions when failures occur
3. Common options include sending error messages to a dedicated Slack channel or logging to a database

### Adding Labels and Assignee Filtering

You can modify the **Only New Issues** node to filter based on labels or assignees:

1. Click on the **Only New Issues** node
2. Add additional conditions in the **Conditions** section
3. Example conditions:
   - Only process issues with specific labels
   - Only process issues assigned to certain team members
   - Ignore issues with specific keywords in the title

## 🔍 Troubleshooting

### Webhook Not Receiving Events

- Verify your n8n instance is publicly accessible (required for GitHub webhooks)
- Check GitHub webhook delivery logs:
  1. Go to your repo's **Settings** > **Webhooks**
  2. Click on your webhook
  3. Scroll to **Recent Deliveries** to see status codes and responses
- Ensure your workflow is activated in n8n

### Slack Messages Not Appearing

- Test the Slack node directly using the **Execute Node** button
- Verify the webhook URL is correctly copied
- Check that your Slack app has permission for the channel you selected
- Ensure the channel name in the node configuration matches exactly

### Execution Errors

- In n8n, check the **Executions** tab to see logs of recent workflow runs
- Common issues include:
  - Missing or malformed data in GitHub events
  - Incorrect expressions in the Format Message node
  - Connectivity problems with GitHub or Slack

## 🔄 Updating the Workflow

To update this workflow to a newer version:

1. Download the latest `github_issues_slack_notification.json` file
2. In n8n, go to your workflow and select **Import from File**
3. Choose to **Overwrite** the existing workflow
4. Re-enter any credentials or custom settings

---

If you encounter any issues or have questions about this workflow, please open an issue in the GitHub repository or contact the workflow creator.
