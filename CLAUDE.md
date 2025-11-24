# CLAUDE.md - AI Assistant Guide for Tender Repository

## Repository Overview

This repository contains an **n8n workflow automation** designed to monitor, extract, process, and distribute Bahrain Tender Board announcements via Telegram.

**Repository Name:** Tender
**Primary Purpose:** Automated tender notification and analysis system
**Technology Stack:** n8n workflow automation, AI/LLM integration (Google Gemini), Telegram Bot API
**Target Region:** Bahrain Government Tenders

---

## Project Architecture

### High-Level Workflow

```
[Trigger] → [Data Extraction] → [HTML Parsing] → [Data Processing] → [AI Analysis] → [Telegram Notification]
```

### Core Components

1. **Trigger Mechanisms**
   - Schedule Trigger: Daily execution at 8:00 AM (Asia/Bahrain timezone)
   - Gmail Trigger: Monitors incoming tender notification emails (currently disabled)
   - Webhook: External HTTP trigger endpoint for manual invocations

2. **Data Extraction Layer**
   - HTTP Request nodes: Fetch tender data from Bahrain Tender Board website
   - HTML parsing nodes: Extract structured information from tender pages
   - Code nodes: Custom JavaScript for data transformation and validation

3. **Processing Pipeline**
   - Data normalization and cleaning
   - Field extraction (tender numbers, dates, fees, descriptions)
   - Business logic (calculate days remaining, validate eligibility)

4. **AI Enhancement** (Currently Disabled)
   - Google Gemini LLM integration
   - AI Agent with specialized prompt for tender analysis
   - Generates CEO-focused summaries optimized for 10-second decision-making

5. **Notification System**
   - Telegram Bot API integration
   - HTML-formatted messages with strategic highlights
   - Priority indicators based on urgency and sector

---

## File Structure

```
Tender/
├── .git/                                              # Git repository metadata
├── Bahrain Tender Board - Fixed & Optimized 3.json   # n8n workflow definition
└── CLAUDE.md                                          # This file - AI assistant guide
```

### Primary File: `Bahrain Tender Board - Fixed & Optimized 3.json`

**Type:** n8n workflow configuration (JSON)
**Size:** ~186 KB | 3,919 lines
**Status:** Active workflow
**Total Nodes:** 44

**Node Distribution:**
- Code Nodes (JavaScript): 11
- Data Transformation Tables: 8
- HTTP Request Nodes: 5
- HTML Parsing Nodes: 4
- Set/Transform Nodes: 3
- Conditional Logic (Filter/If): 4
- AI/LLM Integration: 2 (AI Agent + Gemini Chat)
- Communication: 1 (Telegram)
- Triggers: 3 (Schedule, Gmail, Webhook)
- Other: 3 (Memory Buffer, Sticky Notes)

---

## Development Workflows

### Workflow Modification Process

When modifying the n8n workflow:

1. **Import to n8n**
   ```bash
   # The JSON file must be imported into an n8n instance
   # Can be done via n8n UI: Workflows → Import from File
   ```

2. **Edit in n8n UI**
   - Make changes using the visual workflow editor
   - Test execution with sample data
   - Validate error handling and edge cases

3. **Export Updated Workflow**
   ```bash
   # Export from n8n UI: Workflow Settings → Download
   # Replace the existing JSON file in repository
   ```

4. **Commit Changes**
   ```bash
   git add "Bahrain Tender Board - Fixed & Optimized 3.json"
   git commit -m "Update workflow: [description of changes]"
   git push
   ```

### Adding New Features

When extending the workflow:

- **New Data Fields:** Add extraction logic in the HTML parsing or Code nodes
- **Additional Triggers:** Add new trigger nodes (Email, Webhook, Cron schedule)
- **Enhanced AI Processing:** Modify the AI Agent system prompt in node parameters
- **New Notification Channels:** Add nodes for Slack, Email, SMS, etc.
- **Data Storage:** Add database nodes (PostgreSQL, MongoDB, Google Sheets)

### Testing Strategy

```javascript
// Use n8n's built-in testing features:
// 1. Manual execution with static test data
// 2. Webhook testing with curl/Postman
// 3. Schedule trigger override for immediate testing
// 4. Node-by-node execution to debug pipeline
```

---

## Key Conventions & Patterns

### 1. Data Structure Standards

**Tender Object Schema:**
```javascript
{
  tenderNumber: String,        // e.g., "90/2025/TWR"
  tenderTitle: String,          // Full tender title
  issuedBy: String,             // Issuing authority
  internalExternal: String,     // "Internal" or "External"
  tenderBoardContact: String,   // Contact information
  publishDate: Date,            // YYYY-MM-DD format
  salesStart: Date,             // Document sale start date
  closingDate: Date,            // Bid submission deadline
  purchaseBefore: Date,         // Document purchase deadline
  daysRemaining: Number,        // Calculated field
  isAlternateBidAllowed: Boolean,
  initialBond: String,          // Financial requirement
  tenderFees: String,           // Purchase cost
  contractDuration: String,     // Project duration
  bidValidity: String,          // Bid validity period
  tenderDescription: String,    // Full description
  tenderUrl: String,            // Direct link to tender
  emailBody: String,            // Original email content
  subject: String,              // Email subject line
  timestamp: DateTime           // Processing timestamp
}
```

### 2. Date Handling

- **Input Format:** `DD-MMM-YYYY HH:MM:SS` (e.g., "15-OCT-2025 14:30:00")
- **Processing:** Convert to ISO 8601 format for calculations
- **Output Format:** `YYYY-MM-DD` for display in Telegram messages
- **Timezone:** All dates in Asia/Bahrain timezone (UTC+3)

### 3. AI Prompt Engineering

The AI Agent uses a specialized prompt optimized for:
- **Role:** Senior procurement analyst
- **Output:** Telegram-optimized HTML
- **Constraints:** CEO must decide in <10 seconds
- **Enhancements:**
  - Emoji categorization (🛢️ Oil/Gas, 💻 IT, ⚕️ Healthcare, ⚡ Energy)
  - Urgency indicators (bold/underline for ≤5 days deadline)
  - SME preference highlighting
  - Link formatting with `<u>` tags

### 4. Error Handling Patterns

```javascript
// Standard error handling in Code nodes:
try {
  // Processing logic
  return [{ json: result }];
} catch (error) {
  return [{
    json: {
      error: error.message,
      timestamp: new Date().toISOString()
    }
  }];
}
```

### 5. Telegram Message Formatting

**HTML Tags Supported:**
- `<b>bold</b>` - Bold text
- `<i>italic</i>` - Italic text
- `<u>underline</u>` - Underlined text
- `<code>code</code>` - Monospace code blocks
- Links: `<u>url</u>` for underlined links

**Message Structure:**
```html
<b>Today's Tenders: [count]</b>

<b>Tender Number:</b> <code>[number]</code>
<b>Title:</b> [emoji] [shortened title]
<i>Date of Email:</i> [date]
<i>Sales Start:</i> [date]
<u><b>Bid Submission Closing Date: [date]</b></u>

• <b>Key Scope:</b> [1-line summary]
• <b>Key Eligibility:</b> [1-line requirements]

<u>[Direct tender board link]</u>
```

### 6. Node Naming Conventions

- Descriptive names: "Extract Tender Details", "Format for Telegram"
- Action-oriented: "Send a text message", "Parse HTML", "Calculate Days"
- Avoid generic names: Use "HTTP Request - Fetch Tender" instead of "HTTP Request1"

---

## Configuration & Credentials

### Required Credentials

1. **Telegram Bot API**
   - Credential ID: `RIh346E9l0ZWD0hj`
   - Chat ID: `6390929640`
   - Setup: Create bot via @BotFather on Telegram
   - Required scopes: Send messages, parse HTML

2. **Google Gemini API** (for AI features)
   - Model: gemini-pro or later
   - Used in: AI Agent node (currently disabled)
   - Rate limits: Check Google AI Studio quotas

3. **Gmail API** (optional, currently disabled)
   - OAuth 2.0 credentials required
   - Scopes: `gmail.readonly`
   - Used for: Gmail Trigger node

### Environment Variables

```bash
# n8n instance configuration
N8N_TIMEZONE=Asia/Bahrain
N8N_EXECUTION_ORDER=v1
N8N_SAVE_EXECUTION_PROGRESS=true
N8N_CALLER_POLICY=workflowsFromSameOwner
```

---

## Common Development Tasks

### Task 1: Add a New Data Field

**Example:** Add "Tender Category" field

1. Locate the HTML parsing node that extracts tender details
2. Add regex or CSS selector for the category field
3. Update the data transformation node to include `category` field
4. Modify the AI Agent prompt to incorporate category in output
5. Update Telegram message template to display category

**Estimated Complexity:** Low
**Files Modified:** Workflow JSON (specific nodes: HTML parser, Code transformer)

---

### Task 2: Change Notification Schedule

**Current:** Daily at 8:00 AM
**To Modify:**

1. Open workflow in n8n UI
2. Navigate to "Schedule Trigger" node
3. Update `triggerAtHour` parameter
4. Alternative: Add multiple schedule triggers for different times
5. Export and commit updated workflow

**Estimated Complexity:** Trivial
**Files Modified:** Workflow JSON (Schedule Trigger node parameters)

---

### Task 3: Enable AI Enhancement

**Current State:** AI Agent node is disabled
**To Enable:**

1. Configure Google Gemini API credentials in n8n
2. Enable the "AI Agent" node (remove `"disabled": true`)
3. Enable the "AI Chat Model" node
4. Test with sample tender data
5. Monitor token usage and costs
6. Export and commit changes

**Prerequisites:**
- Active Google Gemini API key
- n8n LangChain nodes installed
- Budget allocation for API costs

**Estimated Complexity:** Medium
**Files Modified:** Workflow JSON (AI Agent, Chat Model nodes)

---

### Task 4: Add Database Storage

**Requirement:** Store all processed tenders in a database

**Implementation Steps:**

1. Add a new database node (PostgreSQL, MySQL, or MongoDB)
2. Define schema matching tender object structure
3. Insert node after data processing, before Telegram notification
4. Add error handling for duplicate tenders (unique constraint on tenderNumber)
5. Optional: Add data retention policy (delete old tenders after N days)

**Recommended Node:** `n8n-nodes-base.postgres` or `n8n-nodes-base.googleSheets`

**Schema Example (PostgreSQL):**
```sql
CREATE TABLE tenders (
  id SERIAL PRIMARY KEY,
  tender_number VARCHAR(50) UNIQUE NOT NULL,
  tender_title TEXT,
  issued_by VARCHAR(255),
  publish_date DATE,
  closing_date DATE,
  days_remaining INTEGER,
  tender_url TEXT,
  processed_at TIMESTAMP DEFAULT NOW()
);
```

**Estimated Complexity:** Medium
**Files Modified:** Workflow JSON (add database node + connection)

---

### Task 5: Implement Multi-Channel Notifications

**Goal:** Send notifications to Telegram, Email, and Slack simultaneously

**Implementation:**

1. Add parallel branches after data processing
2. Add Slack node (requires Slack webhook or bot token)
3. Add Email node (SMTP or SendGrid)
4. Configure each with appropriate formatting
5. Use "Merge" node if you need to track all deliveries

**Node Structure:**
```
[Data Processing]
    ├─→ [Telegram]
    ├─→ [Slack]
    └─→ [Email]
```

**Estimated Complexity:** Medium
**Files Modified:** Workflow JSON (add Slack, Email nodes)

---

## Debugging & Troubleshooting

### Common Issues

#### 1. Workflow Execution Fails

**Symptoms:** Workflow stops mid-execution, no Telegram message sent

**Debugging Steps:**
1. Check n8n execution log for error messages
2. Verify node-by-node execution in n8n UI
3. Common causes:
   - HTTP request timeout (increase timeout in node settings)
   - Invalid HTML structure (website changed layout)
   - Missing credentials (Telegram token expired)
   - Rate limiting (too many requests to tender board website)

**Solution:** Use n8n's "Execute Node" feature to test each step individually

---

#### 2. HTML Parsing Returns Null Values

**Symptoms:** Tender fields are empty or null in output

**Root Cause:** Bahrain Tender Board website structure changed

**Fix:**
1. Manually visit the tender board website
2. Inspect HTML structure using browser DevTools
3. Update CSS selectors or regex patterns in HTML parsing nodes
4. Test with recent tender announcements

**Prevention:** Add fallback extraction methods, implement schema validation

---

#### 3. Telegram Messages Not Formatted Correctly

**Symptoms:** HTML tags displayed as plain text, links broken

**Causes:**
- `parse_mode` not set to "HTML" in Telegram node
- Invalid HTML (unclosed tags, unsupported tags)
- Special characters not escaped

**Fix:**
1. Verify Telegram node parameter: `additionalFields.parse_mode = "HTML"`
2. Validate HTML using: `{{ $json.output }}` expression
3. Escape special characters: `<`, `>`, `&` → `&lt;`, `&gt;`, `&amp;`

---

#### 4. Schedule Trigger Not Firing

**Symptoms:** Workflow doesn't execute at scheduled time

**Checklist:**
- [ ] Workflow is set to "Active" (check workflow toggle)
- [ ] n8n instance is running (not stopped/crashed)
- [ ] Timezone correctly set to Asia/Bahrain
- [ ] Execution history shows past scheduled runs
- [ ] No conflicting triggers disabled the schedule

**Verification:**
```bash
# Check n8n logs for scheduled execution
docker logs n8n | grep "Schedule Trigger"
```

---

#### 5. AI Agent Returns Generic Responses

**Symptoms:** AI summary doesn't follow template, missing key details

**Causes:**
- System prompt too long/unclear
- Input data incomplete (missing fields)
- LLM temperature too high (increase determinism)
- Token limit reached (truncated output)

**Fix:**
1. Review system prompt in AI Agent node parameters
2. Add explicit output format examples
3. Reduce temperature to 0.2-0.3 for consistency
4. Increase max tokens if output is cut off

---

## Git Workflow & Branch Strategy

### Current Branch

- **Working Branch:** `claude/claude-md-midlwjwhrpc26kxg-01Pzs71DZJASu8V8Z1MVkY5S`
- **Main Branch:** (not specified in context)

### Commit Guidelines

**Commit Message Format:**
```
<type>(<scope>): <subject>

[optional body]
```

**Types:**
- `feat`: New feature or node added
- `fix`: Bug fix or correction
- `docs`: Documentation updates (like this CLAUDE.md)
- `refactor`: Workflow restructuring without behavior change
- `config`: Credential or settings updates
- `test`: Testing changes

**Examples:**
```bash
git commit -m "feat(notification): Add Slack integration for tender alerts"
git commit -m "fix(parser): Update CSS selectors for tender board layout change"
git commit -m "docs: Create CLAUDE.md AI assistant guide"
git commit -m "config(schedule): Change trigger time from 8 AM to 9 AM"
```

### Push Strategy

**Always push to the designated branch:**
```bash
git push -u origin claude/claude-md-midlwjwhrpc26kxg-01Pzs71DZJASu8V8Z1MVkY5S
```

**Network Retry Logic:**
- Retry up to 4 times on network failures
- Exponential backoff: 2s, 4s, 8s, 16s

---

## AI Assistant Best Practices

### When Working on This Repository

1. **Always Import Workflow First**
   - Cannot edit JSON directly in a meaningful way
   - Use n8n UI for all workflow modifications
   - Export after changes to update repository

2. **Validate Data Flow**
   - Check connections between nodes
   - Ensure data transformations preserve required fields
   - Test with real tender examples

3. **Preserve Credentials**
   - Never commit credential values to repository
   - Credential IDs are safe to include (just references)
   - Document required credential types in this file

4. **Respect Disabled Nodes**
   - Some nodes disabled for cost reasons (AI Agent)
   - Others disabled due to incomplete setup (Gmail Trigger)
   - Ask before enabling expensive features

5. **Monitor Execution Costs**
   - AI/LLM calls have API costs
   - HTTP requests to external APIs may have rate limits
   - Document expected execution frequency

6. **Documentation Updates**
   - Update this CLAUDE.md when making significant changes
   - Document new nodes, credentials, or workflows
   - Include troubleshooting steps for new features

---

## Security Considerations

### Sensitive Data

- **Telegram Chat ID:** Not secret, but avoid changing without verification
- **API Keys:** Stored in n8n credential store, never in JSON file
- **Webhook URLs:** Can be made public, but prefer authentication

### Data Privacy

- Tender data is public information (from government website)
- No personal user data collected
- Email content (if Gmail trigger used) should be monitored for PII

### Access Control

- Limit n8n instance access to authorized users
- Use n8n's built-in authentication
- Implement webhook signature verification if exposing endpoints

---

## Performance Optimization

### Current Bottlenecks

1. **HTTP Requests:** Sequential fetching of tender pages
   - **Optimization:** Use parallel HTTP requests for multiple tenders

2. **HTML Parsing:** Multiple nodes for parsing same HTML
   - **Optimization:** Combine into single comprehensive parser

3. **AI Processing:** LLM calls have 2-5s latency (when enabled)
   - **Optimization:** Cache similar tender summaries, batch processing

### Resource Usage

- **Execution Time:** ~30-60 seconds per workflow run
- **Memory:** Minimal (text data only, no large files)
- **Network:** Outbound HTTP requests to tender board + Telegram API

---

## Future Enhancements

### Planned Features

- [ ] Database storage for historical tender tracking
- [ ] Web scraping for tender updates (not just new tenders)
- [ ] Multi-language support (Arabic + English)
- [ ] Tender recommendation engine (ML-based relevance scoring)
- [ ] User subscription management (per-category notifications)
- [ ] Tender document download and OCR
- [ ] Competitive analysis (track competitor bids)

### Integration Opportunities

- **CRM Systems:** Auto-create opportunities in Salesforce/HubSpot
- **Project Management:** Create tasks in Jira/Asana for tender prep
- **Calendar:** Add deadline reminders to Google Calendar
- **Analytics:** Export metrics to Google Analytics/Mixpanel

---

## Related Resources

### n8n Documentation

- [n8n Official Docs](https://docs.n8n.io/)
- [n8n Workflow Templates](https://n8n.io/workflows)
- [LangChain Integration](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.agent/)

### Bahrain Tender Board

- [Official Website](https://www.tenderboard.gov.bh/)
- Tender URL Format: `https://www.tenderboard.gov.bh/TenderDetails/?id={TENDER_NUMBER}`

### Telegram Bot API

- [Bot API Documentation](https://core.telegram.org/bots/api)
- [HTML Formatting Guide](https://core.telegram.org/bots/api#html-style)

---

## Contact & Support

### For Technical Issues

1. Check n8n community forum: [community.n8n.io](https://community.n8n.io/)
2. Review n8n GitHub issues: [github.com/n8n-io/n8n](https://github.com/n8n-io/n8n)
3. Consult this CLAUDE.md for common problems

### For Workflow Questions

- This repository is a single-workflow project
- All logic contained in: `Bahrain Tender Board - Fixed & Optimized 3.json`
- Refer to "Debugging & Troubleshooting" section above

---

## Version History

### Current State (2025-11-24)

- **Workflow Status:** Active
- **Total Nodes:** 44
- **AI Integration:** Disabled (cost optimization)
- **Execution Schedule:** Daily at 8:00 AM Asia/Bahrain
- **Notification Channel:** Telegram only

### Recent Changes

- `d162d9b`: Delete He directory
- `1b70959`: Rename README.md to He/README.md
- `0fc52f6`: Add files via upload
- `e19bc1c`: Initial commit

---

## Appendix: Node Reference

### Full Node List (44 nodes)

1. AI Agent (LangChain) - Disabled
2. Telegram: Send a text message
3. Code1 (JavaScript) - Disabled
4. Schedule Trigger (Daily 8 AM)
5. Gmail Trigger - Disabled
6. Webhook1
7. HTTP Request (x5 nodes)
8. HTML Parsing (x4 nodes)
9. Code (JavaScript) (x11 nodes)
10. Data Transformation Tables (x8 nodes)
11. Set Nodes (x3)
12. Filter Nodes (x2)
13. If Conditional (x2)
14. Google Gemini Chat Model
15. Memory Buffer Window
16. Sticky Notes

### Node Connections

The workflow follows a linear pipeline with some branching:
- Single entry point (trigger)
- Multiple HTTP request branches for data gathering
- Convergence at data processing stage
- Split again for different output formats
- Final convergence at Telegram notification

---

**Last Updated:** 2025-11-24
**Maintained For:** AI Assistants (Claude, GPT, etc.)
**Repository:** Tender - Bahrain Tender Board Automation
