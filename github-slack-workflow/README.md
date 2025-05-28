# GitHub Issues to Slack Notification Workflow

## 🔍 Overview

This n8n workflow creates an automated notification system that sends real-time alerts to Slack whenever a new issue is created in your GitHub repository. Perfect for development teams who want to stay informed about issues without constantly checking GitHub.

## ✨ Features

- **Real-time notifications** for newly created GitHub issues
- **Rich formatted messages** with complete issue details (title, number, creator, creation time)
- **Issue preview** with description excerpt and labels
- **Direct link** to the GitHub issue for quick access
- **Error handling** for reliable operation
- **Customizable** notification format and channel destination

## 🔧 Technical Details

- Uses GitHub's webhook system to detect new issue events
- Filters to only process newly opened issues, ignoring updates/comments
- Formats messages with professional Slack formatting including emojis and markdown
- Includes custom bot name and icon for better visibility in Slack

## 📋 Requirements

- n8n instance (self-hosted or n8n.cloud)
- GitHub repository with administrative access (to set up webhooks)
- Slack workspace with permissions to create incoming webhooks

## 🚀 Setup and Usage

Detailed setup instructions are available in the [setup.md](setup.md) file.

## 🔄 Workflow Structure

1. **GitHub Trigger** - Listens for GitHub issue events via webhook
2. **Issue Filter** - Processes only newly opened issues
3. **Message Formatter** - Extracts and formats issue data for Slack
4. **Slack Notifier** - Sends the formatted message to your Slack channel
5. **Error Handler** - Manages any execution errors

## 📝 Customization

This workflow can be easily customized to:
- Change the notification format
- Include additional issue details
- Send to multiple Slack channels
- Filter for specific issue types or labels

## 🔗 Importing

Simply import the [github_issues_slack_notification.json](github_issues_slack_notification.json) file directly into your n8n instance.

---

*Created with the n8n Workflow Generator*
