---
title: "TAM/SAM/SOM Market Sizing for Enterprise B2B"
description: "Stop saying 'TAM is $100B' because 'everyone has a procurement department.' Learn to size markets rigorously: top-down from Gartner/analyst data, bottom-up from contract economics, triangulated with reality checks. Build the business case that investors and your board will actually believe."
purpose: "Build credible TAM/SAM/SOM analysis that withstands scrutiny from investors, executives, and sales leaders. Calculate addressable markets using both top-down and bottom-up methodologies, document assumptions, and use sanity checks to validate your numbers."
key_concepts:
  - "TAM (Total Addressable Market): Entire market opportunity if you captured 100% of serviceable market"
  - "SAM (Serviceable Addressable Market): Market you can realistically serve given product, GTM, geography"
  - "SOM (Serviceable Obtainable Market): Your realistic revenue target given competitive dynamics and sales capacity"
  - "Top-down methodology: Starting from total market, filtering by addressable segments"
  - "Bottom-up methodology: Building from unit economics, contract value, customer count"
  - "Sanity checks: Triangulating top-down and bottom-up to catch errors and validate assumptions"
  - "Segment-based sizing: Different ICPs have different TAM; don't hide behind total market"
  - "Replacement vs. new spend: What market are you actually capturing?"
---

## Purpose & Context

Most PMs hand-wave market sizing: "There are 10,000 companies with procurement teams × $500K ACV = $5B TAM." Then investors ask three follow-up questions and your analysis crumbles.

Enterprise market sizing requires rigor. You need to:
- **Start with credible data sources** (Gartner, IDC, Bureau of Labor Statistics, SEC filings), not assumptions
- **Distinguish TAM from realistic market** (you won't get 100% of procurement; you'll get maybe 5-10%)
- **Segment by buyer profile** (enterprise has different TAM than mid-market; different product positioning means different addressable market)
- **Calculate from unit economics** (if your contract is $300K and you need $100K CAC, your unit economics only work in companies with $5M+ spend; that narrows your SAM)
- **Validate with bottom-up logic** (if TAM is $100B and there are 10,000 addressable companies, that's $10M ACV; does that match what you're seeing in deals? If not, something's wrong)

This skill teaches you to build market sizing that answers these questions:
1. "Is this market big enough to build a meaningful business?" (TAM must be >$500M for venture-scale company)
2. "Can we realistically serve this market?" (SAM must be reachable with our GTM)
3. "What revenue can we realistically build?" (SOM must justify spending levels)

---

## TAM Sizing: Top-Down Methodology

### Step 1: Define Your Total Market (Starting Point)

Begin with analyst market sizing. This is easier than building from scratch.

**Key Data Sources:**

**Gartner Magic Quadrant Reports** (Most credible for enterprise software)
- Analyst research on market size, growth rate, and key players
- "The Procure-to-Pay software market was $4.2B in 2023, growing 8% CAGR"
- Cost: $2,000-$5,000 per report (worth it; your board will cite it)
- Where to access: Gartner.com, or ask your network for access

**IDC Forecasts** (Good for infrastructure and platform markets)
- Market sizing and vendor share data
- Usually publicly available forecasts
- Where: IDC.com, analyst reports

**Forrester Wave Reports** (Good for comparative analysis)
- Market sizing and vendor positioning
- "Procure-to-Pay software buyers evaluate 15-20 vendors"
- Where: Forrester.com

**LinkedIn Talent Data** (For function-based sizing)
- "There are 450,000 procurement professionals in the US"
- If 40% work at companies >$500M revenue, that's 180,000 potential buyers
- Where: LinkedIn Recruiter, Linkedin.com/jobs

**U.S. Bureau of Labor Statistics** (For function-based sizing by company size)
- "23-2099: Procurement clerks: 124,000 employees" (as of 2023)
- Distributed by company size (available via NAICS codes)
- Where: bls.gov

**SEC Filings & Annual Reports** (For validation and customer data)
- Public companies disclose spending on software categories
- "Acme Corp spent $12M on procurement software in 2023" (you can extrapolate)
- Where: SEC.gov (10-K filings)

### Step 2: Filter to Addressable Market

Not every dollar in the total market is addressable to you.

**Example: Procure-to-Pay (P2P) Market Sizing**

Starting point (from Gartner): **Global P2P software market = $4.2B (2023)**

Now filter:

**Filter 1: Geography**
- Your GTM is North America first → $1.8B (43% of global)
- *(This excludes EMEA, APAC, which have different dynamics and you're not selling to yet)*

**Filter 2: Company Size / Spend Threshold**
- Your product only makes sense for companies with 500+ employees (because smaller companies don't have headcount for complex AP)
- U.S. has approximately 20,000 companies with 500+ employees
- Of those, 85% have procurement/AP functions that could use your product
- That's **17,000 potential customers**

**Filter 3: Functional Category Alignment**
- Total P2P market = invoice processing + purchase order management + vendor management + contract management + expense management
- Your product is focused on invoice matching (AP side), not full P2P
- Invoice matching is ~35% of P2P market value
- Your addressable slice: $1.8B × 35% = **$630M**

**Filter 4: Segment Mix**
- Not all of that $630M is at price point your product supports
- Enterprise segment (>$5B revenue): $420M TAM, avg ACV = $500K
- Mid-Market ($500M-$5B revenue): $180M TAM, avg ACV = $150K
- SMB (<$500M revenue): $30M TAM, avg ACV = $30K
- **You're focusing on Enterprise + Mid-Market: $600M of $630M**

**Result:**
- Total P2P market = $4.2B
- Filtered TAM (N.A., 500+ employees, invoice matching focus, excludes low-end SMB) = **$600M**

### Step 3: Document Assumptions

This is critical. Every filter you apply is an assumption; investors will question them.

**Assumption Documentation Template:**

| Assumption | Value | Source / Reasoning | Sensitivity |
|---|---|---|---|
| Global P2P market size (2023) | $4.2B | Gartner Magic Quadrant, 2024 | ±15% ($3.6B-$4.8B) |
| North America % of global | 43% | Gartner regional breakdown | ±5% (38-48%) |
| U.S. companies 500+ employees | 20,000 | BLS data, LinkedIn validation | ±10% (18,000-22,000) |
| % with AP function | 85% | Industry assumption (most companies have AP) | ±5% (80-90%) |
| Invoice matching % of P2P market | 35% | Based on typical P2P spending breakdown | ±10% (25-45%) |
| Enterprise + Mid-Market focus | 95% | Company GTM strategy | ±5% (90-100%) |
| **Result TAM** | **$600M** | Sum of filters | **±25% ($450M-$750M)** |

**What Investors Look For:**
- Are your sources credible? (Gartner, BLS = yes. "I think" = no)
- Are your filters realistic? (85% of large companies have AP = reasonable. 99% use AI = unrealistic)
- Do you acknowledge uncertainty? (±25% = honest. Exactly $600M = looks made up)

---

## TAM Sizing: Bottom-Up Methodology

Top-down gives you market size. Bottom-up validates whether that size is real by building from customer fundamentals.

### Step 1: Define Your Addressable Customer Base

Start narrow. Who are you actually selling to?

**Example: Enterprise AP Automation**

**Primary ICP (Ideal Customer Profile):**
- Company size: >$500M annual revenue
- Function: Accounts Payable (CFO's org)
- Invoice volume: 5,000+ invoices/month
- Problem: Manual exception handling consuming 30%+ of team time
- Budget: $150K-$500K annual software spend on AP function

**Secondary ICP (but different sales/product strategy):**
- Mid-market: $100M-$500M revenue
- Invoice volume: 2,000-5,000/month
- Budget: $50K-$150K annual software spend

### Step 2: Estimate Total Addressable Customers

Use multiple data sources to triangulate:

**Method A: LinkedIn Data**

LinkedIn (via LinkedIn Sales Navigator or Recruiter):
- Search: "AP Manager" + "Chief Financial Officer" + "Accounts Payable Manager"
- Filter by company size (>$500M revenue)
- Filter by location (U.S. initially)
- LinkedIn estimate: ~15,000 companies with >$500M revenue searching for AP talent (2023)
- *Reasoning: If a company is hiring AP managers, they likely have meaningful AP function*

**Method B: Bureau of Labor Statistics**

BLS data on "AP clerks" by company size:
- Total AP clerks in U.S.: ~124,000 employees (as of 2023)
- In companies with 500+ employees: ~65,000 employees (52%)
- Average AP team size: 3.5 people per company
- **Estimated companies with AP function: 65,000 / 3.5 ≈ 18,600 companies**

**Method C: Business Database Data**

Using business registries (Dun & Bradstreet, Bloomberg, S&P Capital IQ):
- Companies >$500M revenue: ~20,000 in North America
- % with procurement/AP function: 85% (reasonable assumption)
- **Addressable: 17,000 companies**

**Triangulation Result:**
- LinkedIn estimate: 15,000
- BLS estimate: 18,600
- Business registry estimate: 17,000
- **Conservative middle estimate: ~16,000 companies**

### Step 3: Calculate Average Contract Value (ACV)

What's your realistic pricing?

**Method A: Price Based on Savings**

Your product saves the customer money. Price it as % of savings.

Example:
- Exception handling costs enterprise $400K/year (50 hours/week × 2 people × $50/hour × 52 weeks)
- Your product reduces exceptions by 60%, saving $240K/year
- Price your solution at 25% of annual savings = **$60K ACV**
- (Leaves customer with $180K net savings; ROI is 3:1, which is attractive)

For mid-market:
- Exception handling costs: $150K/year (1.5 FTE)
- Your product saves 60%: $90K/year
- Price at 30% of savings: **$27K ACV**

**Method B: Price Based on Benchmarks**

What are competitors charging?

Research via:
- Customer reviews (G2, Capterra): "Pricing is $X-$Y per year"
- Win/loss interviews: "Competitor A quoted us $Z"
- Sales team: "Enterprise deals are typically $300K-$500K"

For AP automation market:
- Enterprise: $300K-$500K ACV (typical range)
- Mid-market: $100K-$150K ACV
- **Use $400K enterprise, $125K mid-market**

**Method C: Price Based on Similar Categories**

What do companies pay for comparable software?

Benchmarks:
- ERP (SAP, Oracle): $2M-$10M implementation + $500K-$2M annual
- Spend analytics: $200K-$500K annual
- Procurement platforms: $300K-$1M annual
- Point solutions (like invoice matching): $100K-$500K annual

**For invoice matching: $200K-$400K for enterprise seems right**

### Step 4: Calculate Bottom-Up TAM

**Formula:**
```
Bottom-Up TAM = Number of Addressable Customers × Average ACV
```

**Example Calculation:**

Enterprise segment:
- Addressable customers: 16,000 × 60% (only large companies need your product) = **9,600 companies**
- Average ACV: **$350K** (blended enterprise)
- Segment TAM: 9,600 × $350K = **$3.36B**

**Wait.** This is 5.6x your top-down TAM of $600M. Something's wrong.

### Step 5: Reconcile Top-Down vs. Bottom-Up

When they don't match, investigate.

**Option A: Your Bottom-Up is Too High**

Possible reasons:
- You over-estimated addressable customers (not all 16,000 need your product)
- You over-estimated ACV (competitors are cheaper than you assumed)
- You're not accounting for market share realities (your product is getting 5% market share, not 100%)

**Recalibration:**

"Our TAM is the total available to winners in invoice matching. But there's market concentration:
- Top 5 vendors (Coupa, Ariba, etc.) capture 60% of market
- We're a point solution, not a platform; we'll realistically capture 10% of remaining 40%
- Our realistic TAM = $600M (top-down) ÷ our expected market share (2-3%)"

**Or:**

"Maybe not all 16,000 companies are addressable. Companies with <$1B revenue might not have budgets for $350K software; they might be using manual processes or older software. Filter to $1B+ revenue = 8,000 companies. Reassess ACV at $300K (lower for smaller enterprises). = $2.4B. Still high."

**Option B: Your Top-Down is Too Conservative**

Possible reasons:
- You filtered too aggressively
- The analyst data is outdated or understates market
- Your specific niche (invoice matching) is growing faster than overall P2P market

**Investigation:**
- Re-examine each filter. Did you exclude a legitimate segment?
- Check Gartner growth rate (8% CAGR for P2P). If your niche (AI-powered matching) is growing 25% CAGR, that changes TAM
- Revisit analyst data; get 2024 reports, not 2023

### Step 6: Final Reconciliation

Converge on a number both sides can support.

**Scenario A: Conservative (investor wants lower TAM)**
- Top-down: $600M
- Bottom-up (adjusted): $1.2B (9,600 × $125K ACV; lower ACV to be conservative)
- **Reconciled TAM: $800M-$900M**

**Scenario B: Optimistic (but defensible)**
- Top-down: $600M (may be under-counting emerging vendors)
- Bottom-up: $2.4B (conservative customer count, mid-range ACV)
- **Reconciled TAM: $1.2B-$1.5B**

**Communicate the range, not a single number:** "Our TAM is $800M-$1.5B depending on market growth assumptions and your unit economics assumptions."

---

## SAM Sizing: Serviceable Addressable Market

TAM is the dream. SAM is what you can realistically capture given your product and GTM.

### Filters to Apply

**Filter 1: Geography**
- Start: N.A. (you've proven go-to-market)
- Year 3: Add EMEA
- Year 5+: Add APAC
- **Current SAM: N.A. only = 60% of global TAM**

**Filter 2: Company Size / Segment**
- You're good at: Enterprise (>$1B) and mid-market ($500M-$1B)
- You're not positioned for: SMB (<$500M) and self-serve
- **Segment-based SAM: 75% of addressable (exclude bottom SMB quartile)**

**Filter 3: Competitive Dynamics**
- Total TAM includes 20+ competitors (Coupa, Ariba, Bill.com, Stampli, etc.)
- Your realistic share: 10-15% of addressable market (given competition)
- **Competitive-adjusted SAM: 15% of TAM**

**Filter 4: Product Positioning**
- You focus on AP exceptions (smart matching)
- This is 35% of P2P market
- But you're not selling P2P; you're selling invoice matching specifically
- Companies buying best-of-breed point solution (not platform): 40% of market
- **Product-focused SAM: 40% of invoice matching market**

**Filter 5: GTM Capacity**
- Direct sales force: 20 reps
- Each rep can support $5M ACV/year (30 accounts × $150K)
- Total direct sales capacity: $100M/year
- To reach $500M revenue (10x sales capacity), you need: partnerships, resellers, self-serve
- Assume 60% direct, 40% channel/self-serve
- **GTM capacity-based SOM (year 5): $500M revenue from $2B SAM (25% capture)**

### SAM Calculation

Using all filters:

```
TAM (Top-Down)                           $600M
× Geography filter (N.A. only)           ×60%
= Geographic addressable market          $360M
× Company size filter (>$100M)           ×75%
= Market with sufficient pain/budget     $270M
× Competitive filter (10-15% realistic)  ×12%
= SAM (serviceable w/ competition)       $32.4M
```

**Wait, that's too low.** Let me recalculate.

Actually, the issue is filters should be multiplicative only if they're independent. Most are:

**Better framing:**

"SAM = TAM filtered by segments we can serve + GTM strategy + competitive reality"

**Segment-based SAM:**
- Enterprise (>$1B): $150M TAM, we can serve 15% = $22.5M SAM
- Mid-Market ($500M-$1B): $80M TAM, we can serve 12% = $9.6M SAM
- **Total SAM: $32M**

No, this still looks low. Let me reconsider the whole analysis.

**The issue:** My top-down TAM of $600M might be invoice matching only. P2P is bigger.

**Recalculated SAM (more realistic):**

- North America P2P market (not filtered to just invoice matching): $1.8B
- Addressable (companies >$500M): ~$1.2B
- Point solutions (vs. platform): 40% = $480M
- Invoice matching (our specific focus): 35% of point solution market = $168M
- **SAM (realistic serviceable market): ~$150M-$200M**

This is more credible. It's 25-33% of our TAM, which is reasonable given market concentration.

---

## SOM Sizing: Serviceable Obtainable Market

SOM is your realistic revenue given your execution, sales strategy, and market position.

### SOM Calculation Method

**SOM = Sales Capacity + Market Share Realistic Scenario**

**Formula:**
```
SOM Year 1 = (Sales Reps × ACV × Close Rate × Quota Attainment)
SOM Year 3 = (Sales Reps × ACV × Close Rate × Quota Attainment) + (Expansion Revenue)
SOM Year 5 = (Full GTM Motion) × (Market Share %)
```

### Example: Enterprise AP Automation SOM

**Year 1: Direct Sales Only**

Assumptions:
- Sales reps: 5 (early stage)
- ACV (enterprise): $350K
- Sales cycle: 120 days (so 3 deals per rep per year)
- Close rate: 15% (1 deal per 6-7 qualified opportunities)
- Opportunities per rep: ~20 per year
- Deals closed per rep: 3 deals/year
- **Year 1 revenue: 5 reps × 3 deals/rep × $350K = $5.25M**

**Year 3: Direct + Mid-Market**

Assumptions:
- Sales reps: 12 (enterprise) + 8 (mid-market)
- Enterprise ACV: $350K × 20 deals/year = $7M
- Mid-market ACV: $125K × 25 deals/year = $3.125M
- Expansion revenue (existing customers adding value): 20% × prior revenue = $2M
- **Year 3 revenue: $7M + $3.125M + $2M = $12.125M**

**Year 5: Full GTM Motion**

Assumptions:
- Direct sales (enterprise): 20 reps × $350K ACV × 4 deals/rep = $28M
- Direct sales (mid-market): 15 reps × $125K ACV × 8 deals/rep = $15M
- Self-serve/SMB: $5M (low ASP, high churn)
- Partnerships/channel: $7M
- Expansion revenue: 30% × prior revenue = $15M
- **Year 5 revenue: $28M + $15M + $5M + $7M + $15M = $70M**

**Five-Year Revenue Projection:**
- Year 1: $5M
- Year 2: $10M
- Year 3: $15M
- Year 4: $35M
- Year 5: $70M

**SOM Calculation:**
- SAM (realistic): $200M
- Our Year 5 revenue projection: $70M
- **SOM (Year 5): $70M (35% of SAM)**

This is aggressive but defensible given strong product and experienced team.

---

## Complete Worked Example: AI-Powered Spend Analytics for Enterprise Procurement

### Context
You're building spend analytics software for mid-market to enterprise procurement teams. It uses AI to identify savings opportunities, detect rogue spend, and consolidate vendor data. This is a real market with real players (Coupa, Ivalua, Determine, BravoSolution).

### TAM Sizing: Top-Down

**Step 1: Starting Market**

From Gartner Magic Quadrant (2024): 
- Spend Analytics software market = $3.2B globally (2023), growing 12% CAGR

**Step 2: Geographic Filter**
- North America: 52% of global = $1.66B
- We start U.S. only first: ~45% of N.A. = $750M (U.S. initial TAM)

**Step 3: Company Size Filter**
- Total addressable (U.S. >$500M revenue): 17,000 companies
- Average spend: $50M/year (assumption; ranges from $10M to $200M+)
- Annual procurement software budget: 0.4% of spend = $200K average
- **Market size: 17,000 × $200K = $3.4B**

This matches Gartner ($3.2B), so we're in the ballpark.

**Step 4: Our Specific Focus**
- Gartner includes: spend analytics, supplier management, sourcing, contracting
- We focus on: spend analytics + savings detection (35% of market)
- Our TAM: $3.4B × 35% = **$1.19B (U.S. only)**

**Step 5: Reality Check**
- Gartner says U.S. market is ~$1.5B
- Our $1.19B is conservative (excludes some overlapping use cases)
- **Final TAM: $1.2B ± 15%**

### TAM Sizing: Bottom-Up

**Step 1: Customer Count**

U.S. companies with >$500M revenue and significant procurement function:
- From LinkedIn: ~18,000 procurement leaders / directors
- From BLS: ~65,000 procurement staff in companies >$500M
- Estimate procurement teams per company: 4-6 people average
- **Estimated addressable companies: 65,000 / 5 = 13,000 companies**

**Step 2: Average Spend on Spend Analytics**

From win/loss interviews and customer conversations:
- Enterprise (>$5B revenue): $300K-$600K ACV (we see $400K average)
- Mid-market ($500M-$5B revenue): $100K-$200K ACV (we see $140K average)
- Percentage enterprise (>$5B): ~15% of market = 1,950 companies
- Percentage mid-market: ~85% of market = 11,050 companies

**Step 3: Calculate Bottom-Up TAM**

- Enterprise segment: 1,950 × $400K = $780M
- Mid-market segment: 11,050 × $140K = $1.547B
- **Bottom-Up TAM: $2.327B**

### Reconciliation

Top-down: $1.2B
Bottom-up: $2.3B

**Discrepancy Analysis:**
- Bottom-up ACV ($400K enterprise, $140K mid-market) might be high
- Or top-down TAM is under-counting (missing some procurement spend categories)
- Or customer count is lower than estimated (not all 13,000 companies are true ICP)

**Revised Analysis:**

Assume:
- Actual addressable customers: 12,000 (1,800 enterprise, 10,200 mid-market)
- Revised ACV: $350K (enterprise), $110K (mid-market) — lower due to competition and price sensitivity
- Revised bottom-up: 1,800 × $350K + 10,200 × $110K = $630M + $1.122B = **$1.75B**

**Converged TAM estimate:**
- Top-down: $1.2B (conservative, based on Gartner)
- Bottom-up (revised): $1.75B (based on customer count + ACV)
- **Reconciled TAM: $1.4B-$1.5B (middle of two estimates)**

### SAM Sizing

**Filters Applied:**
1. Geography (U.S. only initially): 100% (already filtered in TAM)
2. Company size (>$500M): 100% (already filtered)
3. Procurement pain (needing spend analytics): 80% (20% use manual Excel/BI tools, don't perceive need)
4. Budget availability: 70% (30% have budget frozen or allocated elsewhere)
5. Competitive consideration: 25% (4-5 major competitors; we get 25% consideration share)

**SAM Calculation:**
```
TAM: $1.45B
× Procurement pain filter: ×80%
× Budget filter: ×70%
× Competitive filter: ×25%
= SAM: $204M
```

**Alternative framing (by GTM motion):**

We can realistically serve:
- Enterprise (direct sales): $630M market, 15% realistic share = $95M
- Mid-market (direct + partners): $1.122B market, 12% realistic share = $135M
- **Total SAM: ~$230M**

(This aligns with the filtered calculation above.)

### SOM Sizing

**Year 1-3 SOM (Direct Sales Focus)**

Year 1:
- Sales team: 3 enterprise reps, 2 mid-market reps
- Enterprise: 3 reps × 2 deals/rep/year × $400K = $2.4M
- Mid-market: 2 reps × 4 deals/rep/year × $120K = $960K
- **Year 1 SOM: $3.36M**

Year 2:
- Sales team: 6 enterprise, 5 mid-market
- Enterprise: $4.8M
- Mid-market: $2.4M
- Expansion (10% of Y1): $336K
- **Year 2 SOM: $7.5M**

Year 3:
- Sales team: 12 enterprise, 10 mid-market
- Enterprise: $9.6M
- Mid-market: $4.8M
- Expansion (15% of prior): $1.8M
- Partnerships/SMB: $1M
- **Year 3 SOM: $17.2M**

**Year 5 SOM (Full GTM)**

Year 5:
- Direct enterprise: $20M (20 reps, mature)
- Direct mid-market: $12M (15 reps, mature)
- Self-serve/SMB: $2M
- Partnerships/channel: $3M
- Expansion revenue (25% of prior): $8.5M
- **Year 5 SOM: $45.5M**

**SOM as % of SAM:**
- Year 1: $3.4M / $230M = 1.5% (early days)
- Year 3: $17.2M / $230M = 7.5% (gaining traction)
- Year 5: $45.5M / $230M = 20% (market leadership position)

This is a realistic trajectory for a well-executed enterprise software company.

---

## Sanity Checks: Validating Your Numbers

Always do these checks before presenting TAM/SAM/SOM to your board or investors.

### Sanity Check 1: TAM Per Customer

**Check: Is your average ACV reasonable given problem severity?**

Formula:
```
Average ACV = TAM / Number of Addressable Customers
```

Example:
- TAM: $1.45B
- Addressable customers: 12,000
- **Implied ACV: $121K average**

Question: Does $121K average ACV make sense?
- If enterprise is $400K and mid-market is $140K, and mix is 15% enterprise + 85% mid-market:
- Blended: (0.15 × $400K) + (0.85 × $140K) = $60K + $119K = **$179K**

Mismatch. Our $1.45B TAM assumes lower ACV ($121K) than we actually see in deals ($179K).

**Resolution:** Either TAM is higher ($12,000 × $179K = $2.15B) or customer count is lower.

This is why you do sanity checks—you catch calculation errors before embarrassing yourself with investors.

### Sanity Check 2: Market Growth

**Check: Is your TAM growing with market?**

If market is growing 12% CAGR:
- Year 1 TAM: $1.45B
- Year 5 TAM: $1.45B × (1.12^5) = $2.56B

Your Year 5 SOM of $45.5M is capturing 1.8% of a $2.56B market—reasonable for a new player against entrenched competitors.

### Sanity Check 3: Competitive Dynamics

**Check: Is your SOM realistic given competition?**

Market leaders (Coupa, Ivalua) have ~$500M+ revenue. If TAM is $1.45B and leaders have 35-40% share:
- Leader market share: ~$500M revenue = 34% of TAM
- Remaining competitive set: 10-15 other vendors share ~$300M (21% of TAM)
- Opportunity for new vendor: ~25% of TAM = ~$360M realistic market share ceiling

Your Year 5 SOM of $45.5M is 3% of TAM, 12% of the realistic opportunity ceiling—conservative and credible.

### Sanity Check 4: Sales Productivity

**Check: Are your rep productivity assumptions realistic?**

Assumptions:
- $400K enterprise ACV
- 2 deals/rep/year
- $800K productivity per rep per year
- Add overhead (ramp time, non-productive time), actual cost per deal ≈ $200K CAC target
- **Rule of 40 economics: ($400K ACV - $200K CAC) / $200K CAC = 1.0x multiple, or must hit 8+ year retention to work**

This is tight. Enterprise deals need higher productivity (4+ deals/rep/year) or lower CAC (partner-driven), or higher ACV.

This sanity check tells you: your Year 1 SOM assumptions are aggressive. You need to either:
- Land larger deals (ACV $500K+)
- Improve rep productivity (4+ deals/rep)
- Lower CAC via partner channel
- Improve retention (reduce churn so you hit 8+ year LTV multiple)

### Sanity Check 5: Market Share Ceiling

**Check: Could this company realistically become market leader?**

Coupa (market leader) has ~$500M ARR, which is 34% of $1.45B TAM.

Could our company reach $500M ARR in 10 years?

- Year 1: $3.4M
- Growing 50% CAGR for 10 years: $3.4M × (1.50^10) = $656M
- **Yes, possible—but requires 50% growth for a decade, which is demanding**

More realistic: Build to $150M-$200M ARR (top 5 player), exit via acquisition or IPO.

---

## Common Pitfalls & How to Avoid

### Pitfall 1: "TAM = Everyone Who Could Possibly Benefit"

**What it looks like:**
- "Every company has procurement"
- "Procurement is 5% of total corporate spending"
- "Global corporate spending is $100T"
- "TAM = $100T × 5% = $5T"

**Why it's wrong:**
- You're conflating total procurement spend with software TAM
- 80% of companies don't use sophisticated software for procurement
- Your pricing only works for companies >$500M revenue (not SMB)
- You're ignoring competitive dynamics

**How to fix it:**
- Start with analyst reports on software market (Gartner, IDC), not total spend
- Filter by company size, software maturity, willingness to pay
- Acknowledge your actual addressable market is <10% of "everyone"

### Pitfall 2: "Our Market Share Assumption is Unrealistically High"

**What it looks like:**
- TAM: $1.5B
- Year 5 revenue target: $400M
- Market share assumption: 27%

**Why it's wrong:**
- Coupa has 35% market share and took 15 years to get there
- You're assuming you'll displace entrenched leaders
- Market share that high usually requires consolidation or acquisition

**How to fix it:**
- Reality-check against market leaders: "Our Year 5 target is 3% market share; market leader has 30%."
- Build SOM bottoms-up from sales capacity, not top-down from market share percentage
- If SOM doesn't work with 3-5% share, the market isn't right for you

### Pitfall 3: "Mixing TAM and SAM or SAM and SOM"

**What it looks like:**
- "Our TAM is the companies with 500+ employees that have procurement functions"
- "Our SAM includes all companies >$100M revenue plus mid-market"
- "Our SOM is everything except Fortune 100"

**Why it's wrong:**
- TAM/SAM/SOM are nested (TAM > SAM > SOM)
- If TAM only includes >$500M companies, SAM can't include >$100M companies (that's moving outward, not inward)

**How to fix it:**
- TAM = biggest theoretical market (everyone who needs your product)
- SAM = subset of TAM we can realistically serve (given product, GTM, competition)
- SOM = subset of SAM we'll realistically win (given execution, sales capacity, budget)
- Each step is more conservative, not broader

### Pitfall 4: "TAM Doesn't Account for Replacement vs. New Spend"

**What it looks like:**
- TAM = $1.5B total spend on software category
- Your messaging positions you as displacing legacy solutions
- You assume you get 25% of TAM
- Actually, you're not expanding spend; you're replacing existing spend
- If customers are already spending $1.5B, and you win 25%, you get $375M—but existing vendors still get $1.125B, and they're fighting hard for their share

**Why it's wrong:**
- You're not growing the pie; you're fighting for someone else's piece
- Replacement motion is slower and more competitive than expansion
- Your TAM should reflect realistic share of replacement spend, not total spend

**How to fix it:**
- Clarify: Is your TAM a "new market" or "replacement market"?
- If replacement: TAM = share of spend available from customer frustration with incumbent, not total spend
- If expansion: TAM = spend that didn't exist before (e.g., customers moving from manual Excel to software)
- Most enterprise software is replacement, not expansion; price accordingly

---

## Presenting TAM/SAM/SOM to Stakeholders

### What Investors Want to Hear

**Investor credibility check:**
1. Is your TAM credible? (Cite Gartner, not assumptions)
2. Is your TAM big enough? (>$500M for venture-scale)
3. Is your SAM reachable? (You have a credible GTM to reach it)
4. Is your SOM realistic? (Not "we'll own 50% of market"; more like "we'll be top-5 player")

**One-page format investors expect:**

---

**MARKET OPPORTUNITY**

**TAM (Total Addressable Market):** $1.4B ± 15%
- Source: Gartner Magic Quadrant (2024), filtered for U.S. companies >$500M revenue
- Validated bottom-up: 12,000 customers × $179K average ACV = $2.15B (reconciles to conservative $1.4B estimate)

**SAM (Serviceable Addressable Market):** $230M
- TAM filtered for: procurement pain severity, budget availability, competitive reachability
- Distribution: 41% enterprise (>$5B), 59% mid-market ($500M-$5B)

**SOM (Serviceable Obtainable Market):** $45M Year 5 revenue
- Based on: 20 enterprise reps × 2 deals/rep × $400K = $16M
- Plus: 15 mid-market reps × 4 deals/rep × $140K = $8.4M
- Plus: Expansion (20% of base) + partnerships = $20.6M

**Market Tailwinds:**
- Spend analytics market growing 12% CAGR
- Procurement function increasing sophistication (AI, automation, compliance)
- Incumbent vendors (Coupa, Ivalua) raising prices; customers seeking alternatives

---

### What Your CEO Wants to Hear

**CEO wants realistic revenue projections:**

"We can credibly build a $45M revenue company by Year 5, capturing 20% of our SAM. This is a $200M-$400M valuation exit (3-5x revenue multiple) for a Series A investor. We're not aiming for $500M revenue (market leader position); we're aiming for top-5 player and exit."

---

## References & Further Reading

- Christensen, Clayton M., Scott D. Anthony, and Erik A. Roth. *Seeing What's Next: Using Theories of Innovation to Predict Industry Change*. Harvard Business School Press, 2004.
- Smith, Michael D. and Rahul Telang. *Streaming, Sharing, Stealing: Big Data and the Future of Entertainment*. MIT Press, 2016. (Market sizing techniques)
- Bureau of Labor Statistics. *Occupational Employment and Wages* (oew.bls.gov) — Free source for function-based market sizing

---

**VERSION:** 1.0  
**LAST UPDATED:** 2026-04-12  
**SAQUIB JAWED ENTERPRISE PM PLAYBOOK**
