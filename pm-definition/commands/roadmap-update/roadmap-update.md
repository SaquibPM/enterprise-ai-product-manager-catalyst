---
name: /roadmap-update
description: Update product roadmap with multiple audience views (exec, engineering, customer) and prioritization rationale
category: pm-definition
tags: [roadmap, planning, prioritization, stakeholder-comms]
---

## Overview
Generates an updated product roadmap with audience-specific views (executive summary, technical details, customer-facing). Each view emphasizes different aspects (business impact, technical complexity, customer value).

## Usage
```
/roadmap-update \
  --quarter q3-2026 \
  --audiences exec,engineering,customer \
  --include-themes true \
  --include-rationale true \
  --capacity-planning true \
  --include-risks true
```

## Skill Chain (Sequential)

### 1. Roadmap-Planning (Processing)
- **Contributes**: Feature priorities, timing, dependencies
- **Input**: Product vision, customer research, competitive context
- **Process**:
  - Group features by theme (AI, Compliance, Integrations, etc.)
  - Prioritize by: revenue impact, strategic importance, customer urgency
  - Map features to quarters
  - Identify dependencies
- **Output**: Theme-based roadmap

### 2. Prioritization-Frameworks (Validation)
- **Contributes**: Scoring rationale, capacity allocation, trade-off analysis
- **Input**: Prioritized feature list
- **Process**:
  - Score features using RICE (Reach, Impact, Confidence, Effort)
  - Identify must-haves vs. nice-to-haves per quarter
  - Highlight trade-offs (if we do X, we can't do Y)
- **Output**: Prioritization justification for each feature

### 3. Stakeholder-Comms (Sequential, from pm-operations)
- **Contributes**: Audience-specific messaging, communication timing
- **Input**: Prioritized roadmap + rationale
- **Process**:
  - Tailor narrative for each audience
  - Exec view: business impact, revenue, risk mitigation
  - Engineering view: technical dependencies, complexity, effort
  - Customer view: value proposition, timeline, problem solved
- **Output**: 3 versions of same roadmap

## Data Handoff

```
Product Vision
    ↓
Roadmap Planning
    ↓ (feature priorities + timing)
Prioritization Frameworks
    ↓ (RICE scores + trade-off analysis)
Stakeholder Comms
    ↓ (audience tailoring)
Final Roadmap
    → Executive summary view
    → Engineering/technical view
    → Customer-facing view
    → Prioritization rationale document
```

## Execution Model
**Sequential** — Prioritization frameworks must run after roadmap planning. Stakeholder comms must see prioritized roadmap to tailor messaging.

**User interaction points**:
1. Review feature list and timing
2. Review RICE scores and trade-offs
3. Approve narrative for each audience
4. Schedule communication for each audience

## Output Format

Each roadmap includes:
- Themed features (e.g., "AI Innovation", "Compliance & Security")
- Timeline (Q2, Q3, Q4 with months)
- Status per feature (In Progress, Planned, Investigating)
- Effort estimate (Small, Medium, Large)
- Brief rationale (why this, why now)

Audience-specific views differ in:
- **Exec**: Revenue impact, competitive positioning, risk
- **Engineering**: Technical complexity, dependencies, effort
- **Customer**: Problem solved, value delivered, timeline

## Estimated Timeline
- Roadmap-Planning: 30-40 minutes
- Prioritization-Frameworks: 20-25 minutes
- Stakeholder-Comms: 15-20 minutes
- **Total**: 65-85 minutes

## Quality Checklist
- [ ] Features grouped into 3-5 clear themes
- [ ] Each feature has RICE score (Reach/Impact/Confidence/Effort)
- [ ] Trade-offs explained (why feature X instead of Y)
- [ ] All features have realistic effort estimates
- [ ] Narrative tailored for exec/engineering/customer audiences
- [ ] Dependencies clearly marked
- [ ] Communication plan includes timing for each audience
