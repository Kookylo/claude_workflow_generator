# Workflow: /heal-repo

This workflow automates the auditing and refactoring of legacy n8n templates.

## Steps

1. **Audit Phase:**
   - Use `gh` or `ls` to identify folders with last-modified dates older than 6 months.
   - List the target files: `ai-email-response-handler`, `article_knowledgebase.json`, etc.

2. **Analysis Phase (Code Mode):**
   - For each legacy JSON, analyze the node versions.
   - Identify deprecated "HTTP Request" logic and replace it with the `addParents/removeParents` API patch used in the Janitor workflow.

3. **Refactor Phase:**
   - Use "Vibe and Replace" to apply the "Savvy Mentor" documentation style to the local README.md.
   - Scrub any hardcoded personal IDs or test emails.

4. **Verification Phase:**
   - Run a schema check to ensure the JSON is valid for n8n v1.0+.
   - Verify the AGENTS.md mandates have been met.

5. **Deployment:**
   - Create a git branch `heal/[workflow-name]`.
   - Use ✨ AI Commit to generate a high-quality commit message.
