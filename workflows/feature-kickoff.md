---
name: Feature Kickoff
description: End-to-end workflow for launching a feature from customer discovery through sprint planning
workflow_type: sequential
estimated_duration: "2-3 hours across multiple sessions"
plugin_chain: ["discovery", "specification", "design", "execution"]
---

## Overview

The Feature Kickoff workflow chains discovery, research synthesis, specification development, design review, and sprint planning into a cohesive process. This workflow takes a feature concept from initial customer validation through to a sprint-ready implementation plan. It ensures that by the time engineering begins work, the feature has been validated against customer needs, documented in a complete PRD, reviewed for compliance and design constraints, and acceptance criteria have been collaboratively defined.

This workflow is designed for features that require comprehensive validation and cross-functional alignment before development begins.

## Skills Involved

| Skill | Plugin | Purpose |
|-------|--------|---------|
| **customer-interviews** | discovery | Conduct structured interviews to validate feature assumptions and understand customer context |
| **research-synthesis** | discovery | Synthesize interview data into key themes, insights, and validated assumptions |
| **prd-development** | specification | Create comprehensive PRD that incorporates customer insights and technical requirements |
| **compliance-review** | design | Validate feature against regulatory, security, and data privacy requirements |
| **acceptance-criteria** | execution | Define testable success criteria with product, design, and engineering alignment |
| **sprint-planning** | execution | Create sprint-ready backlog items with capacity planning and dependency mapping |

## Step-by-Step Sequence

### Step 1: Customer Discovery (Session 1 - Day 1-2)
**Owner:** PM with Customer Interviews skill  
**Input:** Feature hypothesis, target user segment, interview guide template  
**Output:** Interview notes (8-12 interviews), raw user feedback, emerging patterns

Run the **customer-interviews** skill to:
- Recruit 8-12 customers from the target segment
- Conduct structured interviews following the hypothesis exploration framework
- Document customer context, workflows, pain points, and feature reactions
- Flag unexpected insights for deeper exploration
- Collect permission for follow-up discussions during synthesis phase

**User Interaction Point:** PM reviews interview summaries, identifies gaps, decides whether additional interviews are needed before synthesis phase. If synthesis reveals significant gaps, loop back to additional targeted interviews.

---

### Step 2: Research Synthesis (Parallel with Step 3)
**Owner:** Research Lead  
**Input:** Interview notes from Step 1  
**Output:** Synthesis document with themes, insights, validation status, confidence scoring

Run the **research-synthesis** skill to:
- Extract and categorize user needs from interview transcripts
- Identify patterns across user segments and use cases
- Assess feature demand and priority signals from customers
- Create hypothesis validation scorecard (strong/moderate/weak validation)
- Document unexpected findings and edge cases that may affect feature design
- Generate executive summary of key insights

**Data Handoff:** Synthesis document flows to Step 4 (Spec Development) and is referenced in compliance review.

**User Interaction Point:** Research Lead presents 2-3 key themes and validation status. PM confirms that insights align with business objectives and identifies any need for follow-up interviews (returns to Step 1 if >30% uncertainty remains).

---

### Step 3: Compliance & Design Review (Parallel with Step 2)
**Owner:** Design/Compliance Lead  
**Input:** Feature hypothesis, target workflow, initial design concept  
**Output:** Compliance assessment, design constraints, risk flags

Run the **compliance-review** skill to:
- Assess regulatory impact (GDPR, CCPA, industry-specific requirements)
- Review data privacy implications and storage requirements
- Identify security requirements and threat model implications
- Flag accessibility requirements and inclusive design constraints
- Document approval gates if feature touches regulated areas
- Create design constraints checklist for specification phase

**Data Handoff:** Compliance findings and design constraints become mandatory sections in the PRD.

**User Interaction Point:** Compliance lead flags any blockers or regulatory timelines. Design lead documents specific UX/visual design constraints. These constraints become PRD requirements.

---

### Step 4: PRD Development (Session 2 - Day 3-4)
**Owner:** PM  
**Input:** Research synthesis (Step 2), compliance assessment (Step 3), customer insights  
**Output:** Complete PRD with customer evidence, design constraints, success metrics

Run the **prd-development** skill to:
- Structure PRD using customer insights as primary evidence for every feature requirement
- Incorporate compliance constraints as mandatory requirements
- Define user workflows with validated customer language and mental models
- Document success metrics and how they connect to customer outcomes
- Create detailed use case scenarios grounded in interview data
- Define feature scope, non-goals, and trade-offs explicitly
- Include rollout strategy considerations

**User Interaction Point:** PM walks through PRD with design/eng representatives (30 min sync). Team surfaces technical concerns or additional design questions. PM refines PRD based on feedback before spec review.

**Data Handoff:** Complete PRD becomes single source of truth. A version is sent to engineering, design, and compliance for final review.

---

### Step 5: Acceptance Criteria Definition (Session 2 - Day 4)
**Owner:** PM + Engineering + Design  
**Input:** Complete PRD, design system, engineering constraints  
**Output:** Detailed acceptance criteria with testable conditions

Run the **acceptance-criteria** skill to:
- Translate each PRD requirement into testable acceptance criteria
- Define edge cases and error states
- Document performance and scalability requirements
- Create definition of done checklist
- Identify dependencies and assumptions
- Flag areas where customer research should inform QA testing

**User Interaction Point:** Engineering lead reviews criteria for feasibility and identifies any gaps in the PRD. This is a critical alignment moment—if engineering cannot make the feature work within acceptance criteria, scope gets refined with PM/Design.

**Data Handoff:** Acceptance criteria document becomes the contract between product and engineering teams.

---

### Step 6: Sprint Planning & Kickoff (Session 3 - Day 5)
**Owner:** Engineering  
**Input:** Acceptance criteria, PRD, design specs, roadmap dependencies  
**Output:** Sprint backlog with epics, stories, and engineering tasks

Run the **sprint-planning** skill to:
- Decompose feature into sprint-ready stories with story points
- Create epic structure with dependency mapping
- Identify technical spike stories needed before feature work
- Allocate engineering capacity across sprints
- Define sprint goals and acceptance metrics
- Set up rollout and testing plan structure for later phases

**User Interaction Point:** PM participates in story refinement to ensure technical decisions don't break requirements. Engineering surface any newly-discovered constraints that need PRD amendments.

---

## Data Handoff

| From Step | To Step | Data | Format | Reference Point |
|-----------|---------|------|--------|-----------------|
| 1 | 2 | Interview notes, raw feedback | Interview transcript + PM summary | Synthesis document links to original insights |
| 2 | 4 | Key themes, validation scorecard, insights | Markdown synthesis doc | PRD incorporates themes as "Customer Evidence" section |
| 3 | 4 | Compliance findings, design constraints | Compliance checklist + design doc | PRD includes "Compliance & Design" mandatory section |
| 4 | 5 | Complete PRD with all requirements | Markdown PRD document | Acceptance criteria map 1:1 to PRD sections |
| 5 | 6 | Acceptance criteria with testability | Criteria checklist with edge cases | Sprint stories reference specific criteria |

**Data Preservation:** Create a feature working document that maintains links between interview evidence → synthesis insights → PRD requirements → acceptance criteria → sprint stories. This enables traceability if questions arise during development.

---

## Parallelization Opportunities

### Parallel Track 1: Customer Insights (Steps 1-2)
- **Duration:** 3-4 days
- Run customer interviews while compliance review begins
- Both paths merge at PRD development

### Parallel Track 2: Compliance & Design (Step 3)
- **Duration:** 2-3 days
- Runs simultaneously with synthesis phase
- Compliance findings are mandatory PRD content

### Merge Point:** Step 4 (PRD Development)
- Both research and compliance outputs feed into PRD
- No waiting—research team finalizes while PM begins PRD outline
- By merge point, PM has all inputs ready

### Example Timeline:
```
Day 1:  [Interviews start] ────────────────────────────
Day 2:  [Interviews end]   [Compliance review starts] ──
Day 3:  [Synthesis] ────── [Design review] ────────────
Day 4:  [PRD + Acceptance Criteria aligned] ──────────
Day 5:  [Sprint Planning] ─────────────────────────────
```

---

## User Interaction Points

1. **After Interviews (end of Step 1):** PM reviews interview summaries to confirm coverage and determine if additional interviews needed. ~15 min decision point.

2. **After Synthesis (end of Step 2):** Research lead presents themes to PM. PM validates insights align with business strategy. ~30 min discussion.

3. **Compliance Handoff (end of Step 3):** Compliance lead and PM discuss regulatory gates and timelines. Any blockers trigger stakeholder escalation. ~20 min meeting.

4. **PRD Review (end of Step 4):** PM leads 30-min walkthrough with design and engineering leads to surface any PRD gaps before acceptance criteria phase.

5. **Criteria Refinement (during Step 5):** Engineering lead provides technical feasibility feedback. PM + Engineering iterate on scope if needed. ~45 min working session.

6. **Sprint Kickoff (end of Step 6):** Team confirms sprint roadmap and commitment. PM available for questions as engineering begins execution.

**Total PM Time Investment:** ~2.5 hours synchronous + ~4-5 hours asynchronous across all steps.

---

## Estimated Timeline

| Phase | Duration | Critical Path |
|-------|----------|----------------|
| Discovery (Step 1) | 3-4 days | Interviews must be completed before synthesis |
| Synthesis + Compliance (Steps 2-3) | 3-4 days | Parallel—no dependency |
| PRD Development (Step 4) | 2-3 days | Waits for synthesis + compliance inputs |
| Acceptance Criteria (Step 5) | 1-2 days | Depends on PRD; often overlaps with Step 4 |
| Sprint Planning (Step 6) | 1 day | Final step, no feedback loops (if PRD complete) |
| **Total Elapsed Time** | **2-3 weeks** | Including review cycles and stakeholder availability |

**Actual Synchronous Time:** 2-3 hours of real-time PM engagement (rest is asynchronous skill execution and async reviews).

---

## Final Output Summary

By the end of this workflow, you have:

1. **Customer-Validated Feature Concept**
   - 8-12 customer interviews documenting demand, use cases, and feedback
   - Synthesis document showing strong validation across user segments
   - Evidence tying every PRD requirement to customer need

2. **Sprint-Ready PRD**
   - Feature vision, goals, and success metrics with customer context
   - Complete user workflows and use cases grounded in research
   - Design constraints and accessibility requirements
   - Compliance requirements and approval gates (if any)
   - Non-goals and explicit scope boundaries

3. **Compliance Clearance**
   - Regulatory impact assessment with approval sign-offs
   - Data privacy review confirming GDPR/CCPA compliance (if required)
   - Security requirements and threat model
   - Design accessibility and inclusive UX constraints

4. **Acceptance Criteria**
   - Testable, measurable success conditions
   - Edge cases and error states documented
   - Performance and scalability requirements
   - Definition of done for engineering team

5. **Sprint Backlog**
   - Epics and stories with story points
   - Dependency map and sprint roadmap
   - Technical spike stories identified
   - Engineering team committed to delivery timeline

**Handoff Status:** Feature is now in the engineering queue and ready for sprint execution. PM transitions to: answer clarification questions, manage scope changes, and prepare for launch phase.

---

## When to Use This Workflow

✓ **New features** with customer impact and cross-functional dependencies  
✓ **Major product changes** affecting existing workflows  
✓ **Regulated features** (financial data, health information, etc.)  
✓ **High-stakes releases** requiring compliance and design sign-off  
✓ **Platform features** that set design/technical precedent  
✓ **When you have time** (2-3 week runway before sprint execution needed)  

---

## When NOT to Use This Workflow

✗ **Urgent hotfixes** requiring immediate engineering response → Use Sprint Planning skill directly  
✗ **Small, low-risk enhancements** → Use lighter Spec workflow (skip synthesis, compliance stages)  
✗ **Mature product** iterations where requirements are fully defined → Start at Step 4 (PRD Development)  
✗ **When you lack customer access** → Skip Steps 1-2, start at Step 3 with internal hypothesis  
✗ **Competitive reactive features** → Use Competitive Response workflow instead (urgent path)  

**Alternative:** If you're in a hurry, use the **Spec Express** workflow (PRD development only) or skip directly to **Sprint Planning** if feature is already defined.
