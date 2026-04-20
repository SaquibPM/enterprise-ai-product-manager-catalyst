---
title: Pricing and Packaging for Enterprise B2B SaaS
slug: pricing-packaging
description: Design value-metric-based pricing, tiered packaging, and enterprise contracts that accelerate customer acquisition and expansion. Learn procurement negotiation dynamics, willingness-to-pay research, and how to add modules without commoditizing your platform.
author: Enterprise AI PM Team
created: 2026-04-12
version: 1.0
category: Monetization & Growth
difficulty: advanced
time_estimate: 90 minutes
---

# Pricing and Packaging for Enterprise B2B SaaS

## Purpose

Pricing is product strategy. The structure you choose determines which customers you attract, how much they use your platform, and how much revenue you capture.

In enterprise B2B, pricing failures often look like:
- "We price per-seat, so customers minimize logins (the opposite of adoption)"
- "We use platform fees only, so customers compare us to free tools"
- "We custom-quote everyone (no pricing discipline, sales team creates chaos)"
- "We give away key functionality; customers never expand"

This skill teaches you to:
- Identify the value metric your customer truly buys
- Design tiered packaging that compels progression
- Price modules and add-ons to accelerate expansion
- Navigate procurement negotiations without losing margin
- Run willingness-to-pay research

## Key Concepts

### 1. Pricing Model Taxonomy

Each model incentivizes different customer behaviors. Choose wrong, and you're fighting your own monetization.

**Model 1: Per-Seat Pricing**

Structure: $X per user per month

Characteristics:
- Cleanest to implement (each login counts)
- Works for simple products with clear user differentiation
- Customer motivation: Minimize logins (hides product value)
- Enterprise problem: Finance Analyst role critical to success, but customer adds only 1 to save money

Example:
```
Procurement Platform - Per-Seat Model
Pricing: $150/user/month

Cost for typical enterprise customer:
  - Procurement Lead (must-have): 2 users @ $150 = $300/month
  - Finance Analyst (required): 2 users @ $150 = $300/month
  - Department Managers (should-add): 8 users @ $150 = $1,200/month
  - CFO/Executive (nice-to-have): 1 user @ $150 = $150/month
  ────────────────────────────────────────────────────────
  Total: $1,950/month = $23,400/year

Problem:
  Customer stops at "must-have" (4 users) = $7,200/year
  Customer avoids adding Department Managers (would cost $9,600)
  You leave $16,200 on the table

When to use: Simple products, low adoption complexity, legal/compliance tools
When to avoid: Adoption depends on broad stakeholder use, value from data network effects
```

**Model 2: Per-Transaction/Usage Pricing**

Structure: $X per transaction, API call, data processed, or other unit of consumption

Characteristics:
- Aligns perfectly with customer value (more usage = more payment)
- Customers feel "fair" deal (pay only for what you use)
- Implementation challenge: Metering infrastructure
- Revenue risk: Unpredictable monthly bills

Example:
```
Procurement Platform - Per-Transaction Model
Pricing: $0.15 per transaction categorized

Cost for typical enterprise customer:
  - Annual spend volume: $50M
  - Avg transaction value: $5K
  - Transactions/year: 10,000
  - Cost: 10,000 × $0.15 = $1,500/year

Plus platform fee: $12,000/year
Total: $13,500/year

Issue: Unpredictable
  - If customer acquires: 15,000 transactions = $2,250 + $12,000 = $14,250
  - If customer consolidates: 5,000 transactions = $750 + $12,000 = $12,750

Customer motivation: Use platform more (every transaction is a sale)
```

**Model 3: Tiered/Volume Pricing**

Structure: Good tier at $X, Better tier at $Y, Best tier at $Z

Characteristics:
- Clear differentiation (you know what you're getting at each tier)
- Procurement-friendly (easy to understand, compare)
- Incentivizes upgrades (customer can "taste" next tier)
- Most common for SaaS

Example:
```
Procurement Platform - Tiered Model

STANDARD TIER - $50,000/year
  ├─ Spend analytics (all transaction data)
  ├─ Basic AI recommendations
  ├─ Up to 5 users
  ├─ Email support
  └─ 90-day data retention

PROFESSIONAL TIER - $120,000/year
  ├─ Everything in Standard
  ├─ Advanced AI recommendations (savings opportunities)
  ├─ Up to 25 users
  ├─ Phone + email support
  ├─ 2-year data retention
  └─ Custom reports

ENTERPRISE TIER - Custom pricing ($200K-$500K+)
  ├─ Everything in Professional
  ├─ AI + RPA (Robotic Process Automation)
  ├─ Unlimited users
  ├─ Dedicated account manager
  ├─ 7-year data retention
  └─ Custom integrations

Typical expansion:
  Year 1: Customer buys STANDARD ($50K, 5 users)
  Year 2: Expands to PROFESSIONAL ($120K, new department)
  Year 3: Evaluates ENTERPRISE (RPA for approval workflows)
```

When to use: Multiple distinct customer segments, clear feature differentiation, want self-serve for lower tiers
When to avoid: Can't differentiate tiers meaningfully, single homogeneous customer base

**Model 4: Custom Enterprise**

Structure: Negotiate pricing per customer based on scale, commitment, budget

Characteristics:
- Maximize revenue per customer (each deal unique)
- Maximum complexity (no standardization, sales chaos)
- High-touch sales process (6-12 month sales cycles)
- Customer perception: Unfair (why did they pay less?)

When to use: Only for true enterprise deals ($200K+), small number of customers (30-50)
When to avoid: Want to scale to 300+ customers, have 100+ customers, customers are mid-market

### 2. Value Metric Identification Framework

Before choosing a pricing model, identify what customers actually value and buy.

**Definition:** A value metric is the unit of value your customer experiences per dollar they pay.

**How to identify YOUR value metric:**

**Step 1: Interview 5-10 customers**

Ask: "If you could only pick one metric to measure your ROI, what would it be?"

Example answers:
- "Time saved per transaction categorization"
- "% of spend we can automatically categorize"
- "Number of procurement decisions informed"

**Step 2: Map to usage data**

In your platform analytics, find the metric that correlates with:
- Customer willingness to renew
- Customer expansion requests
- Customer support ticket volume (negative indicator)
- Customer satisfaction

**Step 3: Validate with pricing research**

Ask 10 customers: "Would you rather pay per [metric A] or per [metric B]?"

Example:
- "Pay per user (seat-based) or per transaction processed?"
- Answers reveal which metric they feel is most fair

**Worked Example: Procurement Platform**

Candidates:
1. Per-seat (user licenses)
2. Per-transaction (# of transactions categorized)
3. Per-module (which features they adopt)

Customer interviews reveal:
```
Customer 1 (Bank): "We care about % of procurement we can automate"
Customer 2 (Retailer): "We care about time saved per approver"
Customer 3 (Tech): "We care about how many departments use this"
```

Conclusion:
Primary value metric: "Number of Finance Analysts actively engaged"
Secondary: "Spend categories automated"

### 3. Tiered Packaging Strategy: Good/Better/Best

The classic three-tier structure. Getting it right drives expansion.

**Tier Design Principles:**

1. **Good tier is your bread-and-butter** (80% of customers should land here)
   - Price: Accessible to mid-market ($50K-150K/year)
   - Features: Solves core problem
   - Users: Enough for team (5-15 users)
   - Support: Email/self-serve
   - Data retention: 12 months (sufficient for ROI)

2. **Better tier is the expansion path** (15% of customers upgrade here)
   - Price: 2-2.5x Good tier ($120K-375K/year)
   - Features: Adds advanced capabilities (AI, reporting, integrations)
   - Users: Broader stakeholder access (25+ users)
   - Support: Phone + priority queue
   - Data retention: 24 months
   - Designed to be "too much" for initial buyer but obvious next step

3. **Best tier is enterprise** (5% of customers here, negotiated)
   - Price: Custom or 3-4x Better tier
   - Features: All features + custom integrations
   - Users: Unlimited
   - Support: Dedicated account manager
   - Data retention: 7 years (regulatory)

**Feature Gating Logic:**

Don't arbitrarily restrict features. Gate based on clear segmentation:

```
Feature: AI Recommendations
├─ Standard Tier: Basic recommendations (top 3 categories)
├─ Professional Tier: Advanced (ranked by savings opportunity)
└─ Enterprise: Customized + Real-time feedback loop

Feature: Users/Seats
├─ Standard: 5 named users
├─ Professional: 25 named users (added as needed)
└─ Enterprise: Unlimited (SSO, org-wide)

Feature: Support
├─ Standard: Email, 24-hour response time
├─ Professional: Phone + email, 4-hour response
└─ Enterprise: Dedicated account manager, 1-hour response
```

### 4. Enterprise Negotiation Dynamics

When enterprise customers negotiate pricing, understand the procurement playbook.

**Enterprise Procurement Playbook:**

```
STAGE 1: RFP (Request for Proposal)
├─ Goal: Get your lowest possible quote
├─ Tactic: "Tell us your best price for 50 users, 3-year commitment"
├─ Your response: Stick to tiered pricing. Offer discount only for 3-year commitment
└─ Example: Standard Tier $50K/yr × 3 years = $150K total
           → 3-year commit discount: 10% off = $135K total

STAGE 2: Comparison (Bidding War)
├─ Goal: Get you to match competitor pricing
├─ Tactic: "Your competitor quoted $120K; can you match?"
├─ Your response: Don't. Emphasize differentiation
│                 "Our Professional Tier at $120K includes [features they lack]"
└─ Anti-pattern: "Ok, we'll do $115K" (now you're not profitable)

STAGE 3: Volume Discounts
├─ Goal: Get 30-50% discount for "scale"
├─ Tactic: "We're buying for 3 business units; can you give us a discount?"
├─ Your response: YES, but structure it carefully
│                 → 1 BU: Professional $120K
│                 → 2 BU: Professional $115K each ($230K total, 4% discount)
│                 → 3 BU: Professional $110K each ($330K total, 8% discount)
│                 → ENTERPRISE CUSTOM: $300K for all 3 BU unlimited
└─ Logic: Reward multi-year/multi-unit commits, not simple volume

STAGE 4: Terms & Conditions
├─ Goal: Get favorable payment terms (NET 90 instead of NET 30)
├─ Tactic: "We need NET 90 payment terms"
├─ Your response: YES (enterprise standard), but not NET 120+
│                 → NET 30: No discount
│                 → NET 60: 2% discount
│                 → NET 90: 0% discount (standard)
└─ NET 120+: Escalate to CFO, likely no

STAGE 5: Custom Requirements
├─ Goal: Get you to build integrations for free
├─ Tactic: "We need a custom integration to SAP. Can you build it?"
├─ Your response: YES, but with scope and cost
│                 → Standard Tier: Not included (escalate to Professional)
│                 → Professional: 3 integrations/year included
│                 → Enterprise: Custom integrations negotiated per deal
└─ Escalation: "Building SAP integration is $50K. Include it in Enterprise tier deal."

STAGE 6: Multi-Year Incentives
├─ Goal: Get 20%+ discount for 3-year upfront payment
├─ Tactic: "We'll pay 3 years upfront if you give us 25% off"
├─ Your response: Yes, but cap the discount
│                 → 1-year: Full price
│                 → 2-year: 10% discount
│                 → 3-year: 15% discount
└─ Rationale: 3-year = $360K upfront revenue (worth 15% discount for cash flow)
```

## Application: The Complete Worked Example

### Scenario: Procurement Analytics Platform Pricing Redesign

**Current state (custom model—chaos):**
```
Customer 1 (Bank): $180K/year
Customer 2 (Retailer): $220K/year
Customer 3 (Manufacturer): $140K/year
Problem: No consistency. Finance can't forecast.
```

**Expansion Proposal Structure:**

STANDARD TIER: $50,000/year
- Spend analytics (all transaction data)
- Spend by category, vendor, business unit
- Up to 10 users
- Email support (24-hour response)
- Pre-built integrations (NetSuite, Coupa, Ariba)
- 12-month data retention
- API access (read-only, 1M calls/month)
- ROI Target: 6-month payback
  - 25% faster P2P cycle = $80K savings
  - Cost $50K = $30K net benefit, 160% ROI

PROFESSIONAL TIER: $120,000/year
- Everything in Standard, plus:
- AI Recommendations (category predictions, spending patterns)
- Savings Opportunities (AI identifies cost reduction levers)
- Advanced Analytics (cohort analysis, forecasting)
- Custom Reports & Dashboards
- Up to 50 users
- Phone + email support (4-hour response)
- SSO/SAML (Okta, Azure AD)
- 24-month data retention
- Custom integrations (2 per year included)
- ROI Target: 4-month payback
  - P2P cycle 45% faster = additional benefit
  - AI-identified savings: $200K-500K annually
  - Total value: $200K+ savings
  - Cost: $120K = 167% ROI

ENTERPRISE TIER: Custom ($300K-$600K/year)
- Everything in Professional, plus:
- RPA (Robotic Process Automation for approval workflows)
- Supplier Risk Analytics
- Contract Management
- Unlimited users + departments
- Dedicated account manager
- 1-hour support response time
- 7-year data retention (SOX compliance)
- Unlimited API access
- Custom integrations (unlimited)

## Common Pitfalls

### Pitfall 1: Per-Seat Pricing for AI Features

**The trap:** "We'll charge $100/user/month to use AI recommendations"

This kills adoption. Customers add expensive, critical AI users very slowly.

**How to avoid:**
- Use platform-based pricing for AI (not per-seat)
- Use module-based pricing (AI module = $30K/year add-on, not per-user)
- Gate AI on adoption (Professional tier gets AI if they have 70%+ data coverage)

### Pitfall 2: Giving Away Platform Value in "Pro" Tier

**The trap:** "Pro tier includes everything—customers often don't need Enterprise"

If you include RPA in Professional tier, no customer upgrades to Enterprise.

**How to avoid:**
- Standard tier: Core product only (analytics, basic features)
- Professional: Advanced features (AI, custom reports)
- Modules: Expensive capabilities (RPA $80K, Risk $50K, Contracts $40K)
- Enterprise: Unlimited + full suite

### Pitfall 3: No Expansion Path (Tiers Too Far Apart)

**The trap:** "We have Standard ($50K) and Enterprise ($400K)"

Customers get stuck. Can't justify $350K jump to next tier.

**How to avoid:**
- Design three tiers: Good ($50K), Better ($120K), Best ($300K)
- Each tier is ~2.4x previous tier
- Customers can expand in stages
- Add modules to extend within tier

### Pitfall 4: No Pricing Discipline (Sales Negotiating Down)

**The trap:** "Ok, we can do $90K for your Enterprise customer" (Professional tier is $120K)

Once one customer gets $90K, sales will offer it to others. No pricing discipline.

**How to avoid:**
- Define clear discount structure (only for commitments, multi-year, multi-module)
- Empower sales with "discount authority" limits ($2K max without PM approval)
- Track every deviation in CRM
- Monthly review: "Why did we discount this deal?"

### Pitfall 5: Ignoring Procurement Cycle Timing

**The trap:** Raising prices mid-fiscal year when customers have frozen budgets

**How to avoid:**
- Plan price increases 6 months ahead
- Announce in Q1-Q2 (takes effect next fiscal year)
- Grandfather existing customers for 1 year
- Time increases around customer renewal cycles

## References

### Frameworks & Methodologies
- **Value-Based Pricing**: "Monetizing Innovation: How Smart Companies Design the Product That Markets Itself" by Madhavan Ramanujam & Georg Tacke
- **Pricing Psychology**: "The Strategy and Tactics of Pricing" by Thomas Nagle, John Hogan, & Joseph Zale
- **Willingness-to-Pay Research**: Van Westendorp Price Sensitivity Analysis
- **Enterprise Negotiations**: "Never Split the Difference" by Chris Voss

### Enterprise SaaS Benchmarks
- **Average ASP (Annual Spend Per Customer)**:
  - SMB/Trial: $25K-$50K
  - Mid-Market: $50K-$200K
  - Enterprise: $200K-$1M+
- **Expansion Rate**: 30-40% of customer base expanding annually
- **NRR by Pricing Model**:
  - Per-seat: 95-105% NRR (hard to expand without revenue increase)
  - Platform + usage: 105-115% NRR
  - Tiered: 105-120% NRR (tier progression + add-ons)

---

**Next Steps:** Interview 10 customers on their value metric. Run willingness-to-pay study. Map to tiered structure. Calculate module pricing. Brief sales on new model.
