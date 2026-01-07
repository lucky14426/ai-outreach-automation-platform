<h1>Data Model</h1>

<hr/>

<h2>1. Purpose of This Document</h2>

<p>
This document defines the <strong>complete data model</strong> for the AI Outreach Automation system.
It describes all core entities, attributes, relationships, lifecycle states, and data contracts
used across workflows from lead acquisition to autonomous conversation handling.
</p>

<p>
The data model is designed to ensure:
</p>

<ul>
  <li>Deterministic workflow execution</li>
  <li>Cross-workflow data compatibility</li>
  <li>Auditability and traceability</li>
  <li>Clear separation between operational and conversational data</li>
</ul>

<hr/>

<h2>2. Data Model Design Principles</h2>

<ul>
  <li><strong>Single Source of Truth</strong> for operational state</li>
  <li><strong>Append-only enrichment</strong> across workflow phases</li>
  <li><strong>Forward-only state transitions</strong></li>
  <li><strong>Separation of concerns</strong> between lead data and conversation data</li>
  <li><strong>Human-readable + machine-safe</strong> storage formats</li>
</ul>

<hr/>

<h2>3. High-Level Data Architecture</h2>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Data Domain</th>
    <th>Primary Store</th>
    <th>Purpose</th>
  </tr>
  <tr>
    <td>Lead & Campaign Data</td>
    <td>Google Sheets</td>
    <td>Operational state, tracking, visibility</td>
  </tr>
  <tr>
    <td>Conversation Data</td>
    <td>PostgreSQL</td>
    <td>Contextual memory and message history</td>
  </tr>
  <tr>
    <td>Execution Metadata</td>
    <td>n8n Internal Logs</td>
    <td>Debugging and auditing</td>
  </tr>
</table>

<hr/>

<h2>4. Core Entities Overview</h2>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Entity</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Campaign</td>
    <td>Represents a single outbound initiative</td>
  </tr>
  <tr>
    <td>Lead</td>
    <td>Individual prospect record</td>
  </tr>
  <tr>
    <td>Personalization Asset</td>
    <td>AI-generated research and messaging content</td>
  </tr>
  <tr>
    <td>Outreach Event</td>
    <td>LinkedIn or Email action execution</td>
  </tr>
  <tr>
    <td>Conversation Session</td>
    <td>Thread-level LinkedIn messaging context</td>
  </tr>
  <tr>
    <td>Conversation Message</td>
    <td>Individual inbound or outbound message</td>
  </tr>
</table>

<hr/>

<h2>5. Campaign Entity</h2>

<h3>5.1 Description</h3>
<p>
A Campaign represents a single, user-initiated outreach initiative.
All leads, messages, and conversations are scoped to a campaign.
</p>

<h3>5.2 Attributes</h3>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Field</th>
    <th>Description</th>
    <th>Required</th>
  </tr>
  <tr>
    <td>campaign_id</td>
    <td>Unique campaign identifier</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>campaign_name</td>
    <td>Derived from user query</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td>Campaign creation timestamp</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>sheet_id</td>
    <td>Google Sheet identifier</td>
    <td>Yes</td>
  </tr>
</table>

<hr/>

<h2>6. Lead Entity</h2>

<h3>6.1 Description</h3>
<p>
A Lead represents a single prospect sourced from Apollo and enriched throughout the system lifecycle.
</p>

<h3>6.2 Core Identity Fields</h3>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Field</th>
    <th>Description</th>
    <th>Required</th>
  </tr>
  <tr>
    <td>lead_id</td>
    <td>Unique Apollo identifier</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>full_name</td>
    <td>Prospect name</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>linkedin_url</td>
    <td>Public LinkedIn profile</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>email</td>
    <td>Verified email address</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>organization</td>
    <td>Company name</td>
    <td>No</td>
  </tr>
  <tr>
    <td>role</td>
    <td>Job title / seniority</td>
    <td>No</td>
  </tr>
</table>

<hr/>

<h2>7. Lead Lifecycle State Fields</h2>

<p>
Each lead progresses through defined lifecycle states.
State fields are additive and never overwritten.
</p>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Field</th>
    <th>Meaning</th>
  </tr>
  <tr>
    <td>personalized</td>
    <td>AI enrichment completed</td>
  </tr>
  <tr>
    <td>linkedin_invite_sent</td>
    <td>LinkedIn connection request sent</td>
  </tr>
  <tr>
    <td>email_sent</td>
    <td>Email outreach delivered</td>
  </tr>
  <tr>
    <td>invite_accepted</td>
    <td>LinkedIn connection accepted</td>
  </tr>
  <tr>
    <td>conversation_active</td>
    <td>Conversation session initialized</td>
  </tr>
</table>

<hr/>

<h2>8. Personalization Asset Entity</h2>

<h3>8.1 Description</h3>
<p>
Personalization Assets store AI-generated research and messaging content per lead.
</p>

<h3>8.2 Fields</h3>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Field</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>research_summary</td>
    <td>Condensed public research insights</td>
  </tr>
  <tr>
    <td>linkedin_icebreaker</td>
    <td>Short LinkedIn opening line</td>
  </tr>
  <tr>
    <td>email_icebreaker</td>
    <td>Email opening line</td>
  </tr>
  <tr>
    <td>email_subject</td>
    <td>Personalized subject line</td>
  </tr>
  <tr>
    <td>email_body</td>
    <td>Full email message</td>
  </tr>
  <tr>
    <td>linkedin_message</td>
    <td>Post-acceptance LinkedIn message</td>
  </tr>
</table>

<hr/>

<h2>9. Outreach Event Entity</h2>

<h3>9.1 Description</h3>
<p>
Outreach Events represent execution actions taken against a lead.
</p>

<h3>9.2 Attributes</h3>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Field</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>event_id</td>
    <td>Unique execution identifier</td>
  </tr>
  <tr>
    <td>event_type</td>
    <td>LinkedIn Invite / Email Send</td>
  </tr>
  <tr>
    <td>timestamp</td>
    <td>Execution time</td>
  </tr>
  <tr>
    <td>status</td>
    <td>Success / Failure</td>
  </tr>
</table>

<hr/>

<h2>10. Conversation Session Entity</h2>

<h3>10.1 Description</h3>
<p>
A Conversation Session represents a single LinkedIn message thread between the system and a lead.
</p>

<h3>10.2 Attributes</h3>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Field</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>session_id</td>
    <td>Unique conversation identifier</td>
  </tr>
  <tr>
    <td>provider_id</td>
    <td>LinkedIn provider identifier</td>
  </tr>
  <tr>
    <td>lead_id</td>
    <td>Associated lead</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td>Session start timestamp</td>
  </tr>
  <tr>
    <td>message_count</td>
    <td>Total messages exchanged</td>
  </tr>
</table>

<hr/>

<h2>11. Conversation Message Entity</h2>

<h3>11.1 Description</h3>
<p>
Represents a single inbound or outbound message within a conversation session.
</p>

<h3>11.2 Fields</h3>

<table border="1" cellpadding="8" cellspacing="0" width="100%">
  <tr>
    <th>Field</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>message_id</td>
    <td>Unique message identifier</td>
  </tr>
  <tr>
    <td>session_id</td>
    <td>Conversation session reference</td>
  </tr>
  <tr>
    <td>sender_type</td>
    <td>Prospect or AI</td>
  </tr>
  <tr>
    <td>message_text</td>
    <td>Message content</td>
  </tr>
  <tr>
    <td>timestamp</td>
    <td>Message time</td>
  </tr>
</table>

<hr/>

<h2>12. Entity Relationships</h2>

<ul>
  <li>One Campaign → Many Leads</li>
  <li>One Lead → One Personalization Asset</li>
  <li>One Lead → Many Outreach Events</li>
  <li>One Lead → One Conversation Session</li>
  <li>One Session → Many Conversation Messages</li>
</ul>

<hr/>

<h2>13. Data Evolution Across Lifecycle</h2>

<ol>
  <li>Lead identity created</li>
  <li>Personalization assets appended</li>
  <li>Outreach events logged</li>
  <li>Conversation session initialized</li>
  <li>Conversation messages appended</li>
</ol>

<hr/>

<h2>14. Data Integrity & Validation Rules</h2>

<ul>
  <li>Mandatory identity fields validated before processing</li>
  <li>Session existence required before replies</li>
  <li>Message count limits enforced</li>
  <li>No destructive updates allowed</li>
</ul>

<hr/>

<h2>15. Summary</h2>

<p>
The data model of the AI Outreach Automation system is intentionally simple,
explicit, and lifecycle-oriented.
By separating operational lead state from conversational memory,
the system achieves scalability, reliability, and explainability
while supporting advanced AI-driven engagement.
</p>

</body>
</html>
