---
name: Customer Discovery
description: End-to-end discovery workflow from interview planning through insights synthesis to spec development
workflow_type: sequential_with_loops
estimated_duration: "4-6 weeks elapsed time (intensive PM work in weeks 1-2, lighter in weeks 3-6)"
plugin_chain: ["discovery", "research", "cab-management", "specification"]
---

## Overview

The Customer Discovery workflow is designed for running a complete discovery cycle: planning and conducting customer interviews, synthesizing findings into actionable insights, managing advisory board feedback, and converting insights into product specification. This workflow is iterative—if synthesis reveals significant gaps or conflicting customer needs, you return to additional targeted interviews before moving to specification.

This workflow is ideal for launching new product lines, entering new markets, or making major platform pivots where you need deep customer understanding before committing engineering resources.

## Skills Involved

| Skill | Plugin | Purpose |
|-------|--------|---------|
| **customer-interviews** | discovery | Plan interview strategy, recruit customers, conduct structured interviews |
| **research-synthesis** | discovery | Extract themes and insights from interview data, create evidence-based findings |
| **cab-management** | advisory | Manage Customer Advisory Board (CAB) feedback and validate synthesis insights |
| **prd-development** | specification | Convert validated insights into product specification with customer evidence |

## Step-by-Step Sequence

### Step 1: Interview Planning & Recruitment (Week 1)
**Owner:** PM + Research Team  
**Input:** Product hypothesis, target customer profiles, learning objectives  
**Output:** Interview guide, recruitment list, screened participants, confirmed interviews

Run the **customer-interviews** skill to:
- Define interview objectives: What do we need to learn? (e.g., "How do customers currently solve this pain? What's missing in existing solutions?")
- Create target customer profiles: Who should we talk to? (company size, role, industry, use case)
- Design interview guide: 8-12 open-ended questions that explore customer needs without leading them
- Identify recruitment channels: Where do target customers hang out? (LinkedIn, industry events, existing customer base)
- Recruit participants: Aim for 12-15 interviews across 2-3 customer segments (diversity is critical)
- Screen recruits: Confirm they match target profile and have relevant experience
- Schedule interviews: Spread over 2-3 weeks to allow synthesis time between batches

**Interview Planning Template:**
```
DISCOVERY CAMPAIGN: Distributed Team Scheduling Pain Points

LEARNING OBJECTIVES:
1. How do teams manage shift scheduling today? (current state)
2. What problems exist with current approach? (pain points)
3. What job outcomes do they care about? (what job are they trying to do?)
4. How do they evaluate solutions? (decision criteria)
5. Who influences the decision? (decision makers vs. influencers)

TARGET CUSTOMER PROFILES:

Profile 1: Enterprise Operations Manager
- Company size: 500+ employees
- Role: Head of Operations or Shift Manager
- Industry: Healthcare, Hospitality, Retail, Logistics
- Use case: Managing 50-200 shift employees across multiple locations
- Current tools: Excel spreadsheets + manual coordination
- Pain points: Time-consuming scheduling, fairness complaints, last-minute changes

Profile 2: Mid-Market HR/Ops Leader
- Company size: 50-500 employees
- Role: HR Manager, Operations Manager, or Team Lead
- Industry: Service businesses (call centers, staffing, security)
- Use case: Managing 20-100 shift employees, distributed teams
- Current tools: Spreadsheets, maybe basic scheduling software
- Pain points: Scheduling takes 4-8 hours/week, employee satisfaction issues

Profile 3: High-Growth Startup Operations
- Company size: 20-100 employees
- Role: Operations Lead, Founder/CEO
- Industry: Tech companies expanding to multiple offices
- Use case: First time managing distributed global teams
- Current tools: Slack + calendar, no formal scheduling system
- Pain points: Scaling is hard, manual process doesn't work anymore

INTERVIEW GUIDE (8-10 questions, ~30-45 min):

1. "Tell me about your team. How many people? Time zones? Types of roles?"
   [Goal: Understand their specific context]

2. "Walk me through how you currently schedule shifts/meetings/work. What's the process?"
   [Goal: Understand current workflow]

3. "How long does scheduling typically take? How many hours per week do you spend on it?"
   [Goal: Quantify pain magnitude]

4. "What's the hardest part of scheduling? What do you struggle with most?"
   [Goal: Identify core pain points]

5. "How do you handle changes? What happens if someone calls in sick?"
   [Goal: Understand edge cases and failures]

6. "How do you make sure scheduling is fair? How do employees react?"
   [Goal: Understand fairness considerations and employee satisfaction]

7. "What would an ideal solution look like? What would change for you?"
   [Goal: Unconstrained wish list]

8. "If you could solve one problem right now, what would it be?"
   [Goal: Prioritize most important need]

9. "How would you decide between solutions? What matters most?"
   [Goal: Understand evaluation criteria]

10. "Who else should I talk to? Any peers facing similar challenges?"
    [Goal: Snowball sampling for more interviews]

RECRUITMENT STRATEGY:

Segment 1: Enterprise (6 interviews)
- Recruit from: LinkedIn (search "Operations Manager"), industry events, HubSpot CRM
- Value prop: "Help us understand large-scale scheduling challenges for research"
- Interview incentive: $100 gift card or donation to charity
- Target: Mix of 3 industries (healthcare, retail, logistics)

Segment 2: Mid-Market (5 interviews)
- Recruit from: Existing customer referrals, industry forums, Slack communities
- Value prop: "Early access to product feedback, shape roadmap"
- Interview incentive: Free premium tier for 6 months
- Target: Different geographies and use cases

Segment 3: Startup (3-4 interviews)
- Recruit from: Founder networks, Y Combinator alumni, Product Hunt
- Value prop: "Help us understand scaling ops challenges"
- Interview incentive: $50 gift card or early access
- Target: Different stage and growth trajectories

RECRUITMENT TIMELINE:

Week 1, Day 1-2: Design interview guide, finalize profiles
Week 1, Day 3-4: Recruit Segment 1 (Enterprise), aim for 6 yeses
Week 1, Day 5: Recruit Segment 2 (Mid-Market), aim for 5 yeses
Week 2, Day 1-2: Final recruitment push (Segment 3), aim for 3-4 yeses
Target: 14-15 confirmed interviews scheduled over Weeks 2-3

INTERVIEW SCHEDULING:

Week 2: Conduct 5-6 interviews (Segment 1)
Week 3: Conduct 5-6 interviews (Segment 2)
Week 3: Conduct 3-4 interviews (Segment 3)
Between interviews: PM takes notes, highlights key quotes
```

**User Interaction Point:** PM reviews interview guide with research lead and one target customer (quick validation that questions resonate). Recruitment list is approved. 1 hour working session.

**Data Handoff:** Interview guide and recruitment list flow to interview execution. Scheduled interviews become commitments.

---

### Step 2: Interview Execution & Synthesis (Weeks 2-3)
**Owner:** PM + Research Team  
**Input:** Interview guide, recruited participants, confirmed interviews  
**Output:** Interview transcripts/notes, raw data, preliminary themes

**Phase A: Conduct Interviews (Weeks 2-3)**

Run the **customer-interviews** skill to execute interviews:
- Conduct 14-15 structured interviews (30-45 min each) following the guide
- Record interviews (with permission) for later reference
- Take detailed notes with key quotes that illustrate pain points and needs
- Ask follow-up questions to clarify or explore deeper
- End with: "Who else should we talk to?" (for snowball sampling)
- Send thank-you email with next steps and feedback opportunity

**Interview Execution Notes:**
```
INTERVIEW #1: Sarah, Enterprise Operations Manager at Healthcare Provider

Date: [Date], Duration: 42 minutes
Company: 400-bed hospital
Role: Director of Scheduling and Operations
Team size: 60 shift nurses + 30 part-time
Current system: Excel spreadsheets + WhatsApp coordination

KEY QUOTES (for synthesis):
"I spend 8-10 hours per week just on the schedule. It never feels right."
"Fairness is a nightmare. Some nurses get weekends off, others never do. People complain constantly."
"When someone calls in sick at 6am, I have to manually rework the entire schedule. Takes 2 hours."
"We tried a scheduling tool last year. It was too rigid. Couldn't handle our complex rules."
"What I really need: Something that understands healthcare constraints (breaks, ratios, regulations) but gives fair schedules."

PAIN POINTS IDENTIFIED:
1. Time-consuming (8-10 hrs/week)
2. Fairness issues (employees unhappy)
3. Inflexibility (breaks, regulations)
4. Manual overrides for changes (time-consuming)
5. Complex rules (regulatory + internal preferences)

JOBS TO BE DONE:
Primary: "Create fair, compliant shift schedules quickly"
Related: "Handle last-minute changes without chaos"
Outcome: "Keep employees happy and retain them"

NEEDS & WANTS:
Must-haves: Respects healthcare regulations (break requirements, ratios)
Nice-to-haves: AI suggestions, fairness metrics, mobile app
Decision criteria: Ease of use, reliability, fairness, cost

NEXT STEPS:
- Referred me to [2 other hospital operations managers]
- Willing to beta test if we build something
- Prefers demos every 2 weeks (wants to shape product)

[Continue for remaining 13 interviews...]
```

**User Interaction Point:** After 5-6 interviews (end of Week 2), PM and research lead sync to spot any early patterns. Any major surprises? Need to adjust interview guide? If yes, refine and continue. If no, proceed with remaining interviews. 30 min check-in.

**Data Handoff:** Interview notes flow to Step 3 (Synthesis).

---

### Phase B: Research Synthesis (Weeks 3-4)
**Owner:** Research Lead + PM  
**Input:** 14-15 interview transcripts and notes  
**Output:** Synthesis document with themes, insights, jobs-to-be-done, validated needs

Run the **research-synthesis** skill to:
- Extract key themes across all interviews (what patterns emerge?)
- Identify pain points and their frequency (how many customers mentioned this?)
- Document jobs-to-be-done: What outcomes do customers care about?
- Create customer segments based on needs and behaviors
- Build evidence of demand: How many customers need this? How urgent?
- Identify gaps: Where do customer needs diverge? What's still unclear?
- Assess confidence: Do we have enough evidence to spec this? Or do we need more research?

**Synthesis Process:**
```
STEP 1: CODE INTERVIEWS (Tag themes)

Theme codes to track:
  PAIN_TIME: "This takes too long"
  PAIN_FAIRNESS: "Employees complain about unfairness"
  PAIN_COMPLEXITY: "Rules are hard to manage"
  PAIN_REACTIVE: "We can't handle last-minute changes"
  SOLUTION_AI: Customer mentions wanting AI/automation
  SOLUTION_VISIBILITY: Customer wants transparency/visibility
  JOB_OUTCOME: What outcome do they care about?
  CONSTRAINT_REGULATORY: Regulatory requirements
  CONSTRAINT_EMPLOYEE: Employee preference handling
  DECISION_CRITERIA_X: How do they evaluate solutions?

STEP 2: CREATE FREQUENCY TABLE

Theme                          Frequency  Severity  Customer Segments
─────────────────────────────────────────────────────────────────────
Time-consuming (PAIN_TIME)     13/15      High      All segments
Fairness issues (PAIN_FAIRNESS) 11/15     High      Enterprise > Mid-market
Complexity (PAIN_COMPLEXITY)   10/15      Medium    Enterprise >> SMB
Last-minute changes (PAIN_REACTIVE) 9/15  Medium    Enterprise, Healthcare
Wants AI suggestions            8/15      Medium    Enterprise, Startups
Transparency/visibility         7/15      Low       Mid-market, Startups

STEP 3: SEGMENT ANALYSIS

Enterprise (Sarah, Maria, James, David, Elena, Raj):
  Primary need: Fair + compliant scheduling (high complexity)
  Pain intensity: HIGH (spending 8-10 hrs/week)
  Must-haves: Regulatory compliance, fairness metrics
  Nice-to-haves: AI suggestions, mobile app, integrations
  Buy trigger: If saves >50% of scheduling time
  Price sensitivity: LOW (will pay for value)
  Commonality: All manage 50-200 shift employees

Mid-Market (Lisa, Ahmed, Priya, Monica, Kosta):
  Primary need: Faster scheduling (time-saving)
  Pain intensity: MEDIUM-HIGH (spending 4-6 hrs/week)
  Must-haves: Easy to use, handles preferences
  Nice-to-haves: Reporting, analytics, fairness metrics
  Buy trigger: If saves >30% of scheduling time
  Price sensitivity: MEDIUM (budget-conscious)
  Commonality: Managing 20-100 employees, growing rapidly

Startup (Chen, Alicia):
  Primary need: Scalable system as they grow
  Pain intensity: MEDIUM (scaling is becoming hard)
  Must-haves: Easy to set up, mobile-first
  Nice-to-haves: Integrations, analytics
  Buy trigger: As they scale past 50 employees
  Price sensitivity: HIGH (bootstrap budgets)
  Commonality: Global teams, founder-led ops

STEP 4: JOBS-TO-BE-DONE ANALYSIS

Primary Job: "Create fair, equitable shift schedules quickly"
- Related mini-jobs:
  - Collect employee availability and preferences
  - Apply regulatory constraints and rules
  - Optimize for fairness
  - Communicate schedule to team
  - Handle last-minute changes

Emotional Job: "Feel confident I'm treating employees fairly"
Functional Job: "Generate compliant schedules in <30 min"
Social Job: "Be seen as a fair, employee-friendly manager"

STEP 5: DEMAND VALIDATION

Question: How many potential customers exist?
Evidence:
- 15 interviews across 3 segments
- All 15 confirmed need for scheduling solution
- 13/15 said "very interested" if [solution had X feature]
- 12/15 said "would evaluate" in next 6 months

Validation level: STRONG
Confidence: High

Question: How urgent is this need?
Evidence:
- Enterprise segment: HIGH urgency (spending 8-10 hrs/week, acute pain)
- Mid-market: MEDIUM urgency (spending 4-6 hrs/week, growing pain)
- Startup: LOW-MEDIUM urgency (not urgent now, will be in 12 months)

→ Conclusion: Enterprise is your fastest path to revenue. Go after that segment.

STEP 6: IDENTIFIED GAPS (Do we need more research?)

Gap 1: Price sensitivity unclear
  Evidence: Only 2 customers mentioned budget
  Resolution: Ask 3-4 more customers: "What would you pay?"
  Decision: Resolve before spec

Gap 2: Integration needs unclear
  Evidence: Only 1 customer mentioned needing API/integrations
  Resolution: Ask 3-4 more customers: "What systems do you use?"
  Decision: Resolve before spec (might affect product scope)

Gap 3: Regulatory requirements vary by industry
  Evidence: Healthcare has strict rules, retail less so
  Resolution: Interview 1-2 more retail/hospitality customers
  Decision: Resolve before spec (determines feature scope)

SYNTHESIS CONFIDENCE SCORE:

Time-saving pain point:             95% confidence (13/15 mentioned)
Fairness pain point:                85% confidence (11/15 mentioned)
Regulatory complexity:              70% confidence (only healthcare customers mentioned)
AI suggestions helpful:             60% confidence (mentioned but not deeply explored)
Price range / willingness to pay:   40% confidence (not enough data)
Integration requirements:           35% confidence (minimal data)

Overall synthesis readiness: 70%
Recommendation: Address 2-3 gaps before moving to spec
Gap resolution: 1-2 more interviews (~1 week)
```

**User Interaction Point:** Research lead presents synthesis to PM and product leadership. Team reviews themes and discusses what's clear vs. what needs more research. If gaps are significant (especially around pricing or must-have features), PM decides to conduct 2-4 targeted follow-up interviews before moving to specification. If gaps are minor, PM gives green light to proceed to CAB validation. 1.5 hour working session.

**Data Handoff:** Synthesis document (with themes, needs, jobs-to-be-done, confidence scores) flows to Step 3 (CAB Validation) and eventually to Step 4 (PRD Development).

**OPTIONAL LOOP:** If synthesis reveals significant gaps (confidence <70% on critical questions), return to Step 2 with targeted interviews on specific topics. Re-run synthesis loop.

---

### Step 3: Customer Advisory Board (CAB) Validation (Week 4)
**Owner:** PM + CAB Manager  
**Input:** Synthesis document from Step 2  
**Output:** CAB feedback, validation of insights, roadmap input, refined understanding

Run the **cab-management** skill to:
- Present synthesis findings to CAB (8-12 key customers who advise you)
- Get validation: "Does this match your experience? Are we missing anything?"
- Discuss insights: Deep dive on key themes with advisory customers
- Test positioning: "How would you describe this solution to a peer?"
- Gather feature input: "If we built X, would you buy?"
- Build commitment: "Would you be a reference customer if we launched this?"
- Document feedback: What did CAB confirm vs. challenge?

**CAB Validation Process:**
```
CAB MEETING: Customer Discovery Validation

Participants: 8 CAB members (mix of Enterprise, Mid-market, Startup)
Format: 2-hour virtual workshop + 1:1 debrief calls
Agenda:
1. (15 min) Welcome + research objective
2. (30 min) Present synthesis findings (themes, jobs-to-be-done, segments)
3. (30 min) Group discussion: "Does this match your experience?"
4. (20 min) Feature prioritization exercise: Vote on must-haves vs. nice-to-haves
5. (15 min) Positioning test: "How would you describe this to a peer?"
6. (10 min) Closing: Reference customer + next steps

PRESENTATION OUTLINE:

Slide 1: Research Context
- 15 customers interviewed (3 segments: Enterprise, Mid-market, Startup)
- Goal: Understand shift scheduling pain points and needs
- How you'll use feedback: Shape product spec, confirm market fit

Slide 2: Key Themes & Pain Points (The Big Picture)
- Top 5 themes: Time-consuming (13/15), Fairness issues (11/15), Complexity (10/15), etc.
- Jobs-to-be-done: "Create fair, equitable schedules quickly"
- Segment breakdown: Enterprise >>>> Mid-market > Startup (in terms of pain intensity)

Slide 3: Customer Segments (Who We're Building For)
- Enterprise: 50-200 shift employees, 8-10 hrs/week on scheduling, HIGH willingness to pay
- Mid-market: 20-100 employees, 4-6 hrs/week, MEDIUM willingness to pay
- Startup: <100 employees, scaling pain, LOW willingness to pay

Slide 4: What Customers Want (Must-Haves)
- Enterprise: Fairness metrics, Regulatory compliance, Time-saving
- Mid-market: Ease of use, Handles preferences, Fast
- Startup: Scalable, Mobile, Integrations

Slide 5: Discussion Questions
- "Does this match your experience? What's different?"
- "What's the #1 must-have we cannot skip?"
- "Which segment are we most attractive to?"

Slide 6: Feature Voting (Interactive Exercise)
List of potential features. CAB votes: Must-have / Nice-to-have / Don't care
Features:
  - AI-powered scheduling suggestions
  - Fairness metrics + reporting
  - Mobile app
  - API/integrations
  - Regulatory rule engine
  - Employee self-service app
  - Analytics dashboard
  - Slack integration
  - Calendar sync (Google/Outlook)

Slide 7: Positioning Test (Group Workshop)
"If you were explaining this product to a peer, how would you describe it?"
Capture language used. Refine positioning based on feedback.

CAB FEEDBACK CAPTURE:

Question: "Does this match your experience?"
Feedback from Maria (Healthcare): "100% yes. Time and fairness are killing us."
Feedback from Lisa (Mid-market): "The fairness thing is less of an issue for us. Time is huge."
Feedback from Chen (Startup): "We're not there yet, but this will be critical in 6-12 months."

→ Insight: Enterprise has both pain points (time + fairness); Mid-market mostly time.

Question: "What would make you buy this?"
Feedback from Enterprise (5/6): "Save us 50% of scheduling time, prove fairness metrics work"
Feedback from Mid-market (4/5): "Save us 30% of time, easy to use"
Feedback from Startup (2/2): "Free tier while we grow, then upgrade in 12 months"

→ Insight: Different buy triggers by segment. Pricing model needs flexibility.

Question: "Would you be a reference customer?"
Enterprise: 5/6 said YES (with conditions)
Mid-market: 3/5 said YES
Startup: 1/2 said MAYBE

→ Insight: Strong reference potential, especially Enterprise. Build with them.

VALIDATION SCORECARD:

Assumption                      CAB Feedback        Confidence
──────────────────────────────────────────────────────────
Time-saving is key need        CONFIRMED (8/8)     95%
Fairness is Enterprise pain    CONFIRMED (5/6)     90%
Willingness to pay (Enterprise) CONFIRMED (6/6)    90%
Feature priorities correct     MOSTLY CONFIRMED    75%
Positioning resonates          PARTIALLY (needs tweak) 60%
CAB interest as references     STRONG              85%

Overall validation: 80%+
Recommendation: Proceed to spec development with confidence.
Fine-tuning needed: Positioning language, pricing model clarity
```

**User Interaction Point:** PM hosts CAB meeting and observes feedback. Any major surprises or challenges to synthesis? Post-meeting, PM syncs with CAB manager to assess confidence and decide: "Are we ready to spec?" If CAB is skeptical about a key assumption, PM may conduct 1-2 more targeted interviews. If CAB is enthusiastic, PM proceeds to specification with confidence. 2.5 hour time investment (meeting + debrief).

**Data Handoff:** CAB feedback validates synthesis insights and informs feature prioritization for PRD.

---

### Step 4: PRD Development with Customer Evidence (Week 4-5)
**Owner:** PM  
**Input:** Synthesis document (Step 2), CAB validation (Step 3), interview evidence  
**Output:** Complete PRD grounded in customer research, with evidence for every requirement

Run the **prd-development** skill to:
- Structure PRD using customer insights as primary evidence
- Document jobs-to-be-done and customer outcomes
- Define features grounded in customer feedback (avoid feature creep)
- Create use cases based on customer workflows observed in interviews
- Document success metrics based on customer outcomes ("Save 50% of scheduling time")
- Identify scope boundaries based on customer priorities
- Include customer evidence in every section (quotes, segment data, frequency)

**PRD Structure with Customer Evidence:**
```
PRODUCT SPECIFICATION: Shift Scheduling Assistant

VERSION: 1.0 (Based on 15 customer interviews + CAB validation)
DATE: [Date]
AUDIENCE: Enterprise operations teams managing 50-200 shift employees

EXECUTIVE SUMMARY

Problem: Operations managers spend 8-10 hours per week creating fair shift schedules.
Solution: AI-powered scheduling assistant that generates fair, compliant schedules in minutes.
Target Customer: Enterprise operations teams (healthcare, retail, hospitality, logistics)
Key Metrics: 50%+ time savings, 4.0+ fairness rating, 90%+ adoption
Confidence: High (13/15 customers validated need; 5/6 Enterprise CAB interested as references)

CUSTOMER EVIDENCE:
Quote (Sarah, Healthcare): "I spend 8-10 hours per week just on the schedule."
Frequency: 13/15 customers mentioned time-consuming
Segment impact: All segments, highest in Enterprise

VISION & STRATEGY

Vision: "Make fair shift scheduling so simple that ops managers spend <30 minutes per week on it."

Why this matters (customer jobs-to-be-done):
- Functional: "Create fair, equitable shift schedules quickly"
- Emotional: "Feel confident I'm treating employees fairly"
- Social: "Be seen as an employee-friendly manager"

CUSTOMER EVIDENCE:
- 13/15 customers: Need for time-saving
- 11/15 customers: Need for fairness
- 9/15 customers: Need to handle last-minute changes
- Common theme: Employees complain about unfair scheduling; managers feel guilty

WHO WE'RE BUILDING FOR

Primary Segment: Enterprise Operations Teams
- Company size: 500+ employees
- Team size: 50-200 shift employees
- Role: Director of Scheduling, VP of Operations, Head of HR
- Industry: Healthcare, Retail, Hospitality, Logistics
- Pain: Spending 8-10 hrs/week on scheduling; fairness complaints
- Willingness to pay: High (will spend $5k-20k/year to solve this)
- Buy trigger: Saves 50%+ of scheduling time AND proves fairness

Secondary Segment: Mid-Market (Nice-to-have, consider later)
- Company size: 50-500 employees
- Team size: 20-100 shift employees
- Pain: Spending 4-6 hrs/week on scheduling
- Willingness to pay: Medium
- Buy trigger: Saves 30%+ of scheduling time; easy to use

Do NOT build for initially: Startup (market too small, low willingness to pay)

CUSTOMER EVIDENCE:
- 6 Enterprise customers interviewed: All confirmed pain intensity and willingness to pay
- 5 Mid-market customers interviewed: Confirmed need but lower pain intensity
- CAB feedback: 5/6 Enterprise said "Would be reference customer"

MUST-HAVE FEATURES (Cannot ship without)

1. AI Scheduling Engine
   What: AI generates shift schedule suggestions based on employee preferences, skills, and constraints
   Why: Customers spend 8+ hours on manual scheduling; AI cuts this to <30 minutes
   Acceptance Criteria:
     - Generate complete schedule for 50-100 employees in <5 seconds
     - Coverage: 95%+ of shifts assigned on first suggestion
     - Accuracy: Human reviewer approves 90%+ without changes
   
   CUSTOMER EVIDENCE:
   Quote (Maria, Healthcare): "If the AI could suggest schedules, I could review and approve instead of creating from scratch."
   Frequency: 8/15 mentioned wanting AI suggestions
   Segment: Enterprise > Mid-market > Startup

2. Fairness Metrics & Visualization
   What: Dashboard showing fairness metrics (shift distribution, preferences met, etc.)
   Why: Managers struggle with fairness; fairness complaints are a major pain
   Acceptance Criteria:
     - Show fairness score (0-100) for every schedule
     - Compare current schedule to historical average
     - Highlight potentially unfair patterns (e.g., "Bob got weekends off 8/10 weeks")
   
   CUSTOMER EVIDENCE:
   Quote (Sarah, Healthcare): "Fairness is a nightmare. Some nurses get weekends off, others never do."
   Frequency: 11/15 mentioned fairness as pain point
   Segment: Enterprise >> Mid-market

3. Regulatory Compliance Rules Engine
   What: System enforces labor law and industry-specific rules
   Why: Some customers operate in regulated industries; compliance violations are costly
   Acceptance Criteria:
     - Prevent shifts violating break requirements
     - Enforce shift length rules (max 12-hour shifts)
     - Support custom compliance rules per company
   
   CUSTOMER EVIDENCE:
   Quote (James, Healthcare): "Healthcare has strict regulations. Any scheduling tool has to understand that."
   Frequency: 6/15 mentioned regulatory constraints (mostly Enterprise/Healthcare)
   Segment: Enterprise >> Mid-market

4. Manual Override & Feedback
   What: Users can override AI suggestions and provide feedback for model improvement
   Why: Customers have edge cases AI can't handle; feedback improves AI over time
   Acceptance Criteria:
     - One-click override of any suggestion
     - Feedback mechanism ("Why did you change this?")
     - Feedback logged and used for retraining
   
   CUSTOMER EVIDENCE:
   Quote (Maria): "AI will be helpful, but I'll always need to make manual adjustments."
   Frequency: 10/15 mentioned need for flexibility/overrides
   Segment: All segments

NICE-TO-HAVE FEATURES (Consider post-launch)

- Mobile app for on-the-go schedule access
- Slack integration for schedule notifications
- Employee self-service (employees can bid for shifts)
- Integrations (Workday, BambooHR, etc.)
- Advanced analytics (burnout prediction, turnover risk)

CUSTOMER EVIDENCE:
Nice-to-haves ranked by CAB vote:
1. Mobile app (6/8 voted must-have)
2. Slack integration (4/8 nice-to-have)
3. Self-service bidding (3/8 nice-to-have)
4. Advanced analytics (2/8 interested)

FEATURES WE'RE EXPLICITLY NOT BUILDING (Scope Boundaries)

- Employee payroll integration (out of scope, too complex)
- Staffing agency coordination (out of scope, rare use case)
- Facility/asset scheduling (out of scope, different problem)

CUSTOMER EVIDENCE:
Only 1/15 customers mentioned payroll; 0/15 mentioned agencies → Not core need

SUCCESS METRICS (How we'll measure if this worked)

1. Time Savings
   Metric: "How much time do you spend on scheduling per week?"
   Target: 50%+ reduction (from 8hrs to <4hrs)
   Measurement: Customer survey at 30/60/90 days post-launch
   Evidence: 13/15 customers said "save 50% of time" was buy trigger

2. Fairness Improvement
   Metric: "Fairness score" (scale 0-100, based on shift distribution)
   Target: Increase from 60 (average customer baseline) to 80+ within 30 days
   Measurement: Dashboard metric tracked automatically
   Evidence: 11/15 customers complained about fairness

3. User Adoption
   Metric: "% of eligible schedules created using AI"
   Target: 90%+ adoption among Enterprise tier 1 customers by 60 days
   Measurement: Product usage tracking
   Evidence: CAB commitment ("We'd use this")

4. Customer Satisfaction
   Metric: "Would you recommend this to a peer?" (NPS-style)
   Target: 50+ NPS score
   Measurement: Customer survey
   Evidence: CAB feedback confidence level

LAUNCH STRATEGY & TIMELINE

Phase 1: Beta with 3 Enterprise CAB customers (Week 1-4 post-launch)
Phase 2: GA to all Enterprise customers (Week 5+)
Phase 3: Consider Mid-market after Enterprise mature (Month 3+)

CUSTOMER EVIDENCE:
- 5/6 Enterprise CAB members committed to beta testing
- All asked for 2-week iteration cycles (high engagement expected)
```

**User Interaction Point:** PM shares PRD draft with leadership and top customers for feedback. Any gaps or misunderstandings? PM refines PRD based on feedback. Final PRD is locked and becomes source of truth for engineering. 1.5 hour working session.

**Data Handoff:** Customer-evidence-grounded PRD is handed to engineering for implementation.

---

## Data Handoff

| From Step | To Step | Data | Format | Reference |
|-----------|---------|------|--------|-----------|
| 1 | 2 | Interview guide, recruitment list | Interview guide doc + CSV | Guides execution of interviews |
| 2 | 3 | Interview notes + synthesis | Synthesis doc with themes | CAB discussion uses synthesis |
| 3 | 4 | CAB feedback + validation | CAB feedback document | PRD incorporates CAB input |
| 2 + 3 | 4 | All customer evidence | Interview quotes, frequency data | Every PRD requirement cites customer evidence |

**Single Source of Truth:** Create "Customer Research Repository" with all interviews, synthesis, CAB notes, linked in one place. PRD references this repo for evidence.

---

## When Synthesis Reveals Gaps (The Loop Back)

If synthesis shows insufficient confidence (<70%) on critical questions, return to Step 2:

**Gap Resolution Path:**
```
Gap Identified: "Price sensitivity unclear" (40% confidence)
→ Conduct 3-4 targeted follow-up interviews asking specifically: "What would you pay?"
→ Re-run synthesis on expanded dataset
→ Update confidence score (should improve to 70%+)
→ Proceed to CAB validation when ready

Gap Identified: "Integration needs very unclear" (35% confidence)
→ Conduct 2 deep-dive interviews with IT/technical decision-makers
→ Ask specifically: "What systems do you use? What needs to integrate?"
→ Add 1-2 technical requirements to PRD
→ Proceed when confidence >70%

Timeline impact: Each additional research loop adds 1-2 weeks
→ If you identify gaps at synthesis (Week 4), add 1-2 weeks, proceed to CAB in Week 5-6
```

---

## Parallelization Opportunities

### Sequential Critical Path:
```
Step 1 (Planning) ──> Step 2 (Interviews + Synthesis) ──> Step 3 (CAB)
                                                              ↓
                                                    Step 4 (PRD)
```

### Within Step 2 (Parallel Execution):
- Conduct interviews in batches (5-6 per week)
- Between batches, synthesis team works on preliminary themes
- Final synthesis done after all interviews complete

### Timeline:
```
Week 1:   [Step 1: Planning & Recruitment]
Week 2:   [Step 2A: Interviews 1-6] + [Step 2B: Preliminary synthesis]
Week 3:   [Step 2A: Interviews 7-15] + [Step 2B: Refine synthesis]
Week 4:   [Step 2C: Final synthesis] + [Step 3: CAB validation]
Week 5:   [Step 4: PRD Development + Customer validation]
Week 6:   [PRD finalized, ready for engineering]

Total elapsed: 5-6 weeks
Intense work: Weeks 2-3 (interview scheduling + execution)
Lighter work: Week 4-5 (synthesis, CAB, PRD)
```

---

## User Interaction Points

1. **Interview Guide Review (Week 1, Day 2):** PM + research lead validate questions. ~1 hour.

2. **Recruitment Check-in (Week 1, Day 5):** Confirm interviews scheduled, targets met. ~30 min.

3. **Interview Progress Check (Week 2-3, Mid-week):** Review initial interviews, spot any patterns, refine guide if needed. ~30 min.

4. **Synthesis Review (Week 4):** PM reviews synthesis, identifies gaps, decides on additional interviews if needed. ~1.5 hours.

5. **CAB Presentation & Discussion (Week 4, Day 4-5):** Host CAB workshop, gather feedback. ~2.5 hours.

6. **PRD Development & Validation (Week 5):** PM drafts PRD with customer evidence, shares with leadership and select customers. ~1.5 hours.

7. **Final PRD Sign-Off (Week 5, Day 5):** Leadership approves PRD, engineering ready to build. ~1 hour.

**Total PM Time Investment:** 15-20 hours synchronous + 15-20 hours asynchronous (interview prep, synthesis review, PRD writing).

---

## Estimated Timeline

| Phase | Duration | Notes |
|-------|----------|-------|
| Planning & Recruitment (Step 1) | 1 week | Recruit 14-15 interviews |
| Interview Execution (Step 2A) | 2 weeks | Spread interviews over 2 weeks |
| Synthesis (Step 2B + 2C) | 1 week | Preliminary + final themes |
| Gap Resolution (if needed) | 1-2 weeks | Additional interviews if <70% confidence |
| CAB Validation (Step 3) | 1 week | CAB meeting + debrief calls |
| PRD Development (Step 4) | 1 week | Customer-evidence-grounded spec |
| **Total Elapsed Time** | **4-6 weeks** | Typical timeline: 5-6 weeks |

**Actual PM Hours:** 30-40 hours total (distributed across 6 weeks, intense in weeks 2-3).

---

## Final Output Summary

By the end of this workflow, you have:

1. **Research Repository**
   - 14-15 customer interviews transcribed and documented
   - Interview notes with key quotes and observations
   - Contact information and permission for future follow-ups
   - Structured data for ongoing reference

2. **Synthesis Document**
   - Key themes and pain points with frequency data
   - Customer segments identified and profiled
   - Jobs-to-be-done analysis
   - Validated needs and demand assessment
   - Identified gaps and areas of remaining uncertainty
   - Confidence scoring on key assumptions
   - Evidence that supports proceeding to specification (or returning for more research)

3. **CAB Validation**
   - Feedback from 8-12 key customers on synthesis findings
   - Feature prioritization from advisory board
   - Positioning/messaging validation
   - Commitments to reference customers and beta participation
   - Refined understanding of must-haves vs. nice-to-haves

4. **Customer-Evidence-Grounded PRD**
   - Product vision and strategy rooted in customer needs
   - Target customer profile with clear segment prioritization
   - Feature requirements justified by customer feedback (every feature has supporting evidence)
   - Success metrics defined based on customer outcomes
   - Scope boundaries identified based on customer priorities
   - Launch strategy informed by customer readiness

5. **Strategic Direction & Market Confidence**
   - Clear evidence that this market exists and has significant pain
   - Validation that your solution approach resonates with customers
   - Reference customers identified and committed to support
   - Roadmap that aligns with customer needs (not feature creep)
   - Confidence level to move forward to engineering execution

**Handoff Status:** Product team has moved from hypothesis to evidence-based strategy. Engineering can now build with confidence that requirements are grounded in real customer needs. Sales/marketing can reference customer evidence in messaging. Leadership approved because research supports the business case.

---

## When to Use This Workflow

✓ **New product lines** entering new markets  
✓ **Major platform pivots** where you need deep understanding before committing resources  
✓ **When you have 4-6 weeks** available for discovery  
✓ **When customer uncertainty is high** (you don't fully understand the problem)  
✓ **When you have access to target customers** (for interviews and CAB)  
✓ **High-stakes decisions** where evidence of market fit is essential  

---

## When NOT to Use This Workflow

✗ **Mature product** with established customer base (use lighter feedback loops instead)  
✗ **Urgent feature** needed within 2 weeks (too slow)  
✗ **When you lack customer access** for interviews (can't run discovery)  
✗ **Research-stage project** where you're still exploring problem validity (do validation first)  
✗ **When requirements are already locked** by executives or customers (no time for discovery)  

**Alternative:** For faster discovery, use "Express Discovery" workflow (10 interviews only, skip CAB, 3-4 week timeline). For ongoing customer feedback, use "Continuous Discovery" (lighter interviews every sprint, rolling insights).
