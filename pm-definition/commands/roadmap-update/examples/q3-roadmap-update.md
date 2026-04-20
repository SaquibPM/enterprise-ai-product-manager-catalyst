# Q3 2026 Product Roadmap Update

**Reporting Date**: April 2026
**Effective Period**: July - September 2026
**Document Owner**: VP Product
**Status**: Executive Approved

## Executive Summary (For Board & Leadership)

Q3 2026 roadmap focuses on three strategic pillars: (1) AI-powered compliance automation (drives $1.2B TAM opportunity), (2) enterprise integrations (removes deal blockers), and (3) platform reliability (ensures SOX/HIPAA compliance).

**Key deliverables**: AI contract scoring, Ariba/NetSuite integrations, 99.99% uptime SLA
**Investment**: 4 engineering squads, $800k engineering budget
**Expected revenue impact**: $4.5M ARR acceleration (contracts currently blocked on compliance)
**Risk**: AI model accuracy (mitigated by legal team feedback loop)

---

## ROADMAP BY THEME

### Theme 1: AI-Powered Compliance (45% Q3 Engineering Capacity)

#### Feature 1.1: AI Contract Risk Scoring (Priority 1 - MUST HAVE)
- **Status**: In Design (kickoff Week 1 July)
- **Target**: Beta Aug 1, GA Aug 31
- **Audience value**: 
  - **Exec**: $800M TAM, differentiator vs. Coupa, enables Finance/Healthcare vertical expansion
  - **Engineering**: Transform-based ML model, 3 person-weeks, new ML infrastructure
  - **Customer**: Review contracts 4x faster, catch compliance risks automatically
- **RICE Score**: Reach 8/10 | Impact 9/10 | Confidence 8/10 | Effort 7/10 = 10.3
- **Effort**: Large (8 weeks)
- **Dependencies**: ML infrastructure (complete Q2), legal team feedback labels (Q2 collect)
- **Key metrics**: 90%+ accuracy on High-Risk category, 2 min analysis time

#### Feature 1.2: Healthcare Compliance Rule Pack (Priority 1 - MUST HAVE)
- **Status**: Planned
- **Target**: GA Sept 15
- **Audience value**:
  - **Exec**: Healthcare is $200M sub-market, high ASP, high churn risk (compliance is make-or-break)
  - **Engineering**: Rules engine configuration, 1 person-week, low-code
  - **Customer**: Automatically flag HIPAA violations, FDA compliance issues
- **RICE Score**: Reach 6/10 | Impact 9/10 | Confidence 9/10 | Effort 2/10 = 16.2
- **Effort**: Medium (2 weeks)
- **Dependencies**: AI Contract Scoring (Feature 1.1)
- **Key metrics**: 8 healthcare logos using rule pack by Sept 30

#### Feature 1.3: Government Contracts Compliance (Priority 2 - SHOULD HAVE)
- **Status**: Planned
- **Target**: GA October 1 (moved to Q4)
- **Audience value**:
  - **Exec**: Government contracting is $150M sub-market, Defense/Aerospace have highest compliance burden
  - **Engineering**: Rules configuration similar to healthcare, 1.5 person-weeks
  - **Customer**: Flag FAR/DFARS violations automatically
- **RICE Score**: Reach 5/10 | Impact 8/10 | Confidence 8/10 | Effort 2/10 = 13.3
- **Effort**: Medium (1.5 weeks)
- **Dependencies**: AI Contract Scoring (Feature 1.1), Healthcare Rule Pack testing
- **Note**: Moved to Q4 due to capacity constraints; deprioritized vs. enterprise integrations

---

### Theme 2: Enterprise Integrations (35% Q3 Engineering Capacity)

#### Feature 2.1: SAP Ariba Connector (Priority 1 - MUST HAVE)
- **Status**: In Design
- **Target**: Beta July 15, GA Aug 15
- **Audience value**:
  - **Exec**: Removes blocker for 3 pending deals ($450M ARR potential), Ariba is strategic partner opportunity
  - **Engineering**: REST API connector, OAuth, 4 person-weeks, moderate complexity
  - **Customer**: Sync supplier master data from Ariba in real-time, no manual exports
- **RICE Score**: Reach 7/10 | Impact 8/10 | Confidence 9/10 | Effort 6/10 = 10.5
- **Effort**: Large (4 weeks)
- **Dependencies**: API Spec complete (April), OAuth integration (Q2)
- **Key metrics**: 95%+ first-sync success rate, <5 min sync time

#### Feature 2.2: Oracle NetSuite Connector (Priority 1 - MUST HAVE)
- **Status**: Planned (parallel track with Ariba)
- **Target**: Beta Aug 1, GA Sept 1
- **Audience value**:
  - **Exec**: Mid-market dominant (40% use NetSuite), high ASP, quick to deploy
  - **Engineering**: Similar to Ariba (REST API), 3.5 person-weeks, slightly simpler auth
  - **Customer**: Sync supplier + spend data from NetSuite
- **RICE Score**: Reach 8/10 | Impact 7/10 | Confidence 9/10 | Effort 5/10 = 11.2
- **Effort**: Large (3.5 weeks)
- **Dependencies**: API Spec complete, OAuth integration
- **Key metrics**: Support 50+ NetSuite customers by Q4

#### Feature 2.3: Coupa Integration (Priority 3 - NICE TO HAVE)
- **Status**: Planned (Q4 instead of Q3)
- **Target**: GA October 15 (moved to Q4)
- **Audience value**:
  - **Exec**: Coupa is dominant but moving to SAP; integration differentiates us
  - **Engineering**: Coupa API is complex, 5 person-weeks effort
  - **Customer**: Two-way sync: we send spend analytics back to Coupa
- **RICE Score**: Reach 6/10 | Impact 6/10 | Confidence 7/10 | Effort 7/10 = 6.0
- **Effort**: Large (5 weeks)
- **Note**: Lower priority than Ariba/NetSuite (Coupa customers less likely to switch due to SAP deal)

---

### Theme 3: Platform Reliability & Security (20% Q3 Engineering Capacity)

#### Feature 3.1: 99.99% Uptime SLA (Priority 1 - MUST HAVE)
- **Status**: In Progress (started Q2)
- **Target**: GA July 1 (soft launch), public SLA Aug 1
- **Audience value**:
  - **Exec**: SOX/HIPAA compliance requirement for enterprise deals, enables uptime guarantee revenue (5-10% deal premium)
  - **Engineering**: Infrastructure hardening, multi-region failover, 2 person-weeks remaining
  - **Customer**: 99.99% guaranteed uptime (52 min/year max downtime)
- **RICE Score**: Reach 6/10 | Impact 9/10 | Confidence 8/10 | Effort 6/10 = 9.0
- **Effort**: Medium (2 weeks remaining)
- **Dependencies**: Multi-region deployment (Q2)
- **Key metrics**: <52 min/month downtime, 99.99% measured availability

#### Feature 3.2: SOX Audit Trail Enhancements (Priority 2 - SHOULD HAVE)
- **Status**: Planned
- **Target**: GA Aug 15
- **Audience value**:
  - **Exec**: Financial services compliance, enables banking/insurance deals
  - **Engineering**: Database logging improvements, 1.5 person-weeks
  - **Customer**: Complete audit trail, immutable compliance logs
- **RICE Score**: Reach 4/10 | Impact 8/10 | Confidence 9/10 | Effort 3/10 = 12.0
- **Effort**: Medium (1.5 weeks)
- **Note**: Not on critical path, but improves CSAT for compliance-sensitive deals

#### Feature 3.3: GDPR Data Retention Policy Automation (Priority 3 - NICE TO HAVE)
- **Status**: Planned (Q4 instead of Q3)
- **Target**: GA September 30
- **Audience value**:
  - **Exec**: GDPR compliance for EU deals, reduces legal liability
  - **Engineering**: Background job for data purging, 1 person-week
  - **Customer**: Auto-delete customer data per retention policy
- **RICE Score**: Reach 3/10 | Impact 7/10 | Confidence 8/10 | Effort 2/10 = 8.4
- **Effort**: Small (1 week)
- **Note**: Moved to Q4 due to lower priority vs. uptime SLA and integrations

---

## Capacity & Trade-Offs

**Total Q3 Capacity**: 20 person-weeks (4 squads × 5 weeks/squad)

**Allocation**:
- AI Compliance (Themes 1.1 + 1.2): 10 person-weeks (50%)
- Enterprise Integrations (Features 2.1 + 2.2): 7.5 person-weeks (37%)
- Platform Reliability (Features 3.1 + 3.2): 3.5 person-weeks (17%)
- **Total**: 21 person-weeks (over by 1 week; using Q4 buffer)

**Trade-offs Made**:
- **Deferred**: Feature 1.3 (Government compliance) → Q4 (lower revenue impact than integrations)
- **Deferred**: Feature 2.3 (Coupa integration) → Q4 (Coupa customers less likely to switch)
- **Deferred**: Feature 3.3 (GDPR automation) → Q4 (lower priority, can handle manually)

**Rationale**: Prioritized contract scoring (AI differentiation) + Ariba/NetSuite (deal blockers) over breadth. Better to ship 2 integrations well than 3 integrations poorly.

---

## ROADMAP VISUALIZED (GANTT)

```
July         Aug         Sept        Oct
|____________|____________|____________|

THEME 1: AI COMPLIANCE
  1.1 Contract Scoring    [=====|Beta |====|GA]
  1.2 Healthcare Rules         [==|GA]
  1.3 Gov't Rules (Q4)                      [Planned Q4]

THEME 2: INTEGRATIONS
  2.1 SAP Ariba          [===|Beta|==|GA]
  2.2 Oracle NetSuite    [======|Beta|===|GA]
  2.3 Coupa (Q4)                              [Planned Q4]

THEME 3: RELIABILITY
  3.1 99.99% Uptime    [carry-over|GA|]
  3.2 SOX Audit Trail        [===|GA]
  3.3 GDPR Auto (Q4)                      [Planned Q4]
```

---

## AUDIENCE-SPECIFIC VERSIONS

### VERSION A: EXECUTIVE SUMMARY (For Board, VP Sales, Finance)

**Key Message**: "Q3 transforms us into compliance leader while removing integration deal blockers"

| Theme | Key Benefit | Business Impact | Timeline |
|-------|-------------|-----------------|----------|
| AI Compliance | Only vendor with AI-driven contract risk scoring | $1.2B TAM expansion, Finance/Healthcare vertical growth | Aug 1 Beta, Aug 31 GA |
| Integrations | SAP Ariba + Oracle NetSuite unblock 3 pending deals | $450M ARR potential released | Aug-Sept |
| Reliability | SOX-compliant 99.99% uptime | Enterprise deal premium (5-10% ASP increase) | July 1 / Aug 1 GA |

**Revenue Impact**:
- **Conservative**: $2.5M ARR from compliance deals + $1.5M from integration unblocks = $4M
- **Optimistic**: $4M + $2M from uptime SLA premium = $6M

**Competitive Advantage**:
- Coupa: No AI compliance (static rules only)
- Jaggr: No compliance module at all
- Ariba: Rule-based, not AI

**Risk Mitigation**:
- AI accuracy risk: Mitigate with legal team feedback loop + manual override capability
- Integration dependencies: Both Ariba and NetSuite are mature, low-risk APIs

---

### VERSION B: ENGINEERING SUMMARY (For CTO, Tech Leads, Architecture)

**Key Message**: "Manage technical complexity with clear dependencies and parallel tracks"

| Feature | Technical Approach | Complexity | Dependencies | Risk |
|---------|-------------------|-----------|--------------|------|
| Contract Scoring | BERT fine-tuned, async processing, SLA < 2min | High | ML infrastructure (done), training data (Q2) | Model accuracy: 90% target achievable with 500+ contract labels |
| Ariba Connector | REST API, OAuth2 client credentials, batch + webhook sync | Medium | OAuth framework (done), API spec (done) | Ariba API changes rare; medium risk |
| NetSuite Connector | REST API, similar to Ariba, parallel track | Medium | Same as Ariba | NetSuite versioning (managed by scheduled updates) |
| Uptime SLA | Multi-region failover, RTO < 5 min, async replication | High | Infrastructure ready (Q2) | Failover testing critical; test plan ready |

**Architecture Decisions**:
- **Contract Scoring**: Async processing (don't block on ML) with webhook notification
- **Integrations**: Shared connector framework (Ariba + NetSuite use same code pattern)
- **Reliability**: Active-active multi-region (cheaper than active-passive, ensures 99.99%)

**Tech Debt**:
- Defer database optimization (Q4) — sufficient performance for Q3 volume
- Defer GraphQL layer (2027) — REST is sufficient for current integrations

---

### VERSION C: CUSTOMER-FACING ROADMAP (For Customer Advisory Board, Sales, Marketing)

**Key Message**: "We're building the features you asked for: contract automation, system integrations, enterprise reliability"

| Feature | Problem Solved | Value to You | Launch Date |
|---------|----------------|--------------|-------------|
| AI Contract Risk Scoring | Your legal team reviews 800+ contracts manually per year (4-6 hrs each) | Reduce review time to 1 hour per contract; catch compliance risks automatically | August 31 |
| Healthcare/Government Rules | Compliance reviews are manual, slow, inconsistent | Rules automatically flag HIPAA/FAR/DFARS violations | Sept 15 (Healthcare) |
| SAP Ariba Integration | You can't sync supplier data automatically from Ariba | Real-time supplier data sync, no manual exports, fewer errors | August 15 |
| Oracle NetSuite Integration | Mid-market customers need supplier + spend data sync | Consolidated view of supplier master + spend from NetSuite | Sept 1 |
| 99.99% Uptime SLA | You need guaranteed uptime for compliance | SLA-backed guarantee: max 52 minutes downtime per year | August 1 |

**How to Get Early Access**:
- CAB members: Contract scoring beta (July 15)
- Healthcare customers: Healthcare rules beta (August 15)
- SAP/NetSuite customers: Integration beta (July/August)

**What's Next (Q4)**:
- Government compliance rules
- Coupa integration
- Advanced forecasting

---

## Success Metrics (Tracked Weekly)

### Adoption Metrics
- Contract scoring: 80% of legal teams using feature by Sept 30
- Ariba integration: 20+ customers syncing data by Sept 30
- NetSuite integration: 30+ customers syncing data by Sept 30

### Performance Metrics
- Contract scoring: < 2 min per document, 90%+ accuracy
- Uptime: 99.99% availability (52 min/year max downtime)
- Integrations: 95%+ first-sync success rate

### Business Metrics
- Revenue from new compliance deals: $2.5M ARR
- Revenue from integration unblocks: $1.5M ARR
- Uptime SLA premium: 5-10% ASP increase on 10+ enterprise deals

---

## Dependencies & Risks

**Critical Path**:
1. ML infrastructure must be ready (Q2 ✓)
2. API specs finalized (April ✓)
3. Legal team labels for contract training (May-June)
4. OAuth framework (Q2 ✓)

**Risks**:
1. **AI Accuracy Risk** (Medium): Model achieves only 85% accuracy instead of 90%
   - Mitigation: Use conservative thresholds (only flag high-confidence risks), manual override
   - Contingency: Delay GA by 2 weeks for additional training

2. **Integration Delays** (Medium): Ariba/NetSuite APIs more complex than anticipated
   - Mitigation: Parallel tracks, start early (Week 1), dedicated tech lead
   - Contingency: Reduce scope (skip edge cases like multi-subsidiary support)

3. **Capacity Overrun** (Low): Team overbooked, capacity estimates too optimistic
   - Mitigation: Contingencies deferred to Q4 already built in
   - Contingency: Defer Feature 3.2 (SOX audit trail) to Q4

---

## Communication Plan

**Who**: CEO/Board → SVP Sales → Customer Success → Customers/CAB
**Timeline**:
- **Now (April 30)**: Board presentation + CEO announcement
- **May 15**: Sales enablement on AI compliance positioning
- **June 15**: CAB briefing + early access offers
- **July 1**: Public roadmap update (website/blog)
- **Aug 1**: Beta launch announcement + customer webinar
- **Aug 31**: GA announcement + case study release
- **Sept 30**: Q3 recap + Q4 preview

**Key Narrative**: "We're becoming the AI-first, enterprise-ready alternative to Coupa. Contract scoring and integrations prove it."

---

**Prepared by**: VP Product
**Approved by**: CEO, VP Engineering, SVP Sales
**Last Updated**: April 2026
**Next Review**: May 2026 (mid-quarter check-in)
