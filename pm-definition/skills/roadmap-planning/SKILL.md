---
description: "Build enterprise roadmaps that drive quarterly execution. Master Now/Next/Later, time-based quarterly roadmaps, and outcome-based roadmaps. Track customer commitments, contract renewal alignment, and regulatory deadlines. Create audience-specific views: execs see revenue impact, engineers see dependencies, customers see value delivery."
pushy: "Your roadmap is the one promise to every stakeholder: sales, engineering, customers, executives. A weak roadmap creates misaligned teams, missed deadlines, and customer churn. Build it right."
purpose: "Create roadmaps that align teams, communicate strategy, and deliver customer value on time."
---

# Enterprise Roadmap Planning

## Purpose
A roadmap is a communication tool, not a document. It is the contract between:
- Engineering: "Here's what we're building and when"
- Sales: "Here's what to promise customers and when to promise it"
- Customers: "Here's what you're paying for and when you'll get it"
- Executives: "Here's how we hit our revenue and churn targets"

In enterprise, a weak roadmap creates cascading failures:
- Sales promises Feature X for Q2; it slips to Q3. Customer doesn't renew. Churn.
- Engineering commits to Q2; discovers dependency on Platform Refactor that will take 4 months. Chaos.
- Roadmap lacks outcome clarity; team ships features that don't move metrics. Wasted effort.

This skill teaches you to build roadmaps that are realistic, outcome-driven, customer-aware, and aligned with business strategy.

## Key Concepts

### 1. Three Roadmap Formats

Every enterprise uses at least one of these. Pick based on your audience and goals.

#### Format 1: Now/Next/Later (Outcome-Driven)
Best for: Communicating strategy, flexibility, and learning-based planning

**Structure**:
```
NOW (Next 0-3 months):
  Focus: Specific initiatives delivering specific outcomes
  Example: "Deliver AI Risk Scoring to close General Dynamics deal ($500k)"
  Commitment level: 95% (these are committed to customers/strategy)

NEXT (3-6 months):
  Focus: High-priority initiatives with strong customer demand
  Example: "Build multi-currency support for EMEA expansion"
  Commitment level: 70% (good confidence, but not committed)

LATER (6+ months):
  Focus: Strategic bets, exploratory work, platform investments
  Example: "Evaluate custom dashboard builder for mid-market"
  Commitment level: 30% (ideas, not commitments)
```

**Advantages**:
- Communicates that further-out dates are uncertain (lowers expectation-setting risk)
- Forces focus on outcomes, not features
- Flexible; can reprioritize without breaking roadmap narrative

**Disadvantages**:
- Vague on dates; customers may push for specific timelines
- Requires strong product leadership to say "no" to scope creep

#### Format 2: Time-Based Quarterly (Execution-Focused)
Best for: Quarterly planning, committing to dates, tracking execution

**Structure**:
```
Q1 2026 (Jan-Mar):
- AI Risk Scoring (committed; contract requirement)
- Multi-Currency Support (committed; EMEA strategy)
- Supplier Email Notifications (nice-to-have; low effort)
- Total Effort: 15 weeks (out of 13-week quarter with 2-week buffer)

Q2 2026 (Apr-Jun):
- Real-Time Dashboard (committed; retention priority)
- Audit Trail Overhaul (committed; SOC 2 requirement)
- Bulk Upload (committed; ops efficiency)
- Total Effort: 14 weeks

Q3 2026 (Jul-Sep):
- Dashboard Customization (proposed; lower priority)
- Supplier Integration API (proposed; platform investment)
- Total Effort: 12 weeks
```

**Advantages**:
- Clear commitment; dates are explicit
- Easy to track; can measure whether you delivered what you promised
- Sales can confidently tell customers "This ships in Q2"

**Disadvantages**:
- Quarters slip; committing to dates creates broken promises
- Rigid; hard to reprioritize mid-quarter without breaking roadmap

#### Format 3: Outcome-Based (Strategic Alignment)
Best for: Executive communication, connecting features to business goals

**Structure**:
```
Strategic Goal 1: Increase retention from 88% to 90% (reduce churn by 2%)
  Outcome-focused roadmap:
  - Real-Time Dashboard (improves user experience; estimated NPS +2 points)
  - Bulk Upload (reduces ops time; improves workflow satisfaction)
  - Audit Trail (reduces compliance risk; improves buyer confidence)
  - Expected impact: Reduce churn from 12% to 10%

Strategic Goal 2: Expand into EMEA market (add $2M ARR from new EMEA customers)
  Outcome-focused roadmap:
  - Multi-Currency Support (enable EUR, GBP, JPY contracts)
  - GDPR Data Residency (store EU customer data in EU)
  - Localized workflows (adapt for EU procurement regulations)
  - Expected impact: Attract 10-15 new EMEA customers; $2M+ ARR

Strategic Goal 3: Build AI differentiation (increase upsell from risk scoring by 30%)
  Outcome-focused roadmap:
  - AI Risk Scoring v2 (custom risk models per industry)
  - Supplier Performance Scoring (auto-calculate supplier metrics)
  - Predictive Spend Analysis (forecast contract spend)
  - Expected impact: Increase upsell attachment rate from 20% to 26%
```

**Advantages**:
- Clearly ties features to business outcomes
- Executives understand why features matter
- Aligns team around metrics, not features

**Disadvantages**:
- Requires discipline to track outcomes; easy to slip into feature-centric thinking
- Outcomes are harder to estimate than features

### 2. Roadmap Components

Every enterprise roadmap must include:

#### 2a. Committed Items
Features promised to customers in contracts, required for regulatory compliance, or critical to business metrics.

Example:
```
Feature: AI Risk Scoring
Commitment type: Customer contract (General Dynamics, $500k deal)
Target date: Q1 2026 (Jan-Mar)
Commitment level: 100% (non-negotiable; contract is signed)
Risk: Execution risk only (product/engineering risk is low)
Contingency: If it slips, we invoke contract penalty or extend contract
```

#### 2b. High-Priority Items
Features with strong customer demand, clear ROI, and alignment with strategy.

Example:
```
Feature: Multi-Currency Support
Commitment type: Strategic priority (EMEA expansion)
Target date: Q1 2026
Commitment level: 90% (high confidence; lower risk than contract items)
Risk: Scope creep if we over-optimize for edge cases
Contingency: Cut nice-to-have currencies if we slip (ship core EUR/GBP first)
```

#### 2c. Speculative Items
Features with uncertain demand, exploratory work, or platform investments.

Example:
```
Feature: Dashboard Customization
Commitment type: Speculative (low customer demand; unproven feature)
Target date: Q2-Q3 2026 (uncertain)
Commitment level: 40% (low confidence; may defer indefinitely)
Risk: May not deliver value; customers may not adopt
Contingency: Pilot with 2-3 customers first; measure adoption before building for all
```

#### 2c. Dependencies
Platform work, infra work, or other features that must ship before this feature can ship.

Example:
```
Feature: Dashboard Customization
Dependencies:
  - Supplier API v2 (must ship first; enables dashboard data fetching)
  - Design system v2 (must ship first; provides UI components for customization)
  - Admin auth role update (must ship first; enables role-based dashboard permissions)

If any dependency slips, this feature is blocked.
Plan accordingly: Don't commit to dashboard customization if dependencies are speculative.
```

#### 2d. Customer Commitments
Features that specific customers are waiting for; often tied to renewal dates.

Example:
```
Feature: AI Risk Scoring
Customers waiting for this:
  - General Dynamics (contract requires this; renewal in Q2 2026)
  - Boeing (internally considering; renewal in Q3 2026)
  - Lockheed (considering competitor; contract up in Q2 2026)
  
Implication:
  - If AI Risk Scoring slips past Q1, General Dynamics renewal is at risk ($500k)
  - If it slips past Q2, Boeing and Lockheed are at extreme churn risk
  - Priority: Must ship in Q1 to hit General Dynamics renewal
```

#### 2e. Regulatory/Compliance Deadlines
Features required by regulation; have hard deadlines.

Example:
```
Feature: GDPR Data Residency
Regulatory requirement: GDPR Article 32 (data must be stored in region of origin)
Deadline: Q2 2026 (when we launch in EU)
Commitment level: 100% (non-negotiable; legal requirement)
If missed: Can't sign EU customers; potential GDPR fines
```

### 3. Audience-Specific Roadmap Views

Don't have one roadmap for everyone. Tailor the view to the audience.

#### Executive View
Executives care about:
- Revenue impact
- Churn reduction
- Strategy alignment
- Delivery timeline

Example:
```
EXECUTIVE ROADMAP: Q1-Q3 2026

Q1 2026:
- AI Risk Scoring: Expected revenue impact = +$500k (General Dynamics contract)
- Multi-Currency: Expected revenue impact = +$100k (EMEA customers)
- Total expected new ARR: +$600k

Q2 2026:
- Real-Time Dashboard: Expected NPS impact = +2 points; churn reduction = 1%
- Audit Trail: Expected compliance improvement; enables healthcare market expansion
- Bulk Upload: Ops efficiency; improves customer satisfaction

Q3 2026:
- Supplier Scoring: Expected upsell = +30% on AI features
- Custom Dashboards: Potential new market (mid-market) expansion; uncertain demand
```

#### Engineering View
Engineers care about:
- Dependencies
- Technical complexity
- Execution timeline
- Risks

Example:
```
ENGINEERING ROADMAP: Q1-Q3 2026

Q1 2026:
- AI Risk Scoring: 10 weeks effort
  Dependencies: API v1 (already shipped), ML model training (parallel)
  Risks: ML model accuracy <95% (mitigation: pilot with 100 contracts first)

- Multi-Currency: 5 weeks effort
  Dependencies: Database migration (in progress), payment system update
  Risks: Time zone handling complexity (mitigation: start with UTC only)

- Email Notifications: 1 week effort
  Dependencies: Email service integration (already done)
  Risks: None

Q2 2026:
- Real-Time Dashboard: 6 weeks effort
  Dependencies: WebSocket infrastructure (must build first; 2-week task)
  Risks: Performance under load (mitigation: load testing in staging)

[Continue through Q3...]
```

#### Customer-Facing View
Customers care about:
- What's coming that solves their problems
- When they can expect it
- What are the outcomes for them

Example:
```
CUSTOMER ROADMAP: Q1-Q3 2026

Q1 2026:
- AI Risk Scoring: Automatically flag high-risk contract terms. Reduces manual review time from 4 weeks to 1 week.
- Multi-Currency: Support contracts in EUR, GBP, JPY, CAD. Enables global supplier management.

Q2 2026:
- Real-Time Dashboard: See contract status updates live (no more hourly refreshes). Improve visibility and reduce approval delays.

Q3 2026:
- Supplier Scorecards: Auto-generate supplier performance reports. Make data-driven supplier decisions.
```

#### Sales View
Sales cares about:
- What features to promise (and when)
- What features differentiate us from competitors
- What features enable upsells

Example:
```
SALES ROADMAP: Q1-Q3 2026

Q1 2026 - Close-Enabling Features:
- AI Risk Scoring: Differentiator vs. Icertis; enables $500k General Dynamics deal
- Use in pitches: "Reduce contract review time by 75%"
- Expected close rate lift: +15%

Q1 2026 - Upsell Opportunities:
- Multi-Currency: Charge +$50k annual for global support
- Expected attach rate: 8 of 15 enterprise customers = +$400k

Q2-Q3 2026 - Expansion Opportunities:
- Supplier Scorecards: New market (mid-market)
- Expected new segments: 50 mid-market customers
- Expected ARPU: $20k (vs. $100k for enterprise)
```

---

## Application: Step-by-Step Roadmap Development

### Step 1: Gather Inputs

For each potential feature, gather these inputs:

1. **Customer Demand**: How many customers requested it? Are they large/strategic?
2. **Contract Commitments**: Is it in any signed customer contracts? When is it due?
3. **Regulatory Requirements**: Is it required for compliance? When is the deadline?
4. **Strategic Alignment**: Does it support 2026 strategic goals?
5. **Dependencies**: What must ship first?
6. **Revenue/Retention Impact**: What business outcome does it drive?
7. **Effort Estimate**: How many weeks of engineering?
8. **Risk Level**: What could go wrong?

### Step 2: Prioritize (Using RICE or another framework)

See the Prioritization Frameworks skill for detailed guidance. Output: Ranked list of features.

### Step 3: Sequence by Dependencies

Draw a dependency graph. Identify critical path.

Example:
```
Feature A depends on Platform Refactor
Feature B depends on Feature A
Feature C depends on Feature A
Feature D depends on Feature B
Feature E is independent

Dependency graph:
Platform Refactor → Feature A → Feature B → Feature D
              ↓ Feature C

Critical path: Platform Refactor → Feature A → Feature B → Feature D (4 features)
Parallel tracks: Platform Refactor and Feature C can happen simultaneously

Sequencing:
Weeks 1-4: Platform Refactor
Weeks 1-2: Feature C (parallel)
Weeks 5-8: Feature A
Weeks 9-12: Feature B
Weeks 10-13: Feature D (partial overlap with Feature B okay if no other dependency)
```

### Step 4: Assign to Quarters

Take the prioritized list and assign to quarters based on:
- Effort estimates
- Dependencies
- Commitment dates
- Engineering capacity

Example: 13-week Q1, 12 engineers (156 person-weeks total, with 20% buffer = 125 person-weeks available)

```
Ranked features by RICE:
1. AI Risk Scoring (10 weeks effort; contract commitment Q1)
2. Multi-Currency (5 weeks; strategic priority Q1)
3. Supplier Scorecards (8 weeks; nice-to-have Q2)
4. Bulk Upload (3 weeks; ops priority Q1)
5. Real-Time Dashboard (6 weeks; retention priority Q2)
6. Dashboard Customization (6 weeks; low priority Q2-Q3)

Q1 Assignment (125 available weeks):
- AI Risk Scoring: 10 weeks
- Multi-Currency: 5 weeks
- Bulk Upload: 3 weeks
- Total: 18 weeks ✓ (well under 125 capacity)

Q2 Assignment (125 available weeks):
- Real-Time Dashboard: 6 weeks
- Supplier Scorecards: 8 weeks
- Total: 14 weeks ✓ (well under 125 capacity)

Q3 Assignment (125 available weeks):
- Dashboard Customization: 6 weeks
- (Remaining 119 weeks for platform work, tech debt, bug fixes)
```

### Step 5: Document Committed vs. Speculative

Be explicit about what you're committing to vs. what's tentative.

```
Q1 2026 ROADMAP

COMMITTED (95% confidence):
- AI Risk Scoring (contract requirement; General Dynamics deal)
- Multi-Currency (strategic priority; EMEA expansion)
- Bulk Upload (ops improvement; low execution risk)

HIGH PRIORITY (75% confidence):
- (None for Q1; focused on commitments)

SPECULATIVE (40% confidence):
- (None for Q1)
```

### Step 6: Create Audience-Specific Views

Translate the master roadmap into:
1. Executive summary (revenue/churn impact)
2. Engineering detail (dependencies, risks, technical complexity)
3. Customer-facing summary (outcomes, not features)
4. Sales summary (closes and upsells)

### Step 7: Publish with Caveats

Make clear what you're committing to and what might change.

Example:
```
Q1 2026 ROADMAP

Committed Features (95% confidence; will ship or invoke contract penalties):
- AI Risk Scoring
- Multi-Currency Support

High Priority (75% confidence; high likelihood but not guaranteed):
- Bulk Upload

Note: Features may be resequenced or scope may be adjusted if unexpected 
dependencies or technical challenges emerge. We commit to communicating 
changes 30 days in advance.

Contact: [Product Lead contact] for questions about roadmap.
```

---

## Worked Example: Quarterly Roadmap for Enterprise AP Automation Platform

### Scenario
You manage product for an enterprise Accounts Payable (AP) automation platform. It's November 2025; you're planning 2026.

**Business Context**:
- Current ARR: $10M
- 2026 goal: Grow ARR to $12M (+20%)
- Churn is 12%; goal is 10%
- Major customers: General Dynamics, Lockheed, Boeing, Raytheon

**Customer Commitments**:
- General Dynamics: Contract includes AI Invoice Risk Scoring; due Q2 2026 (renewal in Q3)
- Boeing: Multi-currency support promised; due Q2 2026 (renewal in Q4)
- Lockheed: No specific feature commitments; but high churn risk

**Strategic Priorities for 2026**:
1. Retain top enterprise customers (reduce churn from 12% to 10%)
2. Upsell AI features (+$1M ARR target)
3. Expand into mid-market segment (exploratory; target: 50 new customers @ $20k ARPU = +$1M ARR)

### Step 1: Feature Backlog (with RICE scores from Prioritization skill)

```
Feature | RICE Score | Effort | Revenue Impact | Strategic |
---------|-----------|--------|-----------------|-----------|
AI Invoice Risk Scoring | 3,200 | 10 weeks | +$500k (General Dynamics) | Upsell |
Multi-Currency Support | 1,800 | 5 weeks | +$300k (Boeing, etc.) | Retain |
Real-Time Dashboard | 6,000 | 6 weeks | Retention, NPS +2 | Retain |
Audit Trail Compliance | 2,500 | 4 weeks | Retain enterprise | Retain |
Supplier Email Notifications | 400 | 1 week | Retention (low) | Retain |
Bulk Import Validation | 500 | 3 weeks | Ops efficiency | Retain |
Dashboard Customization | 150 | 6 weeks | Mid-market differentiation | Expand |
Predictive Spend Analysis | 800 | 8 weeks | Upsell, +$200k | Upsell |
Workflow Automation (Advanced) | 300 | 12 weeks | Mid-market, exploratory | Expand |
Custom Role-Based Access | 450 | 3 weeks | Enterprise compliance | Retain |
```

### Step 2: Identify Committed vs. Optional

```
COMMITTED (Contract or regulatory requirement):
- AI Invoice Risk Scoring (General Dynamics contract)
- Multi-Currency Support (Boeing contract)
- Audit Trail Compliance (SOC 2 requirement)

HIGH PRIORITY (Strategic goal; customer retention):
- Real-Time Dashboard (top feature request; affects NPS)
- Supplier Email Notifications (workflow improvement)
- Bulk Import Validation (ops efficiency)

SPECULATIVE (Nice-to-have; exploratory):
- Dashboard Customization (mid-market; unproven demand)
- Predictive Spend Analysis (upsell; uncertain adoption)
- Workflow Automation Advanced (mid-market; exploratory)
- Custom Role-Based Access (compliance; lower priority)
```

### Step 3: Assign to Quarters

**Capacity**: 12 engineers, 13 weeks per quarter, 156 person-weeks per quarter, 20% buffer = 125 person-weeks available.

#### Q1 2026 (Jan-Mar)
Priority: Foundation for contractual commitments

```
COMMITTED:
- AI Invoice Risk Scoring: 10 weeks (contract requirement)
- Multi-Currency Support: 5 weeks (contract requirement)
- Audit Trail Compliance: 4 weeks (SOC 2 requirement)

Subtotal: 19 weeks

ADDITIONAL (if capacity allows):
- Supplier Email Notifications: 1 week (quick win; easy)
- Custom Role-Based Access: 3 weeks (supports Audit Trail; complements compliance)

Q1 Total: 23 weeks (out of 125 available)

Q1 Status: Well-resourced. 102 weeks remaining for tech debt, platform work, hiring ramp-up.
```

#### Q2 2026 (Apr-Jun)
Priority: Retention, ship features committed for Q1 (slippage recovery), start upsell work

```
COMMITTED (from Q1 slippage recovery):
- Any AI Risk Scoring, Multi-Currency, Audit Trail work that slipped

HIGH PRIORITY (new items):
- Real-Time Dashboard: 6 weeks (top customer request; retention driver)
- Bulk Import Validation: 3 weeks (ops efficiency; completes Q1 compliance work)
- Predictive Spend Analysis (start): 4 weeks (upsell feature; start pilot)

Subtotal: 13 weeks

SPECULATIVE:
- Dashboard Customization: 2-4 weeks (pilot with 2 customers; measure demand)

Q2 Total: 17 weeks (out of 125 available)
```

#### Q3 2026 (Jul-Sep)
Priority: Upsell features, explore mid-market, platform investments

```
COMMITTED:
- Predictive Spend Analysis (complete from Q2 start): 4 weeks

HIGH PRIORITY:
- Dashboard Customization (full launch if Q2 pilot succeeds): 6 weeks

SPECULATIVE:
- Workflow Automation Advanced (exploratory; for mid-market): 4 weeks
- Platform refactoring / Tech debt: 8 weeks

Q3 Total: 22 weeks (out of 125 available)
```

### Step 4: Create Master Roadmap

```
2026 ENTERPRISE AP AUTOMATION PLATFORM ROADMAP

Q1 2026 (Jan-Mar) - Foundation: Contract Fulfillment & Compliance
COMMITTED (95% confidence):
- AI Invoice Risk Scoring (10 weeks)
  * Automatically flag invoices for fraud risk, duplicate, missing data
  * Reduces manual invoice review from 4 hours to 1 hour per 100 invoices
  * Required for General Dynamics contract renewal (Q3)
  * Expected revenue impact: +$500k from contract commitment

- Multi-Currency Support (5 weeks)
  * Support USD, EUR, GBP, JPY, CAD, AUD
  * Enables global invoice processing; convert to base currency
  * Required for Boeing contract renewal (Q4)
  * Expected revenue impact: +$300k from expanded global reach

- Audit Trail Compliance (4 weeks)
  * Log all invoice access, approvals, changes with immutable audit logs
  * SOC 2 Type II requirement (security review blocker)
  * Expected impact: Close security review delays; enable healthcare/government customers

ADDITIONAL (85% confidence):
- Supplier Email Notifications (1 week)
  * Notify supplier when invoice is approved/rejected
  * Improves workflow; reduces support tickets
  * Expected impact: Churn reduction 1%

- Custom Role-Based Access (3 weeks)
  * Define roles (AP Manager, Approver, Viewer) with granular permissions
  * Supports Audit Trail compliance work
  * Expected impact: Enable enterprise customer deployments

Q1 Total Effort: 23 weeks (out of 125 available)
Q1 Expected Business Impact:
  - Revenue: +$800k (contracts + compliance enablement)
  - Retention: Churn reduction 1% (notification + RBAC features)
  - Customer satisfaction: NPS +1 (compliance improvements)

---

Q2 2026 (Apr-Jun) - Retention: Dashboard & User Experience
COMMITTED (subject to Q1 delivery):
- Real-Time Dashboard Updates (6 weeks)
  * Replace hourly refresh with live WebSocket updates
  * Top customer request (7 of top 15 customers want this)
  * Expected retention impact: NPS +2 points, churn reduction 1%

- Bulk Import Validation (3 weeks)
  * Upload 1000 invoices; validate all before batch import
  * Reduces manual data entry time for large implementations
  * Expected impact: Faster onboarding, churn reduction 0.5%

- Predictive Spend Analysis (Start) (4 weeks)
  * ML-powered forecast of AP spend for next quarter
  * Upsell feature; +$30k per customer
  * Target 10 enterprise customers for upsell = +$300k
  * Expected revenue impact: +$300k (if adoption targets met)

EXPERIMENTAL (50% confidence):
- Dashboard Customization (Pilot) (2 weeks)
  * Pilot with 2 mid-market customers
  * Measure adoption, customer satisfaction
  * Decision point: Full launch in Q3 if pilot successful (75%+ satisfaction)

Q2 Total Effort: 17 weeks (out of 125 available)
Q2 Expected Business Impact:
  - Revenue: +$300k (Predictive Spend upsell)
  - Retention: Churn reduction 1.5% (Real-Time Dashboard + Bulk Import)
  - Customer satisfaction: NPS +2 (dashboard responsiveness)

---

Q3 2026 (Jul-Sep) - Growth: Mid-Market & Upsells
COMMITTED (subject to Q2 outcomes):
- Predictive Spend Analysis (Complete) (4 weeks)
  * Full production launch; all enterprise customers get access
  * Expected revenue: +$300k from Q2 adoption

HIGH PRIORITY (75% confidence):
- Dashboard Customization (Full Launch) (6 weeks)
  * Launch if Q2 pilot achieves >75% satisfaction
  * Target: 50 mid-market customers
  * Expected revenue: +$100k (mid-market ARPU lower than enterprise)

EXPERIMENTAL (40% confidence):
- Workflow Automation Advanced (Exploratory) (4 weeks)
  * Pilot advanced approval workflows for mid-market
  * Goal: Understand mid-market differentiation needs
  * Decision point at end of Q3: Continue investment or defer to Q4?

PLATFORM INVESTMENT:
- Tech Debt / Platform Refactoring (8 weeks)
  * API versioning overhaul (support v1 and v2 simultaneously)
  * Database scaling optimization
  * Expected impact: Enable future feature velocity; reduce production incidents

Q3 Total Effort: 22 weeks (out of 125 available)
Q3 Expected Business Impact:
  - Revenue: +$400k (Predictive Spend completion + Dashboard Customization)
  - New customer acquisition: 50 mid-market customers = +$1M (added to 2027 ARR)
  - Technical health: Platform stability improvements; tech debt reduction

---

2026 FULL-YEAR ROADMAP SUMMARY

Committed Revenue Impact:
  Q1: +$800k (contracts + compliance)
  Q2: +$300k (Predictive Spend upsell)
  Q3: +$400k (Product launch + mid-market)
  Total: +$1.5M new ARR (vs. +$2M goal; shortfall due to mid-market expansion delay)

Churn Impact:
  Q1: -1% (compliance, notifications)
  Q2: -1.5% (real-time dashboard)
  Q3: -0.5% (retention features plateau)
  Net: Reduce churn from 12% to 9% (beats 10% goal)

Strategic Outcomes:
  ✓ Contract fulfillment: AI Risk Scoring, Multi-Currency (both on-time for renewals)
  ✓ Enterprise retention: Churn reduced to 9% (exceeds 10% goal)
  ~ Mid-market expansion: 50 new customers in Q3 (exploratory phase; full expansion in 2027)
  ~ Upsell growth: +$600k from AI features (vs. +$1M goal; shortfall from adoption slowness)

Risk Mitigation:
  - Q1 commitment risk: AI Risk Scoring is complex; 2-week buffer built into schedule
  - Q2 execution: Real-Time Dashboard is engineering-heavy; 6-week estimate is aggressive
  - Q3 platform work: Avoid tech debt spike; allocate 8 weeks for refactoring
  - Mid-market: Exploratory only; don't overcommit until demand validated

Resource Plan:
  - Q1: 23 weeks (out of 125); 102 weeks available for hiring, onboarding, platform work
  - Q2: 17 weeks (out of 125); 108 weeks available for tech debt, platform work
  - Q3: 22 weeks (out of 125); 103 weeks available for future platform investment

Next Steps:
  - Publish this roadmap to all stakeholders (execs, sales, engineering, customers)
  - Schedule monthly roadmap reviews; update if major changes to dependencies or priorities
  - Publish customer-facing version (outcomes-focused, feature-light)
  - Create engineering dependencies map; identify blocking items
```

### Step 5: Customer-Facing Summary

```
2026 ROADMAP FOR OUR ENTERPRISE CUSTOMERS

What We're Shipping for You

Q1 2026 (Jan-Mar):
- AI Invoice Risk Scoring
  Automatically flag high-risk invoices (potential fraud, errors, missing data).
  Reduce manual review time by 75%.

- Multi-Currency Support
  Process invoices in EUR, GBP, JPY, and more. Auto-convert to your base currency.
  Enables global AP management.

Q2 2026 (Apr-Jun):
- Real-Time Dashboard
  See invoice status updates live. No more hourly refreshes.
  Approve invoices faster; visibility into your AP pipeline.

Q3 2026 (Jul-Sep):
- Predictive Spend Analysis
  Forecast your AP spend for next quarter.
  Budget planning and cost management at a glance.

- Advanced Workflow Automation (Pilot)
  Customizable approval workflows to match your company's policies.
  (Pilot program: 2 customers in Q2; full release in Q3 if successful)

---

Questions?
Contact your Account Manager or [product-support@company.com]
```

---

## Common Pitfalls

### Pitfall 1: Over-Committing to Dates
**Anti-Pattern**: "We'll ship AI Risk Scoring in Q1" (high confidence). Then in Month 2 of Q1, you realize the ML model accuracy is only 80% (should be 95%). You have 6 weeks left; can't hit the date.

**Why it fails**: Customers plan around your dates. General Dynamics approvals team schedules training for Q1. If feature slips to Q2, their contract renewal doesn't happen; they churn.

**Right way**:
- Distinguish between "Committed" (95% confidence) and "High Priority" (75% confidence)
- For committed items, add 20-30% schedule buffer
- Communicate clearly: "This ships Q1; this is high priority but may slip to early Q2"

### Pitfall 2: Ignoring Dependencies
**Anti-Pattern**: "Real-Time Dashboard ships in Q2." But it depends on WebSocket infrastructure, which depends on API versioning overhaul (8 weeks). You don't realize the dependency until Month 1 of Q2.

**Why it fails**: You commit to Q2; discover in Month 1 that you're blocked until Month 2 or 3. Feature slips. Customer churn.

**Right way**: Build dependency map before committing to dates.
```
Real-Time Dashboard (6 weeks)
  ↓ depends on
WebSocket Infrastructure (2 weeks)
  ↓ depends on
API Versioning Overhaul (8 weeks)

Critical path: 8 + 2 + 6 = 16 weeks
If you want Real-Time Dashboard by Q2 (13 weeks), you need to start API overhaul in Q1.
```

### Pitfall 3: No Customer Commitment Tracking
**Anti-Pattern**: You create a roadmap. Sales promises features to customers. No one tracks which customers are waiting for which features or when their renewals are due.

Result: AI Risk Scoring slips to Q2. General Dynamics renewal is in Q2 month 2. Feature ships month 1. Margin of safety = 1 week. Churn risk = high.

**Right way**: Create a commitment tracking matrix.
```
Feature | Customer | Contract Renewal | Importance | Risk Level |
---------|----------|------------------|------------|------------|
AI Risk Scoring | General Dynamics | Q2 month 2 | Must-have | HIGH |
AI Risk Scoring | Boeing | Q3 month 1 | Nice-to-have | MEDIUM |
Multi-Currency | Boeing | Q4 month 1 | Must-have | MEDIUM |
```

Review this monthly. If AI Risk Scoring is tracking to miss General Dynamics renewal, escalate immediately.

### Pitfall 4: Speculative Features Taking Committed Spots
**Anti-Pattern**: "Let's explore dashboard customization (exploratory, low demand) in Q2." This takes 6 weeks of engineering. Result: Real-Time Dashboard (committed, high demand) slips to Q3. Churn.

**Why it fails**: Speculative features are less important but feel urgent because they're "new" or exciting. Committed features are already promised; easier to deprioritize because they're "already committed to" (sunk cost fallacy).

**Right way**: Ruthlessly protect committed items.
- Committed = 95% of capacity
- High priority = 5% of capacity + overflow
- Speculative = Future quarters only, or <5% of available buffer capacity

### Pitfall 5: No Audience-Specific Views
**Anti-Pattern**: You publish one roadmap. Executives ask, "What's the revenue impact?" Engineers ask, "What are the dependencies?" Customers ask, "When will this be done?" You have one roadmap; can't answer any of their questions clearly.

**Why it fails**: Each audience has different information needs. One roadmap can't serve all. You end up in endless clarification conversations.

**Right way**: Publish 4 roadmaps:
1. Executive view (revenue/churn impact + timeline)
2. Engineering view (dependencies, technical complexity, risks)
3. Customer view (outcomes + timeline)
4. Sales view (closes + upsells + differentiation)

All 4 are derived from the same master roadmap; just curated for the audience.

### Pitfall 6: Roadmap Without Outcomes
**Anti-Pattern**: "We're shipping Real-Time Dashboard in Q2." But no one defined what success looks like. Does it need to hit a specific NPS improvement? Churn reduction? Load time target?

Result: Feature ships. You don't know if it worked. Did it hit churn reduction target? You never measured. Wasted effort.

**Right way**: Every roadmap item should have:
- **Outcome**: What metric does this move? (NPS, churn, revenue, etc.)
- **Target**: What's the expected impact? (NPS +2, churn -1%, +$500k, etc.)
- **Measurement**: How will we track it? (Survey, retention data, usage metrics, etc.)

Example:
```
Feature: Real-Time Dashboard
Outcome: Improve user experience; reduce approval delays
Target: NPS +2 points; churn -1%; faster invoice approvals (measured by average approval time)
Measurement: Monthly NPS survey; churn rate tracking; invoice workflow analytics
Success criteria: Achieve all three targets by end of Q2
```

At end of Q2, measure. Did you hit the targets? If not, why? Adjust Q3 roadmap based on learnings.

### Pitfall 7: Roadmap Ambiguity on Scope
**Anti-Pattern**: "Multi-Currency Support ships in Q1." But does it include:
- Exchange rate updates (daily? hourly? manual)?
- Currency-specific validations (e.g., JPY doesn't use decimals)?
- Tax/compliance per currency?
- Reporting in multi-currency?

Engineering assumes MVP (just store different currency; no automations). Sales promises everything. Scope creep. Feature slips.

**Why it fails**: Ambiguity leads to misaligned expectations. Engineering ships v1; sales promised v2; customer expects v3.

**Right way**: Define scope crisply.
```
Feature: Multi-Currency Support (Q1 2026, 5 weeks)

INCLUDED (v1):
- Support USD, EUR, GBP, JPY, CAD, AUD
- Manual exchange rate entry (admin can set daily)
- Store invoice amounts in original currency + converted amount
- Reports in base currency with exchange rate transparency

NOT INCLUDED (v2):
- Auto-fetch exchange rates from APIs
- Real-time FX price quotes
- Multi-currency tax handling (each country's rules)
- Multi-currency analytics (group by currency for reporting)

Rationale for v1 scope:
- Limits effort to 5 weeks (enables Q1 delivery)
- Delivers 80% of customer value (basic multi-currency)
- MVP allows validation before investing in full-featured v2
```

Communicate this scope to customers, sales, and engineering. Align expectations.

### Pitfall 8: Feature-Centric Roadmap
**Anti-Pattern**: "We're shipping X, Y, Z in Q1." But you never explain WHY. Why these features? What are the business goals?

Result: Team is misaligned. Engineering doesn't understand strategy. Sales doesn't know how to sell. Customers don't understand value.

**Why it fails**: Feature-centric roadmaps are shipping lists, not strategy. They don't inspire. They don't align. They don't communicate value.

**Right way**: Outcome-driven roadmaps.
```
Q1 2026 GOALS:
1. Fulfill contract commitments (General Dynamics, Boeing)
2. Reduce churn from 12% to 11% (via compliance features)
3. Start upsell motion for AI features

To achieve these goals, we're shipping:
- AI Risk Scoring (fulfills General Dynamics contract; upsell enabler)
- Multi-Currency (fulfills Boeing contract)
- Audit Trail (compliance; reduces churn)
- ...
```

Now the roadmap tells a story. Features are connected to goals. Team understands the "why."

---

## References

- **"Inspired" by Marty Cagan**: Outcome-driven product strategy
- **"Empowered" by Marty Cagan & Chris Jones**: Roadmapping for product leaders
- **ProductTank Roadmapping Guide**: Audience-specific roadmaps
- **Pragmatic Institute: Roadmap Planning**: Best practices for enterprise roadmaps

---

## Next Steps

1. **Choose a roadmap format**: Start with Now/Next/Later (flexibility) or Quarterly (commitment)
2. **Gather customer commitments**: Document which features are in contracts; when renewals are due
3. **Identify regulatory deadlines**: What compliance features must ship when?
4. **Build dependency map**: Identify blocking items; plan critical path
5. **Create master roadmap**: Combine all inputs; sequence features
6. **Generate audience-specific views**: Executive, engineering, customer, sales summaries
7. **Publish & communicate**: Share with all stakeholders; set expectations
8. **Track progress**: Monthly roadmap reviews; update if major changes
9. **Measure outcomes**: At end of quarter, measure whether features delivered expected impact
10. **Iterate**: Refine roadmap planning process based on actual vs. estimated outcomes
