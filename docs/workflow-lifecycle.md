<h1>Workflow Lifecycle</h1>

<hr/>

<h2>1. Purpose of This Document</h2>

<p>
This document defines the <strong>complete operational lifecycle</strong> of the AI Outreach Automation system.
It explains how workflows are initiated, how they transition between phases, how state is preserved,
and how the system evolves from a single user intent into a sustained, AI-driven conversation.
</p>

<p>
The workflow lifecycle is designed to be:
</p>

<ul>
  <li>Deterministic</li>
  <li>Traceable</li>
  <li>Fault-isolated</li>
  <li>Human-reviewable</li>
</ul>

<hr/>

<h2>2. Lifecycle Overview (High-Level)</h2>

<p>
The system lifecycle progresses through <strong>five distinct, ordered workflow phases</strong>.
Each phase has a clearly defined entry condition, execution responsibility, and exit state.
</p>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Phase</th>
    <th>Workflow</th>
    <th>Execution Mode</th>
    <th>Primary Outcome</th>
  </tr>
  <tr>
    <td>Phase 01</td>
    <td>Lead Acquisition</td>
    <td>Batch (Manual Trigger)</td>
    <td>Qualified lead dataset</td>
  </tr>
  <tr>
    <td>Phase 02</td>
    <td>AI Personalization</td>
    <td>Batch (Automated)</td>
    <td>Personalized outreach assets</td>
  </tr>
  <tr>
    <td>Phase 03</td>
    <td>Outbound Outreach</td>
    <td>Batch (Automated)</td>
    <td>LinkedIn + Email messages sent</td>
  </tr>
  <tr>
    <td>Phase 04</td>
    <td>Connection Engagement</td>
    <td>Event-driven</td>
    <td>Conversation initiated</td>
  </tr>
  <tr>
    <td>Phase 05</td>
    <td>AI Reply Bot</td>
    <td>Event-driven</td>
    <td>Ongoing autonomous conversation</td>
  </tr>
</table>

<hr/>

<h2>3. Lifecycle Entry Point</h2>

<h3>3.1 Human-Initiated Campaign Start</h3>

<p>
The lifecycle begins when a human operator submits a natural-language campaign request.
This is the <strong>only manual action required</strong> for the entire system.
</p>

<ul>
  <li>Input format: free-form text</li>
  <li>Represents target audience intent</li>
  <li>Defines campaign scope and identity</li>
</ul>

<p>
From this point onward, the system operates autonomously.
</p>

<hr/>

<h2>4. Phase 01 – Lead Acquisition Lifecycle</h2>

<h3>4.1 Entry Condition</h3>
<ul>
  <li>User intent successfully captured</li>
</ul>

<h3>4.2 Execution Responsibilities</h3>
<ul>
  <li>Translate intent into structured search logic</li>
  <li>Extract leads from external databases</li>
  <li>Validate outreach readiness</li>
</ul>

<h3>4.3 State Transitions</h3>
<ol>
  <li>Intent received</li>
  <li>Search parameters generated</li>
  <li>Leads scraped</li>
  <li>Leads validated</li>
  <li>Campaign dataset created</li>
</ol>

<h3>4.4 Exit Condition</h3>
<ul>
  <li>Campaign-specific Google Sheet populated with qualified leads</li>
</ul>

<hr/>

<h2>5. Phase 02 – AI Personalization Lifecycle</h2>

<h3>5.1 Entry Condition</h3>
<ul>
  <li>Valid lead dataset exists</li>
</ul>

<h3>5.2 Execution Responsibilities</h3>
<ul>
  <li>Research each lead using public data</li>
  <li>Synthesize contextual insights</li>
  <li>Generate personalized messaging assets</li>
</ul>

<h3>5.3 Internal Processing Flow</h3>
<ol>
  <li>Lead batch retrieval</li>
  <li>Public web research</li>
  <li>AI reasoning and summarization</li>
  <li>Icebreaker generation</li>
  <li>Message generation</li>
  <li>Schema validation</li>
</ol>

<h3>5.4 Exit Condition</h3>
<ul>
  <li>All leads enriched with AI-generated content</li>
</ul>

<hr/>

<h2>6. Phase 03 – Outbound Outreach Lifecycle</h2>

<h3>6.1 Entry Condition</h3>
<ul>
  <li>Leads contain validated personalization fields</li>
</ul>

<h3>6.2 Execution Responsibilities</h3>
<ul>
  <li>Execute LinkedIn invites</li>
  <li>Send cold emails</li>
  <li>Apply pacing and anti-detection controls</li>
</ul>

<h3>6.3 Sequential Execution Order</h3>
<ol>
  <li>Lead retrieval</li>
  <li>LinkedIn invite dispatch</li>
  <li>LinkedIn status logging</li>
  <li>Email dispatch</li>
  <li>Email status logging</li>
  <li>Delay injection</li>
</ol>

<h3>6.4 Exit Condition</h3>
<ul>
  <li>Outbound messages sent</li>
  <li>Delivery state persisted</li>
</ul>

<hr/>

<h2>7. Phase 04 – Connection Engagement Lifecycle</h2>

<h3>7.1 Entry Condition</h3>
<ul>
  <li>LinkedIn connection accepted by prospect</li>
</ul>

<h3>7.2 Execution Responsibilities</h3>
<ul>
  <li>Detect acceptance event</li>
  <li>Initiate conversation</li>
  <li>Persist conversation state</li>
</ul>

<h3>7.3 Event-Driven Flow</h3>
<ol>
  <li>Acceptance webhook received</li>
  <li>Lead resolved</li>
  <li>Status updated</li>
  <li>First message sent</li>
  <li>Conversation session created</li>
</ol>

<h3>7.4 Exit Condition</h3>
<ul>
  <li>Conversation thread initialized</li>
</ul>

<hr/>

<h2>8. Phase 05 – AI Reply Bot Lifecycle</h2>

<h3>8.1 Entry Condition</h3>
<ul>
  <li>Inbound message received from prospect</li>
</ul>

<h3>8.2 Execution Responsibilities</h3>
<ul>
  <li>Maintain conversation context</li>
  <li>Generate intelligent replies</li>
  <li>Prevent over-automation</li>
</ul>

<h3>8.3 Conversational Loop</h3>
<ol>
  <li>Inbound message webhook</li>
  <li>Session resolution</li>
  <li>Context assembly</li>
  <li>AI response generation</li>
  <li>Reply dispatch</li>
  <li>State persistence</li>
</ol>

<h3>8.4 Exit Condition</h3>
<ul>
  <li>Conversation naturally ends or is escalated to human</li>
</ul>

<hr/>

<h2>9. State Management Across the Lifecycle</h2>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>State Type</th>
    <th>Storage</th>
    <th>Purpose</th>
  </tr>
  <tr>
    <td>Lead State</td>
    <td>Google Sheets</td>
    <td>Workflow progression</td>
  </tr>
  <tr>
    <td>Conversation State</td>
    <td>PostgreSQL</td>
    <td>Context continuity</td>
  </tr>
  <tr>
    <td>Execution Logs</td>
    <td>n8n</td>
    <td>Debugging & audit</td>
  </tr>
</table>

<hr/>

<h2>10. Failure Isolation Strategy</h2>

<ul>
  <li>Each lead processed independently</li>
  <li>Each event handled atomically</li>
  <li>Failures do not cascade across phases</li>
  <li>State remains consistent on partial failures</li>
</ul>

<hr/>

<h2>11. Lifecycle Completion Criteria</h2>

<p>
The workflow lifecycle is considered complete when:
</p>

<ul>
  <li>No further inbound messages are received</li>
  <li>Reply limits are reached</li>
  <li>Conversation is escalated or closed</li>
</ul>

<p>
At this point, the system becomes dormant for that lead.
</p>

<hr/>

<h2>12. Key Lifecycle Design Principles</h2>

<ul>
  <li>Clear phase boundaries</li>
  <li>Forward-only state transitions</li>
  <li>Event-driven responsiveness</li>
  <li>Audit-first persistence</li>
</ul>

<hr/>

<h2>13. Summary</h2>

<p>
The workflow lifecycle of the AI Outreach Automation system defines a
<strong>predictable, scalable, and compliant operational journey</strong>
from initial intent to sustained conversation.
Each phase builds upon the previous one, ensuring the system behaves
as a controlled, intelligent, and enterprise-grade automation platform.
</p>

</body>
</html>
