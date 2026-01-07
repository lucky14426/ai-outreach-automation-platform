<h1>01 – Lead Acquisition (Apollo + Apify)</h1>

<p>
  This document provides a comprehensive, technical, and production-grade explanation of the
  <strong>01-lead-acquisition</strong> workflow within the <em>AI Outreach Automation</em> system.
  This workflow is responsible for sourcing, validating, and persistently storing qualified leads
  that will be consumed by downstream AI personalization and outreach workflows.
</p>

<hr />

<h2>1. Purpose & Scope</h2>

<p>
  The Lead Acquisition workflow is designed to transform a high-level user intent (natural language query)
  into a structured, validated dataset of outbound-ready leads.
</p>

<ul>
  <li>Acts as the <strong>entry point</strong> of the entire automation system</li>
  <li>Converts human intent into machine-executable lead discovery logic</li>
  <li>Ensures data consistency, traceability, and downstream compatibility</li>
</ul>

<p>
  This workflow does <strong>not</strong> perform outreach. It strictly focuses on lead discovery,
  qualification, and storage.
</p>

<hr />

<h2>2. Business Problem Solved</h2>

<table border="1" cellpadding="6" cellspacing="0">
  <thead>
    <tr>
      <th>Traditional Process</th>
      <th>Automated Solution</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Manual Apollo searches</td>
      <td>AI-generated Apollo search URLs</td>
    </tr>
    <tr>
      <td>Manual CSV exports</td>
      <td>Programmatic Apify extraction</td>
    </tr>
    <tr>
      <td>Inconsistent filtering</td>
      <td>Deterministic validation rules</td>
    </tr>
    <tr>
      <td>Ad-hoc spreadsheets</td>
      <td>Dynamically created, query-specific sheets</td>
    </tr>
  </tbody>
</table>

<hr />

<h2>3. Workflow Position in System Architecture</h2>

<p>
  This workflow is executed <strong>once per campaign</strong> and produces a campaign-specific
  Google Spreadsheet that becomes the single source of truth for all subsequent workflows.
</p>

<ul>
  <li>Upstream dependency: None</li>
  <li>Downstream consumers:
    <ul>
      <li>02 – AI Personalization</li>
      <li>03 – Outbound Outreach</li>
      <li>04 – Connection Engagement</li>
      <li>05 – AI Reply Bot</li>
    </ul>
  </li>
</ul>

<hr />

<h2>4. Trigger Mechanism</h2>

<h3>4.1 Trigger Type</h3>
<ul>
  <li>n8n Chat Trigger</li>
  <li>Human-initiated (manual / conversational)</li>
</ul>

<h3>4.2 Trigger Input</h3>

<p>
  A free-form natural language query describing the target audience.
</p>

<p><strong>Examples:</strong></p>
<ul>
  <li>“Real estate company owners in UAE”</li>
  <li>“Marketing agency founders in California”</li>
  <li>“Healthcare clinic owners in India”</li>
</ul>

<hr />

<h2>5. High-Level Execution Flow</h2>

<ol>
  <li>Receive and normalize user query</li>
  <li>Convert intent into Apollo search URL using AI</li>
  <li>Create a campaign-specific Google Spreadsheet</li>
  <li>Trigger Apollo scraping via Apify</li>
  <li>Poll Apify for job completion</li>
  <li>Retrieve dataset items</li>
  <li>Validate and filter lead records</li>
  <li>Persist qualified leads into Google Sheets</li>
</ol>

<hr />

<h2>6. Detailed Step-by-Step Execution</h2>

<h3>Step 1: User Query Normalization</h3>

<ul>
  <li>Captures raw chat input</li>
  <li>Stores it as a canonical campaign identifier</li>
  <li>Used consistently for naming and traceability</li>
</ul>

<hr />

<h3>Step 2: Apollo URL Generation (AI Agent)</h3>

<p>
  An AI agent parses the user query and deterministically extracts:
</p>

<ul>
  <li>Industry (mapped to Apollo industry IDs)</li>
  <li>Job titles</li>
  <li>Geographic location(s)</li>
  <li>Optional employee size constraints</li>
</ul>

<p>
  The agent outputs a fully-formed Apollo People Search URL, guaranteed to be syntactically valid
  and compatible with Apollo’s internal filtering logic.
</p>

<hr />

<h3>Step 3: Google Spreadsheet Creation</h3>

<p>
  A new Google Spreadsheet is created dynamically with:
</p>

<ul>
  <li>Title derived from the original user query</li>
  <li>A single default worksheet</li>
  <li>Used as the campaign’s canonical datastore</li>
</ul>

<p>
  This spreadsheet ID is passed forward to all downstream workflows.
</p>

<hr />

<h3>Step 4: Apollo Data Extraction via Apify</h3>

<ul>
  <li>The generated Apollo URL is submitted to an Apify Actor</li>
  <li>Apify performs headless scraping and enrichment</li>
  <li>Email extraction is explicitly enabled</li>
  <li>Extraction volume is capped for cost and safety control</li>
</ul>

<hr />

<h3>Step 5: Apify Job Monitoring</h3>

<ul>
  <li>The workflow polls the Apify Run endpoint</li>
  <li>Wait logic prevents premature dataset access</li>
  <li>Ensures deterministic completion handling</li>
</ul>

<hr />

<h3>Step 6: Dataset Retrieval</h3>

<p>
  Once the Apify job completes:
</p>

<ul>
  <li>The default dataset ID is extracted</li>
  <li>Dataset items are fetched in structured JSON format</li>
</ul>

<hr />

<h3>Step 7: Lead Validation & Filtering</h3>

<p>
  Each lead record is evaluated against mandatory criteria:
</p>

<ul>
  <li>LinkedIn profile URL must exist</li>
  <li>Email address must exist</li>
</ul>

<p>
  Records failing validation are safely discarded without side effects.
</p>

<hr />

<h3>Step 8: Iterative Processing</h3>

<ul>
  <li>Qualified leads are processed in batches</li>
  <li>Batch processing ensures scalability</li>
  <li>Protects against memory and API overload</li>
</ul>

<hr />

<h3>Step 9: Data Persistence</h3>

<p>
  Each validated lead is appended to the campaign spreadsheet with the following attributes:
</p>

<ul>
  <li>Unique lead ID</li>
  <li>Full name</li>
  <li>LinkedIn profile URL</li>
  <li>Email address</li>
  <li>Organization name</li>
  <li>Role / seniority</li>
  <li>Geographic metadata</li>
</ul>

<p>
  The resulting sheet is guaranteed to be downstream-compatible without transformation.
</p>

<hr />

<h2>7. Data Contract (Output Schema)</h2>

<table border="1" cellpadding="6" cellspacing="0">
  <thead>
    <tr>
      <th>Field</th>
      <th>Description</th>
      <th>Mandatory</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>Unique Apollo lead identifier</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>name</td>
      <td>Full name of the lead</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>linkedin_url</td>
      <td>Public LinkedIn profile URL</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>email</td>
      <td>Verified email address</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>organization_name</td>
      <td>Company name</td>
      <td>Optional</td>
    </tr>
  </tbody>
</table>

<hr />

<h2>8. External Dependencies</h2>

<ul>
  <li>Apollo.io (lead source)</li>
  <li>Apify (scraping infrastructure)</li>
  <li>Google Sheets API</li>
  <li>OpenAI (URL generation agent)</li>
  <li>n8n (workflow orchestration)</li>
</ul>

<hr />

<h2>9. Security & Compliance Considerations</h2>

<ul>
  <li>No credentials are hardcoded in workflow JSON</li>
  <li>All secrets are injected via environment variables</li>
  <li>No personal data is persisted outside controlled storage</li>
  <li>Lead usage is limited to outbound B2B communication</li>
</ul>

<hr />

<h2>10. Failure Handling & Safeguards</h2>

<ul>
  <li>Invalid user queries fail gracefully</li>
  <li>Empty Apollo results do not trigger downstream workflows</li>
  <li>Apify failures are isolated and retry-safe</li>
  <li>No partial writes occur on failure</li>
</ul>

<hr />

<h2>11. Key Design Decisions</h2>

<ul>
  <li>AI-driven URL generation avoids brittle rule-based parsing</li>
  <li>Google Sheets chosen for transparency and auditability</li>
  <li>Batch iteration ensures horizontal scalability</li>
  <li>Strict validation prevents outreach corruption</li>
</ul>

<hr />

<h2>12. Output Guarantee</h2>

<p>
  Upon successful completion, this workflow guarantees:
</p>

<ul>
  <li>A newly created campaign-specific Google Spreadsheet</li>
  <li>Only outreach-ready leads stored</li>
  <li>Zero credential leakage</li>
  <li>Deterministic downstream execution</li>
</ul>

<hr />

<h2>13. Summary</h2>

<p>
  The <strong>01-lead-acquisition</strong> workflow is the foundational layer of the AI Outreach Automation
  system. It converts human intent into a clean, validated, and traceable lead dataset, enabling all
  subsequent AI-driven personalization and outreach workflows to operate reliably and at scale.
</p>

</body>
</html>
