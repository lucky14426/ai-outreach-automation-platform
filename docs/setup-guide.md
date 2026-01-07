<h1>Setup Guide</h1>

<hr/>

<h2>1. Purpose of This Guide</h2>

<p>
This document provides a <strong>complete, production-grade setup guide</strong> for deploying
and operating the AI Outreach Automation system.
It covers infrastructure prerequisites, external service configuration,
workflow import, environment preparation, and operational validation.
</p>

<p>
This guide is intended for:
</p>

<ul>
  <li>Platform engineers</li>
  <li>Automation engineers</li>
  <li>DevOps and cloud operators</li>
  <li>Technical founders and solution architects</li>
</ul>

<hr/>

<h2>2. System Deployment Overview</h2>

<p>
The AI Outreach Automation system is deployed as a <strong>self-hosted n8n automation platform</strong>
integrated with multiple third-party services.
</p>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Component</th>
    <th>Deployment Responsibility</th>
  </tr>
  <tr>
    <td>n8n</td>
    <td>Self-hosted (Docker / VM / Cloud)</td>
  </tr>
  <tr>
    <td>Google APIs</td>
    <td>Google Cloud Project</td>
  </tr>
  <tr>
    <td>PostgreSQL</td>
    <td>Managed or self-hosted</td>
  </tr>
  <tr>
    <td>External APIs</td>
    <td>Vendor-managed (Apollo, Apify, OpenAI, Unipile)</td>
  </tr>
</table>

<hr/>

<h2>3. Infrastructure Prerequisites</h2>

<h3>3.1 Hosting Environment</h3>

<ul>
  <li>Linux-based server or cloud instance</li>
  <li>Minimum 2 vCPU, 4 GB RAM (recommended 8 GB)</li>
  <li>Stable outbound internet access</li>
</ul>

<h3>3.2 Supported Deployment Models</h3>

<ul>
  <li>Docker-based deployment</li>
  <li>Virtual machine deployment</li>
  <li>Cloud-managed container platform</li>
</ul>

<hr/>

<h2>4. n8n Platform Setup</h2>

<h3>4.1 n8n Installation</h3>

<ul>
  <li>Install n8n in self-hosted mode</li>
  <li>Ensure persistent storage for workflows and executions</li>
  <li>Enable webhook access over HTTPS</li>
</ul>

<h3>4.2 n8n Configuration Requirements</h3>

<ul>
  <li>Webhook base URL publicly accessible</li>
  <li>Execution mode configured for reliability</li>
  <li>Time zone correctly set</li>
</ul>

<hr/>

<h2>5. External Service Accounts Setup</h2>

<h3>5.1 Apollo</h3>

<ul>
  <li>Create an Apollo account with People Search access</li>
  <li>Ensure sufficient export and query limits</li>
</ul>

<h3>5.2 Apify</h3>

<ul>
  <li>Create an Apify account</li>
  <li>Enable Apollo scraping actor</li>
  <li>Generate an API token</li>
</ul>

<h3>5.3 OpenAI</h3>

<ul>
  <li>Create an OpenAI account</li>
  <li>Enable GPT-4o and GPT-4o-mini models</li>
  <li>Generate a secure API key</li>
</ul>

<h3>5.4 SerpAPI</h3>

<ul>
  <li>Create a SerpAPI account</li>
  <li>Enable Google Search API access</li>
</ul>

<h3>5.5 Unipile</h3>

<ul>
  <li>Create a Unipile account</li>
  <li>Connect LinkedIn account via Unipile dashboard</li>
  <li>Enable LinkedIn messaging and webhook events</li>
  <li>Obtain account ID and API key</li>
</ul>

<h3>5.6 Google Cloud</h3>

<ul>
  <li>Create a Google Cloud project</li>
  <li>Enable Google Sheets API</li>
  <li>Create a service account</li>
  <li>Grant Sheets read/write permissions</li>
</ul>

<hr/>

<h2>6. Database Setup (PostgreSQL)</h2>

<h3>6.1 Database Requirements</h3>

<ul>
  <li>PostgreSQL version 13 or higher</li>
  <li>Persistent storage enabled</li>
  <li>Network access from n8n instance</li>
</ul>

<h3>6.2 Database Purpose</h3>

<ul>
  <li>Conversation session storage</li>
  <li>Message history persistence</li>
  <li>AI context reconstruction</li>
</ul>

<hr/>

<h2>7. Environment Configuration</h2>

<h3>7.1 Environment Variables</h3>

<p>
All credentials and configuration values must be injected via environment variables.
</p>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Category</th>
    <th>Purpose</th>
  </tr>
  <tr>
    <td>AI</td>
    <td>OpenAI API access</td>
  </tr>
  <tr>
    <td>Scraping</td>
    <td>Apify and SerpAPI authentication</td>
  </tr>
  <tr>
    <td>Outreach</td>
    <td>Unipile LinkedIn automation</td>
  </tr>
  <tr>
    <td>Email</td>
    <td>Gmail API access</td>
  </tr>
  <tr>
    <td>Storage</td>
    <td>PostgreSQL and Google Sheets</td>
  </tr>
</table>

<p>
No credentials are stored inside workflow definitions.
</p>

<hr/>

<h2>8. Workflow Import Process</h2>

<h3>8.1 Import Order (Mandatory)</h3>

<ol>
  <li>01 – Lead Acquisition</li>
  <li>02 – AI Personalization</li>
  <li>03 – Outbound Outreach</li>
  <li>04 – Connection Engagement</li>
  <li>05 – AI Reply Bot</li>
</ol>

<h3>8.2 Import Validation</h3>

<ul>
  <li>Ensure all nodes load successfully</li>
  <li>Verify no missing credentials</li>
  <li>Confirm webhook URLs are generated</li>
</ul>

<hr/>

<h2>9. Credential Mapping in n8n</h2>

<p>
Each workflow requires credential mapping inside n8n.
</p>

<ul>
  <li>OpenAI credentials</li>
  <li>Google Sheets credentials</li>
  <li>Gmail credentials</li>
  <li>HTTP credentials for Apollo, Apify, SerpAPI, Unipile</li>
</ul>

<p>
Credentials must be shared across workflows consistently.
</p>

<hr/>

<h2>10. Webhook Configuration</h2>

<h3>10.1 Required Webhooks</h3>

<ul>
  <li>LinkedIn invite acceptance webhook</li>
  <li>LinkedIn inbound message webhook</li>
</ul>

<h3>10.2 Webhook Registration</h3>

<ul>
  <li>Register n8n webhook URLs in Unipile dashboard</li>
  <li>Verify webhook delivery using test events</li>
</ul>

<hr/>

<h2>11. Pre-Production Validation Checklist</h2>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Check</th>
    <th>Status</th>
  </tr>
  <tr>
    <td>All workflows imported</td>
    <td>Required</td>
  </tr>
  <tr>
    <td>Credentials validated</td>
    <td>Required</td>
  </tr>
  <tr>
    <td>Google Sheet creation tested</td>
    <td>Required</td>
  </tr>
  <tr>
    <td>LinkedIn invite test</td>
    <td>Required</td>
  </tr>
  <tr>
    <td>Email test delivery</td>
    <td>Required</td>
  </tr>
  <tr>
    <td>Webhook events received</td>
    <td>Required</td>
  </tr>
</table>

<hr/>

<h2>12. First Campaign Dry Run</h2>

<ol>
  <li>Submit a small test audience query</li>
  <li>Verify lead acquisition output</li>
  <li>Confirm AI personalization fields</li>
  <li>Execute outbound outreach for 1–2 leads</li>
  <li>Accept LinkedIn invite manually</li>
  <li>Send test reply to trigger AI Reply Bot</li>
</ol>

<hr/>

<h2>13. Go-Live Readiness Criteria</h2>

<ul>
  <li>All workflows executing without errors</li>
  <li>No credential warnings in n8n</li>
  <li>Correct data appearing in Sheets and database</li>
  <li>Human-like pacing confirmed</li>
</ul>

<hr/>

<h2>14. Operational Best Practices</h2>

<ul>
  <li>Start with low daily outreach volume</li>
  <li>Monitor LinkedIn account health</li>
  <li>Review AI-generated replies periodically</li>
  <li>Rotate API keys as per security policy</li>
</ul>

<hr/>

<h2>15. Summary</h2>

<p>
This setup guide defines a <strong>safe, scalable, and enterprise-ready deployment process</strong>
for the AI Outreach Automation system.
When followed precisely, it ensures the platform operates reliably,
securely, and in compliance with platform usage policies.
</p>

</body>
</html>
