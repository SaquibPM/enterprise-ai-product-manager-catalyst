---
name: sales-enablement
title: "Sales Enablement for Enterprise Product Launches"
category: "Sales & Go-to-Market"
description: "Build battlecards, demo scripts, and ROI calculators to enable enterprise sales teams on complex product launches"
difficulty: "Advanced"
skills_required: ["Sales Strategy", "Product Knowledge", "Deal Economics"]
duration_minutes: 45
---

# Sales Enablement for Enterprise Product Launches

## Purpose

Enterprise software sales requires a completely different playbook than SMB sales. You're not measuring short demos or quick closes. You're orchestrating multi-stakeholder buying committees, handling IT security reviews, navigating procurement questions, and quantifying ROI in terms that CFOs understand. This skill teaches you how to build sales enablement materials (battlecards, demo scripts, ROI frameworks) that actually help sales teams win complex enterprise deals.

## Key Concepts

### The Enterprise Sales Buying Committee

Every enterprise deal involves multiple buyers with different priorities:

**Economic Buyer** (Controls budget, approves vendor)
- Usually: CFO, VP Finance, VP Procurement, Head of Category
- Cares about: Total cost of ownership, ROI payback period, vendor stability
- Objection: "How much does this really cost? What's the payback?"
- Your answer: Quantified ROI, reference customer economics, payment terms flexibility

**Technical Buyer** (Evaluates if it works, security, integration)
- Usually: IT Security, VP IT, Systems Architect, Integration Lead
- Cares about: Security certifications (SOC 2, HIPAA, FedRAMP), API availability, data residency, audit trails
- Objection: "Does this meet our security requirements? How do you handle our data?"
- Your answer: Security questionnaire response library (SIG, CAIQ, custom), data architecture diagrams, audit logs

**User Buyer** (Will actually use the product)
- Usually: Category Manager, Procurement Analyst, VP Operations, Department Head
- Cares about: Does this solve my actual workflow problem? Will my team use it?
- Objection: "This doesn't work like our current process. Our team won't adopt it."
- Your answer: Workflow walk-through, change management resources, training plan

**Procurement Buyer** (Manages vendor selection process, RFP, contract)
- Usually: Procurement Manager, Chief Procurement Officer, Legal
- Cares about: Vendor evaluation criteria scorecard, RFP response accuracy, contract terms, SLAs
- Objection: "Your response to question 3.2.1 is unclear. Can you clarify?"
- Your answer: RFP response library, contract templates, SLA documentation

Your battlecard and demo script must address all four buyers simultaneously.

### Battlecard Template (For Sales Team Use)

```
PRODUCT NAME BATTLECARD
=======================

SITUATION
When should a sales rep pull out this card?
Example: "When a prospect asks about cost savings in invoice processing"

POSITIONING STATEMENT (40 words max)
Your core value proposition that resonates with all four buyers.
Example: "AI-powered invoice matching reduces manual invoice processing by 85%, 
eliminating duplicate payments, coding errors, and vendor disputes. Implementation: 
4 weeks. ROI payback: 6 weeks."

KEY PROOF POINTS (Specific, Quantified, Enterprise-Relevant)
- [Customer] saved $[X]M in [X] months through [specific capability]
- [Customer] reduced invoice processing time from [X] hours to [X] hours
- [Industry benchmark] shows [X]% ROI in year 1 for similar implementations

Example proof points for invoice matching:
- Fortune 500 manufacturing customer: $8.2M in duplicate payments eliminated 
  in first 90 days (caught by ML before approval)
- Healthcare system: Reduced invoice approval cycle from 14 days to 3 days 
  (enabled 40 FTEs to focus on exceptions, not routine coding)
- Global retail customer: 2.3% savings through vendor consolidation insights 
  (our ML identified suppliers with >20% price variance)

VALUE QUANTIFICATION FRAMEWORK
Define the economic math so sales can adapt to customer size:

Base Variables:
- Annual invoice volume: [Gather from customer]
- % manual invoices: [Typical: 30-40%]
- Cost per manual invoice: [Typical: $8-15]
- Duplicate/error rate: [Typical: 2-5%]

Calculations:
1. Manual Processing Cost: 
   Annual invoices × % manual × cost per invoice
   Example: 100K invoices × 35% manual × $12 per = $420K annual

2. Savings from Automation:
   Manual cost × 85% automation rate = $357K
   
3. Savings from Error Prevention:
   Total invoices × 3% error rate × avg error cost ($2,000)
   100K × 3% × $2K = $6M in duplicate/fraud prevention

4. Total Year 1 Benefit:
   $357K + $6M = $6.357M

5. Implementation Cost:
   Software: $80K
   Services (4-week setup): $30K
   Internal effort (100 hours × $150/hr): $15K
   Total: $125K

6. ROI:
   ($6.357M - $125K) / $125K = 5,086% Year 1 ROI
   Payback: $125K / ($6.357M / 365 days) = 7 days

Note: Conservative scenario shows only automation savings ($357K), still 
386% ROI with 5-week payback.

HANDLES FOR COMMON OBJECTIONS

Objection: "We already have AP automation"
Response: "Most teams do have basic workflow automation. But you're probably 
still manually coding 30-40% of invoices (3-way matches, exceptions, new vendors). 
Our customers tell us the game-changer is AI understanding what's unusual—matching 
our system caught duplicate vendor invoices their existing tool missed, because it 
was just matching PO/invoice/receipt, not catching that this invoice was already 
paid 3 months ago. Would that kind of intelligence be valuable for your team?"
Proof point: [Reference specific customer catching duplicates]

Objection: "Implementation will disrupt our process"
Response: "I hear that—change is hard. Here's what we've learned: the setup 
itself is 4 weeks, but we do it in parallel with your team. We don't require 
system downtime. By week 3, you see the first set of AI exceptions for your 
team to review—some might be errors you catch, some legitimate, some to investigate. 
You're not changing your approval workflow on day 1. You're adding an extra lens 
on top. Your team's adoption cycle is usually 3-4 weeks, not the implementation 
4 weeks."
Proof point: [Healthcare customer case study: 0 downtime, staged adoption]

Objection: "What if the AI makes mistakes and approves invalid invoices?"
Response: "Great question—this is exactly why enterprises care about explainability. 
Our AI never approves invoices. It's a recommendation engine. It flags the top 5% 
most likely to be errors or duplicates (with reasoning: 'This invoice matches a 
prior PO, but we already received/paid for same scope on [date]'). Your team 
reviews and approves/rejects. If the AI misses something or flags something 
incorrectly, your team corrects it and the model learns. One customer in month 3 
is now catching 97% of duplicates vs. ~40% pre-implementation, because their team 
is now equipped to think about what 'unusual' looks like."
Proof point: [Audit trail dashboard showing model learning over time]

Objection: "Your competitor has been around longer. Are you stable enough?"
Response: "Fair question. Here's what I'd point to: (1) We're backed by [investor], 
committed to this space for decade. (2) Security: SOC 2 Type II, HIPAA, FedRAMP 
in progress. (3) Customer retention: 98% net retention, customers expanding usage, 
not churning. (4) Enterprise support: You get dedicated success manager, 24/7 
support, 99.9% uptime SLA. And honestly, the innovation speed—most of our recent 
customers chose us over [competitor] because we ship new AI capabilities monthly 
while [competitor] is still on annual releases."
Proof point: [Customer testimonial comparing us to competitor they were 
considering]

COMPETITIVE DIFFERENTIATION

vs. Competitor A [Describe their strength/weakness]
- They win on: [What they're good at]
- We win on: [What we're better at]
- Price anchor: [How do we position pricing vs. them]
- Their advantage: [Acknowledge it, don't deny it]
- How to sell against them: [Specific positioning language]

Example:

vs. UiPath (RPA-Based Automation)
- They win on: Widespread brand awareness, huge customer base, rule-based 
  workflow automation
- We win on: AI-driven exception handling (UiPath can't learn from your data), 
  faster ROI (UiPath requires IT/dev resources, we're business-user-friendly), 
  invoice-domain expertise (they're general automation, we're deep in AP)
- Price anchor: We position at 40-50% UiPath cost (they're expensive, heavy 
  infrastructure)
- Their advantage: Established vendor, broader RPA use cases beyond AP
- Sell against them: "UiPath is powerful if you have IT resources to build and 
  maintain automations. But with invoice diversity (format variations, new vendors), 
  you end up with exception rate >20% still handled manually. We start with 
  invoice expertise and let AI learn your specific patterns and exceptions."

PRICING GUIDANCE FOR SALES REPS

Base Pricing: [Your standard pricing for user seat count / transaction volume]
Example: $80K base for up to 100K invoices/year

Volume Tiers:
- 0-50K invoices: $50K
- 50K-200K invoices: $80K
- 200K-500K invoices: $150K
- 500K+: Custom

Discount Authority:
- Standard rep authority: 10% discount (needs no approval)
- Sales manager authority: 15% discount (sales manager approval)
- VP Sales authority: 20% discount (VP approval, board visibility)
- Max discount: 25% (CEO approval only, for strategic accounts)

Pricing Negotiation Scenarios:
- Multi-year commitment (3-year): 10% discount on annual cost
- Bundled with other products: 8-12% discount
- Quick decision (sign within 7 days): 5% discount
- Procurement committee evaluation (needs to look competitive): 10-12% discount

Deal Structures to Offer:
- Annual contract: Standard pricing
- 3-year prepaid: 5% discount on total cost ($240K → $228K)
- Monthly subscription: +15% premium (example: $80K annual = $6,667/month standard, 
  or $7,667/month for monthly)
- SaaS vs. On-premise: Same pricing, on-premise = support premium (20% additional)

Contract Terms That Win Deals:
- 60-day implementation SLA (builds confidence)
- 30-day termination clause if ROI not achieved (removes customer risk)
- Volume discounts on added transaction volume (upsell incentive)
- Multi-year pricing lock (customer incentive)

NEXT STEPS FRAMEWORK

If customer engaged/interested:
→ Schedule demo with [User Buyer persona] (category manager, accounts payable)
→ Include [Technical Buyer] (IT, security) for security questions
→ Offer 2-week POC (bring 10K invoices, run our ML, show actual results)
→ Identify procurement process: RFP? Direct vendor? Committee evaluation?
→ Get economic sponsor involved early (CFO, VP Procurement—not just end user)

If customer skeptical:
→ Share reference customer case study matching their size/industry
→ Offer POC to prove ROI on their actual data
→ Escalate to solutions engineer for technical deep-dive
→ Arrange VP-to-VP conversation (your VP Sales with their VP Procurement)

If customer stalled:
→ Understand the real blocker (usually budget, competing projects, or security)
→ If budget: "Can we scope a Phase 1 for smaller invoice population to prove ROI?"
→ If competing: "What would make this a priority vs. [competing project]?"
→ If security: Loop in your compliance team for questionnaire/audit

FAQs FOR SALES TEAM

Q: How long is implementation really?
A: 4 weeks standard. Week 1: setup, data loading, security review. Week 2-3: 
model training, testing. Week 4: go-live. Customer adoption usually 3-4 weeks 
after go-live.

Q: Do we need their IT team heavily involved?
A: Minimal IT involvement. They need to: (1) approve data sharing (our security 
review), (2) set up API integration (if they want real-time, optional), (3) test 
for 1 week before go-live. Most of the work is our professional services team.

Q: What's the biggest objection we encounter?
A: "We don't trust AI." Answer: "Fair. Most of your customers said the same thing 
until they saw it catch duplicate invoices they missed manually. It's not replacing 
people, it's helping people catch what humans miss."

Q: How do we handle "your competitor said X?"
A: Get specific. Ask them what competitor, what capability, what ROI claim. Then 
provide counter-proof-point from similar customer. "Competitor X claims 90% automation. 
Our customers average 82% automation because we're realistic about exceptions. But 
that 82% automation saves our typical customer $400K+ annually, which is the number 
that matters."

```

### Demo Script Framework (For Sales & Solutions Engineers)

```
DEMO SCRIPT: [PRODUCT NAME]
Typical length: 25-30 minutes
Audience: Category Manager, AP Manager, CFO, IT Security Lead (all 4 buyers if possible)

DISCOVERY PHASE (5 minutes)
Before demoing, understand their world:

1. Current Process Pain
"Walk me through how you currently handle invoice discrepancies. If an invoice 
comes in for $10K and your PO was for $8K, what happens?"

[Listen for: manual email hunt, multiple approvals, days of back-and-forth, 
spreadsheets, vendor calls]

Why ask: This tells you what they value. If they said "it takes 3 days to figure 
out", your ROI story emphasizes speed. If they said "we miss some duplicates", 
emphasize accuracy.

2. Stakeholders & Decision Process
"Who's in the room making decisions? Who ultimately owns AP? Who controls budget?"

[Goal: Understand who's most important. If CFO is there, emphasize ROI. If AP 
Manager is there, emphasize workflow fit.]

3. Competitive Context
"Are you evaluating other solutions? How are you comparing them?"

[Listen for: RPA vs. AI vs. basic workflow automation, procurement vs. internal IT 
vs. business buy]

4. Timeline
"If we could show that your team is missing 3-5% of duplicate invoices (worth 
finding), how quickly could you move to a decision?"

[Goal: Understand budget cycle. "We have budget in Q3" vs. "Need to wait for 
annual planning" completely changes your approach.]

PAIN VALIDATION (5 minutes)

Now that you understand their world, validate their core problem:

"Let me make sure I understand. You're seeing three pain points:
1. Manual invoice review takes 3 days when there are discrepancies
2. You're probably missing 2-3% of duplicate invoices because your team can't 
   review everything
3. Your vendors sometimes dispute invoices because they're not coded correctly on 
   your side

Are those the top issues? What would change if you could catch duplicates before 
approval and resolve discrepancies in 2 hours instead of 3 days?"

[Goal: Get them to articulate the problem in their own words. If they're not 
feeling the pain, your demo won't land. This is the emotional/business case 
foundation.]

SOLUTION DEMO PHASE (12 minutes)

Walk through real customer data (theirs if POC, ours if not):

1. Current State Dashboard (2 min)
"Here's what we typically see. This is real data from [similar customer]. They 
have about 80K invoices a year coming through. On average, 35% of invoices require 
manual review because they don't match the PO or receipt perfectly."

[Show: Invoice volume by vendor, % manual vs. automatic, average resolution time, 
current error rate]

Why: Benchmarks their situation against real customers. If they're at 40% manual 
and industry standard is 35%, they know they're slightly worse than average (good 
for your case).

2. Data Upload (1 min)
"We take your invoice and PO data as a secure upload. No API required. We've 
loaded the last 30 days of [similar customer]'s data."

Why: Removes technical objections. "It's easy to set up" is important for IT 
buyer confidence.

3. The AI Model's Work (3 min)
"Our model analyzes every single invoice against three dimensions:

(a) PO Matching: Does the invoice match a PO? If yes, is it reasonable (amount 
    within variance tolerance, quantity matches)?
(b) Vendor Pattern Analysis: Is this vendor's invoice pattern normal? Or are they 
    sending invoices 2x larger than usual?
(c) Duplicate Detection: Has this invoice (or something suspiciously similar) 
    already been paid?

Every invoice gets a 'confidence score' from 0-100. Zero means 'definitely normal, 
approve automatically.' 100 means 'definitely unusual, human review required.'"

[Show visualization: histogram of scores. Most invoices should cluster at 0-20 
(normal), with outliers at 80-100 (exceptions)]

Why: Demonstrates the AI is explainable, not a black box. This is critical for 
IT/Procurement buyers.

4. Exception Handling & Recommendations (4 min)
"Here are the top 10 flagged exceptions from [customer]'s data. Let me show you 
what the model found:

Invoice #54223: Flagged 92/100
- Vendor: [Vendor A]
- Amount: $12,500
- Unusual because: Last 5 invoices from this vendor averaged $2,000. This is 5x 
  normal.
- PO: Purchase order says $3,000, so this is 4x PO amount.
- Model recommendation: 'Human review—vendor over-billing'

What actually happened: [Customer] called vendor, vendor agreed was clerical error, 
credit issued."

[Show 2-3 more examples. Mix: (1) caught duplicate, (2) caught error, (3) legitimate 
exception model flagged]

Why: Concrete examples build confidence. Shows the model catches real problems.

5. Explainability (2 min)
"For each flag, we show exactly why. Not a black box. Your team can look at the 
model's reasoning and say 'that's valid' or 'no, that vendor actually charges 
premium for rush delivery—I know they're expensive but correct.' Model learns that 
pattern."

[Show audit trail: model flag → human override → model updates its understanding]

Why: Addresses the "we don't trust AI" objection. Explainability = control.

VALUE QUANTIFICATION (3 minutes)

Now tie it to their economics:

"You mentioned you process about [X] invoices annually. Let me run the math:

Current State:
- Invoice volume: 100K/year
- % requiring manual review: 35% = 35K
- Cost per manual review: $12 (labor)
- Annual cost: 35K × $12 = $420K

With our system:
- We automate 85% of manual reviews
- Manual invoices drop to 5.25K
- Cost: 5.25K × $12 = $63K
- Savings on automation: $357K

Error Prevention:
- Your current error rate (duplicates, miscoding): ~3%
- 3% × 100K = 3,000 errors/year
- Average value of each error: $2,000 (duplicate payment, coding loss)
- Value of errors you're currently losing: $6M
- Our model catches 85% of these: $5.1M in prevented losses

Total Year 1 Benefit:
- Process savings: $357K
- Error prevention: $5.1M
- Total: $5.457M

Implementation Cost:
- Software: $80K
- Professional services (4 weeks): $30K
- Your internal effort (100 hours): $15K
- Total: $125K

ROI Calculation:
($5.457M - $125K) / $125K = 4,256% ROI in Year 1
Payback period: 9 days ($125K / [$5.457M / 365 days])

Year 2 and beyond: No implementation cost, same benefits = 5,457% ROI"

[Show slide with clear ROI math. Make it printable—CFO wants to show board this 
math.]

Why: This is the economic case that wins budget approval. Make it specific to their 
data. Generic benchmarks don't persuade CFOs.

OBJECTION HANDLING DURING DEMO

If prospect says: "This is too good to be true. 4,256% ROI?"
Response: "I get that reaction. Let me break down where the ROI comes from. About 
90% of it is error prevention (catching duplicates). That's not us being clever—
it's your teams currently missing 3% of exceptions because they can't manually 
review everything. We're not creating savings, we're preventing losses you're 
already incurring. The other 10% is automation of manual reviews. Conservative case: 
if you only capture 50% of that error prevention (not 85%), you're still at 2,000% 
ROI and 30-day payback."

If prospect says: "We'd need to see this on our actual data."
Response: "Absolutely. In fact, that's what I'd recommend. Let's do a 2-week POC. 
Bring 10K invoices, we'll run our model, show you what it flags from your actual 
data, validate accuracy with your team. No commitment. You'll know in 2 weeks if 
this is real for your situation."

If prospect says: "Our IT team would need to evaluate this."
Response: "Good instinct. What are their main concerns? Data security? We're SOC 2 
Type II certified, HIPAA-ready. Integration? We have REST APIs, Salesforce/Coupa 
connectors. Performance? Our SLA is 99.9% uptime. What should I schedule with them?"

NEXT STEPS (2 minutes)

Don't leave it vague. Every demo ends with specific commitments:

If engaged:
"Here's what I'd recommend: (1) We schedule a follow-up with [CFO/Procurement 
person] to walk through the ROI. (2) We schedule a technical review with your IT 
security team—typical questions, security questionnaire, etc. (3) We scope a POC 
for 2 weeks starting [date]. Sound good? Let me send over the POC scope."

If less engaged:
"I know this is early in your evaluation. Two things that might help: (1) I'll 
send you a reference customer case study matching your size—take a look at their 
numbers. (2) If this is something worth exploring in the next quarter, let's grab 
coffee and understand your budget timing better."

If objections remain:
"Sounds like the main question is whether the model can actually catch your 
specific errors. The only way to answer that is with your data. Willing to do a 
quick POC just to validate? No risk, and you'll know in 2 weeks."
```

### ROI Calculator Framework

```
ENTERPRISE ROI CALCULATOR: [PRODUCT NAME]
=========================================

Purpose: Customize ROI for each prospect based on their specific metrics.
Accuracy: Use their data (not benchmarks) wherever possible.

INPUT ASSUMPTIONS (Gather from customer discovery)

Business Metrics:
1. Annual transaction volume: _______ [invoices, orders, documents, etc.]
2. Current % manual transactions: _______ (e.g., 35%)
3. Average processing cost per manual transaction: $_______ (e.g., $12)
4. Current error/exception rate: _______ % (e.g., 3%)
5. Average cost of each error: $_______ (e.g., $2,000 duplicate payment)

Implementation Metrics:
6. Implementation timeline: _______ weeks (usually 4)
7. Internal resources (FTE hours): _______ (usually 100 hours)
8. Cost per FTE hour: $_______ (usually $150/hour)

Model Performance (Industry averages if no customer data):
9. Automation rate: _______ % (typical: 80-85%)
10. Error detection rate: _______ % (typical: 85-90%)

CALCULATIONS

Line Item 1: Process Efficiency Savings
Formula: (Annual volume × % manual × cost per transaction) × automation rate

Example calculation:
- 100,000 invoices × 35% manual × $12 per = $420K manual processing cost
- 85% automation = 85,000 invoices no longer require manual review
- Cost saved: 85,000 × $12 = $1.02M

Conservative (50% automation): 50,000 × $12 = $600K savings
Upside (90% automation): 90,000 × $12 = $1.08M savings

Line Item 2: Error Prevention & Loss Mitigation
Formula: (Annual volume × error rate × error cost) × detection rate

Example calculation:
- 100,000 invoices × 3% error rate = 3,000 errors/year
- Average error cost: $2,000 (duplicate payment, miscoding, vendor dispute)
- Total annual loss: 3,000 × $2,000 = $6M in undetected errors
- Error detection rate: 85%
- Losses prevented: 6,000,000 × 85% = $5.1M

Conservative (50% detection): $6M × 50% = $3M prevented
Upside (95% detection): $6M × 95% = $5.7M prevented

Line Item 3: Working Capital Improvement
Formula: (Annual volume / 365 days) × reduction in processing time × cost of capital

Example:
- 100K invoices / 365 = 274 invoices/day average
- Processing time reduction: 2 days faster (from 5 days to 3 days)
- 274 × 2 days = 548 invoices in transit at any time
- Average invoice: $5,000
- Value of WC freed up: 548 × $5,000 = $2.74M
- Cost of capital (carry rate): 8% annually
- Value: 2.74M × 8% = $219K annual

Note: Use only if customer has significant working capital sensitivity (especially 
manufacturing, retail).

Line Item 4: Headcount Reduction (Optional, if relevant)
Formula: Employees no longer needed × fully-loaded annual cost

Example:
- Current AP team: 5 FTEs
- With 85% automation: need 2.5 FTEs (still reviewing exceptions, coding)
- Reduction: 2.5 FTEs
- Fully-loaded cost per FTE: $80K
- Savings: 2.5 × $80K = $200K

Note: Many customers prefer to redeploy rather than reduce headcount. Use this 
conservative.

YEAR 1 COST STRUCTURE

Software Cost:
- Annual subscription: $_______ [standard pricing]
- Volume-based: $_______ (if applicable)
- Total: $_______

Implementation Cost:
- Professional services (setup, integration): $_______ (typical: $30K)
- Data migration: $_______ (typical: $10K)
- Training: $_______ (included in services, or $5K if standalone)
- Total: $_______

Customer Internal Effort:
- Hours required: _______ hours (typical: 100 hours @ $150/hr = $15K)
- Total: $_______

YEAR 1 ROI CALCULATION

Total Year 1 Benefit:
- Process efficiency: $_______ [Line 1]
- Error prevention: $_______ [Line 2]
- Working capital: $_______ [Line 3]
- Headcount reduction: $_______ [Line 4]
- Total Benefit: $_______

Total Year 1 Cost:
- Software: $_______
- Implementation: $_______
- Internal effort: $_______
- Total Cost: $_______

Net Year 1 Benefit:
Total Benefit - Total Cost = $_______

ROI:
Net Year 1 Benefit / Total Year 1 Cost = _______% ROI

Payback Period:
Total Year 1 Cost / (Total Benefit / 365 days) = _______ days

Example completed:
Benefits: $6.7M (process savings $420K + error prevention $5.1M + WC $219K + 
headcount $1M)
Costs: $125K (software $80K + services $30K + internal $15K)
Net: $6.575M
ROI: 5,260%
Payback: 7 days

SENSITIVITY ANALYSIS

What if model performance is 50% lower?
- Process automation 50% instead of 85%: Adjust Line 1
- Error detection 50% instead of 85%: Adjust Line 2
- Recalculate ROI
- Typical result: Still 2,000%+ ROI and 30-day payback

What if implementation takes 8 weeks instead of 4?
- Delay benefits realization by 4 weeks
- Year 1 cost impact: Minimal (implementation cost same, but less time to realize savings)
- Result: Still positive Year 1, full benefit Year 2

What if adoption is slower than expected?
- Reduce automation rate (e.g., 60% instead of 85%)
- Reduce error detection rate (e.g., 65% instead of 85%)
- Recalculate
- Result: Conservative case still 1,500%+ ROI

PRESENTING TO DIFFERENT BUYERS

To CFO/Economic Buyer:
- Lead with total benefit: "$6.7M"
- Emphasize payback: "7 days"
- Highlight ROI: "5,260%"
- Use in their terms: "IRR, payback period, cost of capital"

To Category/Process Owner (User Buyer):
- Lead with process time: "Cut invoice processing time 60%"
- Emphasize workload: "5 FTE team handling 2x volume with same headcount"
- Show adoption timeline: "Your team adopts over 4 weeks, not immediately"

To IT Security (Technical Buyer):
- Lead with error prevention: "Catch 85% of duplicates and coding errors"
- Emphasize compliance: "Full audit trail, explainability, governance"
- Show implementation: "4 weeks, parallel with your operations, no downtime"

To Procurement (Procurement Buyer):
- Lead with cost/benefit: "What does this really cost us?"
- Show TCO: "Software $80K, services $30K, total year 1 investment $125K"
- Compare to benefit: "$6.7M benefit = 53x cost"
- Position ROI as "proof of vendor stability/success"
```

## Application

### Building Your First Battlecard

1. **Identify the selling situation**: When will your reps use this? (e.g., "When customer asks about ROI" or "When competitor comes up")
2. **Define the value proposition**: What's the core message that resonates with all 4 buyers?
3. **Gather proof points**: Find 3-5 quantified customer examples (with permission to reference)
4. **Build objection handlers**: For every "but...", prepare a response with proof
5. **Create positioning vs. competitors**: For each competitor, define win/loss scenarios
6. **Establish pricing guidelines**: What discounts can reps give? When do they need manager approval?
7. **Define next steps**: How does sales move the deal forward from here?

### Running Your Sales Enablement Launch

**Week 1-2 Before Launch**: Battlecard drafted, reviewed by sales leadership
**Week 2-3**: Demo script created, record video, test on reps
**Week 3-4**: ROI calculator validated with customer data
**Week 4 (Launch Week)**: Sales kickoff (live training), Q&A, practice on mock customers
**Post-Launch**: Track CRM adoption of battlecard, demo recordings, ROI calculator usage

## Worked Example: AI-Powered Invoice Matching Battlecard

See embedded examples in Key Concepts section for full specifications.

## Common Pitfalls

### Anti-Pattern 1: Feature Dumping in Demos

**What Happens**: Sales rep shows every feature. Customer sees 20 capabilities and gets lost.

**Why It Fails**: Complexity kills decision-making. Buyers care about their specific problem, not everything you built.

**Fix**: Demo only the features relevant to their stated pain. "You mentioned processing time is your challenge. I'll show you the automation pieces, skip the reporting for now."

### Anti-Pattern 2: No ROI Story

**What Happens**: "The system is very powerful" but no economic justification for purchase.

**Why It Fails**: CFO won't approve without numbers. Procurement won't include in evaluation.

**Fix**: Every demo ends with ROI. Use their data, not benchmarks.

### Anti-Pattern 3: Ignoring the Economic Buyer

**What Happens**: Rep sells features to end users, never talks to CFO. Deal stalls in procurement.

**Why It Fails**: End users can't approve spending. CFO won't approve without ROI.

**Fix**: By week 2 of sales process, CFO/VP Procurement must be involved.

### Anti-Pattern 4: Not Prepping Sales Team

**What Happens**: Battlecard sent via email. Reps never read it. They wing demos.

**Why It Fails**: Sales teams don't self-train.

**Fix**: Live training required. Role-play. Record examples. Track usage.

### Anti-Pattern 5: Static Materials

**What Happens**: Battlecard created, never updated. Old competitive positioning, outdated ROI.

**Why It Fails**: Sales tools become stale. Reps trust outdated materials.

**Fix**: Quarterly reviews. Update proof points, refresh competitive positioning, adjust ROI for new pricing.

## References

**Sales Battlecard Best Practices**
- Seismic: "The Modern Battlecard" playbook
- Forrester: "Sales Enablement Buyer's Guide"
- TOPO: Sales battlecard frameworks

**Enterprise Sales Methodology**
- Meddic methodology (qualification framework)
- SPICED (opportunity assessment)
- SPIN selling (discovery questioning)

**ROI & Economic Selling**
- Gartner: Value selling for complex B2B
- CEB: Sales effectiveness and ROI proof points

**Demo & Presentation Skills**
- SAP Sales Academy: Demo best practices
- Reforge: Sales strategy and execution

---

*Last updated: 2026-04-12 | Enterprise PM Catalyst v2.0*
