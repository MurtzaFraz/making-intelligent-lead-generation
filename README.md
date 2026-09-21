# Making Intelligent Lead Generation

## 1. Project Overview

**Making Intelligent Lead Generation** is Apex Square's internal AI-powered lead-generation and sales-intelligence system.

The system is designed to discover businesses based on a flexible target market, collect and normalize company information, enrich prospects through their websites, analyze them using AI, identify potential business pain points and automation opportunities, and store the resulting intelligence in Airtable for sales use.

The long-term objective is to evolve this system into an **agentic AI SDR** capable of independently researching, qualifying, prioritizing, personalizing, and eventually engaging prospects.

---

# 2. Current Version

**Current version: V2 — Intelligent Lead Generation**

V2 is primarily an **automated workflow with AI analysis**.

It is not yet a fully agentic system.

The workflow determines _how_ the process runs, while AI is currently responsible primarily for interpreting and analyzing the information collected by the workflow.

---

# 3. Current Architecture

```text
                    TARGET MARKET
                         │
                         ▼
                 ┌─────────────────┐
                 │  Search Query   │
                 │   Generation    │
                 └────────┬────────┘
                          ▼
                 ┌─────────────────┐
                 │    SERP API     │
                 │    Discovery    │
                 └────────┬────────┘
                          ▼
                 ┌─────────────────┐
                 │ Extract Company │
                 │      Data       │
                 └────────┬────────┘
                          ▼
                 ┌─────────────────┐
                 │ Clean / Normalize│
                 └────────┬────────┘
                          ▼
                 ┌─────────────────┐
                 │ Filter / Dedup  │
                 └────────┬────────┘
                          ▼
                 ┌─────────────────┐
                 │ Filter Business │
                 │    Websites     │
                 └────────┬────────┘
                          ▼
                 ┌─────────────────┐
                 │ Website HTML    │
                 │   Extraction    │
                 └────────┬────────┘
                          ▼
                 ┌─────────────────┐
                 │ Clean Website   │
                 │      Text       │
                 └────────┬────────┘
                          ▼
                 ┌─────────────────┐
                 │   OpenAI LLM    │
                 │ AI Lead Analysis│
                 └────────┬────────┘
                          ▼
                 ┌─────────────────┐
                 │ Parse AI Data   │
                 └────────┬────────┘
                          ▼
                 ┌─────────────────┐
                 │ Airtable CRM /  │
                 │  Lead Database  │
                 └─────────────────┘
```

---

# 4. Input

The system is intentionally designed so that the target market is **not hardcoded**.

The same underlying workflow should be capable of being used for different combinations such as:

```text
Industry: Construction
Location: United Kingdom
```

or:

```text
Industry: HVAC
Location: Sydney, Australia
```

or:

```text
Industry: Mobile Car Repair
Location: Toronto, Canada
```

The target market is therefore treated as an input rather than a permanent workflow configuration.

---

# 5. Lead Discovery

## Trigger

The workflow currently starts with a **Manual Trigger / Execute Workflow**.

This is intentional during development and testing.

The workflow can therefore be run on demand without relying on a schedule.

---

## Search Query Generation

The target industry and location are converted into search queries.

The purpose is to produce search-engine queries capable of discovering relevant businesses rather than relying on a fixed company list.

---

## SERP API

The generated queries are sent to the SERP API.

The API returns search results containing potential businesses and websites.

The system then extracts relevant company information from those results.

---

# 6. Data Extraction and Normalization

After discovery, the workflow extracts the company information returned by the search results.

The data then passes through cleaning and normalization steps.

The purpose is to make inconsistent search results usable downstream.

Typical normalization includes:

- Company name cleanup
- Website normalization
- Domain extraction
- Location normalization
- Removal of irrelevant search-result information

---

# 7. Filtering and Deduplication

The workflow filters discovered records before expensive AI processing.

This includes:

### Duplicate filtering

The same business may appear across multiple search queries.

Duplicate records are therefore removed before enrichment.

### Business website filtering

The workflow attempts to distinguish actual business websites from directories, aggregators and other irrelevant domains.

Known blocked/irrelevant domains include examples such as:

- Yelp
- Houzz
- event/expo sites
- lead-generation directories

The objective is to analyze the **business itself**, rather than a third-party directory page.

---

# 8. Website Enrichment

For qualifying businesses, the workflow sends an HTTP request to retrieve the company's website HTML.

The raw HTML is then cleaned to produce usable website text.

The AI should receive useful business information rather than raw HTML noise.

The resulting website text becomes one of the primary evidence sources for AI analysis.

---

# 9. AI Lead Analysis

The cleaned website information is passed to an OpenAI model.

The current AI analysis is designed around the role of a:

> **B2B Sales Intelligence Analyst for an AI Automation Agency**

The model is instructed to analyze businesses using evidence found in the available data.

The system explicitly aims to avoid inventing business problems or capabilities that cannot be supported by evidence.

---

# 10. Current AI Analysis

The AI currently evaluates areas including:

### Business qualification

- Is this a relevant business?
- Does it fit the target market?
- Does the company appear suitable for Apex outreach?

### Digital maturity

The AI looks for evidence of the company's current digital processes and infrastructure.

### Automation potential

The AI identifies areas where business processes may potentially benefit from automation.

### Pain indicators

The system looks for observable signals suggesting operational or customer-process problems.

### Automation opportunities

Potential opportunities include:

- Lead capture
- CRM processes
- Enquiry qualification
- Requirement extraction
- Construction quote preparation
- Quote workflows
- Follow-ups
- Email communication
- Document extraction
- Administration
- Reporting
- Routing

### Recommended pitch

The system generates a potential Apex sales angle based on the observed evidence.

### AI reasoning

The system stores the reasoning supporting the analysis.

---

# 11. Current Airtable Database

The analyzed leads are stored in Airtable.

Current information includes fields such as:

| Field                    | Purpose                        |
| ------------------------ | ------------------------------ |
| Company                  | Business name                  |
| Website                  | Company website                |
| Domain                   | Normalized domain              |
| Location                 | Business location              |
| Industry                 | Business category              |
| Email                    | Available contact email        |
| Phone                    | Available phone number         |
| Lead Score               | AI-generated prioritization    |
| Qualification            | Overall qualification          |
| Digital Maturity         | Observed digital maturity      |
| Automation Potential     | Potential for automation       |
| Pain Indicators          | Evidence of potential problems |
| Automation Opportunities | Potential automation areas     |
| Recommended Pitch        | Suggested Apex approach        |
| AI Reasoning             | Supporting reasoning           |
| Source                   | Discovery source               |
| Status                   | Sales pipeline state           |
| Discovered At            | Discovery timestamp            |
| Company Complexity       | Estimated complexity           |
| Recommended Automation   | Proposed automation            |

The database is currently functioning as both a **lead repository and sales-intelligence layer**.

---

# 12. Important Design Decisions

## Flexible targeting

The system should not be permanently tied to one industry or location.

The goal is:

```text
Target Market
      ↓
Same system
      ↓
Different prospect pool
```

---

## Evidence-based AI

AI recommendations should be based on observable evidence.

The system should not invent:

- technologies
- business processes
- problems
- staffing levels
- CRM systems
- lead volumes
- operational practices

when those facts have not been established.

---

## Simple Airtable field types

During development, Airtable single-select fields caused unnecessary type and workflow errors.

Many fields were therefore changed to **single-line text** where strict option enforcement was not essential.

This makes the n8n → Airtable integration more tolerant and reduces failures caused by exact option matching or typecasting.

---

# 13. What V2 Can Currently Do

Given a target such as:

```text
Small construction companies
+
United Kingdom
```

the system can:

1. Generate search queries.
2. Search for potential companies.
3. Extract company information.
4. Normalize the data.
5. Filter irrelevant results.
6. Deduplicate businesses.
7. Identify usable business websites.
8. Retrieve website information.
9. Clean website content.
10. Send evidence to an AI model.
11. Analyze the business.
12. Identify potential pain points.
13. Identify automation opportunities.
14. Generate a recommended sales approach.
15. Store the resulting intelligence in Airtable.

This makes V2 substantially more useful than a simple lead scraper.

---

# 14. Current Limitations

V2 is still fundamentally a **workflow-driven system**.

The workflow decides the sequence of operations.

The AI primarily performs analysis after the information has already been collected.

The current system does not yet fully provide:

- Autonomous research planning
- AI-controlled tool selection
- Agentic research loops
- Persistent conversational memory
- Autonomous prospect-state management
- Automated outreach decisions
- Reply handling
- Automated follow-up strategy
- Human escalation logic
- Continuous campaign management
- Outcome-based optimization

These limitations define the next development stage.

---

# 15. V2 → V5 Direction

The purpose of V5 is not to throw V2 away.

Instead, V2 becomes the **data and automation foundation** underneath the agentic system.

The planned architecture is:

```text
                    TARGET
                      │
                      ▼
              DISCOVERY SYSTEM
                      │
                      ▼
              RESEARCH AGENT
                      │
                      ▼
           QUALIFICATION AGENT
                      │
                      ▼
           OPPORTUNITY AGENT
                      │
                      ▼
              SDR AGENT
                      │
                      ▼
             HUMAN APPROVAL
                      │
                      ▼
                 OUTREACH
                      │
                      ▼
                  CRM
```

Over time, individual deterministic V2 components can be replaced or augmented with agents where reasoning and decision-making are beneficial.

---

# 16. Long-Term Objective

The long-term system is intended to become an **AI SDR for Apex Square**.

Its responsibilities would eventually include:

```text
Discover prospects
        ↓
Research businesses
        ↓
Determine relevance
        ↓
Identify business opportunities
        ↓
Determine whether outreach is appropriate
        ↓
Personalize communication
        ↓
Contact prospects
        ↓
Understand replies
        ↓
Qualify conversations
        ↓
Follow up when appropriate
        ↓
Escalate qualified opportunities
        ↓
Update CRM
        ↓
Learn from outcomes
```

The immediate objective is not to make every component autonomous.

The immediate objective is to introduce **agentic reasoning and tool use** into the existing system while preserving the reliable deterministic infrastructure already built.

---

# 17. Development Philosophy

The system should follow this principle:

> **Use deterministic automation for deterministic tasks and AI agents for tasks requiring reasoning, judgment, research or adaptive decision-making.**

Examples:

### Deterministic

- API calls
- Data transformation
- Deduplication
- Filtering
- Database updates
- Schema validation
- Error handling
- Guardrails

### Agentic

- Deciding what information is needed
- Research planning
- Evaluating evidence
- Determining whether additional research is necessary
- Identifying potential business opportunities
- Selecting an appropriate sales angle
- Understanding prospect replies
- Deciding when human intervention is required

This separation is fundamental to the architecture of the future system.

---

# 18. Current Position

**V2 is the foundation.**

It already performs:

> **Discovery → Enrichment → AI Analysis → Sales Intelligence**

The next stage is to add:

> **Reasoning → Tool Use → Decisions → Memory → Actions**

That transition is what turns the existing lead-generation workflow into an **agentic AI SDR system**.
