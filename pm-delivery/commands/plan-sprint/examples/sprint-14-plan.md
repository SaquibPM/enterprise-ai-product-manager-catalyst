# Sprint 14 Plan: Procurement Platform Team

**Sprint Period**: May 19-30, 2026 (2 weeks)
**Sprint Owner**: Senior PM (Procurement Platform)
**Team**: 5 engineers, 1 QA, 1 designer
**Prepared**: April 15, 2026

---

## Sprint Goal

**Goal**: "Enable legal teams to review contracts 80% faster with AI-powered risk scoring MVP"

**Success Criteria**:
- AI contract scorer processes 50+ real contracts with < 2 min turnaround
- 90%+ accuracy on High-Risk category
- Legal team can override/confirm each risk flag
- System logs all feedback for model training

**Why this goal**: Contracts are largest deal blocker (3 pending deals, $450M ARR opportunity). MVP unblocks sales by August 1.

---

## Capacity Overview

**Team Size**: 5 engineers (40 person-days total)
**Historical Velocity**: 35 points/sprint (7 pt/person-day)
**Projected Capacity**: 35 person-days (with 5-day slack for blockers)

**Allocation**:
- Features (Sprint goal): 28 person-days (80%)
- Tech Debt: 7 person-days (20%)
- **Total**: 35 person-days

---

## Features & Stories

### Story 1: Extract & Parse Contract Text (8 points)
**Acceptance Criteria**:
- [ ] Accept PDF/DOC uploads (max 50MB)
- [ ] Extract text using OCR (handle scanned documents)
- [ ] Parse document structure (headers, sections, clauses)
- [ ] Handle edge cases: 500+ page contracts (process first 30 pages)
- [ ] Handle edge cases: Non-English contracts (alert user, don't process)
- [ ] Timeframe: < 30 seconds per document
- [ ] Error handling: Return helpful error message if parse fails

**Engineering Estimate**: 3 person-days
**Dependencies**: None (can start immediately)
**Definition of Done**:
- Code reviewed by senior engineer
- Unit tests cover 90%+ of code paths
- QA tested with 20 real contracts (including edge cases)
- Staging environment passes smoke tests

**Owner**: Engineer A (Backend Lead)

---

### Story 2: Call ML Model & Return Risk Scores (13 points)
**Acceptance Criteria**:
- [ ] Integrate with trained contract risk model (model provided by ML team)
- [ ] Request format: parsed contract text + contract metadata
- [ ] Response format: risk scores (0-100) for 5 categories: Data Privacy, IP Liability, Payment Terms, Supplier Stability, Regulatory Compliance
- [ ] For each risk category: return top 3 flagged clauses with confidence scores
- [ ] Handle model timeouts: if model takes > 90 seconds, return graceful timeout error
- [ ] Async processing: don't block user on submission (response within 2 seconds)
- [ ] Webhook notification: notify client when scoring completes
- [ ] Performance: process 50 documents in parallel without degradation

**Engineering Estimate**: 4 person-days
**Dependencies**: ML model trained and accessible via REST API (External: ML team responsibility)
**Risks**:
- ML model accuracy < 85% (acceptable risk; we'll improve with feedback)
- Latency > 2 min per document (acceptable risk; adjust timeout)

**Definition of Done**:
- Integration tested with sample contracts
- Load test: 50 concurrent requests, verify latency
- Error handling: all timeout/failure scenarios tested
- Logging: all predictions logged for monitoring

**Owner**: Engineer B (Integration Lead)

---

### Story 3: Implement Risk Flagging UI (8 points)
**Acceptance Criteria**:
- [ ] Display risk scores in summary panel (5 risk categories, each 0-100)
- [ ] Color coding: Red (>75), Yellow (50-74), Green (<50)
- [ ] Show top 3 flagged clauses per risk category
- [ ] Link clause to location in original document
- [ ] Highlight flagged text in document viewer
- [ ] Show confidence score for each flag
- [ ] User can click "Not a Risk" to dismiss individual flags
- [ ] User can click "Higher Risk" to add custom flag
- [ ] All actions logged to audit trail

**Engineering Estimate**: 2.5 person-days
**Dependencies**: Story 2 (need actual risk scores to display)
**Definition of Done**:
- Design approved by product & design leads
- Responsive design (desktop first, tablet secondary)
- Accessibility: WCAG 2.1 AA compliant
- Cross-browser tested (Chrome, Safari, Firefox)

**Owner**: Engineer C (Frontend Lead)

---

### Story 4: Legal Team Feedback & Model Training Loop (6 points)
**Acceptance Criteria**:
- [ ] User can add notes explaining why they're overriding a flag
- [ ] Feedback stored to database (immutable)
- [ ] Dashboard shows: "Model improved 2% this week" (aggregate feedback stats)
- [ ] Export feedback data for ML team to retrain model
- [ ] Audit trail shows: who overrode, when, why
- [ ] Monthly report: flags most commonly overridden (signals model weaknesses)

**Engineering Estimate**: 2 person-days
**Dependencies**: Stories 2-3 (need flagged data to collect feedback on)
**Definition of Done**:
- Feedback API tested
- Audit trail verified immutable (no modification after creation)
- Export format validated with ML team
- Monthly report dashboard working end-to-end

**Owner**: Engineer D (Data Lead)

---

### Story 5: Contract Metadata & Audit Trail (5 points)
**Acceptance Criteria**:
- [ ] Store contract metadata: upload date, uploader name, contract type, supplier name
- [ ] Immutable audit trail: timestamp, action, user, change
- [ ] 7-year retention policy implemented (no data deleted for 7 years)
- [ ] Encryption at rest: contracts encrypted with AES-256
- [ ] Compliance logging: all data access logged
- [ ] Privacy: personally identifiable information redacted in logs

**Engineering Estimate**: 2.5 person-days
**Dependencies**: Database schema changes (coordinate with infrastructure team)
**Risks**: Encryption performance impact (mitigated: benchmark before commit)

**Definition of Done**:
- Security review completed
- Compliance checklist signed off (SOX, HIPAA requirements)
- Encryption performance tested (< 1% overhead)
- Audit trail manually verified for completeness

**Owner**: Engineer E (Infrastructure Lead)

---

## Tech Debt Items (20% Allocation = 7 person-days)

### Tech Debt 1: Refactor Contract Processing Pipeline (3 days)
**Description**: Current OCR library is slow and buggy. Need to upgrade to modern library (OCR v4).

**Why now**: 
- Blocking us from handling 500+ page contracts efficiently
- Current library crashes on 5% of scanned PDFs (unacceptable for GA)
- Effort is 3 days; blocks Feature story 1 if we don't do this now

**Benefits**: 
- 30% faster OCR processing
- 99%+ success rate (vs current 95%)
- Sets us up for future multi-language support

**Owner**: Engineer A (Backend Lead)

---

### Tech Debt 2: Database Schema Optimization (2 days)
**Description**: Contract table is not indexed properly. Need to add indexes for contract_type, upload_date, audit_trail queries.

**Why now**: 
- Query performance will degrade as we hit 10,000+ contracts (expected by end Q3)
- Easier to fix now than after performance issues in production
- Effort is low (index creation); high impact

**Benefits**: 
- Query performance: 5x faster for compliance queries
- Prevents performance regression in Q3

**Owner**: Engineer E (Infrastructure Lead)

---

### Tech Debt 3: Improve Error Handling & Observability (2 days)
**Description**: Error messages are generic. Need structured logging and distributed tracing for debugging in production.

**Why now**: 
- Contract processing is critical path; need visibility into failures
- Will help us diagnose model timeout issues
- Effort is 2 days; high value for operational maturity

**Benefits**: 
- Faster incident response (30% reduction in MTTR)
- Better user error messages
- Observability dashboard for team

**Owner**: Engineer D (Data Lead)

---

## Risks & Mitigation

### Risk 1: ML Model Accuracy Lower Than 85% (Medium Risk)
**Description**: If model achieves only 75% accuracy, feature will have too many false positives.

**Probability**: 30% (model team is confident, but real data may differ from training data)
**Impact**: Feature launch delayed by 1-2 weeks

**Mitigation**:
- Use conservative thresholds (only flag high-confidence risks, >85%)
- Manual review mode: require legal team to confirm before blocking
- Rapid feedback loop: collect override data immediately to retrain

**Owner**: Engineer B (responsible for model integration testing)

---

### Risk 2: Contract Viewer Performance (Medium Risk)
**Description**: Highlighting 1,000+ clauses in 500-page contract may cause browser lag.

**Probability**: 40%
**Impact**: Poor UX, feature unusable for long contracts

**Mitigation**:
- Limit highlighted clauses to top 10 per category (total 50)
- Implement virtual scrolling in document viewer
- Load test with 500-page contract before QA handoff

**Owner**: Engineer C (Frontend Lead responsible for performance testing)

---

### Risk 3: External Dependency on ML Model Team (Low Risk)
**Description**: ML team has not finished training the model. If delayed, Story 2 is blocked.

**Probability**: 10% (ML team committed to model delivery April 30)
**Impact**: 5-day slip in feature delivery

**Mitigation**:
- Weekly sync with ML team (starts this week)
- Provide sample contracts early (this week) so they have training data ready
- Fallback: use mock model with placeholder scores to unblock frontend development

**Owner**: Engineer B (owns ML integration, checks status weekly)

---

### Blocker 1: Database Migration Script (Must Resolve by May 19)
**Description**: Schema changes for contract storage require data migration (no data to migrate yet, but script needs approval).

**Status**: In security review (due April 20)
**Action**: Follow up with security team this week to unblock

**Owner**: Engineer E (Infrastructure Lead)

---

## Definition of Done

**For all stories**:
- [ ] Code reviewed by 2 engineers
- [ ] Unit tests: 90%+ coverage
- [ ] Integration tests: story acceptance criteria tested end-to-end
- [ ] QA testing: manual testing with real data
- [ ] Documentation: API docs updated, user guide drafted
- [ ] Performance: benchmarks meet SLAs
- [ ] Security: security review completed
- [ ] Accessibility: WCAG 2.1 AA compliant (for UI stories)
- [ ] Monitoring: logging and alerts configured in staging

**Sprint review demo**:
- All stories demoed to product/stakeholder
- Demo uses real customer contract (anonymized)
- Feedback documented for next sprint

---

## Team Assignments

| Story | Owner | Support | Story Points | Estimate |
|-------|-------|---------|-------------|----------|
| 1. Extract & Parse | Eng A | Eng E | 8 | 3d |
| 2. ML Integration | Eng B | Eng A | 13 | 4d |
| 3. Risk Flagging UI | Eng C | Designer | 8 | 2.5d |
| 4. Feedback Loop | Eng D | Eng B | 6 | 2d |
| 5. Metadata & Audit | Eng E | Eng A | 5 | 2.5d |
| **Tech Debt 1** | **Eng A** | | **3d** | **3d** |
| **Tech Debt 2** | **Eng E** | | **2d** | **2d** |
| **Tech Debt 3** | **Eng D** | | **2d** | **2d** |

**Total**: 40 points + 7 days tech debt = 35 person-days

---

## Sprint Schedule

**Monday, May 19** (Kickoff Day)
- 10am: Sprint kickoff meeting (30 min)
- 10:30am-4pm: Focused development time
- 4pm: Daily standup #1 (15 min)

**Tue-Thu, May 20-22**: 
- 9:30am: Daily standup (15 min)
- All day: Development
- 5pm: Engineering sync (30 min, if needed for blockers)

**Friday, May 23** (Mid-Sprint Check-In)
- 9:30am: Daily standup (15 min)
- 10am: Midpoint check-in (Eng leads + PM) — Are we on track?
- Status: All 5 feature stories should be 50%+ done
- All day: Development

**Monday, May 26** (Memorial Day - Optional)
- Team may work or take day off; no standup required

**Tue-Thu, May 27-29**:
- 9:30am: Daily standup (15 min)
- All day: Development + QA testing
- Friday: Prepare for sprint review

**Friday, May 30** (Sprint Review & Planning)
- 10am: Sprint review with stakeholders (demo, feedback)
- 12pm: Sprint retro with team
- 2pm: Sprint 15 planning kickoff

---

## Success Metrics (Measured at Sprint Review)

**Velocity**: 40 points delivered (7 pt/person-day × 5 engineers)
**Quality**: 0 critical bugs in QA (acceptable: max 2 medium bugs)
**Scope**: All 5 feature stories + tech debt 1-3 completed
**Stakeholder feedback**: "Ready for August 1 beta" (from legal team stakeholder)

---

## Sign-Offs

| Role | Name | Date | Approval |
|------|------|------|----------|
| Product Manager | [Name] | | [  ] |
| Engineering Lead | [Name] | | [  ] |
| QA Lead | [Name] | | [  ] |

---

**Prepared by**: Senior Product Manager
**Approved by**: Engineering Lead, VP Product
**Last Updated**: April 15, 2026
**Next Review**: Mid-sprint (May 23)
