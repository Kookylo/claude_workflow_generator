# HubSpot → Instantly Manual/Scheduled Sync Workflow (n8n)

This project includes a ready-to-use n8n workflow for integrating HubSpot and Instantly.ai, allowing you to sync contacts on demand or on a schedule.

---

## Workflow Overview

**Purpose:**  
Sync HubSpot contacts to Instantly on demand or on a schedule.

**Features:**  
- Trigger manually or on a schedule.  
- Pulls contacts from a HubSpot list (or uses mock/demo data).  
- Formats and sends contacts to Instantly.  
- Logs successes and errors.

---

## How to Use

1. Import the workflow into n8n.
2. Set the trigger (manual or schedule).
3. Enter your HubSpot credentials and list ID (or use mock data).
4. Enter your Instantly API key and campaign ID.
5. Run the workflow and check logs for results.

---

### 🧪 To Test the Manual/Scheduled Sync Workflow (Mock Data)

You can use the included mock data node to simulate HubSpot contacts, or configure the HubSpot node with your real credentials and list ID.

**What happens when you run it:**  
- Contacts are sent to your chosen Instantly campaign.  
- Logs show which contacts were synced and any errors encountered.

---

## Best Practices & Notes

- **Security:** Use n8n credentials/environment variables for API keys.
- **Custom Mapping:** You can easily add more fields or logic as needed.
- **Extensibility:** The workflow is modular—add more nodes for enrichment, notifications, or CRM updates.

---

## Handoff

- All nodes are clearly named and commented.
- This markdown file provides step-by-step usage instructions.

---

**Questions or need further customization?**  
Just ask!
