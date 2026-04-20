---
name: Launch Sequence
description: End-to-end product launch workflow from GTM planning through post-launch metrics tracking
workflow_type: phased_sequential
estimated_duration: "8-12 weeks (T-8 through T+4)"
plugin_chain: ["launch", "enablement", "marketing", "operations", "analytics"]
---

## Overview

The Launch Sequence workflow orchestrates a complete product launch across 12 weeks: 8 weeks of pre-launch preparation (GTM planning, sales/customer enablement, release documentation) followed by 4 weeks of post-launch monitoring and iteration. This workflow ensures that when a feature or product goes live, the market, sales team, support organization, and existing customers are all prepared and that launch success is measured against clear metrics.

This workflow applies to major features, product tier launches, and significant platform changes.

## Skills Involved

| Skill | Plugin | Purpose |
|-------|--------|---------|
| **gtm-planning** | launch | Develop go-to-market strategy with positioning, messaging, and campaign timeline |
| **sales-enablement** | enablement | Create sales tools, training, and messaging for direct sales teams |
| **release-notes** | marketing | Document feature details and user-facing changes for customer communication |
| **customer-migration** | operations | Plan upgrade/migration paths for existing customers and handle technical transitions |
| **metrics-analysis** | analytics | Define launch success criteria and track KPIs post-launch |

## Step-by-Step Sequence

### Step 1: GTM Strategy & Positioning (Week T-7 to T-5)
**Owner:** PM + Marketing  
**Input:** Feature specification, competitive positioning, target customer segment, pricing (if applicable)  
**Output:** GTM strategy doc with positioning, messaging, pricing, campaign calendar, budget allocation

Run the **gtm-planning** skill to:
- Define target customer personas (who are the earliest adopters?)
- Craft product positioning statement (how does this compare to status quo and competitors?)
- Create messaging hierarchy: value prop → benefits → features
- Develop positioning for different customer segments (Enterprise vs SMB vs individual contributors)
- Build 12-week campaign calendar (email, in-app, sales outreach, webinars, blog posts)
- Allocate marketing budget and resource plan
- Identify key launch moments (beta announcement, GA launch, first milestone celebration)
- Document pricing strategy and pricing messaging if applicable

**GTM Strategy Outline:**
```
Positioning: "The first AI-native operations platform built for distributed teams"
Competitive Set: [Competitors], Differentiation: [Specific AI capabilities]

Target Segments:
  - Segment A: Global enterprises (500+ employees) - emphasize scalability
  - Segment B: Mid-market ops (50-500 people) - emphasize ease of setup
  - Segment C: Ops practitioners - emphasize productivity gains

Launch Campaign (12 weeks):
  Week T-7 to T-5: Internal soft launch, beta signups
  Week T-4 to T-2: Beta program (100 customers), content marketing starts
  Week T-1 to T: GA launch, press release, webinar, sales blitz
  Week T+1 to T+4: Momentum phase, case studies, customer testimonials, optimization
```

**User Interaction Point:** Leadership reviews positioning and confirms it aligns with brand/strategy. Marketing confirms budget and resource allocation. 45 min working session.

**Data Handoff:** Positioning and messaging framework flows to Step 2 (Sales Enablement) and Step 3 (Release Notes).

---

### Step 2: Sales Enablement & Training (Week T-5 to T-3, Parallel with Step 1)
**Owner:** Sales Leader + PM  
**Input:** GTM positioning from Step 1, feature spec, pricing, competitive intelligence  
**Output:** Sales playbook, training videos, battle cards, pricing guide, customer scenarios

Run the **sales-enablement** skill to:
- Create "Sales Playbook" with discovery questions, objection handling, and close scenarios
- Develop competitive battle cards showing feature comparison vs. alternatives
- Build pricing guide with discount authority, packaging options, and ROI calculator
- Create customer scenario templates (how does this solve for [industry] customers?)
- Develop training curriculum for sales team (slides, videos, certification quiz)
- Build sales tools: one-pagers, demo scripts, customer success stories
- Create customer migration stories (how to position to existing customers)

**Sales Playbook Structure:**
```
Discovery Questions:
  "How do you currently manage operations across time zones?"
  "What tools are you using today? Any pain points?"
  "How many people touch this process monthly?"

Objections & Responses:
  Objection: "This looks like [Competitor]. What's different?"
  Response: "Great question. [Competitor] does [X], but we're the only one that does [Y] natively..."

Close Scenarios:
  Scenario 1: "Price-sensitive mid-market customer"
    Playbook: Lead with ROI (hours saved), offer annual discount
  Scenario 2: "Enterprise customer doing POC"
    Playbook: Lead with data security, reference similar customer
```

**User Interaction Point:** Sales leadership reviews playbook and training. Any gaps or unclear messaging gets refined. Sales team takes training and provides feedback on real objections encountered. 1 hour training session + async Q&A.

**Data Handoff:** Sales playbook and training materials are published to sales team. Messaging from GTM positioning stays consistent across all channels.

---

### Step 3: Release Notes & Customer Documentation (Week T-4 to T-2, Parallel with Step 2)
**Owner:** PM + Technical Writer  
**Input:** Feature specification, user workflows, customer segments, GTM positioning  
**Output:** Release notes, customer migration guide, feature documentation, FAQ

Run the **release-notes** skill to:
- Write user-facing feature description (benefits first, then features)
- Document how the feature improves customer workflows
- Create step-by-step usage guide with screenshots
- Build FAQ addressing common questions and edge cases
- Document any breaking changes or upgrade requirements
- Write "What's New" email announcement with clear call-to-action
- Create in-app messaging (tooltips, modals, announcement banners)
- Build customer migration guide for existing users (old way → new way)

**Release Notes Template:**
```
# Feature Announcement: AI Operations Assistant

## What is it?
[Customer benefit first] - Save 5+ hours/week on routine ops tasks

## Who should use it?
Ops professionals managing 50+ team members
Operations leaders needing visibility into distributed team workflows

## What you can do:
1. Automate shift scheduling - intelligently balances availability
2. Get AI-powered analytics - spot trends in team performance
3. Set up smart alerts - notify you of policy violations or gaps

## How to get started:
1. Go to Settings > AI Assistant > Enable (flipped on by default)
2. Choose scheduling preferences (link to detailed docs)
3. Grant required permissions (link to privacy docs)

## Migration from old flow:
Old Way: Manual scheduling in spreadsheet
New Way: 1-click setup, auto-suggestions, real-time optimization

## FAQ:
Q: Does this cost extra?
A: Included in Pro plan and above. See pricing.

Q: What data is the AI trained on?
A: Your data only. Full privacy policy: [link]

Q: Can I disable it?
A: Yes, per-workspace setting in Settings > AI > Disable
```

**User Interaction Point:** Product team reviews release notes for accuracy. Marketing confirms messaging aligns with positioning. Legal/Compliance reviews for any regulatory implications (if applicable). 30 min review.

**Data Handoff:** Release notes and documentation are locked and ready for publication. In-app messaging templates are provided to engineering for implementation before T-day.

---

### Step 4: Customer Migration & Upgrade Path (Week T-3 to T-1, Parallel with Steps 2-3)
**Owner:** Customer Success + Engineering  
**Input:** Feature spec, existing customer base, upgrade/migration requirements  
**Output:** Migration plan, customer communication timeline, support documentation, escalation plan

Run the **customer-migration** skill to:
- Map existing customer base and identify who's impacted (breaking changes, mandatory upgrades, etc.)
- Design migration path: auto-upgrade, opt-in beta, phased rollout timeline
- Create customer communication plan: email announcing change, deadline if applicable, links to help docs
- Develop support runbook: common issues, troubleshooting, escalation path
- Build data migration scripts if needed (old format → new format)
- Create rollback plan if problems arise (how to revert customers quickly)
- Schedule support team training on new feature (troubleshooting common issues)
- Set up monitoring for failed migrations or customer issues post-launch

**Customer Migration Plan Example:**
```
Phase 1 (T-3): Announcement email to all customers
  Message: "New AI Operations Assistant launching [Date]. Automatic for all Pro+ plans."
  CTA: "Learn more" link to release notes and feature docs
  Deadline: N/A (auto-enabled)

Phase 2 (T-1): Support documentation live
  Pages: Feature guide, migration guide, troubleshooting, privacy info
  Support team: Trained on all Q&A, armed with script for proactive outreach

Phase 3 (T-day): Feature goes live
  Auto-enable for Pro+ customers (80% of base)
  Freemium customers: Feature available but requires beta opt-in
  Existing beta customers: Graduated from beta to full product

Phase 4 (T+1 to T+7): Support surge plan
  Extra support staff on-call for issues
  Daily metrics review (adoption, errors, support tickets)
  If >5% critical issues: rollback plan activated

Escalation Path:
  - Critical issue (data loss, security): VP of Engineering + PM + Customer Success VP
  - Major adoption blocker: PM + Customer Success
  - FAQ questions: Support tier 1 handles with scripts
```

**User Interaction Point:** Customer Success confirms communication plan reaches all segments. Engineering confirms rollback/monitoring scripts are in place. Support team reviews training materials. 1 hour working session.

**Data Handoff:** Customer communication plan and migration timeline syncs with GTM campaign calendar (T-day GA launch). Support runbook is published internally and ready for support team.

---

### Step 5: Launch Execution & Monitoring (Week T to T+2)
**Owner:** PM + Engineering + Marketing + Support  
**Input:** All preparation from Steps 1-4, feature ready to ship, monitoring setup  
**Output:** Feature shipped, launch campaign live, support active, metrics tracking enabled

Execute the **launch**:
- **T-2 hours:** Final checks—feature gate enabled, rollback ready, support team on-call
- **T-0:** Launch announcement goes out (marketing email, in-app announcement, sales alert)
- **T+1 hour:** Sales team equipped with early customer success stories, monitor support queue
- **T+1 day:** Monitor key metrics (adoption, errors, support tickets), publish launch blog post
- **T+3 to T+7:** Customer migration settles, monitor for escalations, gather feedback from beta customers
- **T+2 weeks:** First retrospective with launch team, identify optimization opportunities for next phase

**Launch Monitoring Dashboard:**
```
Real-time metrics (updated hourly for first 24hrs, then daily):
  - Feature adoption: % of eligible customers who enabled feature
  - Critical errors: Any data loss, auth, or security issues
  - Support tickets: Volume, types, resolution time
  - Customer feedback: NPS sentiment, feature requests from early users

Go/No-Go Decision Points:
  - T+4 hours: Any critical errors? → Rollback if needed
  - T+24 hours: Support impact manageable? Continue or pause new customer signups?
  - T+3 days: Adoption tracking to plan? Any messaging/positioning misses?
  - T+2 weeks: Feature ready for marketing case study phase?
```

**User Interaction Point:** Launch day command center with PM, Engineering lead, Support lead, and Marketing. Daily sync for first week. PM makes go/no-go decisions if issues arise. 30 min daily standup.

**Data Handoff:** Launch metrics and early feedback flow to Step 5 (Metrics Analysis).

---

### Step 6: Post-Launch Metrics & Iteration (Week T+2 to T+4)
**Owner:** PM + Data Analytics  
**Input:** Launch data, customer feedback, support tickets, usage metrics  
**Output:** Launch success report, iteration roadmap, customer success stories

Run the **metrics-analysis** skill to:
- Score launch against pre-defined success criteria (adoption targets, support impact, customer sentiment)
- Analyze customer cohorts: which segments adopted fastest? Any laggards?
- Review support tickets: what are the most common questions/issues?
- Assess feature usage: which workflows are customers using? Which are untapped?
- Gather qualitative feedback: customer interviews, review star ratings, community posts
- Create launch retrospective: what went well? What would we do differently?
- Identify optimization opportunities for next release cycle
- Build customer success stories and case studies from early adopters

**Metrics Report Structure:**
```
Success Criteria Scorecard:
  - Adoption: Target 25% of eligible customers by T+14 → Actual 28% ✓ (exceeded)
  - Support impact: <1% of support tickets on new feature → Actual 0.3% ✓
  - Customer sentiment: 4.0+ rating → Actual 4.2 stars ✓
  - No critical data loss → 0 incidents ✓

Insights by Segment:
  - Enterprise (500+): 45% adoption, highest engagement
  - Mid-market (50-500): 22% adoption, slow uptake on [specific feature]
  - Freemium: 12% beta opt-in, indicate lower value perception

Usage Patterns:
  - Most used: Shift scheduling automation (65% of adopters)
  - Least used: Predictive analytics (18% of adopters) - opportunity for doc improvement
  - Average session: 8 minutes, up from 6 minutes pre-launch

Optimization Roadmap:
  - Quick wins (T+2 weeks): Improve docs for predictive analytics
  - Medium-term (next quarter): Build guided tutorial for first-time users
  - Long-term: Premium tier bundling AI features (pricing strategy)

Customer Success Stories:
  - Featured customer A saved 12 hrs/week on scheduling
  - Featured customer B reduced hiring time by 30%
  - Create case studies for marketing/sales reuse
```

**User Interaction Point:** PM and data team review metrics and agree on success assessment. Leadership gets executive summary. Team discusses optimization roadmap. Marketing packages customer success stories. 1 hour retrospective.

**Data Handoff:** Success metrics become baselines for future features. Optimization roadmap feeds into next quarter's planning cycle.

---

## Data Handoff

| From Step | To Step | Data | Format | Reference |
|-----------|---------|------|--------|-----------|
| 1 | 2 + 3 | Positioning, messaging, campaign calendar | GTM strategy doc | Sales playbook and release notes both reference core messaging |
| 2 | (Published) | Sales playbook, battle cards, training | Shared drive/LMS | Sales team trained before T-day |
| 3 | (Published) | Release notes, FAQs, migration guides | Published to help center + in-app | Customers can self-serve during launch week |
| 4 | (Executed) | Customer comms, support runbook | Email + internal docs | Support team equipped T-day morning |
| 5 | 6 | Launch metrics, adoption data, feedback | Spreadsheet + qualitative notes | Metrics analysis builds on raw launch data |

**Single Source of Truth:** Create a "Launch Master Doc" that timestamps each major milestone (GTM approved, Sales trained, Docs published, T-day launch, first metrics review, optimization roadmap locked). Updates to any step reference this doc.

---

## Parallelization Opportunities

### Critical Path (Sequential):
```
Step 1 (GTM Strategy) ──must complete──> Steps 2, 3, 4 (can run in parallel)
                                           ↓
                                    Step 5 (Launch Execution)
                                           ↓
                                    Step 6 (Metrics & Iteration)
```

### Parallel Tracks (Weeks T-5 to T-1):

**Track A: Sales Enablement (Step 2)**
- Duration: 2-3 weeks
- Deliverables: Playbook, battle cards, training videos
- Depends on: GTM positioning from Step 1
- Team: Sales lead + PM

**Track B: Customer Documentation (Step 3)**
- Duration: 2-3 weeks
- Deliverables: Release notes, feature docs, FAQ, in-app messaging
- Depends on: GTM positioning + feature spec
- Team: PM + technical writer

**Track C: Customer Migration (Step 4)**
- Duration: 1-2 weeks
- Deliverables: Migration plan, customer comms, support runbook
- Depends on: Feature spec + GTM timeline
- Team: Customer Success + Engineering

**These three tracks run in parallel** from Week T-5 through T-1. Merge point is T-day launch (Step 5).

### Example Timeline:
```
Week T-7 to T-5: [GTM Planning]
Week T-5 to T-3: [Sales Enablement (parallel)]
                  [Customer Docs (parallel)]
                  [Migration Planning (parallel)]
Week T-3 to T-1: [Final polish on all three]
Week T to T+2:   [Launch Execution]
Week T+2 to T+4: [Metrics & Iteration]
```

---

## User Interaction Points

1. **GTM Approval (Week T-5):** Leadership reviews positioning and campaign plan. Budget and resource allocation confirmed. 45 min sync.

2. **Sales Training Kickoff (Week T-4):** Sales leader runs training for sales team. Q&A on objections and battle cards. 1-2 hours live training.

3. **Docs Review (Week T-3):** Product and legal review release notes. Marketing reviews messaging consistency. 30 min review.

4. **Migration Planning Sync (Week T-3):** Customer Success and Engineering confirm migration timeline and support readiness. 1 hour working session.

5. **T-day Launch Readiness (Week T-1):** Final checklist review. All teams confirm readiness (launch, support, marketing, engineering). Feature gate enabled, rollback tested. 30 min pre-flight check.

6. **Launch Day Command Center (T-day):** Real-time monitoring with PM, Engineering, Support, Marketing. Any critical issues trigger escalation. 8 hour coverage from launch through +4 hours.

7. **Daily Standups (T+1 to T+7):** 15-30 min daily syncs on metrics, support volume, and any issues. Decisions on rollback, pause new signups, or continue full throttle.

8. **Metrics Retrospective (T+14):** Full team (PM, Eng, Marketing, Support, Data) reviews success metrics and optimization roadmap. 1 hour retrospective.

**Total PM Time Investment:** ~30-40 hours over 8 weeks (mostly pre-launch, lighter post-launch). Launch day itself is 8 hours live monitoring.

---

## Estimated Timeline

| Phase | Duration | Notes |
|-------|----------|-------|
| GTM Planning (Step 1) | 2 weeks | Weeks T-7 to T-5 |
| Sales Enablement (Step 2) | 2 weeks | Parallel, Weeks T-5 to T-3 |
| Customer Documentation (Step 3) | 2 weeks | Parallel, Weeks T-4 to T-2 |
| Customer Migration (Step 4) | 1-2 weeks | Parallel, Weeks T-3 to T-1 |
| Launch Execution (Step 5) | 2 days | T-day + T+1 day active |
| Post-Launch Monitoring (Step 5 cont.) | 2 weeks | T+2 to T+14 (daily standups) |
| Metrics & Iteration (Step 6) | 2 weeks | T+2 to T+4 |
| **Total Elapsed Time** | **8-12 weeks** | T-8 through T+4 |

---

## Final Output Summary

By the end of this workflow, you have:

1. **Executed GTM Strategy**
   - Feature positioned in market with clear differentiation
   - 12-week campaign calendar with email, content, and sales blitz coordinated
   - Budget allocated and marketing milestones tracked
   - Messaging consistent across sales, marketing, and customer communications

2. **Sales Team Enabled**
   - Playbook with discovery questions, objection handling, and close scenarios
   - Battle cards showing competitive differentiation
   - Pricing guide with discount authority and ROI calculator
   - Training completed with certification; sales team confident in messaging
   - Early customer success stories for social proof

3. **Customer Documentation Complete**
   - Release notes and feature guides published
   - FAQ addressing common questions and edge cases
   - Migration guides for existing customers
   - In-app messaging and tooltips ready for deployment
   - Video tutorials and screen captures for guided onboarding

4. **Customer Migration Executed**
   - Existing customers notified with clear upgrade path
   - Support team trained and ready for surge in tickets
   - Rollback plan in place if issues arise
   - Monitoring and escalation procedures documented

5. **Successful Launch**
   - Feature shipped to production with zero critical incidents
   - Launch metrics tracked and exceeding targets (e.g., 28% adoption vs. 25% target)
   - Support team handling volume smoothly with <1% of tickets on new feature
   - Customer feedback collected and positive sentiment (4.2 stars vs. 4.0 target)
   - Sales team reporting early wins and customer enthusiasm

6. **Post-Launch Metrics & Optimization Plan**
   - Launch success report with comparison to pre-defined criteria
   - Cohort analysis showing which customer segments adopted fastest
   - Usage patterns documented (which features customers are using, which untapped)
   - Customer success stories and case studies for marketing reuse
   - Optimization roadmap for next iteration (quick wins, medium-term improvements, long-term strategy)

**Handoff Status:** Feature is live, market is aware, sales is selling, support is handling issues, and you have clear data on what's working and what to optimize next. Product enters the steady iteration phase.

---

## When to Use This Workflow

✓ **Major feature launches** requiring coordination across marketing, sales, and support  
✓ **New product tiers or pricing changes** needing go-to-market strategy  
✓ **Platform changes** affecting large portions of customer base  
✓ **When you have 8+ weeks** to plan a coordinated launch  
✓ **High-stakes launches** where GTM strategy and execution matter  
✓ **When existing customer base** needs migration or upgrade path  

---

## When NOT to Use This Workflow

✗ **Urgent emergency releases** → Skip to Step 5 directly (launch only, no GTM prep)  
✗ **Small incremental features** → Use lightweight "Mini Launch" (release notes + customer comms only)  
✗ **Internal-only features** (tools for employees) → Skip Steps 1-2, just do documentation  
✗ **Beta or experimental features** → Use lighter approach, no full launch sequence yet  
✗ **When you're not sure feature should launch** → Do validation first, then come back to launch planning  

**Alternative:** For faster launches, use "Express Launch" workflow (GTM only, skip detailed sales enablement; release notes + migration guides only).
