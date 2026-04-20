---
title: "OKR Management for Product Organizations"
description: "Design and execute Objectives and Key Results that drive product strategy, align cross-functional teams, and measure what truly matters in enterprise product development."
author: "Zycus AI PM"
category: "PM Leadership"
level: "Senior/Staff"
estimated_reading_time: "16 minutes"
---

## PURPOSE

OKRs are not task lists, and they're not stretch goals disguised as accountability. In enterprise product organizations, poorly designed OKRs create sandbagging (teams always hit 1.0 because targets are set too low), misalignment (product builds features finance doesn't care about), and theater (impressive-sounding OKRs that don't move business metrics). This skill teaches you to write OKRs that connect strategy to execution, cascade across organizations, score honestly, and drive real product outcomes—not just shipping dates.

---

## KEY CONCEPTS

### 1. Objectives vs. Key Results (The Architecture)

**Objective**: Qualitative, inspiring, directional. Answers "What do we want to accomplish?" Should make people excited to work toward it.
- Good: "Make contract intelligence trusted for compliance decisions"
- Bad: "Improve contract risk detection accuracy" (too mechanical, not inspiring)

**Key Result**: Quantitative, time-bound, measurable. Answers "How will we know if we achieved the Objective?" Typically 2-4 per Objective.
- Good: "95%+ contract risk accuracy on 500 contracts; 10 customer validation sessions confirming risk scores match legal opinion; zero false negatives on critical liability clauses"
- Bad: "Launch contract intelligence feature" (that's a task, not a Key Result; doesn't measure outcome)

**The Golden Rule**: An Objective without Key Results is a wish. A Key Result without an Objective is a metric. Together, they explain *why* a metric matters.

**Enterprise-specific nuance**: Enterprise outcomes often span multiple teams. "Procurement AI adoption" isn't just a product metric; it depends on sales enablement, customer success, implementation. Your OKRs should reflect this cross-functional reality.

### 2. OKR Writing Framework

**Write Objectives as inspiring, qualitative statements:**
- Use aspirational language ("transform," "revolutionize," "own," "dominate," "establish")
- Make them meaningful to your team ("Make procurement teams love us" > "Ship contract intelligence v1.0")
- Keep them to one sentence (if you need 50 words, it's not clear enough)
- Avoid metrics in the Objective (that's what Key Results are for)

**Write Key Results with precision:**
- Lead with the metric: "Contract risk accuracy to X%"
- Include baseline: "From current 45% to 95%"
- Include timeline: "By end of Q2"
- Make them specific enough that scoring is binary (achieved or not) or on a 0-1.0 scale
- Avoid vanity metrics; measure impact

### 3. The 0-1.0 Scoring System (And Why Target is 0.7, Not 1.0)

**The Scoring Scale:**
- **0.0**: We didn't make progress; the bet was wrong or execution failed
- **0.3**: We made progress but significantly missed; maybe 30% of the way there
- **0.7**: Fully achieved; we hit what we said we'd hit
- **1.0**: Over-achieved; we did more than we set out to do

**Why target 0.7, not 1.0?**

If you consistently hit 1.0, your OKRs are sandbagged (set too low). Real innovation requires risk. If you're always 100% confident you'll hit a number, that number isn't ambitious enough.

**What healthy 0.7 looks like**: Teams stretch, hit 0.7-0.8 on average, occasionally hit 1.0 on well-executed bets, and hit 0.3-0.5 when they're wrong about the market or technology.

**Enterprise-specific nuance**: Enterprise outcomes are harder to control (sales cycles, customer budget constraints, regulatory approval delays). Your Key Results should reflect this; don't penalize teams for external factors.

### 4. OKR Cascading: Company → Product → Team

**Company OKRs** (set by CEO/Board): "Become the leading autonomous procurement platform for mid-market enterprises"

**Product Organization OKRs** (set by Chief Product Officer): Translate company OKRs into product bets
- Example: "Make contract intelligence the table-stakes feature in procurement" (supports company OKR)
- How measured: Customer adoption of risk dashboard; competitive win/loss analysis showing risk as key decision factor

**Team OKRs** (set by individual PMs): Translate product org OKRs into actionable team work
- Example: "Achieve 95%+ contract risk accuracy and prove it through 10 customer validation sessions" (supports product org OKR)
- How measured: ML model performance on test set; customer feedback on accuracy; legal review validation

**Critical rule for cascading**: Each lower-level OKR should clearly support at least one higher-level OKR. If a team OKR doesn't ladder up, it shouldn't exist (or it's an initiative, not an OKR).

**Dependency mapping**: Enterprise product organizations have massive cross-functional dependencies. Your OKRs should explicitly call out "This OKR depends on Engineering completing ML model by [date]" or "Sales closing 5 customers by [date]."

### 5. Measuring Platform Outcomes, Not Just Feature Output

**Anti-pattern**: "Deliver contract intelligence feature; launch Workday integration"
- This measures shipping, not impact

**Better**: "Achieve 95%+ contract risk accuracy; 10 customers validating decision impact; 3 of them stating ROI of $100K+ in year 1"
- This measures whether the feature actually solves the problem

**For enterprise products especially**: You need three layers of measurement:

1. **Product Adoption**: Are customers using it? (Daily active users in risk dashboard; % of contracts processed through AI)
2. **Customer Impact**: Does it solve their problem? (Time to contract reduction; risk catch rate vs. legal review; customer willingness to expand usage)
3. **Business Impact**: Does it drive revenue? (NPS lift; expansion revenue; win/loss attributed to risk feature; customer retention)

### 6. OKR Cascading Pattern for Enterprise Multi-Product Lines

When you have multiple products (Contract Intelligence, Supplier Matching, Procurement Automation), you need clear hierarchy:

**Company OKR**: "Establish Autonomous Procurement as leading enterprise platform"

**Product Line 1 OKR** (Contract Intelligence): "Make contract risk assessment table-stakes, achieve 40% adoption in customer base"

**Product Line 2 OKR** (Supplier Matching): "Launch supplier intelligence; achieve 15% adoption among customers with 5+ suppliers"

**Product Line 3 OKR** (Automation): "Enable 50% of customers to automate routine PO approvals"

**Cross-functional OKR**: "Achieve 4.5+ NPS average across all products; drive 20% net revenue retention from expansion"

The cross-functional OKR ensures product lines don't optimize locally at expense of overall customer experience.

### 7. OKR vs. Initiative (What Goes Where)

**OKRs answer "What outcome do we want?"**
- "Achieve 95% contract risk accuracy"
- "Prove procurement teams save 10+ hours/week"
- "Establish ourselves as supplier intelligence leaders"

**Initiatives answer "How will we achieve the outcome?"**
- "Build v1 contract risk ML model"
- "Launch Workday integration"
- "Partner with 5 industry associations for supplier data"

Initiatives are the work. OKRs are the outcome you're trying to move. When writing OKRs, avoid listing initiatives as if they're outcomes.

---

## APPLICATION

### Step 1: Strategic Input (Align to Vision and Strategy)

Before writing OKRs, you need:

1. **Strategy clarity**: What are this quarter's strategic bets? (From your vision/strategy document)
2. **Company OKRs**: What is the company trying to accomplish?
3. **Customer/market input**: What problems matter most to customers right now?
4. **Resource constraints**: What can we realistically staff this quarter?

For enterprise products: You also need to understand enterprise sales cycles. If your largest deals close in March, Q1 OKRs should support closing them. If a key customer has contract renewal in Q2, your product OKRs should support their renewal conversations.

### Step 2: Write 1-3 Company-Level OKRs

If you're a product organization, coordinate with CEO/executive team:

**Example for Autonomous Procurement Platform (Company Level):**

OKR 1: Establish market leadership in AI-powered procurement
- Key Result 1: Deploy with 20 enterprise customers (each $150K+ ACV)
- Key Result 2: Achieve 60+ NPS average; zero NPS below 50
- Key Result 3: Win 3 direct competitive deals against Coupa/Ariba

OKR 2: Prove procurement automation ROI at enterprise scale
- Key Result 1: 10 customers report $100K+ year-1 savings quantified
- Key Result 2: Average time-to-signature reduced from 21 days to 8 days
- Key Result 3: Contract risk accuracy validated at 95%+ by legal teams

OKR 3: Build platform foundation for ecosystem expansion
- Key Result 1: Ship API and sandbox; attract 3 third-party developers
- Key Result 2: Legal and Finance teams using Contract Intelligence as non-procurement users
- Key Result 3: Successful beta with one ERP vendor (Workday or SAP) on supplier data integration

### Step 3: Write Product Organization OKRs (2-4 total)

These translate company OKRs into product strategy:

**Example for Product Organization (Procurement Platform):**

OKR 1: Make contract intelligence the table-stakes, must-have feature
- Key Result 1: 95%+ contract risk accuracy on 500+ production contracts; zero critical false negatives
- Key Result 2: 10 customer validation sessions confirming accuracy matches legal opinion and shows ROI
- Key Result 3: 65%+ of paying customers actively using risk dashboard weekly

OKR 2: Prove procurement workflow integration value; establish ERP partnership model
- Key Result 1: Ship production Workday integration; live in 3 customer accounts with 8+ week implementations
- Key Result 2: Workday executive sponsor commits to co-selling and API partnership beyond pilot
- Key Result 3: Time-to-value reduced to 4 weeks for Workday-integrated customers

OKR 3: Expand procurement platform beyond contract intelligence into adjacent workflows
- Key Result 1: Supplier onboarding workflow (beta) deployed with 3 customers; 70%+ adoption of beta feature
- Key Result 2: Finance dashboards (beta) accessed by CFO in 2 customers; clear willingness-to-pay signal for finance expansion
- Key Result 3: Legal team usage (contract obligation tracking) piloted with 2 customers; path to separate Legal product clear

OKR 4: Drive net revenue retention and reduce churn through customer success
- Key Result 1: NPS average 60+; net NPS growth of +10 points from Q4
- Key Result 2: Zero enterprise customer churn; 100% renewal rate
- Key Result 3: 20% net revenue retention from expansion into adjacent workflows/new user seats

### Step 4: Cascade to Team OKRs

Now each PM owns one or more product org OKRs and writes team-specific OKRs:

**PM 1 (Owned Product Org OKR 1): Contract Intelligence**

Team OKR 1: Make contract intelligence trusted for compliance decisions
- Key Result 1: Achieve 95%+ accuracy on contract risk detection; validate against 50 customer contracts with legal review
- Key Result 2: Ship refined risk taxonomy with legal inputs; 10 customers using custom risk rules; NPS on intelligence accuracy 70+
- Key Result 3: Zero critical risk misses (false negatives); audit trail of risk decision-making available for compliance review

Team OKR 2: Prove procurement time-to-signature impact; drive adoption to 65% of active customers
- Key Result 1: 8 customers measured reduction in contract cycle time; average 21 days → 8 days
- Key Result 2: Daily active users in risk dashboard increase from 35% to 65% of paying customers
- Key Result 3: 3 expansion conversations with customers crediting risk feature as driver; 2 expand to enterprise deal size

**PM 2 (Owned Product Org OKR 2): Platform and Integration**

Team OKR 1: Deliver production Workday integration; establish template for future ERP partnerships
- Key Result 1: Workday integration live in production; 3 customers deployed and using (writing risk summaries back to Workday)
- Key Result 2: 8-week implementation cycle achieved; customers at 2-week time-to-first-insight
- Key Result 3: Workday exec sponsor secured for co-selling pilot; API partnership framework documented

Team OKR 2: Reduce friction in customer onboarding; improve time-to-value to 4 weeks for integrated customers
- Key Result 1: Workday-integrated customers average 4-week time-to-value; non-integrated customers 8 weeks
- Key Result 2: Onboarding playbook refined based on 3 customer implementations; playbook adopted by Customer Success
- Key Result 3: Customer satisfaction with onboarding increases to 8/10; zero onboarding-related churn

**PM 3 (Owned Product Org OKR 3): Expansion Workflows**

Team OKR 1: Validate supplier onboarding workflow demand and build beta
- Key Result 1: Beta shipped and deployed with 3 customers; 70%+ feature adoption in beta group
- Key Result 2: Customer feedback confirms top 3 use cases for supplier onboarding; ROI story clear
- Key Result 3: 3 customers express intent to expand to supplier onboarding in production; committed budget identified

Team OKR 2: Establish finance dashboard as path to Finance buyer expansion
- Key Result 1: Finance dashboard beta deployed with CFOs in 2 largest customers; 2 weeks of active usage
- Key Result 2: CFOs confirm spend analytics and risk aggregation solve their top 3 procurement reporting pains
- Key Result 3: 2 CFOs sponsor expansion budget conversations; finance expansion product vision documented and approved by leadership

---

## WORKED EXAMPLE: Complete Q2 OKR Set for Enterprise Procurement Product Team

### Q2 OKR Framework (April-June 2025)

**Context**: We're 6 months into product launch. We have 12 customers, $1.5M ARR. Risk intelligence is working well (92% accuracy); Workday integration is 4 weeks away. Q2 priorities: finalize Workday, prove ROI quantitatively, expand into legal workflow, and drive adoption.

---

### COMPANY OKR (Set by CEO and Board)

**Company OKR: Achieve profitability trajectory while maintaining 25%+ growth**
- Key Result 1: Hit $3M ARR (100% growth from Q1)
- Key Result 2: Achieve 65+ NPS; zero NPS below 50
- Key Result 3: Hit 75% gross margin; path to positive unit economics clear

*Why this matters*: Board wants to see sustainable growth, not burn. $3M ARR and 65+ NPS proves product-market fit; 75% gross margin shows unit economics work.

---

### PRODUCT ORGANIZATION OKRs (Set by Chief Product Officer)

**Product Org OKR 1: Make contract intelligence table-stakes and establish defensible differentiation**

Objective: "Contract intelligence becomes the must-have feature enterprise procurement teams won't change"

Key Results:
- KR1: Achieve 95%+ accuracy on contract risk assessment; validate against 100 production contracts reviewed by legal teams; zero false negatives on critical liability clauses
- KR2: 65%+ of active customers using risk dashboard weekly; daily active user growth of 30% QoQ
- KR3: Win 2 competitive deals directly attributing contract intelligence accuracy as decision factor vs. Coupa/Ariba

*Rationale*: Contract intelligence is our defensible moat. LLMs are commoditizing; accuracy and legal validation aren't. If we own this, we own procurement.

*Scoring rubric*: 
- 0.7 = Hit 94% accuracy, validated by 75 contracts; 60% DAU adoption; 1 competitive win attributed
- 1.0 = Hit 95%+ accuracy, 100+ validation contracts; 70%+ DAU; 2-3 competitive wins attributed
- 0.3 = Hit 90-92% accuracy; 40-50% DAU; 0 competitive wins

---

**Product Org OKR 2: Establish ERP partnership model; prove integrated workflow value**

Objective: "ERP integration becomes the enterprise table-stakes, accelerating time-to-value and strategic partnership revenue"

Key Results:
- KR1: Workday integration live in production by end of Q2; deployed in 3 customer accounts with full implementations
- KR2: Workday executive sponsor commits to co-selling pilot and API partnership beyond Q2 pilot
- KR3: Time-to-value reduced to 4 weeks for Workday-integrated customers vs. 8 weeks for non-integrated; prove 50% reduction in implementation cost

*Rationale*: Enterprise SaaS without ERP integration dies a slow death. If Workday succeeds, we've proven the model and have partnership revenue stream.

*Scoring rubric*:
- 0.7 = Workday integration production-ready; 2 customers deployed; sponsor commits to partnership; 5-week time-to-value for integrated customers
- 1.0 = Workday integration production, 3 customers deployed, strong sponsor commitment, 4-week time-to-value
- 0.3 = Workday integration incomplete; 1 customer deployed; no clear sponsor commitment; 7+ week time-to-value

---

**Product Org OKR 3: Expand into legal and finance workflows; establish path to platform**

Objective: "Prove procurement platform extensibility; unlock adjacent buyer personas and revenue expansion"

Key Results:
- KR1: Supplier onboarding workflow beta deployed with 3 procurement customers; 70%+ adoption of beta feature; clear ROI story articulated
- KR2: Finance dashboard (spend analytics + risk aggregation) beta deployed with 2 CFOs; confirm top 3 customer pain points solved; willingness-to-pay signal for expansion
- KR3: Legal workflow (contract obligation tracking) scoped with 1 customer; detailed requirements and 3-month build estimate documented; legal expansion product charter approved

*Rationale*: Our land product is procurement. But platform value is across procurement, legal, finance. Q2 is about proving extensibility and locking in early adopters of new workflows.

*Scoring rubric*:
- 0.7 = Supplier onboarding beta deployed, 60% adoption, 2 workflows piloted; finance dashboard in 1 CFO; legal requirements gathered
- 1.0 = Supplier onboarding 70%+ adoption, 3 customers; finance dashboard in 2 CFOs with strong feedback; legal charter approved
- 0.3 = Supplier onboarding launched but <50% adoption; finance dashboard not deployed; legal scoping incomplete

---

**Product Org OKR 4: Drive customer success and Net Revenue Retention to fund growth**

Objective: "Ensure customer success and expansion velocity funds next phase of product development"

Key Results:
- KR1: NPS average 60+; 50%+ of customers 70+ NPS; zero NPS below 40
- KR2: 100% enterprise customer renewal; zero churn from top 12 accounts
- KR3: 20% net revenue retention from expansion (new workflows, additional user seats, larger deal sizes); 4 expansion deals >$50K incremental

*Rationale*: Growth funded by expansion is the highest quality growth. If we can achieve 20% NRR, we're 3.5x-ing revenue off existing base. Funds next phase without massive outside funding.

*Scoring rubric*:
- 0.7 = 58+ NPS, 40% of customers 70+ NPS, one at-risk renewal saved; 18% NRR; 3 expansion deals >$50K
- 1.0 = 60+ NPS, 50%+ at 70+, zero churn, 20%+ NRR, 4 expansion deals
- 0.3 = 55 NPS, 30% at 70+, one customer churned, 12% NRR, 1-2 expansion deals

---

### TEAM OKRs (Set by Individual Product Managers)

**Team 1: Contract Intelligence (PM: Sarah Chen)**

**Team OKR 1: Contract Intelligence - Establish as trusted legal compliance tool**

Objective: "Contract intelligence becomes enterprise legal standard for risk identification"

Key Results:
- KR1: Achieve 95%+ accuracy on contract risk identification; validate against 100 production contracts where legal teams provide ground truth; zero false negatives on 15 critical liability clause types
- KR2: 10 detailed customer validation sessions where legal teams sign off that AI risk assessment matches or exceeds their manual review
- KR3: Refined risk taxonomy (50 rule types) adopted by 8 customers; 50%+ of contracts processed with custom risk rules

*Measurement methodology*:
- Accuracy = (Correct Predictions) / (Total Test Contracts); validate using hand-labeled contracts + customer legal team feedback
- Validation sessions = structured customer interviews; measure confidence, accuracy, likelihood to trust AI for daily decisions
- Custom rules = contract rules configuration audit in product

*Scoring detail*:
- KR1: 95%+ accuracy = 0.7; 96%+= 1.0; 92-94% = 0.5; <92% = 0.3
- KR2: 10 sessions = 0.7; 12+ = 1.0; 6-8 = 0.5; <6 = 0.3
- KR3: 8 customers, 50% adoption = 0.7; 10 customers, 60% = 1.0; 5 customers, 30% = 0.5

**Target Score**: 0.7 (3/3 KRs at 0.7 = 0.7 overall)

---

**Team OKR 2: Drive Contract Intelligence Adoption to 65% of Active Customers**

Objective: "Make contract intelligence the default workflow for all procurement teams"

Key Results:
- KR1: Daily active users in risk dashboard grow from 35% of paying customers (Q1) to 65% (Q2)
- KR2: 8 customers document quantified impact (time-to-signature reduction); average 21 days → 8 days
- KR3: 3 expansion conversations driven by contract intelligence ROI; 2 customers expand to enterprise deal size ($250K+)

*Measurement methodology*:
- DAU = % of customer organizations with at least one user accessing risk dashboard in any given week; measured via analytics
- Impact documentation = customer case study format; document baseline, post-implementation, time savings, cost savings
- Expansion conversations = sales pipeline attributed; measure incremental ACV from deals sourcing contract intelligence as key driver

*Scoring detail*:
- KR1: 65% = 0.7; 70%+ = 1.0; 55-60% = 0.5; <50% = 0.3
- KR2: 8 customers, avg 8 days = 0.7; 10 customers, avg 6 days = 1.0; 4 customers, avg 10 days = 0.5
- KR3: 3 conversations, 2 expansions = 0.7; 4+ conversations, 3+ expansions = 1.0; 2 conversations, 1 expansion = 0.5

**Target Score**: 0.7

---

**Team 2: Platform & Integration (PM: Marcus Rodriguez)**

**Team OKR 1: Deliver Production Workday Integration; Establish ERP Partnership Model**

Objective: "Workday integration proves ERP integration model and accelerates time-to-value"

Key Results:
- KR1: Workday integration ships to production by May 15; deployed in 3 customer accounts with full production implementations; zero critical bugs in first month
- KR2: Workday executive sponsor (VP Sales, VP Product, or Chief Architect) commits to API partnership, co-selling pilot, and 2026 product roadmap integration
- KR3: Time-to-value for Workday-integrated customers reaches 4 weeks (vs. 8 weeks baseline); implementation cost reduced by 50%

*Measurement methodology*:
- Production ready = feature-complete, tested, zero critical bugs in 30 days post-launch; deployment logs in 3 customers
- Executive sponsor commitment = signed partnership agreement or MOU; co-selling pilot SLA defined; roadmap integration documented
- Time-to-value = calendar days from contract signature to first customer-confirmed value (risk summaries flowing back to Workday); measure in 3 customer accounts
- Implementation cost = internal hours + CS hours + professional services hours; baseline from non-integrated customers

*Scoring detail*:
- KR1: Production + 3 deployments + 0 bugs = 0.7; 1.0 if all above plus >95% uptime; 0.5 if production + 2 deployments; 0.3 if not production-ready
- KR2: Sponsor commitment + partnership agreement = 0.7; 1.0 if above + co-selling SLA signed; 0.5 if verbal commitment only; 0.3 if no commitment
- KR3: 4 weeks, 50% cost reduction = 0.7; 3.5 weeks, 55% reduction = 1.0; 5 weeks, 40% reduction = 0.5; 6+ weeks = 0.3

**Target Score**: 0.7

---

**Team OKR 2: Accelerate Onboarding; Reduce Time-to-First-Insight to 2 Weeks**

Objective: "Fast implementation and early wins drive customer confidence and expansion velocity"

Key Results:
- KR1: Onboarding playbook refined from 3 Workday implementations; documented for CS team; non-integrated customers hit 4-week mark with documented improvements
- KR2: Customer satisfaction with onboarding experience increases to 8/10; net onboarding NPS 50+
- KR3: Time-to-first-insight (customer sees first meaningful risk summary in dashboard) drops to 2 weeks for all new customers

*Measurement methodology*:
- Playbook refinement = CS operational review; document each improvement; audit onboarding efficiency gains
- Satisfaction = post-implementation survey; 1-10 scale on "implementation was smooth and professional"
- Time-to-first-insight = calendar days from contract signature to date customer sees their first risk summary in dashboard; measure in 3 implementations
- Onboarding NPS = "How likely are you to recommend our implementation team to peers?"

*Scoring detail*:
- KR1: Documented playbook + improvements + 4-week target hit = 0.7; playbook, improvements, 3.5-week hit = 1.0; partial documentation, 5-week = 0.5
- KR2: 8/10 satisfaction, 50+ NPS = 0.7; 8.5+, 60+ = 1.0; 7.5, 40+ = 0.5
- KR3: 2 weeks = 0.7; 1.5 weeks = 1.0; 3 weeks = 0.5; 4+ weeks = 0.3

**Target Score**: 0.7

---

**Team 3: Expansion Workflows (PM: Aisha Patel)**

**Team OKR 1: Validate and Ship Supplier Onboarding Workflow Beta**

Objective: "Prove market demand for supplier onboarding; establish second major workflow pillar"

Key Results:
- KR1: Supplier onboarding workflow beta shipped and deployed with 3 customers; 70%+ adoption of beta feature (measured by active users / total users in account)
- KR2: 10+ customer feedback sessions validate top 3 use cases; ROI story quantified (time saved, supplier compliance, data quality improvement)
- KR3: 3 customers formally commit to expanding to supplier onboarding in production (signed expansion orders or signed LOIs for H2 production release)

*Measurement methodology*:
- Beta deployment = 3 customer accounts with feature flipped on; adoption = weekly active users in supplier onboarding / total users in that account
- Feedback sessions = structured customer interviews; document use cases, pain points, willingness-to-pay
- Expansion commitment = signed expansion order or LOI specifying supplier onboarding; minimum $30K incremental commitment each

*Scoring detail*:
- KR1: Beta shipped, 3 customers, 70% adoption = 0.7; 75%+ adoption = 1.0; 2 customers, 60% = 0.5; 1 customer, <50% = 0.3
- KR2: 10+ sessions, 3 use cases validated, ROI quantified = 0.7; 12+ sessions, 4 use cases, detailed ROI = 1.0; 6 sessions, 2 use cases, qualitative = 0.5
- KR3: 3 expansion commitments = 0.7; 4+ commitments = 1.0; 2 commitments = 0.5; 0-1 = 0.3

**Target Score**: 0.7

---

**Team OKR 2: Establish Finance Buyer Expansion Path; Validate CFO Use Cases**

Objective: "Finance becomes second major buyer persona; unlock CFO-driven procurement investment"

Key Results:
- KR1: Finance dashboard (spend analytics + contract risk aggregation) beta deployed with CFOs in 2 largest customers; deployed with 2+ weeks of active usage
- KR2: CFO validation sessions confirm top 3 pain points solved (spend visibility, risk aggregation, vendor health); strong willingness-to-pay signal for finance expansion product
- KR3: Finance expansion product charter approved by leadership; 3-month build roadmap documented; Sales has credible Finance go-to-market story

*Measurement methodology*:
- Beta deployment = 2 CFO users with dashboard access; weekly active usage = log dashboard opens, report generation, configuration changes
- CFO validation = 3+ structured interviews with CFOs; document top use cases, satisfaction, estimated ROI for finance team
- Charter approval = documented product charter, go-to-market, target buyer profile, financial projections; approved by CEO/CPO
- Sales readiness = Sales enablement training completed; Finance value prop documented; 1 Finance pilot customer identified

*Scoring detail*:
- KR1: 2 CFOs, 2+ weeks active usage = 0.7; 2 CFOs, active daily usage = 1.0; 1 CFO, 2 weeks = 0.5; 1 CFO, <2 weeks = 0.3
- KR2: 3 CFO sessions, top 3 pain points, strong WTP = 0.7; 4 sessions, clear ROI, very strong WTP = 1.0; 2 sessions, 2 pain points, moderate WTP = 0.5
- KR3: Charter approved, 3-month roadmap, Sales ready = 0.7; all above + 1 Finance pilot LOI = 1.0; charter drafted, roadmap partial = 0.5

**Target Score**: 0.7

---

### Mid-Quarter Check-In Assessment (May 15, Q2 Midpoint)

**Purpose**: Assess progress toward Q2 OKRs, make course corrections, secure additional resources if needed.

**Template**:

| OKR | Status | Progress | Confidence | Risk | Action |
|-----|--------|----------|------------|------|--------|
| Contract Intelligence: 95% accuracy | On Track | 94% on 60 test contracts | 85% → 0.7 | Legal validation slower than expected | Recruit 1 contract analyst to accelerate validation |
| Contract Intelligence: 65% DAU | At Risk | 48% (growing from 35%) | 65% → 0.5 | Adoption slower in mid-market; need better onboarding | Invest in onboarding playbook; customer success targeted outreach |
| Contract Intelligence: 2 competitive wins | On Track | 1 win confirmed (Coupa); 1 in closing | 90% → 0.7 | None | Continue sales enablement |
| Workday Integration: Production + 3 deployments | On Track | Beta complete; 1 customer in production | 80% → 0.7 | Customer 2/3 implementation pace slower | Allocate 1 additional engineer to customer success support |
| Workday: Executive sponsor commitment | On Track | VP Sales agreed to partnership pilot | 90% → 0.7 | Partnership not yet formalized | Schedule formal partnership kick-off for June |
| Workday: Time-to-value 4 weeks | At Risk | First customer hit 5 weeks | 75% → 0.5 | Implementation dependent on customer; scope creep in customer 1 | Tighten onboarding scope; implement playbook with customers 2/3 |
| Supplier Onboarding: Beta deployment + 70% adoption | On Track | 2 of 3 customers deployed; 65% avg adoption | 75% → 0.7 | 3rd customer delayed; pilot features underused in 1 account | Customer success deep dive with low-adoption account; accelerate 3rd customer |
| Supplier Onboarding: 3 expansion commitments | On Track | 2 customers expressed strong intent | 70% → 0.7 | Expansion orders depend on additional customization scoping | Allocate PM 50% to expansion scoping/sales support in June |
| Finance Dashboard: 2 CFO deployments | At Risk | 1 CFO beta deployed; 2nd delayed | 60% → 0.3 | Sales team not prioritizing; procurement focus overrides | Executive alignment on Finance expansion; Sales incentive adjustment |
| Finance Dashboard: CFO validation sessions | On Track | 2 of 3 sessions completed; strong feedback | 80% → 0.7 | 3rd CFO scheduling delayed | Customer Success to prioritize 3rd CFO interview |

---

**Corrective Actions Decided**:
1. Allocate 1 contract analyst to accelerate legal validation
2. Tighten Workday onboarding scope; implement playbook immediately
3. Executive alignment on Finance expansion path; CPO sponsors 2nd CFO beta
4. Increase customer success investment in adoption for mid-market

---

## OKR RETROSPECTIVE TEMPLATE

At the end of Q2 (end of June), conduct retrospective with full team:

**Format**: 60-minute meeting with all PMs, Engineering Lead, Customer Success Manager, Sales Leader

**Template**:

```
# Q2 OKR Retrospective

## Part 1: Score Recap (10 min)
| Team | OKR 1 | OKR 2 | Avg Score |
|------|-------|-------|-----------|
| Sarah (Intelligence) | 0.85 | 0.65 | 0.75 |
| Marcus (Platform) | 0.70 | 0.75 | 0.725 |
| Aisha (Expansion) | 0.75 | 0.50 | 0.625 |
| **Org Average** | | | **0.70** |

## Part 2: What We Got Right (10 min per team)
- What strategies/bets paid off?
- What did we learn?
- What should we repeat?

## Part 3: What We Got Wrong (10 min per team)
- What bets were wrong?
- Why did we miss?
- What should we change?

## Part 4: Insights for Q3 (20 min)
- Did we learn anything about market, customer, or product?
- How should Q3 strategy change based on learnings?
- Are we still confident in 3-year vision?

## Part 5: Q3 OKR Planning (Start of next week)
- Based on Q2 performance and learnings, what should Q3 focus be?
- What did we deprioritize that we should reconsider?
```

---

## COMMON PITFALLS

**1. OKRs That Are Task Lists**
- Problem: "Launch contract intelligence feature; deploy Workday integration; hire 1 ML engineer"
  - These are initiatives, not outcomes
- Fix: Focus on outcomes. What should change in the world if we complete these initiatives?
  - "Achieve 95% contract risk accuracy; deploy integration in 3 customers; achieve 4-week time-to-value"

**2. Sandbagging (Always Hitting 1.0)**
- Problem: Team sets OKRs they're 100% sure to hit. End of quarter: all 1.0
  - This means OKRs are too easy; team isn't stretching
- Fix: Explicitly discuss "What would make this 1.0 vs. 0.7?" during goal-setting
  - 0.7 should be "good execution with some uncertainty"; 1.0 should be "exceeded our expectations"

**3. Too Many OKRs**
- Problem: 7-8 OKRs per team; no focus; team context-switches constantly
- Fix: Ruthless prioritization. Aim for 3 OKRs per team max. Each OKR should be worth 25%+ of team's quarter

**4. No Mid-Quarter Check-In**
- Problem: Set OKRs in January; don't revisit until March 31 review
  - By then, it's too late to course-correct
- Fix: Mid-quarter check-in (day 45-50 of quarter) to assess progress and decide: Continue as-is, course-correct, or pause

**5. No Clear Measurement**
- Problem: "Improve customer satisfaction" as OKR
  - How do you measure? What's the baseline? What's success?
- Fix: Make Key Results precise enough that scoring is unambiguous
  - "NPS from 45 to 55; achieve 8/10 on CSAT survey; zero churn from top 10 customers"

**6. OKRs Disconnected from Strategy**
- Problem: OKRs don't ladder up to vision or strategy
  - Team runs off in random direction; good work on wrong priorities
- Fix: Start with strategy document. Explicitly map each OKR to one strategic bet.
  - "This OKR advances Strategic Bet 1: AI contract intelligence becomes table-stakes"

**7. Ignoring External Factors in Enterprise**
- Problem: Enterprise customer delays contract renewal by 6 months; team marked OKR as "failed"
  - But delay wasn't execution failure; it was customer's budget cycle
- Fix: Design Key Results that account for enterprise realities
  - "Achieve renewal with X customers; sales execution on 5 new customer closes; product delivers [feature] on schedule"
  - Measure what you can control; acknowledge dependencies

---

## REFERENCES & FURTHER READING

- **Measure What Matters** (John Doerr): Definitive OKR guide; enterprise case studies
- **Radical Focus** (Christina Wodtke): Quick, practical OKR framework
- **An Elegant Puzzle** (Will Larson): OKRs in scaling engineering organizations
- **Inspired** (Marty Cagan): Connecting strategy to OKRs to execution
- **The Lean Product Playbook** (Dan Olsen): Product strategy and metrics connection to OKRs
