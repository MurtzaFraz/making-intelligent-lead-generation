# Intelligent Lead Generation — V2

## Overview

V2 is an AI-powered lead generation and sales intelligence workflow built with **n8n**.

The system discovers businesses based on a configurable industry and location, filters and normalizes the results, enriches prospects using their websites, and uses an LLM to analyze each business for qualification and potential automation opportunities.

The objective is not simply to collect business names.

V2 is designed to turn publicly available business information into **structured sales intelligence** that can be used to identify companies that may benefit from automation services.

---

## Workflow

```text
Target Industry + Location
          ↓
    Build Search Queries
          ↓
       SERP API
          ↓
   Extract Companies
          ↓
   Clean & Normalize
          ↓
 Filter & Deduplicate
          ↓
 Filter Business Websites
          ↓
    Fetch Website HTML
          ↓
    Clean Website Text
          ↓
       AI Analysis
          ↓
    Parse AI Results
          ↓
    Airtable CRM
```

---

## What V2 Does

### 1. Configurable Targeting

The workflow accepts targeting parameters such as:

* Industry
* Location
* Search requirements

The system is not tied to a single city or industry.

For example, the same workflow can be adapted to search for:

* Construction companies in the UK
* HVAC companies in Sydney
* Automotive businesses in Toronto
* Other service-based businesses in different markets

This allows the underlying system to be reused across campaigns.

---

### 2. Business Discovery

Search queries are generated from the selected targeting criteria and sent through a SERP API.

The discovery stage collects publicly available business results and extracts relevant company information.

Typical data includes:

* Company name
* Website
* Location
* Source information
* Search result information

The purpose of this stage is to create an initial prospect pool rather than immediately deciding whether a company is qualified.

---

### 3. Data Cleaning & Normalization

Raw search results are not treated as clean CRM data.

The workflow normalizes extracted information before continuing.

This includes:

* Cleaning company names
* Normalizing website URLs
* Extracting domains
* Handling missing values
* Standardizing fields
* Removing malformed results

This creates a consistent structure for later processing.

---

### 4. Filtering & Deduplication

The workflow removes results that should not enter the prospect database.

Filtering can exclude:

* Duplicate companies
* Non-business websites
* Directory websites
* Irrelevant domains
* Search result noise

Known irrelevant domains can also be blocked through filtering logic.

This prevents the AI analysis stage from wasting processing and API resources on unsuitable records.

---

## 5. Website Enrichment

For qualifying prospects, the workflow attempts to retrieve the company's website.

The HTML is then processed into cleaner text before being passed to the AI analysis stage.

This gives the AI access to information actually published by the business rather than relying only on search-result snippets.

Potential evidence includes:

* Services offered
* Contact/enquiry processes
* Quote forms
* Booking processes
* Customer communication methods
* Website functionality
* Operational information
* Signs of manual processes

---

## 6. AI Sales Intelligence

The cleaned website information is analyzed using an LLM.

The AI acts as a **B2B sales intelligence analyst for an automation agency**.

The analysis focuses on identifying evidence of:

* Lead capture problems
* CRM opportunities
* Enquiry qualification opportunities
* Requirement extraction
* Quote preparation
* Quote workflows
* Customer follow-up
* Email communication
* Document processing
* Administrative work
* Reporting
* Routing and internal workflows

The AI is instructed to distinguish between **observed evidence** and assumptions.

It should not invent business processes that cannot reasonably be supported by available information.

---

## 7. Structured Prospect Intelligence

The AI analysis is parsed into structured fields before being stored.

The prospect database can contain information such as:

| Field                    | Purpose                                    |
| ------------------------ | ------------------------------------------ |
| Company                  | Business name                              |
| Website                  | Company website                            |
| Domain                   | Normalized domain                          |
| Location                 | Business location                          |
| Industry                 | Business category                          |
| Email                    | Available contact email                    |
| Phone                    | Available phone number                     |
| Lead Score               | AI-assisted qualification indicator        |
| Qualification            | Prospect qualification                     |
| Digital Maturity         | Observed digital maturity                  |
| Automation Potential     | Potential for automation                   |
| Pain Indicators          | Evidence of potential operational problems |
| Automation Opportunities | Potential automation use cases             |
| Recommended Pitch        | Suggested sales angle                      |
| AI Reasoning             | Reasoning behind the analysis              |
| Source                   | Discovery source                           |
| Status                   | Prospect lifecycle status                  |
| Discovered At            | Discovery timestamp                        |
| Company Complexity       | Estimated operational complexity           |
| Recommended Automation   | Suggested automation direction             |

---

## Architecture

V2 follows a mostly deterministic automation architecture with an AI reasoning layer.

### Deterministic Components

n8n handles predictable operations such as:

* Workflow execution
* API requests
* Data transformation
* Filtering
* Deduplication
* Website retrieval
* Record creation
* Record updates
* Error handling

### AI Component

The LLM handles tasks where interpretation is required:

* Understanding website content
* Identifying potential pain points
* Assessing digital maturity
* Identifying automation opportunities
* Generating sales intelligence
* Producing a recommended pitch

This separation is intentional.

The system does not use AI simply because AI is available. Deterministic operations remain deterministic wherever possible.

---

## Data Flow

A simplified representation of the system is:

```text
INPUT
Industry
Location
    │
    ▼
DISCOVERY
Search Query Generation
SERP API
    │
    ▼
NORMALIZATION
Company
Website
Domain
Location
    │
    ▼
FILTERING
Business Website Check
Deduplication
Blocked Domains
    │
    ▼
ENRICHMENT
Website HTML
    │
    ▼
TEXT PROCESSING
Clean Website Content
    │
    ▼
AI ANALYSIS
Qualification
Pain Indicators
Automation Opportunities
Recommended Pitch
    │
    ▼
STRUCTURED DATA
Airtable
    │
    ▼
SALES INTELLIGENCE
Prospect ready for outreach
```

---

## Current Technology Stack

* **n8n** — workflow orchestration
* **SERP API** — business discovery through search results
* **HTTP requests** — website retrieval
* **OpenAI / LLM** — website analysis and sales intelligence
* **Airtable** — prospect database and CRM layer

---

## Design Decisions

### Evidence Before AI Conclusions

The system attempts to provide the AI with actual website information before asking it to identify opportunities.

This reduces reliance on generic assumptions based solely on company names or search snippets.

### Flexible Targeting

Industry and location are inputs rather than hardcoded workflow logic.

This allows the same architecture to support different campaigns.

### Structured Data

AI output is parsed into defined fields before being written to the CRM.

This makes the output usable by downstream automation rather than leaving the result as unstructured text.

### Simple Database Types

The Airtable schema intentionally favors simple field types where strict field typing does not add meaningful value.

For example, many classification fields use text values rather than unnecessary single-select constraints.

This reduces workflow fragility when AI-generated values vary.

### Deterministic Automation + AI Reasoning

V2 establishes an architectural principle that will continue into later versions:

> Use deterministic automation for deterministic tasks and AI for tasks that require interpretation, reasoning, or judgment.

---

## Current Capabilities

V2 can currently:

* Accept configurable targeting criteria
* Discover potential businesses
* Extract company information
* Normalize prospect data
* Remove duplicates
* Filter irrelevant websites
* Retrieve website content
* Clean website text
* Analyze prospects with an LLM
* Identify potential automation opportunities
* Generate sales intelligence
* Store structured prospect records in Airtable

---

## Current Limitations

V2 is still primarily a **workflow-driven system**.

It does not yet provide full autonomous SDR behavior.

Current limitations include:

* Discovery is primarily search/API driven
* Research is largely predefined
* AI analysis follows a fixed workflow path
* The system does not independently decide which research tools to use
* Qualification does not dynamically request additional research
* Outreach generation is not yet fully agentic
* Response handling is not yet autonomous
* Persistent prospect memory is limited
* Human approval remains necessary for future outbound stages

These limitations are intentional boundaries of V2 rather than failures of the architecture.

---

# Evolution Toward V5

V2 is the foundation for an agentic sales system.

The planned evolution is:

```text
V2
Intelligent Lead Generation
        ↓
V3
Agentic Research
        ↓
V4
Agentic Qualification
        ↓
V5
Agentic AI SDR
        ↓
V6
Autonomous SDR Platform
```

The transition toward V5 introduces agents that can make decisions about what information they need and what action should happen next.

A simplified V5 architecture is:

```text
TARGET
  ↓
DISCOVERY
  ↓
RESEARCH AGENT
  ↓
QUALIFICATION AGENT
  ↓
OPPORTUNITY AGENT
  ↓
SDR AGENT
  ↓
HUMAN APPROVAL
  ↓
OUTREACH
  ↓
RESPONSE / CRM
```

One important planned capability is an agentic research loop:

```text
Research
   ↓
Qualification
   ↓
Enough information?
   ├── YES → Opportunity Analysis
   │
   └── NO
        ↓
   More Research
        ↓
   Qualification
```

This represents a shift from a fixed automation pipeline toward a system capable of making decisions about its own next research action within defined boundaries.

---

## V2 → V5 Architectural Principle

The goal is **not** to replace every n8n node with an AI agent.

Instead:

### n8n remains responsible for

* APIs
* Data transformation
* Filtering
* Deduplication
* Database operations
* Validation
* Error handling
* Guardrails
* Workflow orchestration

### Agents become responsible for

* Deciding what to research
* Choosing appropriate research actions
* Evaluating evidence
* Determining whether additional information is required
* Qualifying prospects
* Identifying business opportunities
* Personalizing outreach
* Understanding responses
* Determining when human intervention is required

This separation is intended to make the system more reliable, controllable, and scalable.

---

## Project Objective

The long-term objective is to develop an AI-powered sales system that can:

1. Discover potential customers
2. Research their businesses
3. Determine whether they fit the target market
4. Identify operational problems and automation opportunities
5. Generate personalized outreach
6. Handle responses
7. Follow up appropriately
8. Maintain prospect state
9. Escalate important conversations to a human
10. Continuously improve based on outcomes

The system will initially be used internally to support **Apex Square's own business development**.

Successful components can later be productized as a managed AI SDR / sales automation service.

---

## Version Status

| Version | Description                                         | Status         |
| ------- | --------------------------------------------------- | -------------- |
| V1      | Initial lead generation concepts                    | Completed      |
| **V2**  | Intelligent lead generation + AI sales intelligence | **Working**    |
| V3      | Agentic research                                    | Planned        |
| V4      | Agentic qualification & opportunity discovery       | Planned        |
| V5      | Agentic AI SDR                                      | In development |
| V6      | Autonomous SDR platform                             | Future         |

---

## Repository

This repository contains the evolving implementation of the system.

The V2 workflow export is located at:

```text
workflows/v2/intelligent-lead-generation-v2.json
```

Future versions will be maintained separately so that the evolution of the architecture remains visible.

---

## Disclaimer

This system is intended for legitimate business research and sales development.

Prospect information should be validated before commercial use, and outreach should comply with applicable privacy, anti-spam, platform, and data-protection requirements in the target market.
