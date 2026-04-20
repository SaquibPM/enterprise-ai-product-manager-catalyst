---
name: design-ai-agent
description: >
  Generate a comprehensive AI agent design document covering architecture, safety guardrails, human oversight, and UX patterns. Use when starting design of any AI-powered feature that involves autonomous reasoning, decision-making, or action-taking. Triggers on: design AI agent, agent design doc, agentic feature design, AI system architecture.
---

# /design-ai-agent Command

## Usage

Invoke this command when you need to design an AI agent system from the ground up:

```
/design-ai-agent
```

The command will guide you through a structured 4-step design process, producing a complete, production-ready agent design document. Each step builds on the previous one and includes validation checkpoints.

## What This Command Does

This command chains four specialized skills into an end-to-end agent design workflow. It produces a 25-35 page design document suitable for sharing with engineering teams, security reviews, and leadership. The output covers:

- **Complete system architecture** (topology, state management, memory)
- **Safety & compliance framework** (guardrails stack, regulatory mapping, cost controls)
- **Human oversight design** (HITL workflows, approval thresholds, escalation paths)
- **UX patterns** (confidence indicators, progressive disclosure, error states)
- **Operational excellence** (cost model, monitoring, rollout plan, success metrics)

This is a sequential workflow. Each step must complete before the next begins because downstream steps depend on architectural decisions made upstream.

## Skill Chain

### Step 1: Agentic Architecture
**Skill:** `agentic-architecture`  
**Duration:** 5-7 minutes

This skill establishes the foundational topology and behavioral model:
- **Outputs:** Defines agent topology (orchestrator-worker, hierarchical, etc.), identifies all autonomous decision points, specifies memory architecture (session/conversational/long-term), catalogs tools the agent needs, and lists integration points with external systems.
- **Artifacts:** System diagram (ASCII or reference format), decision flowchart, tool inventory table, memory layer specifications
- **Validates:** Can this agent actually accomplish its stated goals? Are dependencies clear? Is memory strategy appropriate?

**User Checkpoint:** "Does this architecture look right? Any constraints I should know?" This is where the PM validates that the technical approach matches business requirements. Common feedback: cost constraints, latency requirements, existing system integrations that must be respected.

---

### Step 2: Guardrails Design
**Skill:** `guardrails-design`  
**Duration:** 5-7 minutes

This skill applies comprehensive safety controls based on Step 1 architecture:
- **Inputs:** Agent topology from Step 1, list of autonomous actions the agent can take
- **Outputs:** Complete guardrails stack (5 layers: input validation, prompt injection defense, output filtering, cost controls, compliance enforcement), compliance requirement mapping, audit trail design
- **Artifacts:** Guardrails architecture diagram, compliance matrix (regulations vs. guardrails), cost limit specifications, monitoring/audit query templates
- **Validates:** Can this agent harm users, violate regulations, or exceed budget? Are all autonomous actions properly constrained?

**User Checkpoint:** "Are there additional compliance requirements?" This is where the PM adds domain-specific constraints (healthcare privacy, financial regulations, industry standards). The skill refines the guardrails based on feedback.

---

### Step 3: Human Oversight Design
**Skill:** `human-in-loop`  
**Duration:** 4-6 minutes

This skill designs the human review and approval layer:
- **Inputs:** Agent architecture from Step 1, guardrails from Step 2, and agent confidence/risk scoring model
- **Outputs:** HITL workflow definition (which decisions require approval, which can auto-execute), confidence/risk thresholds for triggering escalation, approval SLAs and routing logic, audit trail requirements
- **Artifacts:** HITL decision matrix, threshold specifications, workflow diagrams, escalation rules
- **Validates:** Are high-risk decisions reviewed before execution? Can humans intervene? Is the approval burden reasonable (not too high, not too low)?

---

### Step 4: AI-Native UX Design
**Skill:** `ai-native-ux`  
**Duration:** 3-5 minutes

This skill designs how users interact with and understand agent behavior:
- **Inputs:** Full agent architecture, guardrails, HITL workflows, and confidence scoring
- **Outputs:** UX patterns for agent transparency (confidence indicators, reasoning disclosure, decision explainability), error state designs, progressive disclosure patterns, user control mechanisms
- **Artifacts:** UX pattern library, wireframe descriptions, interaction flows, confidence/risk indicator specs
- **Validates:** Do users understand what the agent is doing? Can they intervene? Is trust built appropriately?

---

## Data Handoff Between Skills

| From | To | Artifact | Content |
|------|----|---------:|---------|
| Architecture | Guardrails | System diagram + tool inventory | Agent topology, autonomous actions, external integrations |
| Guardrails | HITL | Guardrails spec + compliance matrix | Risk categories, confidence scoring needs, approval thresholds |
| HITL | UX Design | HITL workflows + approval rules | Decision points requiring transparency, escalation patterns |
| UX Design | Final Doc | All upstream outputs | Integrated into complete design document |

Each skill explicitly validates its input before proceeding and surfaces any incompatibilities or gaps for the PM to address.

## Parallelization Strategy

**This command is SEQUENTIAL by necessity:**

- **Step 1 → Step 2:** Guardrails design must know the exact topology, tools, and autonomous actions. These come from Step 1.
- **Step 2 → Step 3:** HITL thresholds depend on guardrails layer. A decision with strong guardrails may need less human review; a risky decision needs escalation. This dependency is tight.
- **Step 3 → Step 4:** UX patterns must reflect actual HITL workflows. Users need to understand which decisions are auto-executed vs. require approval. This must flow from Step 3's decisions.

**Why not parallel?** Early parallelization would require making speculative assumptions about downstream constraints. This increases rework. The sequential approach is faster end-to-end and produces better designs because each step refines the previous.

## User Interaction Points

### **After Step 1 Completes (Architecture)**
The command pauses and presents the architecture summary:
```
✓ Architecture defined: [topology type] with [N] autonomous decision points
✓ Memory strategy: [type + capacity]
✓ Tool inventory: [N] tools, [K] external integrations
✓ Estimated latency: [time range]
✓ Estimated cost per operation: [$X.XX]

Question: Does this architecture look right? 
Any constraints I should know? (cost, latency, regulatory, existing integrations)
```

**User response expected:** Approval, or feedback on constraints (e.g., "We can't exceed $0.50 per operation" or "Must integrate with Salesforce's API" or "GDPR-sensitive data").

The command updates Step 2 inputs based on feedback.

### **After Step 2 Completes (Guardrails)**
The command pauses and presents the guardrails summary:
```
✓ Guardrails stack: [5 layers with summary of each]
✓ Compliance coverage: [Regulations mapped to guardrails]
✓ Cost controls: [Limits enforced where]
✓ Audit trail: [What gets logged, where]

Question: Are there additional compliance requirements?
(e.g., HIPAA, SOX, PCI-DSS, industry-specific standards)
```

**User response expected:** Additional requirements, or approval to proceed.

### **Steps 3 and 4**
These run without mandatory pauses unless the user explicitly flags concerns. If concerns are raised, the skill pauses for clarification.

## Output Format

The final AI agent design document follows this structure (25-35 pages):

1. **Title & Metadata**
   - Agent name, purpose, version, author, date, stakeholders

2. **Executive Summary** (1 page)
   - What the agent does, why it matters, key design decisions, risks & mitigations

3. **Agent Architecture** (3-4 pages)
   - Topology and system diagram
   - Decision flowchart with all autonomous actions
   - Tool/MCP inventory with integration points
   - Memory architecture (types, capacity, persistence)
   - State management approach

4. **Memory Architecture Detail** (1-2 pages)
   - Session/conversational state
   - Long-term memory / vector store
   - Context window management
   - Persistence strategy

5. **Tool & MCP Inventory** (1 page)
   - Catalog of all tools the agent can invoke
   - Integration dependencies
   - Rate limits and costs

6. **Guardrails Design** (4-5 pages)
   - Layer 1: Input validation & normalization
   - Layer 2: Prompt injection & adversarial attack defense
   - Layer 3: Output filtering & safety checks
   - Layer 4: Cost & resource controls
   - Layer 5: Compliance enforcement
   - Compliance requirement mapping
   - Monitoring & alert specifications

7. **Human Oversight Design** (3-4 pages)
   - HITL workflow definition
   - Decision approval matrix (which decisions require review)
   - Confidence/risk thresholds for escalation
   - Approval routing logic
   - SLA specifications
   - Escalation paths and fallback behaviors

8. **UX Design** (3-4 pages)
   - Confidence indicators (how users see agent confidence)
   - Progressive disclosure patterns (showing reasoning step-by-step)
   - Error state designs
   - User control mechanisms (override, pause, cancel)
   - Accessibility & internationalization

9. **Cost Model** (1 page)
   - Per-operation cost breakdown
   - Monthly/annual projections at scale
   - Cost optimization opportunities
   - Budget guardrails

10. **Operational Excellence** (2-3 pages)
    - Monitoring & observability (metrics, logs, traces)
    - Incident response playbook
    - Performance benchmarks & targets
    - Rollout plan (pilot → limited → full)

11. **Success Metrics** (1 page)
    - Business metrics (what success looks like)
    - Technical metrics (reliability, latency, cost)
    - Behavioral metrics (user trust, adoption)
    - Measurement plan

12. **Risks & Mitigations** (1 page)
    - Key risks identified during design
    - Mitigation strategies

13. **Appendices**
    - Detailed glossary
    - Regulatory reference guide
    - Compliance checklist
    - Tool API specifications
    - Sample prompts & system messages

## Estimated Time

**Total end-to-end:** 15-25 minutes (depending on complexity)

- Step 1 (Architecture): 5-7 min
- User checkpoint: 2-5 min (user review + feedback)
- Step 2 (Guardrails): 5-7 min
- User checkpoint: 1-3 min (user review + feedback)
- Step 3 (HITL): 4-6 min
- Step 4 (UX Design): 3-5 min
- **Total: 15-25 minutes for a complete, production-ready design document**

## When to Use This Command

- Starting design of any autonomous agent, bot, or agentic feature
- Building AI systems that make decisions without human review
- Designing systems that integrate with external APIs or customer data
- Creating features subject to regulatory oversight
- Building high-stakes features (financial, healthcare, legal, security)
- When you need a design document for engineering/security review

## When NOT to Use

- Simple chatbot or Q&A system (use simpler design patterns)
- Pure retrieval-augmented generation without autonomous actions
- Low-stakes, read-only features
- One-off prototypes (though it's good practice even for early-stage work)

---

**Next Steps After Command Completes:**

1. Share design document with engineering for technical review
2. Share with security/compliance teams for regulatory sign-off
3. Socialize with leadership for buy-in on cost, timeline, risk model
4. Create engineering epics based on architecture and rollout plan
5. Begin pilot implementation with Phase 1 limited scope
