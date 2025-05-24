# HubSpot → Instantly Auto-Sync Workflow (Webhook-Based, n8n)

This workflow enables **automatic syncing of HubSpot contacts to Instantly** using n8n. It is triggered by a webhook, allowing real-time integration whenever a contact is added to a HubSpot list.

---

## Features
- **Webhook Trigger:** Starts the workflow automatically when HubSpot (or any system) sends a POST request.
- **Mock Data for Testing:** Includes a node to generate mock HubSpot contacts for safe, repeatable demos.
- **Batch Processing:** Splits incoming data to process each contact individually.
- **Data Formatting:** Formats each contact for Instantly’s API requirements.
- **API Integration:** Sends the contact to Instantly (uses httpbin.org/post for safe testing; swap for production endpoint).
- **Success/Error Logging:** Logs the outcome for each contact, making troubleshooting easy.

---

## How to Use
1. **Import the Workflow:**  
   Open n8n, go to Workflows > Import, and select the JSON file.
2. **Get Your Webhook URL:**  
   Copy the webhook URL from the Webhook node in the workflow.
3. **Test the Workflow:**  
   Use Postman (or a similar tool) to send a POST request to the webhook URL.  
   Use the sample payload provided in the included Postman collection or the example below.
4. **Review Execution:**  
   n8n will process each contact, attempt to send to Instantly, and log the result.
5. **Go Live:**  
   Replace the mock data node with real HubSpot data (or configure HubSpot to send real events to the webhook URL).  
   Update the HTTP Request node to use your real Instantly API endpoint and credentials.

---

## Example Test Payload
```json
{
  "objectId": "contact_12345",
  "subscriptionId": "mock_list_567",
  "subscriptionType": "contact.listMembership",
  "email": "test@company.com",
  "firstname": "Jane",
  "lastname": "Smith",
  "company": "Test Corp",
  "jobtitle": "Marketing Director"
}
```

---

## Best Practices & Notes
- **Testing:** Keep the mock data and httpbin.org/post endpoint for safe testing. Swap to production endpoints only when ready.
- **Security:** Store API keys and sensitive data using n8n credentials.
- **Customization:** You can add nodes for deduplication, notifications (Slack/Email), or advanced error handling as needed.
- **Documentation:** This markdown can be added as a README node or sticky note for easy handoff and onboarding.

---

## Handoff Checklist
- [x] Workflow JSON included and clearly named.
- [x] Postman collection provided for easy testing.
- [x] All nodes are clearly named and commented.
- [x] This README explains setup, testing, and production steps.

---

**Questions or need further customization?**  
Just reach out!
