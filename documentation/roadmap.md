# AI SDR Development Roadmap

## Project Evolution

The Making Intelligent Lead Generation system is being developed progressively from an automated lead-generation workflow into an agentic AI SDR.

The objective is not to replace reliable deterministic automation with AI unnecessarily. Instead, AI agents will be introduced where reasoning, research, decision-making, and adaptive behavior provide value.

---

## V2 — Intelligent Lead Generation

**Status: Working**

The current system combines automated prospect discovery, website enrichment, AI analysis, and Airtable-based lead intelligence.

### Core flow

```text
Target Market
    ↓
Search Query Generation
    ↓
SERP Discovery
    ↓
Company Extraction
    ↓
Cleaning / Normalization
    ↓
Filtering / Deduplication
    ↓
Website Enrichment
    ↓
Website Text Extraction
    ↓
AI Lead Analysis
    ↓
Airtable Lead Database
```

### Current capabilities

* Flexible industry and location targeting
* Automated prospect discovery
* Data cleaning and normalization
* Duplicate filtering
* Business website filtering
* Website enrichment
* AI-powered company analysis
* Lead qualification
* Pain-point identification
* Automation opportunity detection
* Recommended sales approach
* Airtable lead intelligence

---

# V3 — Agentic Research

**Status: Planned**

The first major transition from workflow automation to agentic behavior.

A Research Agent will be given access to research tools and will be able to determine what information it needs about a prospect.

### Planned behavior

```text
Prospect
    ↓
Research Agent
    ↓
Determine information required
    ↓
Use available research tools
    ↓
Collect evidence
    ↓
Produce structured research
```

The goal is to move from:

> Workflow collects information → AI analyzes information

toward:

> Agent determines what information it needs → Agent uses tools to obtain it → Agent analyzes the evidence

---

# V4 — Agentic Qualification & Opportunity Discovery

**Status: Planned**

The system will introduce additional reasoning around whether a prospect is worth pursuing and what commercial opportunity may exist.

### Planned architecture

```text
Research
    ↓
Qualification Agent
    ↓
Is more information required?
    ├── Yes → Additional Research
    └── No
          ↓
Opportunity Agent
          ↓
Identify business problem
          ↓
Identify supporting evidence
          ↓
Identify potential automation
          ↓
Recommend Apex approach
```

The system should be able to distinguish between:

* Poor-fit prospects
* Insufficiently researched prospects
* Qualified prospects
* High-opportunity prospects

---

# V5 — Agentic AI SDR

**Status: In Development**

V5 combines the existing lead-generation infrastructure with agentic research, qualification, opportunity discovery, and sales preparation.

### Target architecture

```text
Target Market
    ↓
Discovery
    ↓
Research Agent
    ↓
Qualification Agent
    ↓
Opportunity Agent
    ↓
SDR Agent
    ↓
Personalized Outreach
    ↓
Human Approval
    ↓
Outreach
    ↓
CRM
```

### V5 capabilities

The initial V5 prototype is intended to:

* Discover prospects
* Research companies
* Evaluate qualification
* Identify potential business problems
* Identify automation opportunities
* Generate prospect-specific sales intelligence
* Generate personalized outreach
* Maintain structured prospect information
* Require human approval before outreach

V5 is intended to be a functional agentic prototype rather than a fully autonomous production SDR.

---

# V5.x — Progressive Autonomy

After the initial V5 prototype is working, autonomy will be introduced incrementally.

### V5.1 — Tool Use

Agents gain controlled access to external tools such as:

* Search
* Website retrieval
* CRM
* Email
* Calendar
* Other business APIs

---

### V5.2 — Persistent Prospect State

The system maintains a persistent understanding of each prospect.

Example:

```text
Discovered
    ↓
Researched
    ↓
Qualified
    ↓
Contacted
    ↓
Replied
    ↓
Engaged
    ↓
Human Handoff
```

The system should understand what has already happened rather than treating every event as a new interaction.

---

### V5.3 — Response Handling

The system can interpret incoming prospect responses and determine the appropriate next action.

Possible outcomes:

* Interested
* Question
* Objection
* Not interested
* Request for information
* Meeting request
* Unclear / requires human review

---

### V5.4 — Automated Follow-up

The system can determine when a follow-up is appropriate while respecting campaign rules, prospect state, and suppression conditions.

---

### V5.5 — Human Escalation

The system identifies situations requiring human involvement.

Examples:

* High-value opportunities
* Complex technical questions
* Pricing discussions
* Ambiguous conversations
* Strong buying intent
* Situations outside the agent's authority

---

### V5.6 — Evaluation & Optimization

The system begins measuring its own outcomes.

Potential measurements include:

* Qualification accuracy
* Outreach response rate
* Positive response rate
* Meeting conversion
* Follow-up performance
* False-positive qualification
* False-negative qualification
* Agent tool usage
* Human handoff rate

The goal is to improve the system based on actual outcomes rather than assumptions.

---

# V6 — Autonomous SDR Platform

**Status: Long-term**

The long-term objective is a continuously operating AI SDR system.

### Target behavior

```text
Campaign Configuration
        ↓
Continuous Prospect Discovery
        ↓
Research
        ↓
Qualification
        ↓
Opportunity Identification
        ↓
Personalization
        ↓
Outreach
        ↓
Conversation Handling
        ↓
Follow-up
        ↓
Human Escalation
        ↓
CRM
        ↓
Outcome Evaluation
        ↓
Strategy Improvement
        ↺
```

The system should eventually be capable of operating multiple campaigns and verticals while maintaining strict business rules, human escalation, and operational guardrails.

---

# Development Principle

The system follows one core architectural principle:

> **Use deterministic automation for deterministic tasks and AI agents for tasks requiring reasoning, research, judgment, or adaptive decision-making.**

### Deterministic responsibilities

* API calls
* Data transformation
* Deduplication
* Filtering
* Database operations
* Schema validation
* Error handling
* Guardrails

### Agentic responsibilities

* Research planning
* Tool selection
* Evidence evaluation
* Deciding whether more information is required
* Prospect qualification
* Opportunity identification
* Sales personalization
* Conversation interpretation
* Deciding when human intervention is required

---

# Commercial Objective

The system is initially being developed as an internal Apex Square growth tool.

The first objective is to use the AI SDR to generate and qualify prospects for Apex Square's own automation services.

Once the system is reliable and produces measurable results, it can be productized as a managed AI sales/outbound service for clients.

The intended progression is:

```text
Build
  ↓
Use internally
  ↓
Generate real opportunities
  ↓
Validate commercially
  ↓
Productize
  ↓
Sell as a managed service
```

The system is therefore both:

1. An internal business-growth system.
2. A long-term AI automation product opportunity.

---

# Current Development Priority

The immediate priority is:

**Build V5 as a functional agentic prototype.**

The focus is on demonstrating the transition from:

> Automated lead generation

to:

> Agentic sales intelligence and SDR behavior

before adding full autonomous outreach, persistent conversational memory, continuous operation, and advanced optimization.
