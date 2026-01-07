<h1>Architecture Overview</h1>

<hr/>

<h2>1. Introduction</h2>

<p>
This document provides a <strong>comprehensive, system-level architecture overview</strong> of the
<strong>AI Outreach Automation</strong> platform.
It explains how all workflows, services, integrations, and data stores interact
to deliver an end-to-end automated Email and LinkedIn outbound outreach system.
</p>

<p>
The architecture is designed with the following primary goals:
</p>

<ul>
  <li>Scalability across multiple campaigns and clients</li>
  <li>Clear separation of concerns between phases</li>
  <li>Deterministic execution and traceability</li>
  <li>Compliance-aware, human-like automation</li>
  <li>Extensibility for future channels and capabilities</li>
</ul>

<hr/>

<h2>2. System Boundaries & Scope</h2>

<p>
The AI Outreach Automation platform operates within clearly defined boundaries.
It does not generate inbound demand or manage CRM pipelines.
Instead, it focuses on <strong>outbound engagement automation</strong>.
</p>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th align="left">Included</th>
    <th align="left">Excluded</th>
  </tr>
  <tr>
    <td>Lead sourcing from third-party databases</td>
    <td>Inbound lead capture (forms, ads)</td>
  </tr>
  <tr>
    <td>AI-driven research and personalization</td>
    <td>CRM deal management</td>
  </tr>
  <tr>
    <td>LinkedIn and Email outreach execution</td>
    <td>Marketing newsletters</td>
  </tr>
  <tr>
    <td>Conversation automation</td>
    <td>Payment or billing systems</td>
  </tr>
</table>

<hr/>

<h2>3. High-Level System Architecture</h2>

<p>
At a high level, the system follows a <strong>layered, pipeline-oriented architecture</strong>.
Each layer has a single responsibility and communicates with adjacent layers via
well-defined data contracts.
</p>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Layer</th>
    <th>Responsibility</th>
  </tr>
  <tr>
    <td>Trigger & Control Layer</td>
    <td>Initiates workflows and orchestrates execution</td>
  </tr>
  <tr>
    <td>Acquisition Layer</td>
    <td>Sources and validates leads</td>
  </tr>
  <tr>
    <td>Intelligence Layer</td>
    <td>Research, reasoning, and personalization</td>
  </tr>
  <tr>
    <td>Execution Layer</td>
    <td>Outbound communication across channels</td>
  </tr>
  <tr>
    <td>Engagement Layer</td>
    <td>Conversation initiation and continuation</td>
  </tr>
  <tr>
    <td>Persistence & Tracking Layer</td>
    <td>State management, auditing, and observability</td>
  </tr>
</table>

<hr/>

<h2>4. Core Orchestration Engine</h2>

<p>
The entire system is orchestrated using <strong>n8n</strong>, which serves as:
</p>

<ul>
  <li>Workflow execution engine</li>
  <li>State transition coordinator</li>
  <li>Integration hub for external services</li>
</ul>

<p>
Each workflow is implemented as an independent, deployable automation unit,
allowing isolation, debugging, and reuse.
</p>

<hr/>

<h2>5. Workflow Architecture (Phase-Based)</h2>

<h3>5.1 Phase 01 – Lead Acquisition</h3>
<ul>
  <li>Human-triggered workflow</li>
  <li>AI-generated Apollo search logic</li>
  <li>Apify-based data extraction</li>
  <li>Campaign-specific Google Sheet creation</li>
</ul>

<h3>5.2 Phase 02 – AI Personalization</h3>
<ul>
  <li>Public web research via SerpAPI</li>
  <li>AI reasoning and synthesis</li>
  <li>Icebreaker and message generation</li>
  <li>Structured enrichment of lead records</li>
</ul>

<h3>5.3 Phase 03 – Outbound Outreach</h3>
<ul>
  <li>Multi-channel execution (LinkedIn + Email)</li>
  <li>Sequential dispatch logic</li>
  <li>Human-like pacing and delays</li>
  <li>Persistent delivery tracking</li>
</ul>

<h3>5.4 Phase 04 – Connection Engagement</h3>
<ul>
  <li>Event-driven acceptance detection</li>
  <li>Immediate post-acceptance messaging</li>
  <li>Conversation session initialization</li>
</ul>

<h3>5.5 Phase 05 – AI Reply Bot</h3>
<ul>
  <li>Inbound message webhooks</li>
  <li>Context-aware AI replies</li>
  <li>Conversation memory management</li>
</ul>

<hr/>

<h2>6. Data Flow Architecture</h2>

<p>
Data flows through the system in a strictly controlled, forward-only manner.
Each phase enriches the data without mutating upstream state.
</p>

<ol>
  <li>User intent captured via chat trigger</li>
  <li>Lead dataset created and stored in Google Sheets</li>
  <li>Enrichment fields appended per lead</li>
  <li>Outreach status fields updated per channel</li>
  <li>Conversation metadata persisted in database</li>
</ol>

<hr/>

<h2>7. Data Stores & Persistence Strategy</h2>

<h3>7.1 Google Sheets</h3>
<ul>
  <li>Primary operational datastore</li>
  <li>Human-readable and auditable</li>
  <li>Tracks lead state across all phases</li>
</ul>

<h3>7.2 PostgreSQL</h3>
<ul>
  <li>Structured conversation memory</li>
  <li>Session-level state management</li>
  <li>Used by AI Reply Bot for context</li>
</ul>

<hr/>

<h2>8. External Integrations</h2>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Service</th>
    <th>Role</th>
  </tr>
  <tr>
    <td>Apollo</td>
    <td>Lead database</td>
  </tr>
  <tr>
    <td>Apify</td>
    <td>Scalable scraping infrastructure</td>
  </tr>
  <tr>
    <td>SerpAPI</td>
    <td>Public web research</td>
  </tr>
  <tr>
    <td>OpenAI</td>
    <td>Reasoning, personalization, replies</td>
  </tr>
  <tr>
    <td>Unipile</td>
    <td>LinkedIn automation and webhooks</td>
  </tr>
  <tr>
    <td>Gmail API</td>
    <td>Email delivery</td>
  </tr>
</table>

<hr/>

<h2>9. Event-Driven vs Batch Processing</h2>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Mode</th>
    <th>Workflows</th>
    <th>Purpose</th>
  </tr>
  <tr>
    <td>Batch</td>
    <td>01, 02, 03</td>
    <td>Controlled, scalable execution</td>
  </tr>
  <tr>
    <td>Event-Driven</td>
    <td>04, 05</td>
    <td>Real-time engagement</td>
  </tr>
</table>

<hr/>

<h2>10. Security & Credential Architecture</h2>

<ul>
  <li>No credentials stored in workflow JSON</li>
  <li>Environment variable-based secret injection</li>
  <li>Scoped API tokens per service</li>
  <li>Webhook endpoints protected via secret paths</li>
</ul>

<hr/>

<h2>11. Compliance & Risk Controls</h2>

<ul>
  <li>Public data usage only for research</li>
  <li>No unsolicited messaging after connection</li>
  <li>Rate-limited, human-like automation</li>
  <li>Clear opt-out and disengagement paths</li>
</ul>

<hr/>

<h2>12. Scalability & Extensibility</h2>

<p>
The architecture supports horizontal growth through:
</p>

<ul>
  <li>Campaign-level isolation</li>
  <li>Workflow-level modularity</li>
  <li>Replaceable AI models</li>
  <li>Additional channels (e.g., WhatsApp, CRM sync)</li>
</ul>

<hr/>

<h2>13. Observability & Auditability</h2>

<ul>
  <li>Google Sheets for operational visibility</li>
  <li>Database-backed conversation history</li>
  <li>Deterministic workflow logs in n8n</li>
</ul>

<hr/>

<h2>14. Key Architectural Design Decisions</h2>

<ul>
  <li>Separation of intelligence from execution</li>
  <li>State externalization (Sheets + DB)</li>
  <li>Event-driven engagement handling</li>
  <li>Human-like automation safeguards</li>
</ul>

<hr/>

<h2>15. Summary</h2>

<p>
The AI Outreach Automation architecture is a <strong>modular, scalable, and compliance-aware system</strong>
designed to automate outbound Email and LinkedIn engagement end-to-end.
By combining deterministic workflows, AI reasoning, and robust tracking,
the system achieves human-quality outreach at machine scale.
</p>

</body>
</html>
