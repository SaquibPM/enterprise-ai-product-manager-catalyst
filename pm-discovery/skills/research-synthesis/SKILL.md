---
title: "Research Synthesis: From Raw Data to Actionable Insights"
description: "Learn to synthesize research across 5 different sources—interviews, surveys, support tickets, CAB feedback, win/loss data—without cherry-picking to confirm your hypothesis. Master affinity mapping, theme extraction, signal weighting, and insight quality frameworks so you can tell the real story your data is telling."
purpose: "Synthesize disparate research sources into coherent, prioritized insights that inform product strategy and feature prioritization. Distinguish genuine customer needs from noise, weight evidence by source quality and confidence, and surface non-obvious patterns."
key_concepts:
  - "Affinity mapping methodology: How to organize hundreds of data points into meaningful patterns"
  - "Theme extraction: From observation → pattern → insight → opportunity"
  - "Source weighting: Treating CAB feedback differently from 1:1 interviews differently from support tickets"
  - "Signal vs. noise: Distinguishing genuine needs from one-off complaints"
  - "Insight quality rubric: What separates 'interesting observation' from 'actionable insight'"
  - "Buyer vs. user synthesis: Weighting conflicting signals from different stakeholder types"
  - "Confidence scoring: How certain are you about this insight?"
---

## Purpose & Context

You've done the research. You have 15 customer interviews, 200 NPS survey responses, 45 support tickets from last month, 3 CAB sessions, and win/loss data from 25 deals. Now what?

Most PMs get this wrong. They:
- Cherry-pick quotes that support their hypothesis
- Give equal weight to a CEO's offhand comment and a pattern across 50 customers
- Find 27 "key insights" (which means they found zero insights)
- Present findings without acknowledging conflicting data
- Turn every customer request into a feature ("customers want X → we should build X")

This skill teaches you to:
- **Organize raw research** into coherent themes without biasing toward your preferred outcome
- **Weight evidence** by source type and confidence level
- **Surface patterns** that matter while ignoring noise
- **Synthesize conflicting signals** from different customer segments
- **Articulate insights** at the right level of abstraction (not "users want a filter" but "users want to focus on high-value exceptions without distraction")
- **Build conviction** by showing the evidence, not just telling the conclusion

---

## Step 1: Collect & Organize Raw Data

Before you can synthesize, you need to capture data in a consistent format.

### Research Source Categories

**Tier 1: Direct Customer Feedback (Highest Confidence)**
- 1:1 customer interviews (transcribed or detailed notes)
- CAB session recordings or detailed notes
- Win/loss interviews with clear decision factors
- Customer advisory board written feedback
- *Why high confidence*: Customer expressed this directly to you; you can verify

**Tier 2: Structured Customer Input (Medium Confidence)**
- NPS survey responses with text feedback
- Post-purchase surveys
- Support tickets with customer problem statements
- Feature request tracking (with customer name attached)
- RFP evaluation forms from deals you won/lost
- *Why medium confidence*: Customer said it, but in a structured format that might limit their expression

**Tier 3: Behavioral Data (Medium Confidence, Different Type)**
- Feature usage analytics (what customers actually use vs. claim to use)
- Support ticket volume and category trends
- Churn and renewal patterns
- Deal win/loss patterns by segment/persona
- *Why medium confidence*: Inference about customer intent from behavior, not direct statement

**Tier 4: Indirect or Secondary Data (Lower Confidence)**
- Analyst reports about market trends
- Competitor customer review sites (G2, Capterra)
- Industry reports
- Anecdotal feedback from sales team
- *Why lower confidence*: Once or twice removed from your actual customers

### Data Organization Template

Create a master research log. Each row = one data point.

| Research ID | Date | Source Type | Source Detail | Customer Segment | Quote/Observation | Category | Stakeholder Role | Confidence |
|---|---|---|---|---|---|---|---|---|
| INT-001 | 2025-03-15 | 1:1 Interview | Janet, AP Manager, mid-market CPG | Mid-Market | "Exception management is 50% of my team's job" | Pain Point | User | High |
| INT-001 | 2025-03-15 | 1:1 Interview | Janet, AP Manager, mid-market CPG | Mid-Market | "We'd hire one fewer person if we could reduce exceptions by 60%" | Metric | Buyer | High |
| NPS-047 | 2025-03-10 | NPS Feedback | Response to "What's one thing we should improve?" | Enterprise | "Mobile approval would save us time during month-end" | Feature Request | User | Medium |
| SUP-234 | 2025-03-12 | Support Ticket | Ticket about invoice matching errors | Enterprise | "We're getting false positives on our 2-way match; sometimes correct invoices are flagged as exceptions" | Technical Problem | User | High |
| CAB-Q1 | 2025-03-05 | CAB Session | Q1 2025 CAB meeting with 8 strategic customers | Mixed | "The market is shifting toward AI-powered matching; every vendor will need this in 12-18 months" | Trend | Buyer | High |
| WINNLOSS-089 | 2025-02-20 | Win/Loss Interview | Lost to Competitor A, $500K deal | Mid-Market | "They could implement in 60 days; you need 90. That was the deciding factor for our CFO" | Competitive Loss | Buyer | High |

**Why this matters:**
- You can filter by confidence, source type, stakeholder role
- You won't accidentally treat "one person mentioned this in NPS" the same as "8 CAB customers all said this"
- You can trace back from insight to evidence
- You can identify gaps (e.g., "All feedback is from users; we have no buyer perspective on mobile approval")

---

## Step 2: Affinity Mapping

Affinity mapping is the structured way to find themes in messy data.

### The Process (60-90 minutes for 200 data points)

**Phase 1: Data Preparation**

1. Write each observation/quote on a digital "card" or spreadsheet row (one per row)
2. Include source and confidence level with each card
3. Ensure you have observations from all source types, not just interviews
4. Include both positive and negative feedback (what works and what doesn't)

**Phase 2: Initial Grouping (Without Forcing Themes)**

1. Read through all cards without judgment
2. Start grouping similar observations together
3. Don't force a theme yet; just say "these seem related"
4. Allow cards to surprise you—notice patterns you didn't expect
5. Create 15-25 initial buckets

**Example Initial Groupings (before naming themes):**

Bucket A (8 cards):
- "Exception management takes too long" (INT-003)
- "We spend 40% of our time on exceptions" (INT-007)
- "Exceptions are my biggest bottleneck" (INT-015)
- "I wish I could focus on vendor relationships instead of exceptions" (INT-018)
- "Every month-end we have exception chaos" (CAB-Q1)
- Support ticket about exception resolution delays (SUP-045)
- Support ticket about exception response time (SUP-067)
- NPS feedback: "Slow exception handling" (NPS-089)

Bucket B (6 cards):
- "Our CFO wants to know exceptions are accurate before we auto-approve" (INT-004)
- "We need an audit trail showing why we approved each exception" (INT-012)
- "Compliance wants visibility into all overrides" (WINNLOSS-045)
- "We were nervous about auto-approving without a paper trail" (INT-020)
- IT Security requirement: "Must maintain audit log" (RFP-DATA-012)
- Finance concern: "How do we prove this for audit?" (CAB-Q2)

Bucket C (4 cards):
- "SAP integration took 6 months and $200K" (INT-006)
- "We're hesitant about another big integration project" (INT-014)
- "If this requires a new IT project, it's dead on arrival" (INT-019)
- Lost deal reason: "Integration timeline was too long" (WINNLOSS-067)

**Phase 3: Theme Naming**

For each bucket, name the underlying theme:

**Bucket A → Theme: "Exception Management Consumes Disproportionate Time"**
- Core problem: Customers spend 30-50% of AP team time on exception handling (20-30% of invoices)
- Impact: Blocks time for strategic work (vendor management, contract optimization)
- Urgency: High (affects hiring plans, team morale)

**Bucket B → Theme: "Risk Aversion: Compliance & Audit Trail Requirements"**
- Core problem: Automation threatens audit trail visibility; compliance/internal controls concerns
- Impact: Customers won't adopt auto-approval without proof of legitimate exceptions
- Urgency: High (blockers deal adoption)

**Bucket C → Theme: "Integration Complexity is a Blocker"**
- Core problem: Previous integration projects took 6+ months and $200K+; customers burnt out
- Impact: High implementation risk perception; procurement skeptical of new vendor projects
- Urgency: High (customer willing to pay more for easier integration)

**Phase 4: Identify Cross-Theme Relationships**

Once you have themes, look for connections:

- Theme A (Exception Management) → Theme B (Compliance): "We could auto-approve exceptions, but only if we can prove they were legitimate"
- Theme B (Compliance) → Theme C (Integration): "Integration complexity means we can't add new systems; so compliance visibility must be built into existing SAP workflows"

These connections often reveal the *real* product strategy.

---

## Step 3: From Theme to Insight

Not every theme is an insight. Some are observations. Some are complaints. The best insights point to actionable opportunities.

### The Observation → Pattern → Insight → Opportunity Ladder

**OBSERVATION** (Raw data point)
- "Janet said exception management takes too long"
- "Three support tickets about exception resolution delays"

❌ This is not enough to act on. One person said it; you haven't verified if it's real.

→

**PATTERN** (Multiple data points pointing the same direction)
- 8 out of 12 customer interviews mention exception management consuming significant time
- Support ticket volume on exception handling is 3x higher than other categories
- CAB customers consistently mention exceptions as time sink
- Win/loss analysis shows "faster exception handling" is mentioned in 60% of wins

✓ Now you have evidence. This is a real pattern.

→

**INSIGHT** (What the pattern tells you about customer behavior or need)
- "Exception management is the primary productivity bottleneck for mid-market AP teams, consuming 30-50% of team time despite being 20-30% of invoice volume. Current manual exception resolution takes 15-45 minutes per invoice (depending on complexity), even though 80% of exceptions are categorizable rule-based issues (UOM mismatches, rounding, known surcharges)."

✓ This is an insight. It's specific, measurable, backed by evidence.

→

**OPPORTUNITY** (What this insight suggests you should do)
- "Build intelligent exception categorization that auto-approves rule-based exceptions (UOM, rounding, known vendor surcharges) and surfaces only true discrepancies. This would reduce exception resolution time from 20 minutes average to 3 minutes, freeing 10-15 hours per week per AP team."

✓ This is actionable. Engineering can scope it. Sales can measure ROI.

### Insight Quality Rubric

Use this to distinguish "interesting observation" from "investment-worthy insight":

| Dimension | Weak | Strong |
|-----------|------|--------|
| **Evidence** | "One customer mentioned it" or "We assume this" | "6+ customers independently mentioned it; supported by support ticket volume; validated in CAB; shown in usage data" |
| **Scope** | Applies to 1 customer or niche use case | Applies to 30%+ of target customer base; affects multiple personas |
| **Impact** | Nice-to-have (improves UX by 10%) | Material (saves 5+ hours/week, reduces cost by $50K+, enables new segment) |
| **Actionability** | Vague ("customers want better X") | Specific ("reduce exception resolution from 20min to 3min by auto-approving rule-based exceptions") |
| **Confidence** | Medium (some evidence but conflicting signals) | High (consistent across sources, no contradictions) |
| **Business Relevance** | Interesting but not tied to revenue/retention | Directly tied to deal win, customer retention, or segment expansion |

**Scoring Framework:**

Rate each potential insight on these dimensions (1-5 scale). Calculate average.

- **4.5-5.0**: Investment-worthy insight. Put on roadmap.
- **3.5-4.4**: Valid insight. Consider for roadmap. Validate further before committing.
- **2.5-3.4**: Interesting observation. Monitor for pattern growth. Don't invest yet.
- **Below 2.5**: Customer request or one-off feedback. Track but don't prioritize.

---

## Step 4: Handle Conflicting Signals

Your data will contradict itself. Enterprise customers want different things.

### Conflict Resolution Framework

**Conflict Type 1: Different Stakeholder Types Want Opposite Things**

Example:
- Users want: "Mobile app to approve invoices anywhere"
- Buyers want: "Reduced headcount spend, not new software"
- IT wants: "No new integrations; keep everything in SAP"

**How to resolve:**
1. Acknowledge the conflict explicitly: "Mobile approval matters to users; CFOs worry about ROI. These aren't contradictory—they're different jobs."
2. Determine which job is the *real* constraint: "What would prevent a deal?"
   - Mobile app absence = nice-to-have. Deal might still happen.
   - CFO ROI concern = deal blocker. Without ROI, they won't buy.
   - Integration complexity = deal blocker. Without light integration, they can't deploy.
3. Weight in product priority: Mobile app is P2 (nice-to-have). ROI clarity and integration simplicity are P0 (deal-makers).

**Conflict Type 2: Different Segments Want Different Things**

Example:
- Enterprise (>$1B revenue) wants: "Deep SAP integration, multi-entity consolidation"
- Mid-market ($100M-$1B) wants: "Fast implementation, simple integration"
- SMB (<$100M) wants: "Cheap, mobile-friendly, can set up in a day"

**How to resolve:**
1. Accept this is real: Different ICPs have different needs.
2. Determine which segment is your *primary* ICP: If you're focused on mid-market, that's your conflict resolver.
3. Build for primary ICP; make secondary segments work-able (not optimized).
4. In messaging, segment clearly: "For mid-market teams that need to deploy fast..."

**Conflict Type 3: Longitudinal Conflict (Needs Change Over Time)**

Example:
- Q4 2024 customer interviews: "We need to reduce exceptions"
- Q1 2025 same customers (renewed): "We need better vendor consolidation / duplicate detection"

**How to resolve:**
1. This is natural: First job is solving the pain point (exceptions). Second job is strategic improvement (consolidation).
2. Recognize customer journey: They're graduating from "fix my pain" to "optimize my process"
3. Build for the journey: "First release solves exception management. Second release adds vendor consolidation."
4. In GTM: Early messaging emphasizes pain relief. Later messaging emphasizes strategic value.

### Weighting Framework for Conflicting Signals

When signals conflict, use this to decide whose voice matters most:

```
SIGNAL WEIGHT = (Source Quality) × (Confidence) × (Scope) × (Business Impact)

Source Quality (1-5):
- 1 = Anecdotal, second-hand ("Someone said...")
- 2 = NPS survey feedback (customer responded to generic question)
- 3 = 1:1 interview (dedicated, but small sample)
- 4 = CAB feedback (strategic customers, deliberate setting)
- 5 = Win/loss data (proven by customer's actual decision)

Confidence (1-5):
- 1 = Speculative ("Customers might want...")
- 2 = Low ("One customer mentioned...")
- 3 = Medium ("3-4 customers mentioned independently")
- 4 = High ("6-8 customers consistently mentioned")
- 5 = Very high ("Shown in multiple data sources; usage data validates; all segments mention")

Scope (1-5):
- 1 = Niche use case (<5% of ICP affected)
- 2 = Small segment (5-15% of ICP affected)
- 3 = Meaningful segment (15-40% of ICP affected)
- 4 = Majority (40-70% of ICP affected)
- 5 = Broad (70%+ of ICP affected)

Business Impact (1-5):
- 1 = Nice-to-have (doesn't affect deal or retention)
- 2 = Hygiene factor (table-stakes; customers expect it)
- 3 = Meaningful differentiator (affects 10-15% of deals)
- 4 = Major differentiator (affects 30-50% of deals)
- 5 = Deal-maker / churn driver (deal blocker or primary retention driver)
```

**Example Conflict:**

Signal A: "We need mobile approval" (from NPS feedback)
- Source Quality: 2 (NPS survey)
- Confidence: 1 (One respondent mentioned it)
- Scope: 2 (Mobile approval use case affects maybe 10% of AP teams in field)
- Business Impact: 1 (Users want it; not a deal blocker)
- **Total Weight: 2 × 1 × 2 × 1 = 4**

Signal B: "Faster implementation is critical" (from win/loss interviews)
- Source Quality: 5 (Win/loss data)
- Confidence: 5 (Mentioned in 15 of 25 deals; clearly drove 60% of deal outcomes)
- Scope: 5 (Affects all customers; universally mentioned)
- Business Impact: 5 (Deal-maker; procurement requires <90 day deployment)
- **Total Weight: 5 × 5 × 5 × 5 = 625**

**Conclusion:** Signal B (faster implementation) matters 150x more than Signal A (mobile approval). Don't build mobile. Invest in implementation speed.

---

## Step 5: Worked Example—Synthesizing Enterprise Procurement Analytics Research

**Context:** You're a PM at a spend analytics company. You've collected research from 12 customer interviews, 200 NPS responses, 45 support tickets, 3 CAB sessions, and 25 win/loss deals. Your job: Extract 5-7 prioritized insights that will inform your next 6 months of product development.

### Raw Research Summary

**Interviews (12 customers, 8 hours total):**
- 4 enterprise procurement directors (>$10B annual spend)
- 4 mid-market procurement managers ($500M-$5B spend)
- 4 financial operations VPs (companies using spend analytics for cost control)

**NPS Survey (200 responses, 400-word feedback):**
- 42% promoters (NPS 9-10), saying: "Saves us thousands in duplicate detection," "Finally can see where our money goes," "Integration with our systems is seamless"
- 34% passives (NPS 7-8), saying: "Good tool but learning curve is steep," "Reporting is limited," "Data quality issues when vendors don't standardize names"
- 24% detractors (NPS 0-6), saying: "Doesn't integrate with our ProcurementBridge system," "Too expensive for the ROI we're seeing," "Takes too long to clean up vendor master data"

**Support Tickets (45 this month):**
- 12 about vendor master data quality (duplicates, inconsistent naming)
- 8 about reporting limitations (can't slice data the way they want)
- 7 about integration errors (failed data syncs, missing transactions)
- 6 about performance (system slow with 2M+ transactions)
- 5 about training/onboarding
- 7 miscellaneous

**CAB Sessions (Q1 2025, 9 customers attended):**
- All wanted: "Better visibility into tail spend and rogue spend"
- 7 mentioned: "We need to consolidate vendors; we're buying from thousands of variations of the same vendor"
- 5 said: "We're losing money to duplicate invoices from the same vendor under different names"
- All said: "Implementation and data cleanup took longer than we expected"

**Win/Loss Interviews (25 deals: 15 wins, 10 losses):**
- Wins (15): Average decision factors mentioned: "Better ROI" (11), "Easier integration" (10), "Vendor consolidation" (7)
- Losses (10): "Price too high relative to value" (6), "Didn't address our specific need" (5), "Competitor had pre-existing integration with our system" (7)

### Synthesis Process

**Raw Data Organization:**

Create master research log:

| ID | Source | Segment | Quote | Category | Stake | Conf |
|---|---|---|---|---|---|---|
| INT-001 | Interview | Enterprise | "We have 4,200 vendor variations for 1,100 unique vendors" | Problem | Buyer | H |
| INT-002 | Interview | Mid-Market | "Consolidating vendors saves us $2M+ per year in improved discounts" | Impact | Buyer | H |
| INT-003 | Interview | FinOps | "We lost $450K to duplicate payments last year" | Cost of Problem | Buyer | H |
| INT-004 | Interview | Enterprise | "Implementation took 18 weeks and our team did 500 hours of data cleanup" | Timeline | Buyer | H |
| NPS-042 | NPS | Unknown | "Reporting is too limited for our needs" | Feature Gap | User | M |
| NPS-089 | NPS | Unknown | "Doesn't integrate with our ProcurementBridge system" | Integration Gap | Tech | M |
| SUP-001 | Support | Mid-Market | "Vendor master data has 50% duplicates we have to manually clean" | Data Quality | User | H |
| CAB-Q1-01 | CAB | Enterprise | "We need to consolidate vendors; we're buying from thousands of variations" | Need | Buyer | H |
| WINNLOSS-001 | Win/Loss | Mid-Market | "Vendor consolidation was the deciding factor; they could reduce our vendor count from 3K to 500" | Win Reason | Buyer | H |
| WINNLOSS-010 | Win/Loss | Enterprise | "Implementation took too long for us; we chose competitor who had faster timeline" | Loss Reason | Buyer | H |

**Affinity Mapping:**

Bucket A (15 cards): Vendor consolidation/deduplication pain
- 4 interview quotes about vendor count explosion
- 7 CAB customers mentioned vendor consolidation need
- 2 win/loss deals won on vendor consolidation feature
- 1 support ticket about duplicate vendor detection
- 1 NPS response about needing better dedup

→ Theme: **"Vendor Master Data Chaos Drives 30%+ of Procurement Costs"**

Bucket B (11 cards): Implementation & data cleanup burden
- 4 interviews: implementation took 18+ weeks, 500+ hours of data cleanup
- 5 CAB customers: "Longer than expected"
- 1 support ticket: vendor master data cleanup
- 1 win/loss loss reason: "Implementation timeline too long"

→ Theme: **"Data Cleanup & Implementation Complexity is a Deployment Barrier"**

Bucket C (8 cards): Integration gaps
- 2 NPS: "Doesn't integrate with our system" (ProcurementBridge, others)
- 3 support tickets: "Failed data syncs"
- 2 win/loss: "Competitor had existing integration"
- 1 interview: "We couldn't connect to our procurement system"

→ Theme: **"Integration Gaps Create Data Flow Friction"**

Bucket D (6 cards): Reporting limitations
- 2 NPS: "Reporting is limited"
- 4 support tickets: "Can't slice data the way we need"

→ Theme: **"Reporting Flexibility is Table-Stakes Expectation"**

Bucket E (5 cards): ROI & pricing concerns
- 4 NPS detractors: "Too expensive for ROI we're seeing"
- 1 win/loss: "Price too high relative to value"

→ Theme: **"ROI Clarity & Pricing Transparency Matter for Mid-Market"**

### Theme-to-Insight Translation

**Theme A: Vendor Master Data Chaos**

Insight (draft):
"Enterprises are managing 2,000-5,000 active vendors, but due to inconsistent naming, address variations, and duplicate master records, they actually source from 4,000-8,000 vendor variations. This causes: (1) lost volume discounts because spend is fragmented, (2) compliance risk from rogue/unapproved vendor spend, (3) duplicate invoice risk. One enterprise customer measured $2M+ in lost discounts annually from vendor fragmentation."

Evidence:
- 4 interviews, all enterprise: "We have 4K+ variations for 1.2K unique vendors"
- 7 CAB customers (all large enterprises): "Vendor consolidation is priority"
- 2 win/loss wins: Decided on us because of vendor consolidation capability
- 12 support tickets: Vendor master data cleanup
- ROI measurement: $2M+ annual savings per enterprise customer

Confidence: **5/5**. This is consistent across all data sources.

Impact:
- **Opportunity A**: Make vendor consolidation a first-class feature, not an add-on
- **Opportunity B**: Create ROI calculator showing "$2M saved per $500K annual cost = 4x ROI"
- **Opportunity C**: In marketing, lead with vendor consolidation (not spend visibility)

---

**Theme B: Implementation Complexity**

Insight (draft):
"Customers expect spend analytics deployment to take 8-12 weeks. Current implementations average 18-24 weeks, primarily due to vendor master data cleanup (500+ hours manual work per customer). This misalignment drives: (1) deal delays/pushback on timeline, (2) customer dissatisfaction at go-live (they expected faster), (3) support burden (customers call asking 'why is this taking so long?'). CAB customers said 'implementation took longer than expected' in all 9 sessions."

Evidence:
- All 12 interviews: Implementation took 18-24 weeks (unexpected)
- CAB feedback: "Longer than expected" (all 9)
- 1 win/loss loss: Competitor promised 12 weeks; we said 18+
- 5 support tickets: Questions about implementation duration

Confidence: **4.5/5**. Consistent but mostly from customers we sold to (potential bias that they're more patient). No data from customers who *rejected* us over timeline.

Impact:
- **Opportunity A**: Offer a "fast-track" implementation (12 weeks) with fixed vendor consolidation rules (not custom)
- **Opportunity B**: Improve vendor matching algorithms to reduce manual data cleanup hours
- **Opportunity C**: In sales, set expectations correctly and over-deliver ("18 weeks average, but we've delivered in 12")
- **Opportunity D**: Create "implementation playbook" so customers understand what they're signing up for

---

**Theme C: Integration Gaps**

Insight (draft):
"Customers evaluate us based on integration with their procurement system (SAP Ariba, Coupa, ProcurementBridge, Oracle Procure-to-Pay). When we lack native integration, customers choose competitors with plug-and-play connections. This affects ~30% of deals (based on win/loss data: 3 out of 10 losses cited 'pre-existing integration' as deciding factor)."

Evidence:
- 2 NPS detractors: Can't connect to their system
- 3 support tickets: Failed data syncs (we have API; they struggle to implement)
- 2 win/loss losses: Competitor had native integration
- 1 interview: They couldn't use us because of integration gap

Confidence: **3.5/5**. Limited evidence. This affects some deals but may not be universal pain.

Impact:
- **Opportunity A**: Build native connectors to top 3 procurement platforms (Ariba, Coupa, ProcurementBridge) within 6 months
- **Opportunity B**: Until then, publish integration architecture and timeframes so customers know what they're getting
- **Opportunity C**: Create pre-built integration templates for common platforms so customers can self-serve

---

**Theme D: Reporting Limitations**

Insight (draft):
"Power users want flexible reporting (ability to slice by department, cost center, commodity, vendor, geography, etc.). Current reporting is limited to pre-built dashboards. 2 NPS detractors and 4 support tickets mention this. This is likely a *hygiene factor* (table-stakes expectation) rather than a differentiator, but its absence creates friction."

Evidence:
- 2 NPS detractors mentioned reporting gaps
- 4 support tickets: "Can't slice data the way we need"
- 0 interviews mentioned reporting (not top of mind, probably because our interviews focused on procurement/finance teams, not analysts who use reports)

Confidence: **2.5/5**. Low confidence. Only NPS and support data. Interviews (which are higher quality) didn't surface this. Might be niche need or might be table-stakes. Need to validate.

Impact:
- **Opportunity A**: Survey top 10 customers specifically about reporting needs before investing
- **Opportunity B**: If validated, build flexible reporting/BI export within 3-6 months (not urgent)

---

**Theme E: ROI & Pricing**

Insight (draft):
"Mid-market customers ($500M-$5B spend) are price-sensitive. They need to see <12 month ROI on our platform. When they can't calculate clear ROI, they view us as too expensive. This affects mid-market segment (~40% of TAM). When enterprise has $2M+ savings and we cost $500K ACV, ROI is clear. When mid-market has $200K in savings and we cost $200K ACV, ROI feels weak."

Evidence:
- 4 NPS detractors: "Too expensive for ROI"
- 1 win/loss loss: "Price too high relative to value"
- Interviews: Enterprise customers calculate $2M+ ROI; mid-market customers don't mention ROI
- CAB (strategic, large customers): No pricing concerns

Confidence: **4/5**. Consistent signal from NPS and win/loss. But limited insight into *why* mid-market can't find ROI.

Impact:
- **Opportunity A**: Create ROI calculator for mid-market that shows conservative estimates ($50K annual savings from vendor consolidation, duplicate detection, etc.)
- **Opportunity B**: Investigate: Are we missing $200K in ROI that exists but isn't obvious? (Maybe mid-market could save more if they focused on tail spend management)
- **Opportunity C**: Consider tiered pricing: Lower ACV for mid-market, higher for enterprise

---

### Prioritized Insights

(Using insight quality rubric: evidence + confidence + scope + business impact)

**Insight 1: Vendor Master Data Chaos is the Primary ROI Driver (Score: 4.8/5)**

Evidence: 4 enterprise interviews, 7 CAB customers, 2 win/loss wins, 12 support tickets
Confidence: 5/5 (consistent across all sources)
Scope: 5/5 (affects all enterprise procurement teams; affects 40% of revenue opportunity)
Business Impact: 5/5 (primary selling point; drives $2M+ per enterprise customer; affects 30% of wins)

**→ Action:** Make vendor consolidation/deduplication a first-class feature in next release. Reposition marketing message to lead with "Consolidate Your Vendor Fragmentation" (not "Spend Visibility").

---

**Insight 2: Implementation Timeline Misalignment Drives Customer Dissatisfaction (Score: 4.4/5)**

Evidence: 12 interviews, 9 CAB feedback, 1 win/loss loss, 5 support tickets
Confidence: 4/5 (strong but mostly from customers we already sold to; no data from customers who rejected us for timeline)
Scope: 5/5 (affects all customers; universal expectation mismatch)
Business Impact: 4/5 (affects customer satisfaction and support burden; some deal impact)

**→ Action:** Invest in "fast-track" implementation option (12-week SLA for mid-market, 16-week for enterprise). Create implementation playbook to set expectations. Improve vendor matching to reduce manual data cleanup.

---

**Insight 3: Integration Gaps Lose 30% of Competitive Deals (Score: 3.5/5)**

Evidence: 2 NPS, 3 support tickets, 2 win/loss losses, 1 interview
Confidence: 3.5/5 (limited data; only 2 direct losses attributable)
Scope: 4/5 (affects customers already using SAP Ariba, Coupa, ProcurementBridge; maybe 30% of TAM)
Business Impact: 4/5 (deal-maker for customers on these platforms)

**→ Action:** Build native connectors to top 3 procurement platforms (Ariba, Coupa, ProcurementBridge). Within 6 months: at least Ariba native connector. Publish integration roadmap to prospects.

---

**Insight 4: Reporting Flexibility is Expected but Not Differentiating (Score: 2.8/5)**

Evidence: 2 NPS detractors, 4 support tickets, 0 interviews
Confidence: 2/5 (low; not surfaced in interviews; might be niche need)
Scope: 2/5 (affects analysts and power users; not main buyer)
Business Impact: 2/5 (table-stakes expectation; absence creates friction but not deal-maker)

**→ Action:** Survey top 10 power users about reporting needs. If consistent need, prioritize in Q3. If niche, de-prioritize.

---

**Insight 5: Mid-Market ROI Clarity is Pricing Blocker (Score: 3.8/5)**

Evidence: 4 NPS detractors, 1 win/loss loss, pricing structure analysis
Confidence: 4/5 (consistent signal from price-sensitive segment)
Scope: 3/5 (affects mid-market segment; ~40% of TAM)
Business Impact: 4/5 (segment retention and win rate)

**→ Action:** (1) Create ROI calculator showing $50K-$100K annual savings for mid-market. (2) Investigate: do mid-market customers actually have less ROI, or are we just not helping them calculate it? (3) Consider pricing tier for mid-market: $50K-$75K ACV (vs. enterprise $300K+).

---

## Anti-Patterns in Research Synthesis

### Anti-Pattern 1: The Confirmation Bias Trap

**What it looks like:**
- You believed mobile app was critical
- You find 2 NPS responses mentioning mobile app
- You ignore the 12 interviews and CAB feedback that never mentioned mobile
- You conclude "Mobile app is a top-3 priority"

**Why it's dangerous:**
- You build features based on your assumptions, not customer data
- You miss the real priorities hiding in your research
- You waste 3 months building something 2 people wanted
- You ignore signals that contradict your belief

**How to fix it:**
- Start synthesis by listing your pre-existing beliefs: "I think mobile approval is critical"
- As you analyze research, flag when evidence contradicts your belief: "Actually, interviews never mentioned mobile; only 2 NPS responses, and both were detractors"
- Explicitly weight evidence by quality, not by agreement with your hypothesis
- Share research with someone who disagrees with you; ask them to find evidence that contradicts your conclusion

### Anti-Pattern 2: Treating All Research Sources as Equal

**What it looks like:**
- You count: "5 people mentioned need for X"
- 1 from enterprise CAB (strategic customer, highly intentional)
- 1 from mid-market 1:1 interview (dedicated conversation)
- 1 from NPS survey (generic question, took 30 seconds to answer)
- 2 from support tickets (complaining about something, not articulating needs)
- You treat all 5 equally in your analysis

**Why it's dangerous:**
- CAB feedback from a $10M ACV customer matters more than NPS feedback from a churned SMB customer
- You miss the signal buried in high-quality data
- You build for what the masses want, not what the best customers need

**How to fix it:**
- Weight research sources: Win/loss > CAB > 1:1 interviews > NPS > support tickets
- Track source and confidence with every observation
- Calculate "weighted evidence" not just "mention count"
- If the signal comes from detractors and support tickets but contradicts CAB feedback from strategic customers, lean toward CAB

### Anti-Pattern 3: Finding 27 Key Insights (a.k.a. Finding Zero)

**What it looks like:**
- You complete synthesis and present: "Here are 27 key insights"
- Each insight is actionable but they're not prioritized
- Sales team gets confused ("Which should we message?")
- Engineering team can't prioritize ("We can't build all 27 features")
- Nothing gets prioritized; nothing gets done

**Why it's dangerous:**
- Insights without prioritization are noise
- You haven't done the hard work of determining what matters most
- Decision-makers can't act on "27 things"

**How to fix it:**
- Limit yourself to 5-7 core insights (forced constraint)
- Prioritize using impact matrix (confidence × business impact)
- For each insight, specify: "If we act on this, what changes in our business?"
- Insights that don't drive business outcomes are details, not insights

### Anti-Pattern 4: Insight Without Evidence

**What it looks like:**
- Insight: "Customers want better vendor consolidation"
- When asked for evidence, you say: "Everyone mentioned it"
- When pressed: "Well, the CAB session touched on it," "I saw a few support tickets," "One interview was about this"
- You can't point to specific data

**Why it's dangerous:**
- You lose credibility when challenged
- Decision-makers can't evaluate the strength of your recommendation
- You might be wrong and have no way to know
- Next quarter, you can't measure whether you actually solved this

**How to fix it:**
- For every insight, document: The exact quotes, the source, the customer context
- Create an insight evidence package: "Here are 12 data points supporting this insight"
- Be specific: Not "enterprise customers want X" but "Janet, Sarah, and Marcus (3 enterprise procurement directors) independently mentioned X in interviews; 7 CAB customers voted for X as top-3 priority; 2 win/loss deals were won on this capability"
- Share evidence with stakeholders; let them evaluate confidence

---

## Synthesis Documentation Template

For each core insight, create one-page brief:

**INSIGHT BRIEF**

**Insight:** [Clear, specific statement of what you learned]

**Business Implication:** [Why does this matter to revenue, retention, or market positioning?]

**Evidence:**
- [Source 1: High-confidence source, specific quote/data point]
- [Source 2: Medium-confidence, supporting evidence]
- [Source 3: Related behavioral evidence]

**Segment Impact:**
- Enterprise: [Does this apply? How acutely?]
- Mid-Market: [Applies? Severity?]
- SMB: [Applies? Severity?]

**Confidence Level:** [4.8/5 - Consistent across all data sources, backed by win/loss data]

**Conflicting Signals:** [If any data contradicts this, document it. Why does the main insight still hold?]

**Recommended Action:** [If we believe this insight, what should we do? Product change? GTM change? Pricing change?]

**How We'll Measure Success:** [If we act on this, how will we know we succeeded? Metric or qualitative outcome?]

---

## References & Further Reading

- Affinity Mapping: Nielsen Norman Group's guide to card sorting and affinity mapping
- Patton, Jeff. *User Story Mapping: Discover the Whole Story, Build the Right Product*. O'Reilly, 2014.
- Goodman, Elizabeth et al. *Design Research: What We Know*. SXD Group, 2012.
- Krueger, Richard A. and Mary Anne Casey. *Focus Groups: A Practical Guide for Applied Research*. SAGE Publications, 2014. (Synthesis techniques for qualitative research)

---

**VERSION:** 1.0  
**LAST UPDATED:** 2026-04-12  
**SAQUIB JAWED ENTERPRISE PM PLAYBOOK**
