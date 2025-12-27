# HubSpot to Instantly Integration

**Version 1.0** | n8n Workflows for Cold Email Outreach Automation

## Overview

Sync HubSpot contacts to Instantly.ai automatically. Two workflows handle real-time webhook syncing and scheduled batch imports — no manual CSV exports required.

**What's included:**

1. **Auto Webhook Sync** – Real-time contact sync when added to HubSpot lists
2. **Manual/Scheduled Sync** – Batch sync for initial setup or periodic updates

## Requirements

- n8n instance (self-hosted or cloud)
- HubSpot account with API access
- Instantly.ai account with API access

## Workflows Included

### 1. Manual/Scheduled Sync Workflow
- **File:** `n8n-manual-scheduled-sync-workflow.json`
- **Purpose:** Sync HubSpot contacts to Instantly on demand or on a schedule. Great for batch operations or regular imports.

### 2. Automatic Webhook-Based Sync Workflow
- **File:** `n8n-auto-webhook-sync-workflow.json`
- **Purpose:** Instantly syncs new HubSpot list members to Instantly in real time via webhook.

### 3. Postman Collection
- **File:** `n8n-hubspot-instantly-demo.postman_collection.json`
- **Purpose:** Easily test both workflows with mock data and simulate real-world API calls.

---

## How to Use

1. **Import the desired workflow JSON into n8n.**
2. **Follow the included markdown docs for setup, testing, and customization.**
3. **Use the Postman collection for safe, repeatable testing.**

---

## Documentation

- [`n8n-manual-scheduled-sync-workflow.md`](./n8n-manual-scheduled-sync-workflow.md): Full usage guide for manual/scheduled sync.
- [`n8n-auto-webhook-sync-workflow.md`](./n8n-auto-webhook-sync-workflow.md): Full usage guide for webhook-based sync.
- [`n8n-hubspot-instantly-demo.postman_collection.json`](./n8n-hubspot-instantly-demo.postman_collection.json): Postman collection for testing.

---

## Best Practices
- Use n8n credentials for API keys.
- Start with mock data and httpbin.org/post for safe testing.
- Swap in real endpoints and credentials for production.

---

**Questions or need further customization?**
Open an issue or reach out!
