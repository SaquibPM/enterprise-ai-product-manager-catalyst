---
name: Decision Records and Architecture Decision Records (ADRs)
slug: decision-records
description: Document product and technical decisions with clarity, traceability, and future-proofing in enterprise contexts
version: 1.0
tags: [decision-making, documentation, enterprise, governance, compliance]
---

# Decision Records and Architecture Decision Records (ADRs)

## Purpose

Enterprise product decisions ripple through organizations for years. Yet most PMs make decisions without documentation, leading to:
- Lost context when team members leave (why did we choose PostgreSQL and not MongoDB?)
- Repeated debates on the same question (why did we pick this vendor last year?)
- Decisions that conflict with compliance or security posture (later audits find undocumented data processing decisions)
- Inability to assess "why did we do this wrong?" during post-mortems (no record of what was considered)
- Product decisions made in silos without visibility to engineering, sales, or legal

This skill teaches you to document decisions in a way that:
1. Creates searchable institutional memory (your future self will thank you)
2. Forces rigorous thinking (writing alternatives down reveals gaps)
3. Enables compliance audits (regulators care about documented decision rationale)
4. Prevents repeated decisions and bike-shedding (decision already made, here's why)
5. Supports retrospectives and post-mortems (what did we get wrong and why)

Architecture Decision Records (ADRs) were pioneered by Michael Nygard and are now standard in enterprises building compliant, scalable systems.

## Key Concepts

### 1. When to Write an ADR vs. When a Slack Message Suffices

**Write a Formal ADR When:**
- Decision will impact architecture for 6+ months
- Decision affects multiple teams or systems
- Decision has compliance/security/data handling implications
- Decision is reversible only at significant cost (switching databases, rewriting auth system)
- Decision will affect how future features are built
- Decision involves risk tradeoffs that should be visible to leadership

**Examples that warrant ADRs:**
- "Moving from PostgreSQL to MongoDB for vendor data"
- "Choosing between fine-tuned LLM vs. RAG for contract analysis"
- "Adopting multi-tenant architecture vs. single-tenant deployments"
- "Building custom risk scoring engine vs. buying third-party solution"
- "Storing customer data in our region vs. cloud provider's managed service"

**Slack Message or Wiki Suffices When:**
- Decision is reversible in hours/days (UI component choice)
- Decision only affects one team's sprint
- Decision has no compliance/security implications
- Decision is temporary or expires quickly
- Decision is about implementation details, not architecture

**Examples that only need Slack:**
- "Using React hooks instead of class components"
- "Renaming this button label"
- "Running a one-off data migration script"
- "Sprint priority reordering"
- "Which Sprint tool to use for standup"

### 2. The ADR Template (Nygard Format + Enterprise Additions)

**TITLE**
```
ADR [NUMBER]: [CONCISE DECISION TITLE]

Example:
ADR-047: Choosing Fine-Tuned LLM over RAG for Contract Clause Extraction
```

**STATUS**
```
One of: Proposed | Accepted | Deprecated | Superseded by [ADR-XXX]

Use Accepted only after a decision meeting with stakeholders.
Use Superseded if a later ADR reverses this decision (don't delete the old one).
```

**CONTEXT**
```
[3-5 paragraphs covering:]

1. What problem are we solving?
2. What's the business/technical driver?
3. What constraints are we working within?
4. What's the current state and why is it insufficient?
5. Who has input on this decision (engineering, security, compliance, sales)?

Example:
We need to extract specific contract clauses (payment terms, liability limits, 
renewal dates) from vendor agreements to populate our procurement platform's 
risk scoring engine. Currently, this is done manually by legal teams, which takes 
4-6 hours per contract and introduces human error.

We're processing 500 new vendor contracts per month and that number is growing 
to 2,000 by Q3. Manual extraction no longer scales.

Constraints:
- Must work with unstructured PDFs (contracts have varying formats)
- Accuracy target: 90%+ clause extraction rate (human review is fallback)
- Cost per document must be <$0.50 to remain profitable
- Must deploy within 8 weeks (competitive pressure from [Competitor X])
- GDPR/SOC2 compliance required (customer data involved)

Stakeholders: VP Product, Head of Procurement Operations, Chief Security Officer, 
2 customers piloting this feature
```

**DECISION**
```
[Concise statement of what you're deciding to do]

Example:
We will implement a fine-tuned LLM approach (using GPT-3.5-turbo with fine-tuning) 
to extract contract clauses, rather than a Retrieval-Augmented Generation (RAG) approach.

We will:
1. Fine-tune GPT-3.5-turbo on a dataset of 2,000 labeled contracts (150 labels per contract)
2. Implement a secondary validation layer using clause confidence scoring (reject <85% confidence)
3. Route high-confidence extractions directly to procurement platform, low-confidence 
   to manual review queue
4. Monitor extraction accuracy weekly and retrain quarterly on new contract types

We will NOT:
- Build a custom LLM (out of scope for 8-week timeline)
- Use GPT-4 at scale (cost prohibitive at $0.03 per 1K tokens for large contracts)
- Use RAG without fine-tuning (accuracy insufficient)
```

**ALTERNATIVES CONSIDERED**
```
[Each alternative with honest tradeoff assessment]

ALT 1: Retrieval-Augmented Generation (RAG)
- Store contract clause database, use LLM to retrieve matching clauses
- Pros: Faster to implement (4 weeks), lower fine-tuning data requirements
- Cons: Lower accuracy for clause extraction (estimated 75-80%), requires 
  maintaining external knowledge base, less adaptable to new contract types
- Cost: $0.30/doc (cheaper)
- Timeline: 4 weeks to MVP
- Winner: NO — accuracy miss is showstopper. Legal team needs 90%+ confidence.

ALT 2: Custom-Built Extraction Engine (Regex + NLP)
- Use open-source NLP libraries (spaCy) + regex rules to extract clauses
- Pros: Full control, open-source, lowest cost
- Cons: High maintenance burden, brittle (contracts vary widely), requires domain 
  experts, timeline not feasible (12+ weeks)
- Cost: $0.05/doc (lowest)
- Timeline: 12+ weeks
- Winner: NO — 12-week timeline defeats competitive urgency

ALT 3: Third-Party Vendor (LawGeex, Kira Systems, etc.)
- Buy existing contract extraction solution
- Pros: Highest accuracy (95%+), no build/maintenance, instant go-live
- Cons: Cost ~$2-5 per document (prohibitive), vendor lock-in, less customizable 
  for our specific clause types, integration complexity
- Cost: $2.50/doc (too high for volume)
- Timeline: 6-8 weeks (integration + deployment)
- Winner: NO — unit economics don't work at scale. Break-even at 50 contracts/month; 
  we need 500+/month.

ALT 4: Human + Hybrid (LLM + Manual Review)
- Start with 100% manual extraction, gradually introduce LLM with human validation
- Pros: Lower risk, high accuracy from day one
- Cons: Doesn't achieve scalability goal (still takes 4+ hours per contract)
- Cost: $8-10/doc (manual labor)
- Timeline: Immediate, but not a growth solution
- Winner: NO — we need scalability by Q3

WINNER: Fine-tuned LLM because it hits the accuracy target (90%+), cost target 
(<$0.50/doc), timeline target (8 weeks), and enables Q3 scale.
```

**CONSEQUENCES**
```
[Honest assessment of what this decision enables AND constrains]

POSITIVE CONSEQUENCES:
1. Scalability: Can process 500+ contracts/month with <$0.50/doc cost
2. Speed: Clause extraction reduces legal review time from 4-6 hours to 30 minutes
3. Accuracy: 92% extraction accuracy based on benchmark testing, + human review layer
4. Competitive: Feature ships 4 weeks before [Competitor X]
5. Revenue: Enables $180K ARR in contract automation features (pilot customers expanding)
6. Foundation: Fine-tuning approach enables future features (liability limit detection, 
   renewal date automation)

NEGATIVE CONSEQUENCES:
1. Ongoing Maintenance: Requires quarterly retraining as contract types evolve
2. Data Dependencies: Accuracy depends on quality of labeled training data 
   (requires legal team involvement every quarter)
3. Cost Creep: If contract volume exceeds 3,000/month, token costs may exceed 
   $0.50/doc threshold
4. Vendor Lock-in: Dependent on OpenAI API; if API changes, requires retraining
5. Cold Start Problem: First 2 weeks will show lower accuracy as model adapts 
   to production data
6. Compliance Complexity: Fine-tuned data goes to OpenAI; must ensure GDPR/SOC2 compliance 
   in data handling
7. Team Skill Gap: Requires ML/LLM expertise; most of our engineering team lacks this

MITIGATION FOR NEGATIVE CONSEQUENCES:
1. Maintenance: Schedule quarterly retraining; assign 1 FTE for model management
2. Data Dependencies: Create data labeling SLA with legal team (10 contracts/week)
3. Cost Creep: Build cost monitoring dashboard; escalate if cost/doc exceeds $0.50
4. Vendor Lock-in: Evaluate open-source alternatives (Llama 2) in Q4
5. Cold Start: Communicate to pilot customers that accuracy ramps up weeks 1-3
6. Compliance: Work with security/legal on data processing agreement with OpenAI; 
   implement data anonymization in training pipeline
7. Skill Gap: Bring in ML consultant for initial 4 weeks; hire 1 ML engineer starting Q3

FINANCIAL IMPACT:
- Development cost: $120K (8 weeks, 2 engineers + 1 ML consultant)
- Ongoing cost: $45K/year (quarterly retraining, model monitoring)
- Revenue impact: +$180K ARR (pilot customers expanding to production)
- ROI: Breakeven at month 4; cumulative profit by month 8
```

**COMPLIANCE & SECURITY REVIEW**
```
[Enterprise-specific: did you consider compliance/security/data handling?]

DATA HANDLING:
- Customer contract text and extracted clauses go to OpenAI API for fine-tuning
- Must ensure anonymization/pseudonymization of sensitive data
- Need Data Processing Agreement (DPA) with OpenAI
- GDPR implication: OpenAI is processor, we are controller; need lawful basis 
  (contract processing, legitimate interest)

SECURITY:
- API keys stored in encrypted Vault (not in code)
- API calls logged and monitored for unusual patterns
- Rate limiting enforced (prevent brute force of contract text)
- Model weights stored in secure S3 bucket with encryption

COMPLIANCE:
- SOC2 Type II auditor reviewed data flow; signed off
- GDPR: Data processing agreement in place with OpenAI
- HIPAA: N/A (not handling health data)
- PCI: N/A (not handling payment card data)
- Audit trail: All fine-tuning runs logged with timestamp, data, output

SECURITY SIGN-OFF: [CISO Name] approved [DATE]
COMPLIANCE SIGN-OFF: [Chief Compliance Officer Name] approved [DATE]
```

**DECISION AUTHORITY & APPROVAL**
```
Decision Owner: [Your Name], Product Manager
Approvers:
- [VP Engineering] - Technical feasibility
- [VP Product] - Strategic alignment
- [Chief Security Officer] - Compliance/security
- [Chief Procurement Officer] - Procurement implications

Approval Status:
- [VP Engineering]: APPROVED [DATE]
- [VP Product]: APPROVED [DATE]
- [Chief Security Officer]: APPROVED [DATE] with conditions: [security review, DPA]
- [Chief Procurement Officer]: APPROVED [DATE]

Decision made: [DATE]
Implementation starts: [DATE]
Review date: [DATE, 90 days out]
```

**IMPLEMENTATION PLAN**
```
Week 1-2: Data Preparation
- Collect 2,000 labeled contracts from pilot customers
- Anonymize sensitive data (customer names, amounts)
- Split into training (80%), validation (10%), test (10%)

Week 3-4: Fine-Tuning
- Upload training data to OpenAI fine-tuning API
- Run initial fine-tuning; evaluate on test set
- Iterate on prompt engineering and parameters

Week 5-6: Integration
- Build API wrapper for fine-tuned model
- Implement confidence scoring layer
- Build manual review queue for low-confidence extractions

Week 7: Testing
- Test on 500 real contracts from production
- Compare to human extraction (baseline accuracy)
- Load testing for 1,000 contracts/week throughput

Week 8: Deployment
- Deploy to staging; run with pilot customer traffic
- Monitoring and alerting for extraction accuracy
- Gradual rollout to 20% of production traffic
```

**REVIEW DATE & DECISION SUPERSESSION**
```
Review Date: [90 days from decision date, e.g., October 15, 2026]

At review, assess:
1. Is extraction accuracy meeting 90% target? (current: 92%)
2. Is cost per document staying below $0.50? (current: $0.43)
3. Are quarterly retraining cycles sustainable? (yes, 1 FTE sufficient)
4. Has competitive landscape changed? (Competitor X launched similar, still 4 weeks behind)
5. Should we build custom fine-tuned LLM vs. continue using OpenAI? (not yet)
6. Are there open-source alternatives we should evaluate? (Llama 2 promising for Q4)

Decision Status after review: CONTINUE AS-IS, with monitoring on cost/accuracy

If superseded by later decision, link here:
- [If ADR-048 reverses this decision, note it]
```

### 3. Decision Log Management: Making Decisions Searchable

Enterprise organizations accumulate dozens of ADRs. Without proper indexing, you can't find past decisions.

**Decision Log Index (maintained in shared wiki/Confluence):**

```
# Product & Architecture Decisions Index

## By Category

### DATA ARCHITECTURE
- ADR-047: Fine-tuned LLM vs. RAG for contract extraction [Status: ACCEPTED]
- ADR-042: PostgreSQL JSONB vs. MongoDB for vendor data [Status: ACCEPTED]
- ADR-035: Single-tenant vs. multi-tenant architecture [Status: ACCEPTED]

### VENDOR DECISIONS
- ADR-051: Datadog vs. New Relic for monitoring [Status: ACCEPTED]
- ADR-048: AWS vs. GCP for cloud infrastructure [Status: ACCEPTED]
- ADR-044: Auth0 vs. Okta for identity management [Status: ACCEPTED]

### PRODUCT STRATEGY
- ADR-052: Platform marketplace vs. direct integrations [Status: ACCEPTED]
- ADR-046: Procurement automation vs. contract analytics focus [Status: ACCEPTED]
- ADR-041: Freemium vs. enterprise-only pricing [Status: SUPERSEDED by ADR-049]

### COMPLIANCE & SECURITY
- ADR-050: End-to-end encryption for customer data [Status: ACCEPTED]
- ADR-043: GDPR data residency in EU vs. US [Status: ACCEPTED]
- ADR-038: SOC2 Type II vs. Type I compliance [Status: ACCEPTED]

## By Date
[Reverse chronological list]

## By Status
- ACCEPTED: [list of active decisions]
- DEPRECATED: [list of old decisions]
- SUPERSEDED: [list of reversed decisions with link to successor]
```

**Searchability Best Practices:**
1. **Tag every ADR** with category (data, vendor, product, compliance)
2. **Use consistent naming** (date + short title)
3. **Link related ADRs** (if ADR-047 depends on ADR-042, mention it)
4. **Version the decision log** (git history shows when decisions were made)
5. **Require tags** (can't submit ADR without decision category, approval status)

### 4. ADR Format Decision Tree: How to Choose Detail Level

```
Is this decision:

A) Affects multiple teams AND has compliance implications?
   → FULL ADR (12-15 pages, all sections, full approval)
   → Examples: vendor selection, data architecture, platform strategy

B) Affects one team, reversible in weeks?
   → LIGHTWEIGHT ADR (4-6 pages, Context + Decision + Alternatives only)
   → Examples: implementation approach for known feature, tool selection

C) Reversible in hours, no compliance impact?
   → Slack message + Confluence wiki update
   → No ADR needed
```

## Application: Complete Worked Example

**SCENARIO:** You need to decide whether to build a fine-tuned LLM model for contract clause extraction, buy a vendor solution, or use RAG. You have 8 weeks to ship. Here's the full ADR:

```
ADR-047: Fine-Tuned LLM vs. RAG vs. Vendor Solution for Contract Clause Extraction

STATUS
Accepted (approved by VP Engineering, VP Product, CISO on September 15, 2026)

CONTEXT
Enterprise procurement platform needs to automatically extract specific contract 
clauses (payment terms, renewal dates, liability limits, termination clauses) from 
vendor agreements to power risk scoring engine.

Current state: Legal teams manually review each contract (4-6 hours per document). 
This created:
- Bottleneck: Can only process 50-100 contracts/month manually
- Error rate: ~15% of extracted clauses wrong or incomplete
- Cost: ~$8-10 per contract in labor

Market driver: Processing 500 new contracts/month; growing to 2,000/month by Q3. 
Manual extraction is incompatible with this volume.

Competitive pressure: Competitor X launched contract automation feature 6 months ago 
(lower accuracy, 60% extraction rate). We need parity + differentiation in 8 weeks.

Business impact:
- Current pilot customers: [Customer A] wants this feature (contract expansion +$180K ARR)
- Sales pipeline: 5 deals blocked on contract automation capability ($450K pipeline)
- Procurement efficiency: Each customer saves $50K+ annually on legal review costs

Constraints:
1. Timeline: 8 weeks to MVP (competitive window)
2. Cost: <$0.50 per document (contracts will be processed at scale)
3. Accuracy: 90%+ clause extraction (humans review remaining 10%)
4. Compliance: GDPR, SOC2 Type II, data residency requirements
5. Dependency: Our team has 2 backend engineers, no ML experience

Stakeholders providing input:
- VP Product (strategic priority, revenue impact)
- VP Engineering (technical feasibility)
- Chief Procurement Officer (operational feasibility)
- 2 pilot customers (use case definition, accuracy requirements)
- Chief Security Officer (data handling, compliance)

DECISION
We will implement a FINE-TUNED LLM approach (GPT-3.5-turbo with OpenAI fine-tuning 
API) rather than RAG or purchasing a third-party solution.

Specifically:
1. Fine-tune GPT-3.5-turbo on labeled dataset of 2,000 sample contracts
2. Implement confidence scoring (extract only clauses >85% confidence)
3. Route low-confidence extractions to manual review queue
4. Deploy to pilot customers in 8 weeks, production at week 10

Why this approach (vs. doing nothing / manual escalation):
- Achieves 90%+ accuracy (enough for pilot customers to rely on)
- Costs <$0.50 per document (economically viable at 500+ contracts/month)
- Ships in 8 weeks (addresses competitive window)
- Enables future features (liability limit automation, renewal date extraction, etc.)

ALTERNATIVES CONSIDERED

ALTERNATIVE 1: RETRIEVAL-AUGMENTED GENERATION (RAG)
Description: Build a vector database of contract clauses, use LLM to retrieve matching 
clauses for each contract

Advantages:
- Faster implementation (4-5 weeks vs. 8)
- Lower fine-tuning data requirements (500 examples vs. 2,000)
- Easier to update (add new clauses to vector DB)
- Can use open-source embeddings (avoid vendor lock-in)

Disadvantages:
- Lower accuracy for extraction task (estimated 75-80%, based on benchmarks)
- Requires maintaining external knowledge base (ongoing overhead)
- Less adaptable to new contract types / edge cases
- Knowledge base quality directly impacts accuracy

Cost analysis:
- Development: $80K (faster implementation)
- Ongoing: $20K/year (knowledge base maintenance)
- Per-document cost: $0.30

Timeline: 4-5 weeks to MVP

Why not chosen: Accuracy miss is showstopper. 75-80% extraction means ~20 false 
positives per 100 clauses. At 2,000 contracts/month, that's 40,000 errors/month. 
Legal team would spend more time fixing false positives than doing manual extraction. 
Pilot customer explicitly said "accuracy must be 90%+." This approach fails their requirement.

---

ALTERNATIVE 2: CUSTOM EXTRACTION ENGINE (Regex + NLP)
Description: Build custom extraction engine using open-source NLP (spaCy) + regex 
rules trained on our contract dataset

Advantages:
- Full control over implementation
- Open-source, no vendor lock-in
- Lowest cost per document ($0.05, infrastructure only)
- Can integrate domain-specific rules (our clauses, our contracts)

Disadvantages:
- High development complexity (NLP requires ML expertise; our team lacks this)
- Brittle (contracts vary widely; regex doesn't generalize)
- High maintenance burden (each new contract type needs new rules)
- Requires ML hiring or external consultant
- Timeline not feasible (12-16 weeks with current team)

Cost analysis:
- Development: $150K (4 months of senior ML engineer + consultant)
- Ongoing: $80K/year (ML engineer salary + infrastructure)
- Per-document cost: $0.08

Timeline: 12-16 weeks

Why not chosen: 12-16 week timeline defeats competitive urgency (Competitor X ships 
in 6 weeks). By the time we ship, they'll have 6 months of market advantage + customer 
feedback. Also, it requires hiring ML expertise we don't have, which delays timeline 
further.

---

ALTERNATIVE 3: BUY THIRD-PARTY VENDOR (LawGeex, Kira Systems, Contract Intelligence)
Description: License existing contract intelligence vendor; integrate into our platform

Advantages:
- Highest accuracy (95%+ extraction across diverse contract types)
- No build/maintenance required (vendor maintains)
- Instant go-live (6-8 weeks for integration)
- Industry-proven (thousands of enterprises use)
- Handles diverse contract types (not just our specific use cases)

Disadvantages:
- Cost prohibitive: $2-5 per document
- At 500 contracts/month: $1,000-2,500/month = $12K-30K/year
- At 2,000 contracts/month (Q3 target): $4K-10K/month
- Vendor lock-in (switching costs are high)
- Limited customization (can't tailor extraction to our specific clauses)
- Integration complexity (API integration, security review, data handling agreements)

Cost analysis:
- Vendor licensing: $3/document (2,000 contracts/month = $6,000/month = $72K/year)
- Integration: $50K (one-time)
- Ongoing support: $20K/year
- Total year 1: $142K; year 2+: $92K/year

Timeline: 6-8 weeks (plus security review 2-4 weeks)

Why not chosen: Unit economics don't work. Our customers are mid-market procurement 
teams with limited budgets. If contract automation costs $3-5 per document, our 
customers pay $1,500-2,500 per month just to use the feature. This price point limits 
TAM to only the largest enterprises. Fine-tuned LLM at $0.50/doc lets us offer the 
feature at $20-50/month, which is 30x cheaper and opens up mid-market.

Also, we lose competitive differentiation. All customers of LawGeex get the same 
extraction quality. Fine-tuning lets us optimize for OUR contract types, giving us 
a unique advantage.

---

ALTERNATIVE 4: HYBRID (Start Manual, Gradually Introduce LLM)
Description: Start with 100% manual extraction (pay legal contractor) for first 
3 months, then gradually introduce LLM as we build fine-tuned model

Advantages:
- Lowest risk (no AI models, no vendor issues)
- Highest accuracy from day one (100%, human extraction)
- Gives time to collect training data (manual extraction creates labeled dataset)
- Can start pilot customer rollout immediately

Disadvantages:
- Doesn't solve scalability problem (still labor-intensive)
- Costs $8-10 per document (not economically viable at 2,000/month)
- Delays automation feature by 3+ months (competitive disadvantage)
- "Hybrid" is actually just "manual with plans to automate later"

Cost analysis:
- Contractor labor: $10/document x 500/month = $5,000/month = $60K/year
- Plus infrastructure/tools: $10K/year
- Total: $70K/year

Why not chosen: Doesn't achieve our goal. We're trying to AUTOMATE contract review. 
This alternative keeps it manual. It's what we're already doing; we're trying to 
move past it.

---

DECISION RATIONALE

Fine-tuned LLM wins because it:
1. Hits accuracy target (90%+ vs. RAG's 75-80%)
2. Hits cost target (<$0.50/doc vs. vendor's $3/doc)
3. Hits timeline target (8 weeks vs. custom's 12+)
4. Enables competitive differentiation (customized for our clause types)
5. Unlocks revenue ($180K expansion + $450K pipeline)
6. Creates foundation for future features (liability extraction, renewal automation)

Scoring summary:
┌─────────────────────────┬──────────┬──────────┬──────────┬──────────┐
│ Criteria                │ Fine-tune│   RAG    │  Custom  │  Vendor  │
├─────────────────────────┼──────────┼──────────┼──────────┼──────────┤
│ Accuracy (target 90%)   │   ✓ 92% │  ✗ 78%  │  ✓ 95%  │  ✓ 96%  │
│ Cost/doc (target <$0.50)│   ✓      │  ✓      │  ✓      │  ✗ $3   │
│ Timeline (target 8wks)  │   ✓ 8wks│  ✓ 4wks │  ✗ 12wks│  ✓ 8wks │
│ Competitive window      │   ✓ NOW │  ✓ NOW │  ✗ 12wks│  ✓ 8wks │
│ Customization ability   │   ✓ Hi  │  ~ Med  │  ✓ Hi  │  ✗ Low  │
│ Vendor lock-in risk     │   ~ Med │  ✓ Low │  ✓ None │  ✗ Hi  │
│ Team skill requirements │   ~ Med │  ✓ Low │  ✗ Hi  │  ✓ Low  │
├─────────────────────────┼──────────┼──────────┼──────────┼──────────┤
│ WINNER                  │   YES    │   NO     │   NO     │   NO    │
└─────────────────────────┴──────────┴──────────┴──────────┴──────────┘

CONSEQUENCES

POSITIVE CONSEQUENCES

1. Business Impact / Revenue
   - Enables [Customer A] contract expansion: +$180K ARR (already signed LOI)
   - Unblocks 5 sales deals in pipeline: +$450K potential ARR
   - Pilot customers report 40% reduction in legal review time
   - Feature differentiates us from Competitor X (their RAG accuracy is 60%)

2. Product / Customer Experience
   - Legal teams go from 4-6 hour contract review to 30 min (80% time savings)
   - Reduced error rate from 15% to 10% (improvement from 92% AI accuracy)
   - Scales to 2,000 contracts/month by Q3 without adding headcount
   - Foundation for future features: liability limit detection, renewal automation

3. Engineering / Technical
   - Leverages proven GPT-3.5-turbo fine-tuning APIs (no custom ML expertise needed)
   - Enables future AI features (we build institutional knowledge on LLM integration)
   - Reduces engineering toil (no custom NLP rules to maintain)

4. Cost / Operations
   - $0.43 per document (within $0.50 budget)
   - Profitable at 500 contracts/month (current volume)
   - Scales cost-efficiently to 2,000+/month

NEGATIVE CONSEQUENCES & MITIGATION

1. Ongoing Model Maintenance (MEDIUM IMPACT)
   - New contract types emerge; model accuracy may degrade
   - Fine-tuning requires quarterly retraining (labor-intensive)
   
   Mitigation:
   - Assign 1 FTE for model management (ML engineer, starting Q4)
   - Create SLA with legal team (provide 10 new labeled contracts/week)
   - Build monitoring dashboard for extraction accuracy drift
   - Escalate to PM if accuracy drops below 88% (red flag)

2. Data Dependencies & Privacy (MEDIUM IMPACT)
   - Fine-tuning requires 2,000 labeled contracts (must come from customers/legal team)
   - Quality of training data directly impacts model accuracy
   - Customer data goes to OpenAI API (external company, must ensure compliance)
   
   Mitigation:
   - Anonymize customer names/amounts before sending to OpenAI
   - Work with Chief Security Officer on Data Processing Agreement (DPA) with OpenAI
   - Implement automatic data deletion (OpenAI deletes data after fine-tuning complete)
   - Separate sensitive data from training data; use synthetic examples

3. Cost Creep / Scaling Issues (LOW-MEDIUM IMPACT)
   - If we exceed 3,000 contracts/month, token costs may exceed $0.50/doc
   - GPT-4 would be more accurate but cost $0.03/token (prohibitive)
   
   Mitigation:
   - Build cost monitoring dashboard (alert if cost/doc exceeds $0.50)
   - Evaluate open-source alternatives (Llama 2) in Q4 2026
   - If costs spike, trigger architecture review and ADR evaluation
   - Consider caching frequent clause types to reduce token usage

4. Vendor Lock-in Risk (MEDIUM IMPACT)
   - Fully dependent on OpenAI's fine-tuning APIs
   - If OpenAI changes API, pricing, or deprecates fine-tuning, we're affected
   - No easy way to switch to competitor (would need to retrain entirely different model)
   
   Mitigation:
   - Evaluate open-source alternatives quarterly (Llama 2, Mistral)
   - Keep extraction logic separate from LLM (abstraction layer for easy swapping)
   - Avoid long-term contracts with OpenAI; use pay-as-you-go
   - Build proof-of-concept with open-source LLM in Q4 2026 (insurance policy)

5. Cold Start / Ramp-up Period (LOW IMPACT)
   - First 2 weeks of production will show lower accuracy as model adapts to real data
   - Customers may see extraction errors initially
   
   Mitigation:
   - Set expectations: "Accuracy ramps from 88% to 92% in weeks 1-3"
   - Monitor accuracy daily in first 2 weeks; retrain if trend is negative
   - Route all extractions to manual review in week 1 (not production yet)
   - Offer incentive to pilot customers: "3-month free trial in exchange for feedback"

6. Team Skill Gap (MEDIUM IMPACT)
   - Current backend engineering team has no LLM fine-tuning experience
   - Initial implementation may be slower than estimated
   - Ongoing maintenance requires ML expertise
   
   Mitigation:
   - Hire 1 ML consultant for weeks 1-4 (guidance on fine-tuning setup, debugging)
   - Plan to hire 1 junior ML engineer starting Q4 2026
   - Create internal documentation / runbook for fine-tuning process
   - Build monitoring/alerting to catch issues before they become problems

7. Compliance & Data Handling (MEDIUM-HIGH IMPACT)
   - Customer contract data + extracted clauses go to OpenAI (3rd party)
   - Must comply with GDPR (right to deletion), SOC2, and customer contracts
   - Legal review required before rollout
   
   Mitigation:
   - Implement data anonymization: Remove customer names, amounts, dates
   - Get signed Data Processing Agreement (DPA) with OpenAI before any data transfer
   - Document data flow: where data goes, how long it's retained, who can access it
   - Legal review before launch: Have Chief Compliance Officer sign off
   - Audit trail: Log all API calls and training runs for compliance audits
   - Customer communication: Disclose that extractions are done by third-party LLM
   - Data deletion: OpenAI deletes fine-tuning data 30 days post-training

SUMMARY OF IMPACT

Financial Impact:
- Development cost: $120K (2 engineers + 1 consultant, 8 weeks)
- Ongoing cost: ~$45K/year (model management + infrastructure)
- Revenue impact: +$180K ARR from pilot expansion, +$450K pipeline unblocked
- ROI: Breakeven at month 4; cumulative profit $450K+ by end of year

Timeline Impact:
- Implementation: 8 weeks (competitive window timing)
- Future: Foundation for 4+ follow-on features (liability extraction, renewal dates, etc.)

Risk Impact:
- Operational risk: Medium (model maintenance, data handling)
- Financial risk: Low (cost is justified by revenue)
- Compliance risk: Medium (need strict data governance, legal review required)
- Competitive risk: Low (we ship before Competitor X can iterate)

COMPLIANCE & SECURITY REVIEW

Data Processing:
- Customer data source: Contracts provided by customers + legal team (first-party)
- Data in transit: Encrypted HTTPS to OpenAI API
- Data at rest: OpenAI's servers (we don't store fine-tuning data ourselves)
- Data retention: OpenAI deletes training data 30 days after fine-tuning completion
- Data purpose: Fine-tuning LLM to extract clauses (limited, specific use)

Compliance Requirements Met:
- GDPR: Data Processing Agreement signed with OpenAI; right to deletion implemented
- SOC2 Type II: Data encrypted in transit; API access logged; audit trail maintained
- ISO 27001: Approved by Chief Security Officer (conditions: DPA + monitoring)
- Customer Contracts: Reviewed with Legal; no restrictions on using 3rd-party LLM

Data Minimization:
- Customer names: Anonymized (e.g., "Customer A" instead of "Acme Corp")
- Contract amounts: Removed / randomized
- Dates: Removed where not essential to clause extraction
- Sensitive data: Masked SSNs, banking info, etc.

Security Controls:
- API keys: Stored in encrypted Vault (AWS Secrets Manager); rotated quarterly
- API calls: Logged with customer ID, contract ID, extraction results, timestamps
- Monitoring: CloudWatch alarms for unusual API patterns (rate spike = potential breach)
- Rate limiting: Max 100 contracts/hour per API key (prevent abuse)
- Error handling: Extraction failures logged for debugging; never exposed to customer

Post-Launch Security Audits:
- Monthly: Review API logs for suspicious activity
- Quarterly: Retest data anonymization process (confirm PII is removed)
- Annually: Full security assessment by external firm

Sign-offs:
- Chief Security Officer: APPROVED (Sept 14, 2026)
  Conditions: (1) DPA with OpenAI, (2) Monthly security audits, (3) Data anonymization verification
- Chief Compliance Officer: APPROVED (Sept 15, 2026)
  Conditions: (1) GDPR compliance review, (2) Customer disclosure in terms, (3) Data deletion processes

DECISION AUTHORITY & APPROVAL

Decision Owner: [Your Name], Senior Product Manager, Procurement Platform

Required Approvers:
1. VP Engineering — Technical feasibility and resource allocation
2. VP Product — Strategic alignment with product roadmap
3. Chief Security Officer — Security and compliance
4. Chief Procurement Officer — Operational impact

Approval Status:
- VP Engineering ([Jane Smith]): APPROVED, Sept 14, 2026
  Notes: "Feasible with external ML consultant. Recommend hiring ML engineer for ongoing support."
  
- VP Product ([John Chen]): APPROVED, Sept 14, 2026
  Notes: "Aligns with Q3 roadmap. Unlocks $450K pipeline."
  
- Chief Security Officer ([Priya Patel]): APPROVED with conditions, Sept 14, 2026
  Conditions: 
  - DPA with OpenAI executed before any data transfer
  - Monthly security audit of API logs
  - Data anonymization verified before deployment
  
- Chief Compliance Officer ([Marcus Johnson]): APPROVED, Sept 15, 2026
  Notes: "GDPR compliant with safeguards. Customer terms updated to disclose 3rd-party processing."

IMPLEMENTATION PLAN

PHASE 1: SETUP (Week 1-2)
- Collect 2,000 labeled contracts from pilot customers
- Anonymize sensitive data (customer names, amounts, dates)
- Split dataset: 80% training (1,600), 10% validation (200), 10% test (200)
- Set up OpenAI API account; sign Data Processing Agreement (DPA)
- Hire ML consultant (4-week engagement)

PHASE 2: FINE-TUNING (Week 3-4)
- Upload training data to OpenAI fine-tuning API
- Run initial fine-tuning with default parameters
- Evaluate accuracy on validation set; compare to GPT-3.5-turbo baseline
- Iterate on prompt engineering and model parameters
- Target: 90%+ accuracy on test set before production

PHASE 3: INTEGRATION (Week 5-6)
- Build API wrapper layer (abstract OpenAI calls from application logic)
- Implement confidence scoring (extract only >85% confidence clauses)
- Build manual review queue (low-confidence extractions go here)
- Implement logging / monitoring (accuracy tracking, cost per document, error rates)
- Write integration tests (mock LLM responses; test extraction pipeline)

PHASE 4: TESTING & VALIDATION (Week 7)
- Load testing: 1,000 contracts/week throughput validation
- Edge case testing: Unusual contract formats, malformed PDFs
- Comparison testing: 500 real contracts; compare AI extraction vs. human extraction
- Accuracy measurement: Calculate precision, recall, F1 score for each clause type
- Cost validation: Verify per-document cost stays <$0.50
- Security testing: Verify no PII leakage; confirm data anonymization works

PHASE 5: PILOT ROLLOUT (Week 8)
- Deploy to staging environment; run with 10% of pilot customer traffic
- Monitor: Accuracy, cost, error rates, extraction latency
- Gather feedback from pilot customers (legal teams)
- Fix bugs / edge cases discovered
- Prepare for production launch

PHASE 6: PRODUCTION LAUNCH (Week 9-10)
- Gradual rollout: 20% → 50% → 100% of traffic
- Monitor accuracy daily (watch for degradation)
- Manual review queue for all extractions week 1; adjust post-validation
- Customer communication: "Feature now live for pilot customers"
- Sales enablement: Create customer-facing documentation, demo, training

PHASE 7: STABILIZATION (Week 11+)
- Ongoing monitoring: Accuracy, cost, error rates
- Quarterly retraining: Add new contract types, improve accuracy
- Customer success: Help customers maximize value of the feature
- Future: Plan Phase 2 features (liability extraction, renewal automation)

METRICS & SUCCESS CRITERIA

By End of Week 8 (Pilot Launch):
- Extraction accuracy: 92%+ on test set (target: 90%)
- Cost per document: <$0.50 (target met)
- Latency: <5 seconds per contract (acceptable for async processing)
- Uptime: 99.5%+ (no extended API outages)
- Data flow: Zero PII leakage in logs or monitoring (security validation)

By End of Week 12 (Production Launch):
- Customer adoption: 90%+ of pilot customers using the feature
- Customer satisfaction: NPS >8.0 for feature (legal teams rating it)
- Accuracy on production data: 92%+ (matching test set performance)
- Error rate: <5% of extractions require manual correction
- Cost: Staying <$0.50 per document at 500/month volume

By End of Q4 2026 (3-month review):
- Scale: Processing 1,000+ contracts/month
- Expansion revenue: [Customer A] contract expansion closed (+$180K ARR)
- Pipeline: 3+ deals unblocked; pipeline moving to proposal stage (+$300K+)
- Model performance: Accuracy remains >90% as new contract types encountered
- Cost: Costs increasing as volume grows; still <$0.50 per document

REVIEW DATE & SUPERSESSION

Review Date: December 15, 2026 (90 days post-decision)

At review, we'll assess:
1. Is extraction accuracy meeting 92% target? (current assumption: yes)
2. Are per-document costs staying <$0.50? (current assumption: yes at 500-1000/month)
3. Are quarterly retraining cycles sustainable? (feasible with 1 FTE)
4. Has competitive landscape changed? (Competitor X shipped; we're 4 weeks ahead)
5. Should we invest in open-source alternative? (evaluate Llama 2, Mistral cost/accuracy)
6. Are there follow-on features to prioritize? (liability extraction, renewal automation)

Likely decision after review: CONTINUE AS-IS with:
- Q4 investment in ML engineer hiring
- Q1 2027 evaluation of open-source LLM alternatives (risk mitigation for lock-in)
- Q1 2027 planning for Phase 2 features (liability, renewal dates)

Supersession:
If future ADR reverses this decision:
- ADR-055 (if we switch to Llama 2 for cost reduction)
- ADR-060 (if we move to custom NLP engine for full control)
- Will be linked here upon completion

Document Version: 1.0 (Sept 15, 2026)
Last Updated: [Date]
Next Review: Dec 15, 2026
```

## Common Pitfalls and How to Avoid Them

### Pitfall 1: ADRs Without Real Alternatives
**What goes wrong:** You write an ADR where you "consider" 3 alternatives but spend 2 sentences on each. Readers realize you already decided; alternatives are theater.

**Why it happens:** You want to move fast. You know what you want to do. Documenting alternatives feels slow.

**How to fix it:** Spend 20% of your ADR writing time on alternatives. Real consideration means honest tradeoffs for each option.

**Right approach:** Each alternative gets:
- Clear explanation of how it works
- 3-5 honest advantages
- 3-5 honest disadvantages  
- Cost + timeline + accuracy (numerical tradeoffs)
- Scoring vs. decision criteria

### Pitfall 2: Undocumented Decisions (The Biggest Risk)
**What goes wrong:** You make an architecture decision in a standup. 6 months later, a new engineer asks "why did we do this?" and no one remembers.

**Why it happens:** "We'll document it later." Later never comes.

**How to fix it:** Write the ADR BEFORE you start implementation, not after. Decision → Documentation → Implementation.

**Right approach:** Block 2 hours to write the ADR before engineering starts work. It forces clarity. You catch gaps early.

### Pitfall 3: ADRs That Become Outdated
**What goes wrong:** You write an ADR in January. By September, the assumption breaks (competitive landscape changed, volume forecast was wrong). The ADR is now misleading historical artifact.

**Why it happens:** You don't review decisions; you just move on.

**How to fix it:** Schedule review dates. At review, update the ADR with current status or mark it Superseded.

**Right approach:**
```
Review Date: [90 days from decision]

At review:
- Is this decision still valid?
- Should we reverse it? (if yes, create new ADR)
- What changed since we decided?
- Should we update assumptions based on what we learned?
```

### Pitfall 4: No Compliance/Security Section
**What goes wrong:** You decide to store customer data in the cloud. 6 months later, auditors ask "why did you choose this vendor?" You don't have documentation showing you evaluated SOC2, GDPR compliance.

**Why it happens:** PM doesn't think about compliance. Compliance gets bolted on later.

**How to fix it:** Add Compliance & Security Review section to EVERY ADR that touches data, integrations, or third-party vendors.

**Right approach:**
```
COMPLIANCE & SECURITY REVIEW

Data handling: Where does data go? Who can access it? How long is it retained?
Compliance: Which regulations apply? Have we met them?
Security: What's the attack surface? How are we mitigating?
Sign-offs: Who (CISO, Chief Compliance Officer) approved this?
```

### Pitfall 5: Decision Authority Unclear
**What goes wrong:** You write an ADR. It's not clear who actually has the authority to say yes/no. Six stakeholders read it. All four assume it's someone else's decision.

**Why it happens:** You avoid naming the decision owner (feels political).

**How to fix it:** Be explicit. Name the decision owner AND the approvers. One person decides, but others must sign off.

**Right approach:**
```
Decision Owner: [Name, Title]
Required Approvers: [Name, Title], [Name, Title], [Name, Title]

Approval Status:
- [Name]: APPROVED [Date] — [Notes]
- [Name]: APPROVED [Date] — [Notes]
- [Name]: PENDING — [Reason]
```

## References and Further Reading

1. **Documenting Architecture Decisions** (Michael Nygard)
   https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
   The original ADR concept; foundational.

2. **ADR GitHub Template** (adr/adr)
   https://github.com/adr/adr
   Community-maintained ADR templates and examples.

3. **Architecture Decision Records** (ThoughtWorks Technology Radar)
   Highlights ADRs as industry best practice for architectural decisions.

4. **Decisive: How to Make Better Choices in Life and Work** (Chip Heath, Dan Heath)
   Framework for decision-making; useful context for documenting why you chose what you did.

5. **The Effective Executive** (Peter Drucker)
   Chapter on decision-making; emphasizes importance of documenting assumptions.

6. **Compliance in SaaS** (Y Combinator Startup School)
   How to think about GDPR, SOC2, compliance in product decisions.

7. **Enterprise Architecture Best Practices** (Gartner)
   Enterprise governance framework; highlights decision record importance for large orgs.
