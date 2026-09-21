# System Architecture

## Overview

The Intelligent Lead Generation system is designed as a modular pipeline that combines deterministic workflow automation with AI-assisted reasoning.

The architecture separates predictable data-processing operations from tasks that require interpretation and judgment.

The current implementation is **V2**, while the architecture is intentionally designed to evolve toward an agentic AI SDR system.

---

# Architecture Principles

The system follows four core principles:

### 1. Deterministic Where Possible

Tasks with predictable inputs and outputs should remain deterministic.

Examples:

* API requests
* Data transformation
* URL normalization
* Filtering
* Deduplication
* Database operations
* Validation
* Error handling

These operations are handled by n8n.

---

### 2. AI Where Reasoning Is Required

AI is used where understanding context is necessary.

Examples:

* Interpreting website content
* Identifying potential operational pain points
* Assessing digital maturity
* Identifying automation opportunities
* Generating sales intelligence
* Creating a recommended sales angle

This prevents the system from unnecessarily turning simple automation into AI-driven logic.

---

### 3. Structured AI Output

AI-generated information should be converted into structured data before entering downstream systems.

Instead of treating an LLM response as a block of text, the system extracts defined fields such as:

```json
{
  "qualification": "",
  "digital_maturity": "",
  "automation_potential": "",
  "pain_indicators": [],
  "automation_opportunities": [],
  "recommended_pitch": "",
  "ai_reasoning": ""
}
```

This allows AI output to become usable operational data.

---

### 4. Modular Evolution

Each major capability is treated as an independent stage.

This allows individual components to be replaced or upgraded without rebuilding the entire system.

For example:

```text
V2 Research Logic
        ↓
V5 Research Agent
```

The surrounding infrastructure can remain largely unchanged while the reasoning layer becomes more autonomous.

---

# High-Level Architecture

```text
┌───────────────────────────────┐
│        TARGET INPUT           │
│                               │
│ Industry + Location + Criteria│
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│          DISCOVERY            │
│                               │
│ Search Query Generation       │
│ SERP API                      │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│      NORMALIZATION             │
│                               │
│ Company / Website / Domain    │
│ Location / Source             │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│       FILTERING               │
│                               │
│ Deduplication                 │
│ Business Website Validation   │
│ Blocked Domains               │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│       WEB ENRICHMENT          │
│                               │
│ HTTP Request                  │
│ HTML Retrieval                │
│ Text Cleaning                 │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│       AI ANALYSIS             │
│                               │
│ Business Understanding        │
│ Qualification                 │
│ Pain Identification           │
│ Opportunity Discovery        │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│      STRUCTURED DATA          │
│                               │
│ AI Output Parsing             │
│ Validation                    │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│          AIRTABLE             │
│                               │
│ Prospect Database / CRM       │
└───────────────────────────────┘
```

---

# Layered Architecture

The system can be understood as five logical layers.

## Layer 1 — Input

Defines the target market.

### Inputs

* Industry
* Location
* Search criteria
* Campaign requirements

The targeting layer should remain independent from the processing logic.

This allows the same workflow to operate against different markets.

---

# Layer 2 — Discovery & Data Acquisition

This layer finds potential businesses and retrieves publicly available information.

### Components

* Search query generation
* SERP API
* Website HTTP requests

### Responsibilities

* Discover potential companies
* Extract initial business information
* Retrieve website content
* Pass raw information to downstream processing

The discovery layer does not make the final qualification decision.

---

# Layer 3 — Data Processing

This layer converts inconsistent external information into usable structured data.

### Components

* Normalization
* Filtering
* Deduplication
* Website validation
* Text cleaning

### Responsibilities

```text
Raw Search Results
       ↓
Clean Data
       ↓
Validated Prospects
       ↓
Researchable Businesses
```

This layer is intentionally deterministic.

---

# Layer 4 — Intelligence

The intelligence layer interprets the information gathered by the previous stages.

The current V2 implementation uses an LLM for this stage.

### Responsibilities

The AI evaluates:

* Business context
* Digital maturity
* Qualification
* Pain indicators
* Automation potential
* Potential automation opportunities
* Recommended sales approach

The AI should base conclusions on available evidence.

The system should avoid inventing:

* Internal processes
* Technologies
* Staff structures
* Revenue
* Customer volume
* Operational problems

when those details are not supported by available information.

---

# Layer 5 — Persistence

The persistence layer stores the resulting prospect intelligence.

### Current Database

Airtable.

### Stored Information

The database can contain:

```text
Company
Website
Domain
Location
Industry
Email
Phone
Lead Score
Qualification
Digital Maturity
Automation Potential
Pain Indicators
Automation Opportunities
Recommended Pitch
AI Reasoning
Source
Status
Discovered At
Company Complexity
Recommended Automation
```

The database serves as both:

* a prospect repository
* a working sales intelligence layer

Future versions can extend this into persistent prospect state.

---

# Current V2 Data Flow

The current implementation follows a mostly linear architecture:

```text
INPUT
  ↓
SEARCH
  ↓
EXTRACT
  ↓
NORMALIZE
  ↓
FILTER
  ↓
DEDUPLICATE
  ↓
FETCH WEBSITE
  ↓
CLEAN CONTENT
  ↓
AI ANALYSIS
  ↓
PARSE OUTPUT
  ↓
STORE
```

This architecture is reliable for a predictable workflow because every stage has a defined role.

However, the system's reasoning is still largely determined by the workflow itself.

---

# V2 Architecture Characteristics

V2 can be described as:

**Workflow-driven automation with an AI intelligence layer.**

The workflow determines:

* What happens next
* Which information is collected
* When AI is called
* What data is stored
* Which systems are connected

The AI determines:

* What the business appears to do
* Whether certain signals indicate an opportunity
* What automation opportunities may exist
* How the prospect could potentially be approached

This distinction is important for the transition toward agentic architecture.

---

# Transition to Agentic Architecture

The future architecture introduces autonomous reasoning components while retaining deterministic infrastructure.

Instead of:

```text
Research
  ↓
Qualification
  ↓
Opportunity
```

the system can evolve toward:

```text
Research Agent
      ↓
Qualification Agent
      ↓
Need More Information?
   ↙           ↘
 YES            NO
  ↓              ↓
Research       Opportunity
 Agent           Agent
  ↓
Qualification
```

The important architectural change is that the system can determine whether additional information is required.

---

# V5 Target Architecture

The planned V5 architecture is:

```text
                 ┌─────────────────┐
                 │  TARGET INPUT   │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │    DISCOVERY    │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │ RESEARCH AGENT  │◄────┐
                 └────────┬────────┘     │
                          │              │
                          ▼              │
                 ┌─────────────────┐     │
                 │ QUALIFICATION   │     │
                 │     AGENT       │     │
                 └────────┬────────┘     │
                          │              │
                    Need Research?       │
                       │                 │
                 YES ──┘────────────────┘
                       │
                       NO
                       │
                       ▼
                 ┌─────────────────┐
                 │  OPPORTUNITY    │
                 │     AGENT       │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │    SDR AGENT    │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │ HUMAN APPROVAL  │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │    OUTREACH     │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │ RESPONSE / CRM  │
                 └─────────────────┘
```

---

# Agent Responsibilities

## Research Agent

The Research Agent is responsible for understanding what information is required to evaluate a prospect.

Potential tools:

* Web search
* Website retrieval
* Search APIs
* Company information sources
* Existing prospect data

The agent should be able to determine whether more research is required.

---

## Qualification Agent

The Qualification Agent evaluates the available evidence against the campaign's target criteria.

Potential decisions include:

```text
QUALIFIED
NOT QUALIFIED
NEEDS MORE RESEARCH
```

It should provide evidence supporting the decision rather than relying on unsupported assumptions.

---

## Opportunity Agent

The Opportunity Agent focuses on the business problem rather than simply scoring the prospect.

It identifies potential areas such as:

* Lead capture
* CRM
* Enquiry qualification
* Quote preparation
* Customer follow-up
* Document processing
* Internal administration
* Reporting
* Routing

The objective is to answer:

> What could this business potentially automate, and why?

---

## SDR Agent

The SDR Agent converts sales intelligence into an outreach strategy.

Potential outputs include:

* Prospect summary
* Reason for contacting
* Personalization points
* Recommended channel
* Subject line
* Outreach draft
* Call to action
* Follow-up recommendation

Initial V5 architecture keeps a human approval stage before sending outreach.

---

# Tool Architecture

Agents should not directly control every part of the system.

Instead, they should interact with controlled tools.

Conceptually:

```text
                 AGENT
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
   Web Search   Website     CRM
                Fetch
        │          │          │
        └──────────┼──────────┘
                   ▼
              Structured
                Results
```

n8n remains responsible for implementing and controlling these tools.

This provides a separation between:

**Reasoning**

and

**Execution**

---

# State Management

A future version requires persistent prospect state.

Instead of treating every workflow execution as an independent event, prospects can move through defined states.

Example:

```text
DISCOVERED
    ↓
RESEARCHING
    ↓
QUALIFIED
    ↓
OPPORTUNITY_IDENTIFIED
    ↓
OUTREACH_READY
    ↓
CONTACTED
    ↓
ENGAGED
    ↓
QUALIFYING
    ↓
MEETING_REQUESTED
    ↓
HUMAN_HANDOFF
    ↓
MEETING_BOOKED
    ↓
WON / LOST / NURTURE
```

This enables the system to understand what has already happened to a prospect and determine what should happen next.

---

# Human-in-the-Loop Architecture

Autonomy should increase gradually.

The initial V5 system intentionally includes human approval:

```text
AI SDR
   ↓
Generate Outreach
   ↓
Human Review
   ↓
Approve / Reject / Edit
   ↓
Send
```

This provides an evaluation point before the system is allowed to perform more autonomous outbound actions.

Human intervention should remain available for:

* High-value prospects
* Ambiguous situations
* Pricing discussions
* Technical questions
* Sensitive conversations
* Unusual requests
* Explicit escalation

---

# Guardrails

Agentic behavior requires controlled boundaries.

Important guardrails include:

### Prospect Guardrails

* Respect campaign targeting
* Exclude unsuitable industries
* Exclude blocked domains
* Avoid duplicate outreach
* Maintain prospect state

### Research Guardrails

* Prefer evidence over assumptions
* Validate important information
* Avoid unnecessary research loops
* Respect API and service limits

### Outreach Guardrails

* Require approval during initial V5
* Avoid unsupported claims
* Avoid misleading personalization
* Respect opt-outs
* Respect applicable communication regulations

### Technical Guardrails

* Structured outputs
* Schema validation
* Error handling
* Retry limits
* API limits
* Logging
* Fallback paths

---

# Reliability Strategy

The architecture intentionally avoids giving unrestricted control to agents.

A useful rule is:

```text
Agent decides
      ↓
System validates
      ↓
Tool executes
      ↓
Result returns
      ↓
Agent reasons again
```

This makes agentic behavior observable and controllable.

The agent should not be treated as the entire application.

It is one reasoning component inside a larger system.

---

# Scalability Direction

The architecture can eventually evolve from a single campaign workflow into a reusable sales platform.

### Current

```text
One Workflow
     ↓
One Campaign
     ↓
One Prospect Database
```

### Future

```text
Campaign Manager
       │
 ┌─────┼─────┐
 ▼     ▼     ▼
Campaign A  B  C
 │           │
 ▼           ▼
Agents      Agents
 │           │
 ▼           ▼
Prospect State
 │
 ▼
CRM / Outreach / Analytics
```

The same underlying agent capabilities can therefore support different industries, locations, offers, and campaigns.

---

# Architectural Evolution

The project is intended to evolve through increasing levels of autonomy.

```text
V2
Workflow Automation
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
V5.x
Tools + Memory + Responses
        ↓
V6
Autonomous SDR Platform
```

Each stage builds on the previous infrastructure rather than requiring a complete rewrite.

---

# Long-Term System

The eventual system is intended to operate as an AI-powered sales department.

Conceptually:

```text
                 CAMPAIGN
                    │
                    ▼
              PROSPECTING
                    │
                    ▼
               RESEARCH
                    │
                    ▼
              QUALIFICATION
                    │
                    ▼
              OPPORTUNITY
                 DISCOVERY
                    │
                    ▼
                OUTREACH
                    │
                    ▼
                RESPONSE
                    │
                    ▼
              CONVERSATION
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
      AUTOMATION           HUMAN
       CONTINUES           HANDOFF
          │                   │
          └─────────┬─────────┘
                    ▼
                OUTCOME
                    │
                    ▼
               EVALUATION
                    │
                    ▼
             SYSTEM IMPROVEMENT
```

The goal is not simply to automate lead collection.

The goal is to build a system capable of moving from:

**finding a business → understanding the business → identifying an opportunity → initiating a conversation → managing the sales process → handing off when human involvement is required.**

---

# Engineering Objective

The central engineering challenge of this project is the transition from a **fixed workflow** to a **controlled autonomous system**.

V2 establishes the data acquisition, processing, AI analysis, and persistence foundations.

Future versions introduce:

* Agentic reasoning
* Tool use
* Dynamic research
* Persistent state
* Response handling
* Follow-up logic
* Human escalation
* Evaluation
* Continuous improvement

The architecture is therefore designed around one central idea:

> **Deterministic systems execute. AI agents reason. The architecture controls both.**
