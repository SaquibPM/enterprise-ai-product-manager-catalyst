---
name: Quarterly Planning
description: Transform OKRs into prioritized roadmap with audience-specific communication outputs
workflow_type: sequential
estimated_duration: "3-4 hours across a planning week"
plugin_chain: ["strategy", "prioritization", "roadmap", "communication"]
---

## Overview

The Quarterly Planning workflow takes strategic OKRs (Objectives and Key Results) and transforms them into a prioritized roadmap, then generates audience-specific communication plans for executives, engineering, and customers. This workflow ensures that quarterly plans are grounded in clear outcomes, that priorities are transparent and justified, and that different stakeholder groups receive the information they need in formats they understand.

This workflow runs once per quarter and typically spans a planning week with 3-4 synchronous working sessions.

## Skills Involved

| Skill | Plugin | Purpose |
|-------|--------|---------|
| **okr-management** | strategy | Define and refine quarterly OKRs aligned with company goals |
| **prioritization-frameworks** | prioritization | Apply structured frameworks to evaluate and rank initiatives |
| **roadmap-planning** | roadmap | Build detailed roadmap aligned to OKRs with dependencies and milestones |
| **stakeholder-comms** | communication | Create audience-specific roadmap narratives for exec, engineering, customer audiences |

## Step-by-Step Sequence

### Step 1: OKR Definition & Refinement (Session 1 - Planning Day 1)
**Owner:** PM + Leadership  
**Input:** Company strategy, previous quarter performance, market/customer context  
**Output:** Finalized OKRs (3-5 objectives with 3-4 KRs each), scoring methodology, confidence levels

Run the **okr-management** skill to:
- Articulate team/product OKRs aligned to company direction
- Define what success looks like for each objective (measurable outcomes)
- Assess confidence level for each OKR (high/medium/low)
- Document assumptions underlying each KR
- Identify dependencies with other teams
- Set baseline metrics and targets for measurement

**Structure Example:**
```
Objective: "Make AI features accessible to SMB customers"
  KR 1: 500 new SMB signups using AI beta (from 50 current)
  KR 2: 4.0+ average rating on AI feature UX (from 3.2)
  KR 3: <2hr onboarding time for SMB users (from 4hrs)

Assumptions: SMBs prioritize simplicity over features; 3x faster setup drives adoption
Confidence: High (High/Medium/Low)
```

**User Interaction Point:** Leadership aligns on OKRs and confirms strategic direction. Team discusses trade-offs between ambitious vs. realistic targets. 60 min working session.

**Data Handoff:** OKR document becomes the source of truth for Step 2 prioritization and Step 3 roadmap planning.

---

### Step 2: Initiative Prioritization (Session 1 - Planning Day 1, afternoon)
**Owner:** PM  
**Input:** OKRs from Step 1, backlog of potential initiatives/features  
**Output:** Prioritized initiative list mapped to OKRs, effort estimates, risk assessment

Run the **prioritization-frameworks** skill to:
- Map each potential initiative to which OKRs it supports (or if it's exploratory/tech debt)
- Apply prioritization framework (RICE scoring, impact/effort, or custom) to rank initiatives
- Assess each initiative: estimated effort (dev days), risk level, dependencies, customer impact
- Identify mandatory initiatives (compliance, security, technical debt that blocks other work)
- Create "confidence tier" for initiatives (high confidence we can deliver vs. experimental)
- Document trade-offs explicitly (e.g., "Initiative A scores higher but Initiative B is blocking three others")

**Prioritization Framework Example:**
```
Initiative: "AI-native onboarding flow for SMBs"
  RICE Score: Reach=500 (SMB TAM), Impact=3 (high), Confidence=0.8, Effort=40 days
  OKR Alignment: SMB KR1 (signups), SMB KR3 (onboarding time)
  Risk Level: Medium (new design patterns, requires design + eng sync)
  Dependencies: Design system updates (blocking), UX research (non-blocking)
  Go/No-Go: GO - high confidence, clear ROI
```

**User Interaction Point:** PM reviews prioritization with cross-functional leads (design, eng, data). Team debates trade-offs (30 min sync). Any initiatives with unclear ROI get cut or deferred. Final list gets CFO nod if resource-heavy.

**Data Handoff:** Prioritized initiative list flows to Step 3 (Roadmap Planning).

---

### Step 3: Roadmap Planning (Session 2 - Planning Day 2)
**Owner:** PM + Engineering Lead  
**Input:** Prioritized initiatives from Step 2, team capacity, sprint velocity  
**Output:** Quarterly roadmap with milestone dates, inter-team dependencies, risk mitigation plan

Run the **roadmap-planning** skill to:
- Map initiatives to sprint timeline (Week 1-13 of quarter)
- Identify critical path items and dependencies between teams
- Build in buffer time for unknowns (typically 15-20% of capacity)
- Define key milestones (beta launch Week 5, GA Week 10, etc.)
- Flag initiatives that could slip and escalation plan
- Create swimlanes for different teams (Engineering, Design, Data Science, Marketing, Ops)
- Document go/no-go decision criteria for initiatives that might be cut

**Roadmap Structure Example:**
```
Week 1-3: Design SMB onboarding (Design team), begin AI eval framework work
Week 4-5: Dev SMB onboarding (Eng sprint), alpha testing, design starts migration flow
Week 6-8: Beta launch to 100 SMBs, iteration based on usage data
Week 9-10: GA launch, full documentation, begin Q3 planning
Week 11-13: Optimization sprints, feature backlog refinement, team retrospective

Key Milestones:
  - Beta Launch (Week 5): SMB flow deployed to test segment
  - GA (Week 10): Full customer availability, marketing campaign live
  - Completion Criteria: All OKRs scored, data collection in place

Risk Mitigation:
  - If AI eval slips (Week 8 decision point): defer advanced customization to Q2
  - If design takes longer (flag Week 3): use simplified UX and iterate post-launch
```

**User Interaction Point:** Engineering lead confirms capacity and surfaces any feasibility concerns. Design confirms design sprints align with dev sprints. Data team commits to metrics tracking plan. 45 min planning session. If dependencies create bottlenecks, PM negotiates scope trade-offs.

**Data Handoff:** Final roadmap document feeds into Step 4 (Stakeholder Communications). All audience-specific roadmaps reference the same underlying priority data but customize messaging.

---

### Step 4: Stakeholder Communication (Session 2 - Planning Day 2, evening + Session 3 - Day 3)
**Owner:** PM + Marketing + Leadership  
**Input:** Roadmap from Step 3, company narrative, customer/market context  
**Output:** Three audience-specific roadmaps + launch communication plan

Run the **stakeholder-comms** skill to create three distinct communications:

#### Output 1: Executive Roadmap
**Audience:** C-suite, Board, Finance  
**Format:** 1-page slide + 2-3 page narrative  
**Contents:**
- How quarterly plan connects to annual strategy and OKRs
- Resource allocation and budget impact
- Top 3 business outcomes expected
- Key risks and mitigation strategy
- Competitive positioning tie-ins

**Example Narrative:**
"Q2 roadmap drives $2M ARR expansion through SMB segment (OKR: 500 new signups). Three-pronged approach: (1) simplified onboarding reduces setup friction, (2) AI-native UX differentiates vs. Competitor X, (3) managed migration service captures revenue upside. Risk: design complexity may push beta 1 week—have simplified fallback. Expected impact: 40% of new SMB cohort converts to paid tier by end of quarter."

#### Output 2: Engineering Roadmap
**Audience:** Engineering leaders, individual contributors  
**Format:** Detailed roadmap doc + sprint-by-sprint breakdown  
**Contents:**
- Feature-by-feature breakdown with technical requirements
- Sprint capacity allocation and buffer time
- Technical spike stories and dependency mapping
- Performance, security, and scalability requirements
- Testing and QA strategy per initiative
- On-call rotation impact for major launches

**Example Section:**
"SMB Onboarding Initiative (Weeks 1-8, 40 dev days)
  Sprint 1: Architecture + data model design
  Sprint 2: Core flow implementation
  Sprint 3: Integration with existing auth
  Sprint 4: Beta stability pass + monitoring
  Technical Debt: Refactor analytics to support new user journey (3 days, Sprint 3)
  Testing: Automated E2E for happy path + 3 edge cases; manual SMB UX testing W5-8"

#### Output 3: Customer Roadmap
**Audience:** Key accounts, advisory board, marketing  
**Format:** Visual roadmap + narrative + FAQ  
**Contents:**
- Customer-facing benefits timeline
- Feature release dates and what customers should expect
- Migration/upgrade path for existing customers
- Availability windows and support transition plan
- Feedback mechanisms (beta programs, surveys)
- What's NOT in this quarter (managing expectations)

**Example:**
"SMB customers: Simple setup experience launches in beta Week 5 (invite-only, 100 slots). GA Week 10 for all customers. Current Enterprise customers: Existing setup flow continues working unchanged; new simplified flow available as opt-in Q3. Migration tools available W10+ for customers who want to switch."

**User Interaction Point:** Leadership reviews exec narrative and approves key messaging. Engineering lead confirms technical roadmap accuracy. Marketing drafts customer FAQ and gets PM approval. 60 min cross-functional review.

**Data Handoff:** All three roadmaps reference same underlying plan (one source of truth) but customize audience, level of detail, and key messages. Create "Roadmap Master Doc" that maps exec messages → eng tasks → customer features.

---

## Data Handoff

| From Step | To Step | Data | Format | Reference |
|-----------|---------|------|--------|-----------|
| 1 | 2 | OKR definitions with targets | Markdown/doc with KRs and metrics | Initiatives in Step 2 map to OKRs |
| 2 | 3 | Prioritized initiative list with effort | Spreadsheet or ranked list with RICE scores | Roadmap respects priority order |
| 3 | 4 | Detailed roadmap with dates/milestones | Gantt or timeline visual + doc | All three audience roadmaps reference same dates |
| 1 + 3 | 4 | OKRs + roadmap together | Linked documents | Exec roadmap ties roadmap → OKRs → outcomes |

**Maintaining Coherence:** Designate one PM as "Roadmap Owner" who maintains master doc that all three audience versions reference. Updates to one version get reflected in master, then flow to other versions.

---

## Parallelization Opportunities

### Sequential Critical Path:
```
Step 1 (OKRs) ──must complete──> Step 2 (Prioritization)
                                      ↓
                            Step 3 (Roadmap Planning)
                                      ↓
                            Step 4 (Communications)
```

**Why Sequential:** Each step builds on outputs of previous. Prioritization can't happen without OKRs. Roadmap can't be built without priority ranking.

### Parallel Prep (Before Step 1):
- Gather customer feedback, market data, and competitive intelligence
- Collect previous quarter retrospective data
- Pre-brief with CFO on budget constraints
- These run in parallel with Step 1

### Parallel Execution (Within Step 4):
- Three audience roadmaps can be drafted simultaneously once master roadmap is done
- Marketing can start FAQ drafting while exec narrative is being finalized
- Each audience stream can iterate independently

### Example Timeline:
```
Planning Day 0 (Fri):  [Prep: customer data, retro, CFO brief] ────────
Planning Day 1 (Mon):  [Step 1: OKR definition] ────> [Step 2: Prioritization]
Planning Day 2 (Tue):  [Step 3: Roadmap Planning] ─────────────────────
Planning Day 3 (Wed):  [Step 4a: Exec Comms] [Step 4b: Eng Roadmap] [Step 4c: Customer Comms]
Launch (Thu):          [Review + publish] ────────────────────────────
```

---

## User Interaction Points

1. **OKR Alignment (end Step 1):** Leadership confirms OKRs match company strategy. Any misalignment gets corrected before prioritization. 60 min working session.

2. **Priority Debate (end Step 2):** Cross-functional leads debate top initiatives and trade-offs. PM makes final calls on conflicts with CFO input. 30 min sync.

3. **Roadmap Feasibility (end Step 3):** Engineering lead confirms timeline is achievable with available capacity. Design and other teams surface dependencies. Scope gets negotiated if needed. 45 min working session.

4. **Communication Review (end Step 4):** Leadership reviews exec narrative. Marketing confirms customer messaging. Engineering confirms technical accuracy. 60 min final review across three audiences.

5. **Launch Readiness (end Step 4):** PM confirms all stakeholders have roadmaps, key dates are socialized, and communication plan is locked. Publish roadmaps to respective audiences.

**Total PM Time Investment:** ~4-5 hours synchronous + 6-8 hours asynchronous (outlining, reviews, refinements).

---

## Estimated Timeline

| Phase | Duration | Notes |
|-------|----------|-------|
| Prep (customer feedback, data gathering) | 2-3 days | Can start before official planning week |
| OKR Definition (Step 1) | 1 day | Typically Monday of planning week |
| Prioritization (Step 2) | 0.5 days | Same day as Step 1, afternoon |
| Roadmap Planning (Step 3) | 1 day | Tuesday of planning week |
| Communication (Step 4) | 1.5-2 days | Wed + Thu; customer/exec comms can publish Thu PM |
| **Total Elapsed Time** | **3-4 days** | Planning week + prep |
| **Actual PM Hours** | **10-12 hours** | Includes reviews, refinements, stakeholder syncs |

---

## Final Output Summary

By the end of this workflow, you have:

1. **Locked OKRs**
   - 3-5 strategic objectives with measurable key results
   - Baseline metrics and success definitions
   - Risk assessment and assumptions documented
   - Buy-in from leadership

2. **Prioritized Initiative Roadmap**
   - Ranked list of 8-12 initiatives mapped to OKRs
   - Effort estimates, risk levels, and confidence scoring
   - Dependency map showing inter-team blockers
   - Go/no-go criteria and trade-off decisions documented

3. **Quarter-Long Roadmap**
   - Sprint-by-sprint breakdown with key milestones
   - Swimlanes showing cross-team dependencies
   - Buffer time allocated (typically 15-20% of capacity)
   - Risk mitigation plan with escalation triggers
   - Engineering, Design, and Data commitments signed

4. **Three Audience-Specific Roadmaps**
   - **Exec version:** 1-page strategic narrative + board-ready summary
   - **Eng version:** Detailed technical roadmap with sprint allocation and technical requirements
   - **Customer version:** Feature timeline + narrative + FAQ with clear expectations

5. **Launch Communication Plan**
   - All-hands talking points tying roadmap to strategy
   - Customer email/comms announcing new capabilities
   - Sales enablement with messaging and timeline
   - Marketing campaign calendar aligned to feature release dates

**Handoff Status:** Organization is now aligned on quarterly direction. Engineering can begin sprint planning. Marketing can schedule campaigns. Sales can plan customer conversations. Each team operates from the same understanding of priorities and dates.

---

## When to Use This Workflow

✓ **Quarterly planning cycles** (every 3 months)  
✓ **When strategy needs to translate to execution** and you need multi-audience communication  
✓ **Cross-functional alignment** required across eng, design, marketing, ops  
✓ **High-stakes quarters** where OKR misalignment creates expensive rework  
✓ **Scaling organizations** needing clear roadmap transparency  
✓ **When you have 3-4 days** available for concentrated planning effort  

---

## When NOT to Use This Workflow

✗ **Mid-quarter replanning** due to urgent market changes → Use lightweight "Quarterly Replan" (Steps 2-3 only)  
✗ **Startup early stage** with weekly pivots → Use rolling OKRs (continuous update, less formal planning)  
✗ **Team with <5 people** → Compress to single all-hands planning session (1 day)  
✗ **Purely engineering-focused planning** → Skip stakeholder comms, use Roadmap skill directly  
✗ **When strategy is unclear** → Do Strategy Clarification first, then come back to OKR definition  

**Alternative:** For faster planning, use "Lightweight Quarterly" workflow (OKRs + Prioritization only, skip detailed roadmap planning).
