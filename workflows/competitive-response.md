---
name: Competitive Response
description: Urgent workflow for rapid competitive analysis and strategic response planning
workflow_type: urgent_parallel
estimated_duration: "2-3 hours for initial response (T+0 to T+3 hours)"
plugin_chain: ["competitive-analysis", "market-analysis", "roadmap", "communications"]
---

## Overview

The Competitive Response workflow is designed for urgent situations when a competitor launches a significant feature, enters a new market segment, or makes a strategic move that requires immediate analysis and response planning. Unlike the quarterly planning cycle, this workflow operates at high speed with parallel analysis tracks, escalation paths based on threat severity, and rapid decision-making.

This workflow prioritizes getting decision-makers accurate competitive context within 2-3 hours, then builds a response roadmap within 24 hours.

## Skills Involved

| Skill | Plugin | Purpose |
|-------|--------|---------|
| **competitive-analysis** | competitive | Rapid analysis of competitor's feature, positioning, and market implications |
| **market-sizing** | market-analysis | Assess market opportunity and customer migration risk |
| **roadmap-planning** | roadmap | Build counter-response roadmap and timeline |
| **stakeholder-comms** | communication | Rapid escalation and stakeholder updates with clear recommendations |

## Threat Severity Levels

Before starting this workflow, assess severity:

| Level | Trigger | Response Time | Escalation |
|-------|---------|---------------|------------|
| **CRITICAL** | Competitor matches our key differentiator; significant customer churn risk | 1-2 hours | CEO + Board notification within 4 hours |
| **HIGH** | Major feature launch affecting 20%+ of customer base; pricing threat | 2-3 hours | C-level + product leadership within 4 hours |
| **MEDIUM** | Feature launch affecting specific segment; incremental competitive threat | 4-8 hours | Product + sales leadership within business day |
| **LOW** | Cosmetic feature, minor market move, limited customer impact | 24 hours | Team-level discussion in next team sync |

**Example severity assessments:**
- CRITICAL: Competitor launches AI-native version of core workflow (our differentiator)
- HIGH: Competitor cuts pricing 30%, launches SMB tier we don't have
- MEDIUM: Competitor adds nice-to-have feature in adjacent category
- LOW: Competitor rebrands feature, cosmetic UI improvements

---

## Step-by-Step Sequence

### Step 1: Rapid Competitive Intelligence Gathering (T+0 to T+30 min)
**Owner:** PM + Research Team (Parallel across 2-3 subagents)  
**Input:** Competitor announcement, PR, product update, customer report  
**Output:** Competitive brief with feature details, positioning, estimated scope

Run the **competitive-analysis** skill with parallel sub-agents:

**Sub-Agent 1: Feature Deep-Dive**
- Extract exact feature details: What specifically did competitor build?
- Document user workflows: How does it work? What problems does it solve?
- Assess technical approach: Cloud-native? API-first? Custom integrations?
- Compare to our roadmap: Do we have this planned? When?
- Identify gaps: What's missing from their implementation?

**Sub-Agent 2: Positioning & Messaging**
- Analyze competitor's positioning: How are they marketing this?
- Document customer-facing benefits: How are they describing the value prop?
- Review landing pages, emails, ads, social media
- Assess tone (aggressive vs. subtle): Are they calling out competitors directly?
- Identify target segments: Who are they going after?

**Sub-Agent 3: Product History & Trajectory**
- Research competitor's product roadmap: What was announced before this?
- Identify patterns: Is this part of a larger strategic shift?
- Review past launches: How successful were they? Customer adoption?
- Assess resources: Does competitor have engineering/design capacity for follow-up?

**Competitive Brief Template:**
```
THREAT LEVEL: [CRITICAL/HIGH/MEDIUM/LOW]

Competitor: [Name]
Feature: [Feature Name]
Announced: [Date] via [Channel - press release, blog, Twitter, conference]

FEATURE DETAILS:
Description: [What exactly did they build?]
Functionality: [Core workflows affected]
Technology Stack: [How is it built?]
Integration Points: [What systems does it connect to?]
Launch Date: [Available now? Beta? Future?]

POSITIONING:
Target Message: [How are they selling this?]
Value Proposition: [Key benefits they're emphasizing]
Pricing: [Cost to customer? Included in existing plans?]
Target Segments: [Who are they going after?]

COMPETITIVE ASSESSMENT:
Our Differentiation vs. Theirs: [Where are we stronger?]
Their Advantage: [Where are they better?]
Customer Impact: [How many of our customers care about this?]
Migration Risk: [Likelihood of customer churn? What customer profiles are at risk?]

ROADMAP IMPLICATIONS:
Do we have this planned? [Yes/No - when?]
Can we match? [Timeline if we build]
Are we already superior? [Yes - how?]

EXAMPLE CUSTOMERS AT RISK:
Segment A: [Profile] - Risk: High (they explicitly need this)
Segment B: [Profile] - Risk: Low (not relevant to them)
```

**User Interaction Point:** PM reviews brief within 30 min and confirms severity level. If CRITICAL or HIGH, immediately schedule exec escalation call. PM flags any surprises or gaps to address before executive briefing.

**Data Handoff:** Competitive brief flows to Step 2 (Market Sizing) and Step 3 (Response Planning).

---

### Step 2: Market Impact & Customer Risk Assessment (T+30 min to T+1.5 hours, Parallel with Step 1)
**Owner:** Sales + Data Analytics  
**Input:** Competitive brief from Step 1, customer base data, win/loss analysis  
**Output:** Customer migration risk assessment, segment-by-segment impact analysis

Run the **market-sizing** skill to:
- Map which customer segments are most affected (Enterprise vs SMB vs Product-Led Growth)
- Assess customer churn risk: How likely are customers in segment X to switch due to this?
- Identify "at-risk" accounts: Top 20 customers most likely to be targeted by competitor
- Analyze win/loss history: Have we lost customers to this competitor before? Pattern?
- Calculate financial impact: If 10% of top segment churns, what's the ARR impact?
- Assess TAM implications: Does this open a new market we didn't address?
- Benchmark vs. past competitive threats: How does this compare to previous situations?

**Customer Risk Assessment Example:**
```
CUSTOMER SEGMENT ANALYSIS:

Enterprise (500+ employees):
  Total TAM: 400 customers, $8M ARR
  Affected by feature: 280 customers (70%)
  Churn risk: 15-25% (High - this feature addresses their key pain point)
  At-risk accounts ($500k+ ARR): Acme Corp, Global Industries, TechCorp Inc
  Mitigation: Direct outreach from CFO/CEO relationship manager

Mid-Market (50-500 employees):
  Total TAM: 1200 customers, $4M ARR
  Affected by feature: 400 customers (33%)
  Churn risk: 5-10% (Low-Medium - nice-to-have for segment)
  At-risk accounts ($50-200k ARR): [list top 15]
  Mitigation: Sales team outreach with competitive messaging

SMB (10-50 employees):
  Total TAM: 5000 customers, $2M ARR
  Affected by feature: 600 customers (12%)
  Churn risk: 2-5% (Very Low - feature complexity not relevant to SMB)
  At-risk accounts: N/A (too distributed)
  Mitigation: Product-led comms + feature advantage messaging

FINANCIAL IMPACT IF 15% OF AT-RISK SEGMENT CHURNS:
Enterprise: 280 * 15% * $20k/customer = $840k ARR at risk
Mid-Market: 400 * 10% * $3.3k/customer = $132k ARR at risk
SMB: Negligible

Total potential downside: ~$1M ARR if no response (worst case)
Expected loss (50% mitigation): ~$500k ARR (realistic case)

STRATEGIC IMPLICATIONS:
- We must respond to Enterprise segment to protect $8M
- Mid-Market response is optional but recommended
- SMB: Communication only, no product response needed
```

**User Interaction Point:** Sales leader confirms at-risk account list and migration risk assessment. Data team signs off on churn probability estimates. 30 min working session.

**Data Handoff:** Risk assessment informs response urgency and scope. High-risk Enterprise churn flows to Step 4 (Stakeholder Communications).

---

### Step 3: Response Roadmap & Strategic Options (T+1.5 hours to T+2.5 hours, Parallel with Steps 1-2)
**Owner:** PM + Product Leadership  
**Input:** Competitive brief (Step 1), market impact (Step 2)  
**Output:** Response options with timelines, recommended path, roadmap

Run the **roadmap-planning** skill to develop 3-5 strategic options:

**Option 1: Build Equivalent Feature (Match & Differentiate)**
- Timeline: [weeks/months to match feature]
- Effort: [engineering days]
- Competitive strategy: Match feature but add our own twist
- Example: "We'll launch matching feature by [date], but with [our advantage]"
- Pros: Direct response, clear customer messaging
- Cons: Takes engineering time, may not be our best use of resources

**Option 2: Differentiate Differently (Double-Down on Strengths)**
- Timeline: Immediate (announce current advantage, announce new feature by [date])
- Strategy: Play to our strengths, not theirs
- Example: "We're doubling down on [our differentiator]. Here's what we're shipping instead."
- Pros: Keeps roadmap aligned with strategy, faster execution
- Cons: Requires confidence in differentiation, may lose some customers

**Option 3: Reposition & Bundle (Change the Game)**
- Timeline: 6-8 weeks for new positioning, 3-6 months for feature bundling
- Strategy: Reframe how we compete entirely
- Example: "We're not about individual features. We're about [bigger vision]."
- Pros: Can shift entire competitive dynamic
- Cons: High execution risk, message must be bulletproof

**Option 4: Pricing/Packaging Offensive (Commoditize Their Feature)**
- Timeline: 1-2 weeks
- Strategy: Make their feature less valuable by offering it cheaper or as bundle
- Example: "Their feature costs them, we're including it free with [plan]"
- Pros: Fast, directly impacts customer economics
- Cons: May trigger price war

**Option 5: Do Nothing (Strategic Bet)**
- Timeline: Immediate
- Strategy: Assess threat was overstated or not material to business
- Pros: Keeps resources focused on north star
- Cons: High execution risk if you're wrong

**Recommendation Framework:**
```
SEVERITY: HIGH (Enterprise churn risk)

RECOMMENDED PATH: Option 1 + Option 2 Combo
  - Short-term (next 4 weeks): Double-down on [our advantage], proactive customer comms
  - Medium-term (weeks 4-8): Ship matching feature with [our twist]
  - Long-term (8+ weeks): Evolve feature set based on customer feedback

RATIONALE:
  - We can't afford to lose $500k+ ARR without responding
  - Our roadmap already has similar feature (planned Q3) - pull forward to Q2
  - We have technical advantage in [area], market that aggressively
  - Quick comms to at-risk customers buys time for feature development

TIMELINE:
  Week 1: Internal alignment, at-risk customer outreach, feature fast-track decision
  Week 2-4: Feature development, beta testing
  Week 4: Customer soft launch to at-risk segment
  Week 5-6: GA launch to full customer base

RESOURCE ALLOCATION:
  - Engineering: 2 engineers from Q3 roadmap pulled into this (3-4 week sprint)
  - Product: Full-time focus for next 4 weeks
  - Marketing: Competitive messaging, at-risk customer comms
  - Sales: Direct outreach to Enterprise at-risk accounts
```

**User Interaction Point:** Leadership team reviews options and recommends path. Engineering confirms feasibility and resource impact. Decision is made: do we match, differentiate, or reposition? 1 hour decision session.

**Data Handoff:** Recommended response roadmap feeds into Step 4 (Stakeholder Communications). Response strategy informs customer-facing messaging.

---

### Step 4: Stakeholder Escalation & Communication (T+2.5 hours to T+3 hours)
**Owner:** PM + CEO/CFO + Head of Sales  
**Input:** All analysis from Steps 1-3  
**Output:** Executive briefing, internal announcement, customer communication plan

Run the **stakeholder-comms** skill to create layered communications:

#### Level 1: Executive Briefing (Immediate - T+2.5 hours)
**Audience:** CEO, CFO, Board (if CRITICAL)  
**Format:** 10-15 min verbal + 1-page summary  
**Contents:**
- Threat summary (1 sentence): "Competitor launched [feature] targeting our Enterprise segment"
- Customer impact: "$500k ARR at risk if we don't respond"
- Recommended action: "Pull [feature] from Q3 roadmap into Q2; launch matching version by [date]"
- Resource ask: "2 engineers for 3-4 weeks; no budget impact (internal reallocation)"
- Decision needed: "Approve feature acceleration? Any concerns?"

**One-page Executive Brief Template:**
```
COMPETITIVE THREAT SUMMARY
Date: [Today]
Competitor: [Name]
Threat Level: [CRITICAL/HIGH/MEDIUM/LOW]

SITUATION:
Competitor launched [Feature] on [Date] targeting [Segment].
Initial customer feedback: [Positive/Mixed/Alarming]
Our at-risk accounts: [List top 3-5] = $[X]M ARR exposure

FINANCIAL IMPACT:
Downside (no response): $500k-1M ARR churn (high confidence)
Upside (strong response): Retain 90%+ of at-risk segment

OUR COMPETITIVE POSITION:
Where we're strong: [Advantages]
Where they're strong: [Weaknesses]
Can we catch up? [Yes - timeline]

RECOMMENDED RESPONSE:
Action: Accelerate [Feature] from Q3 to Q2 launch
Timeline: Feature ships by [Date]
Messaging: Lead with [our advantage], announce [feature] within [timeframe]
Investment: 2 engineers, 3-4 weeks (reallocated from Q3)

APPROVAL NEEDED:
[ ] Yes, proceed with acceleration
[ ] Need more information
[ ] No, pursue alternative strategy

DECISION BY: [Timestamp - typically within 2 hours of briefing]
```

**User Interaction Point:** CEO reviews brief and approves response path. If CRITICAL threat, board notification happens immediately. Decision is recorded and communicated to team.

---

#### Level 2: Internal Alignment (T+3 hours to T+6 hours)
**Audience:** Full product + engineering + sales + marketing teams  
**Format:** Async Slack post + optional 30 min all-hands  
**Contents:**
- Competitive threat summary
- Customer impact (how many customers? what's at risk?)
- Our response: "We're shipping [feature] by [date] because [business reason]"
- What changes: "Q3 roadmap adjusted: [features deferred], [features accelerated]"
- How it affects different teams: Engineering gets feature brief, Sales gets messaging, Marketing gets customer comms template
- Team sentiment: Transparent about risk, confident in response

**Internal Announcement Template:**
```
INTERNAL: Competitive Response - Accelerated Roadmap

TLDR: Competitor launched feature affecting Enterprise customers.
We're accelerating our matching feature from Q3 to Q2 launch (6-week timeline).
This requires reallocating 2 engineers and adjusting roadmap priorities.
Full details below.

THE THREAT:
[Competitor] launched [Feature] targeting [Segment] on [Date].
Our analysis: 280 of our Enterprise customers affected, $840k ARR at risk if we don't respond.
Customer reactions so far: Mixed - some interested, some monitoring.

OUR RESPONSE:
We're matching their feature with [our advantage] by [date].

WHAT THIS MEANS FOR YOUR TEAM:

Engineering:
  - Feature sprint starting [Date]
  - 2 engineers allocated to feature development (planned for Q3)
  - Q3 roadmap: Deferring [Feature A] and [Feature B], keeping [Feature C]
  - Questions? Talk to [Eng Lead]

Product:
  - Full-time focus on this feature for next 4 weeks
  - Beta testing window: [Dates]
  - Customer feedback integration: [Process]

Sales:
  - Messaging playbook dropping today (link)
  - Talking points: "We're shipping matching feature by [date], and here's where we're better..."
  - Direct outreach to 10 at-risk Enterprise accounts starting [Date]
  - Campaign: [Sales Director] will brief in sales meeting [Date/Time]

Marketing:
  - Blog post/comms planned for [Date]: "Here's what we're shipping"
  - Customer email template ready by [Date]
  - Social messaging approved for [Date]
  - Work with product on technical details

Questions/Concerns?
  Reply in thread or DM [PM]
  All-hands deep-dive [Date/Time] (optional, recorded)
```

---

#### Level 3: Customer Communications (T+4 hours to T+24 hours)
**Audience:** At-risk customers → all customers (staged)  
**Format:** Email, customer success call, public announcement  
**Contents:**

**Immediate (T+4-8 hours): Sales outreach to at-risk Enterprise accounts**
```
Subject: [Competitor] feature announcement - Here's what we're doing

Hi [Customer Name],

You may have seen [Competitor] announced [Feature] this week.
I wanted to reach out directly because this matters to you.

Here's what we're doing:

We're shipping a matching feature by [Date]. Here's how ours is different:
[Our advantage 1]
[Our advantage 2]
[Our advantage 3]

In the meantime, here's how to get the benefits today:
[Workaround or interim solution]

Timeline:
- Beta: [Date]
- GA: [Date]
- Personalized demo: [Offer]

Questions? Let's chat.

[Sales Rep]
```

**Short-term (T+24 hours): Public announcement**
```
Blog post: "We're shipping [Feature]. Here's where we lead."

Headline: [Competitor] shipped [Feature]. We're shipping [Feature] + [difference].
Key points:
- We're matching their feature by [date]
- Here's where we're better: [List]
- Our vision for this category: [Vision]
- Roadmap: [Next steps]
```

**Medium-term (1-2 weeks): Follow-up customer survey**
```
Quick survey to all customers:
1. Awareness: Have you seen competitor's feature?
2. Impact: Does it matter to you?
3. Consideration: Would you consider switching?
4. Preference: If we had same feature, where would you stay? [Open feedback]

Use insights to refine positioning.
```

**User Interaction Point:** Sales leader confirms messaging is compelling and addresses objections. Marketing confirms customer comms are on-brand. CEO/Board are briefed on customer communication strategy if this is CRITICAL threat. 30 min cross-functional sync.

**Data Handoff:** Customer communication plan is published immediately. Sales team equipped with messaging for at-risk account calls.

---

## Data Handoff

| From Step | To Step | Data | Format | Reference |
|-----------|---------|------|--------|-----------|
| 1 | 2 + 3 | Competitive feature brief | Competitive brief document | Market sizing and response planning both reference feature details |
| 2 | 4 | Customer risk assessment | Risk assessment spreadsheet | Customer comms prioritize at-risk accounts |
| 3 | 4 | Response roadmap and strategy | Roadmap document + timeline | All customer messaging references recommended response path |
| 1 + 3 | 4 | Competitive positioning + our response | Combined brief | Executive briefing ties threat → response → outcomes |

**Single Source of Truth:** Create "Competitive Threat Tracker" spreadsheet with: threat date, severity level, competitor, feature, at-risk customers, recommended response, current status, follow-up date.

---

## Parallelization Opportunities

### Parallel Execution (T+0 to T+2.5 hours):

All three analysis tracks run simultaneously:

**Track A: Competitive Intelligence (Step 1)**
- Duration: 30-45 min
- Deliverable: Competitive brief
- Team: Research + Product

**Track B: Customer Risk Assessment (Step 2)**
- Duration: 45-60 min
- Deliverable: Customer impact report
- Team: Sales + Data

**Track C: Response Strategy (Step 3)**
- Duration: 1 hour
- Deliverable: 3-5 options with recommendations
- Team: PM + Product Leadership

**These three tracks merge at T+2.5 hours** for executive briefing (Step 4).

### Example High-Velocity Timeline:
```
T+0:      Competitive threat detected
          Parallel: Competitive brief, Customer risk, Response options
T+30min:  Initial brief review, severity assessment
T+1.5hr:  All three analyses complete
T+2.5hr:  Executive briefing, decision made
T+3hr:    Internal team announcement + messaging published
T+4hr:    Sales reaches out to at-risk customers
T+24hr:   Public announcement / blog post live
T+1wk:    Customer survey + feedback gathering
T+4wks:   Feature ships to beta
T+6wks:   Feature GA launch
```

---

## Escalation Paths by Severity

### CRITICAL (1-2 hour response):
- CEO + CFO notified within 1 hour of assessment
- Board notification (if applicable) within 4 hours
- All-hands team meeting called within 2 hours
- Immediate resource reallocation decision (can engineering stop Q3 work?)
- Customer outreach begins within 4 hours to top 20 at-risk accounts
- CEO personally involved in customer calls for top 10 accounts

### HIGH (2-3 hour response):
- C-level + product leadership notified within 2 hours
- Executive briefing and decision within 3 hours
- Internal team announcement within 6 hours
- Sales team begins at-risk customer outreach within 4-6 hours
- Roadmap acceleration decision within 24 hours

### MEDIUM (4-8 hour response):
- Product + sales leadership notified within 4 hours
- Response strategy ready by end of business day
- Team discussion in next daily standup
- Sales playbook published within 24 hours
- Customer comms scheduled for next week

### LOW (24+ hour response):
- Team discussion in next weekly meeting
- Standard competitive monitoring (not urgent)
- Evaluate for product roadmap (future quarter)
- No immediate customer outreach needed

---

## User Interaction Points

1. **Threat Assessment (T+0):** PM determines severity level. If CRITICAL or HIGH, escalation begins immediately.

2. **Competitive Brief Review (T+30 min):** PM reviews brief, confirms severity, flags any surprises.

3. **Executive Briefing (T+2.5 hours):** CEO/CFO reviews one-page brief and approves response strategy. Decision is recorded.

4. **Internal Team Announcement (T+3 hours):** PM communicates response to full team. Q&A addressed in first available channel.

5. **Sales Outreach Launch (T+4 hours):** Sales leader confirms at-risk account list and begins direct outreach with approved messaging.

6. **Customer Survey (T+1 week):** Product gathers customer feedback on awareness, impact, and consideration. Data feeds into message refinement.

7. **Feature Progress Checkpoints:** Weekly updates on feature development progress vs. timeline through launch.

**Total PM Time Investment:** 10-12 hours over first week (very intensive), then ongoing feature development + customer communication.

---

## Estimated Timeline

| Phase | Duration | Notes |
|-------|----------|-------|
| Threat Assessment & Analysis (Steps 1-3) | 2.5 hours | Parallel execution, high-velocity analysis |
| Executive Decision & Approval | 30 min | Within hours of analysis complete |
| Internal & Customer Communication (Step 4) | 2-6 hours | Ongoing through first week |
| Feature Development (if applicable) | 4-8 weeks | Depends on complexity of matching feature |
| **Total Response Time** | **2-3 hours** | For initial response; full execution varies |

---

## Final Output Summary

By the end of this workflow, you have:

1. **Competitive Intelligence Brief**
   - Feature details and technical assessment
   - Positioning analysis and messaging
   - Competitive comparison to our product
   - Product roadmap implications

2. **Customer Impact Report**
   - Segment-by-segment risk assessment
   - At-risk customer list (top 20) with ARR exposure
   - Churn probability and financial impact
   - Migration risk by customer profile

3. **Strategic Response Roadmap**
   - 3-5 options evaluated (match, differentiate, reposition, pricing, do nothing)
   - Recommended response path with justification
   - Timeline for execution
   - Resource allocation and impact to Q3 roadmap

4. **Executive Briefing**
   - One-page summary of threat and response
   - Financial impact (downside if no response, upside with response)
   - Resource requirements and investment
   - Decision approval and timeline

5. **Activated Sales & Marketing**
   - Sales playbook with messaging for at-risk accounts
   - Customer communication templates and outreach schedule
   - Competitive battle cards and talking points
   - Blog post / public announcement ready for publication

6. **Ongoing Customer Conversation**
   - Direct outreach to at-risk Enterprise accounts
   - Customer feedback gathering (awareness, impact, consideration)
   - Sales tracking of customer decisions and sentiment
   - Foundation for long-term competitive response

**Handoff Status:** Organization has assessed threat, made strategic decision, communicated to all stakeholders, and initiated response execution. Sales team has playbook to retain at-risk customers. Product team is mobilized for feature acceleration (if applicable). Execution phase begins.

---

## When to Use This Workflow

✓ **Competitor launches significant feature** affecting your differentiator  
✓ **Pricing changes** threatening customer economics  
✓ **New market entry** by competitor in your segment  
✓ **Unexpected product pivot** from competitor with implications  
✓ **Customer churn risk** triggered by competitive threat  
✓ **When you need rapid response** (within hours, not days)  

---

## When NOT to Use This Workflow

✗ **Minor feature announcement** with limited customer impact → Monitor and evaluate in normal planning cycle  
✗ **Competitor feature** we're already planning (scheduled for Q3) → No urgent response needed  
✗ **Competitor relaunch/cosmetic update** of existing feature → Standard competitive monitoring  
✗ **Analysis paralysis** situation where we don't know enough yet → Gather more data first  
✗ **When you have time to plan** (not urgent) → Use standard Quarterly Planning workflow  

**Alternative:** For non-urgent competitive threats, add to competitive intelligence tracker and evaluate during next quarterly planning cycle. Use "Competitive Monitoring" (lightweight weekly check-in) instead of urgent response workflow.
