# Evaluation Framework: Intelligent Document Extraction

## Executive Summary

Intelligent Document Extraction is a procurement AI feature that automatically extracts structured data from unstructured business documents (invoices, purchase orders, contracts). The system converts PDFs and images into validated JSON records, reducing manual data entry by 85% and enabling real-time spend visibility.

**Business Impact**: Reduces AP processing time from 3 days to 4 hours, saves $450K annually in labor costs across enterprise customers.

**Evaluation Scope**: End-to-end accuracy, latency, cost, safety, and compliance for invoice, PO, and contract document types.

---

## Feature Description

### What It Does

The Intelligent Document Extraction system accepts vendor documents (invoices, purchase orders, contracts) in PDF or image format. The system:

1. Pre-processes the document (OCR, page detection, table extraction)
2. Extracts structured fields (vendor name, invoice number, amounts, line items, dates)
3. Validates extracted data against business rules
4. Returns validated JSON with confidence scores and audit trail

### Input Types

- **Invoices**: Scanned or native PDFs, 1-10 pages, 50+ vendor formats
- **Purchase Orders**: Structured and semi-structured, internal and supplier-generated
- **Contracts**: Long-form documents (5-50 pages), clause extraction and obligation identification

### Expected Outputs

```json
{
  "document_type": "invoice",
  "extracted_fields": {
    "vendor_name": "Acme Corp Inc.",
    "invoice_number": "INV-2024-001234",
    "invoice_date": "2024-04-10",
    "due_date": "2024-05-10",
    "total_amount": 15750.00,
    "currency": "USD",
    "line_items": [
      {
        "description": "Software License (Annual)",
        "quantity": 1,
        "unit_price": 12000.00,
        "line_total": 12000.00,
        "tax_rate": 0.0875,
        "tax_amount": 1050.00
      }
    ],
    "tax_total": 1050.00,
    "shipping": 700.00,
    "payment_terms": "Net 30"
  },
  "confidence_scores": {
    "vendor_name": 0.98,
    "total_amount": 0.99,
    "line_items": 0.94
  },
  "data_quality_flags": [],
  "validation_status": "PASSED",
  "extraction_time_ms": 1247,
  "audit_trail": {
    "extracted_at": "2024-04-12T14:32:15Z",
    "model_version": "v3.2.1",
    "user_id": "ap-processor-01"
  }
}
```

### Use Cases

1. **Invoice Processing**: Auto-import vendor invoices → accounts payable system
2. **Contract Compliance**: Extract obligations, renewal dates, payment terms
3. **Spend Analytics**: Aggregate invoices across vendors and cost centers for analysis
4. **Duplicate Detection**: Identify duplicate submissions before payment

### Success Criteria

- **Accuracy**: 96% field-level accuracy on invoices, 92% on contracts
- **Speed**: P95 latency < 3 seconds per document
- **Cost**: < $0.02 per document extraction
- **Safety**: Zero PII leakage, 100% cross-tenant isolation

---

## Evaluation Dimensions

### Primary Dimensions (Must Pass)

#### 1. Correctness

**Definition**: Percentage of extracted fields matching ground truth (exact or fuzzy match).

**Metrics**:
- **Field-level accuracy**: Exact match on individual fields (vendor name, invoice number)
- **Amount correctness**: Numeric fields within ±2% (handles rounding, currency conversion)
- **Line-item accuracy**: Correct number of line items, correct per-item amounts
- **Date parsing**: Exact match on ISO 8601 format

**Thresholds**:
- Invoice fields: ≥96% exact match (vendor, invoice number, dates, amounts)
- Contract obligations: ≥92% partial match (LLM-as-judge, semantic similarity ≥0.85)
- Line-item extraction: ≥94% count accuracy, ≥95% total amount accuracy

**Target**: 96.5% overall field-level accuracy

#### 2. Latency

**Definition**: Time from document submission to validated JSON response.

**Metrics**:
- **P50 latency**: 800ms (median)
- **P95 latency**: 3000ms (95th percentile)
- **P99 latency**: 8000ms (99th percentile)

**Breakdown** (target):
- OCR/pre-processing: 300ms
- Field extraction: 700ms
- Validation: 200ms
- JSON serialization: 50ms

**Thresholds**:
- P95 < 3000ms (must pass before prod)
- P99 < 8000ms (alert if trending up)
- No document > 30 seconds

**Target**: P95 ≤ 2500ms

#### 3. Cost Per Extraction

**Definition**: Total infrastructure cost (model inference, OCR, storage) per document.

**Metrics**:
- **Direct cost**: Model API calls, OCR service, storage (per document)
- **Overhead**: Cache hits, failed extractions, retries

**Thresholds**:
- Target cost: < $0.020 per document
- Alert if trending: > $0.025
- Budget ceiling: $0.030 per document (hard stop)

**Cost Targets by Document Type**:
- Invoice (simple, 1-2 pages): $0.012
- Purchase Order: $0.015
- Contract (long-form, 10+ pages): $0.025

### Secondary Dimensions (Improve But Not Blockers)

#### 4. Faithfulness

**Definition**: Extracted data accurately represents the document content (not hallucinated or interpolated).

**Metrics**:
- **LLM-as-judge**: Evaluates if extracted data is grounded in visible document content
- **Confidence scores**: System's own uncertainty estimates aligned with actual errors

**Thresholds**:
- LLM-as-judge faithfulness rubric: ≥0.90 (scale: 0.0-1.0)
- Confidence score calibration: ≤5% error rate in high-confidence predictions (>0.95)

**Target**: 0.92 faithfulness

#### 5. Handling of Edge Cases

**Definition**: Accuracy on unusual or challenging documents (poor scans, unusual formats, multi-currency).

**Metrics**:
- Scanned PDFs (low OCR quality): ≥90% accuracy
- Multi-page documents (>5 pages): ≥94% accuracy
- Handwritten fields: ≥80% accuracy
- Multi-currency invoices: ≥95% amount accuracy

**Target**: ≥92% on edge cases

### Tertiary Dimensions (Nice to Have)

#### 6. Coverage

**Definition**: Percentage of vendor formats supported without human intervention.

**Metric**: Out-of-box extraction success rate

**Target**: Support 95% of vendor formats by document count (Top 100 vendors cover ~70% of incoming documents)

---

## Evaluation Dataset Design

### Dataset Composition

**Total Size**: 1,000 documents across 5 document types

| Document Type | Count | Sources | Language |
|---|---|---|---|
| Invoices | 500 | 45 unique vendors | English, Spanish, German |
| Purchase Orders | 250 | Internal + supplier | English |
| Contracts | 150 | Procurement, MSAs | English |
| Amendments | 75 | Contract changes | English |
| Receipts | 25 | Travel, expenses | English |
| **Total** | **1,000** | - | - |

### Coverage Strategy

**Representing Real Traffic** (80% of data):
- Top 20 vendors by volume (45% of invoices)
- Standard invoice format (basic table structure)
- Clear OCR quality
- Straightforward payment terms

**Edge Cases** (15% of data):
- Scanned/faxed documents (10% poor OCR quality)
- Handwritten fields (3%)
- Multi-page line items (2%)
- Non-standard vendor formats (unique layouts)

**Adversarial Cases** (5% of data):
- Intentionally corrupted PDFs
- Truncated documents
- Documents with logos/images covering fields
- Mixed language invoices (EN/ES/DE)
- Duplicate line items

### Annotation Methodology

**Ground Truth Creation**:
- Human experts manually extract fields for all 1,000 documents
- 2-pass review: initial annotation + QA verification
- Annotation guidelines: 15-page document covering format rules, edge cases, ambiguity handling
- Tools: Custom annotation interface with field validation

**Quality Assurance**:
- Inter-annotator agreement (kappa) target: ≥0.92 on all fields
- Random spot-check: 10% of annotations reviewed by senior annotator
- Ambiguous cases: 3-way review with consensus decision

**Update Cadence**:
- Add 50 new vendor formats quarterly (track new vendors in production)
- Refresh edge case subset every 6 months
- Continuous: Log production extraction errors, add misclassified documents to eval set

---

## Evaluation Methods

### 1. Deterministic Metrics

#### Field-Level Exact Match
**For**: Vendor name, invoice number, invoice date, due date, currency, payment terms

**Process**:
```python
for field in deterministic_fields:
  if extracted[field] == ground_truth[field]:
    score += 1
accuracy = score / len(deterministic_fields)
```

**Tool**: Python script with string normalization (trim whitespace, lowercase for names)

#### Numeric Accuracy (Amount Fields)
**For**: Invoice total, tax, shipping, line-item prices

**Tolerance**: ±2% (handles rounding differences, currency conversion)

```python
tolerance = 0.02
error_rate = abs(extracted - ground_truth) / ground_truth
if error_rate <= tolerance:
  score += 1
```

#### Date Parsing Validation
**For**: Invoice date, due date, delivery date

**Format Check**: ISO 8601 compliance (YYYY-MM-DD)

**Logic Check**: Due date ≥ invoice date, dates within valid range (2020-2030)

#### Schema Validation
**For**: JSON structure, data types, required fields

**Validation Rules**:
- Required fields present: vendor_name, invoice_number, total_amount, invoice_date
- Data types correct: amounts are floats, dates are ISO 8601 strings
- Array structures valid: line_items is non-empty array of objects
- No extra fields outside schema

**Tool**: JSON schema validator (jsonschema library)

---

### 2. LLM-as-Judge Evaluation

**For**: Contract obligations, semantic correctness, faithfulness

#### Rubric: Contract Obligation Extraction

For each extracted obligation, evaluate:

**Completeness** (0-1 scale):
- 1.0: All parties, conditions, and deadlines clearly identified
- 0.67: Two of three dimensions present
- 0.33: One dimension present
- 0.0: Obligation missing or hallucinated

**Accuracy** (0-1 scale):
- 1.0: Extracted text matches document verbatim (or close paraphrase)
- 0.67: Core terms correct, minor details missing
- 0.33: Core terms present, significant context missing
- 0.0: Inaccurate or contradicts document

**Faithfulness** (0-1 scale):
- 1.0: Grounded in visible document text, no interpolation
- 0.67: Mostly grounded, minor inference
- 0.33: Some inference/extrapolation
- 0.0: Hallucinated or contradicts visible content

**Overall Score**: Average of three rubric dimensions

**Prompt Template**:
```
You are evaluating contract obligation extraction. Here is:
1. The original contract text
2. The extracted obligation from our system

[CONTRACT TEXT]

[EXTRACTED OBLIGATION]

Rate this extraction on:
- Completeness (0-1): Are all parties, conditions, deadlines captured?
- Accuracy (0-1): Do extracted terms match the document?
- Faithfulness (0-1): Is it grounded in visible text (not hallucinated)?

Provide a JSON response with scores and brief explanation.
```

**LLM Model**: claude-opus (gpt-4 equivalent for consistency)

**Sample Size**: 150 contracts (100% of contract eval dataset)

**Threshold**: ≥0.90 average across all three rubric dimensions

---

### 3. Human Review

#### Monthly Expert Annotation

**Sample Size**: 50 edge cases per month (documents where system confidence < 0.90 OR model errors detected)

**Annotator Profile**: 
- Procurement domain expert (ex-AP manager or vendor management specialist)
- 2+ years document processing experience
- Access to annotation guidelines and vendor-specific rules

**Review Process**:
1. Annotator reviews extraction vs. original document
2. Marks fields as: CORRECT / INCORRECT / AMBIGUOUS
3. Provides explanation for any errors
4. Flags systematic issues (e.g., "this vendor uses non-standard date format")

**Frequency**: Monthly retrospective review (not real-time)

**Metrics Tracked**:
- Error patterns (which field types, which vendors, which document formats)
- Emerging vendor formats
- Ambiguities in annotation guidelines

**Adjustment Trigger**: If error rate > 5% in monthly review, flag for model retrain

---

### 4. Tool-Based Validation

#### Regex Validation for Amounts

**For**: Invoice numbers, amounts, dates that follow standard patterns

```regex
invoice_number: /^[A-Z]{0,4}-?\d{6,10}$/
amount: /^\d+(\.\d{2})?$/
date: /^\d{4}-\d{2}-\d{2}$/
```

**Action**: Flag non-conforming values for manual review

#### Sanity Checks

```python
# Cross-field validation
assert line_items_total == total_amount - tax - shipping
assert all_line_items_positive
assert invoice_date <= due_date
assert tax_rate in [0.0, 0.05, 0.0875, 0.10, 0.15]
```

---

## Regression Testing Strategy

### Model Upgrade Playbook

**When**: Before deploying a new model version to production

**Pre-Deployment Testing** (2 days):

**Day 1: Offline Evaluation**
- Run new model on full 1,000-document eval set
- Compare metrics to baseline:
  - Correctness: ≥ baseline (no regression)
  - Latency: ≤ baseline + 10%
  - Cost: ≤ baseline + 5%
- Document which vendor formats improved/regressed
- Investigate any fields with > 5% drop in accuracy

**Pass Criteria**:
- Overall correctness ≥ baseline
- P95 latency ≤ baseline + 10%
- Cost ≤ baseline + 5%

**Day 2: Canary Deployment**
- Deploy new model to 5% of production traffic (e.g., 1 vendor or time-based)
- Monitor for 24 hours:
  - Error rate (validation failures, exceptions)
  - User corrections (human feedback indicating extraction errors)
  - Performance (latency, cost)
- If metrics clean, proceed to full rollout
- If issues detected, rollback and debug

**Full Rollout**:
- Deploy to 100% traffic
- Maintain 24-hour "hot standby" of previous model for instant rollback

### Rollback Criteria

**Automatic Rollback if Any**:
- Error rate > 1% (validation failures)
- Correctness drops > 3% vs. baseline
- P95 latency > baseline + 15%
- Cost > baseline + 10%
- Any PII detection failure in guardrail tests

**Manual Rollback if**:
- Customer reports spike in manual corrections
- Critical vendor format broken (e.g., top 5 vendors all failing)
- Safety or compliance issue detected

**Rollback Procedure**:
1. Operator flips model version flag (instant)
2. Monitor error rate for 5 minutes
3. If clean, document issue and schedule post-mortem
4. Previous model remains in hot standby for 48 hours

### A/B Test Design (for model architecture changes)

**When**: Comparing fundamentally different models (e.g., OCR v2 vs. v3)

**Setup**:
- Split traffic 50/50 (model A vs. model B)
- Run for 1-2 weeks (capture variance in vendor mix and document types)
- Measure:
  - Correctness (field-level accuracy)
  - Latency (P50, P95, P99)
  - Cost (infrastructure cost per extraction)
  - User corrections (proxy for field accuracy in production)

**Statistical Significance**:
- Sample size: 5,000+ documents per variant (2 weeks at 400 docs/day)
- Threshold: p-value < 0.05 for any metric change

**Decision Criteria**:
- Model B must outperform on primary metric (correctness) AND not regress on secondary metrics
- Example: "Model B: +2.1% accuracy (p=0.003), P95 latency +150ms (p=0.08, not sig)" → Adopt B

---

## Guardrail Evaluation

### 1. PII Detection and Handling

**Sensitive Fields Detected**:
- Bank account numbers (IBAN, routing numbers)
- Passport/ID numbers
- Social security numbers
- Personal phone numbers, addresses
- Credit card numbers

**Evaluation Tests**:

**Test Set**: 50 documents intentionally containing PII

**Test Cases**:
- Invoice with vendor's personal phone number in contact field: System must redact before storing
- Contract with employee SSN in signature block: System must flag and mask
- PO with employee home address as delivery destination: System must detect and redact

**Acceptance Criteria**:
- ≥99% PII detection (all high-confidence PII flagged)
- ≥95% accuracy (minimal false positives on common business terms like "ACME Corp Ltd.")
- 100% redaction (no PII in stored JSON or logs)

**Monitoring**: Weekly audit of production extractions for PII leakage (sampling 1,000 docs)

---

### 2. Cross-Tenant Isolation

**Setup**: Multi-tenant system where each customer sees only their own data

**Evaluation Tests**:

**Test Cases**:
- Customer A extracts invoice from Vendor 1 → Customer B cannot access extraction via API
- Cache hit on identical invoice from different customers → Each gets only their copy
- Audit logs include customer_id for all extractions and corrections

**Acceptance Criteria**:
- 100% data isolation (zero cases of cross-tenant data leakage in 1-month production audit)
- Access controls enforced (no customer can read/write another's data)
- Audit trail complete (all operations logged with customer_id and user_id)

**Monitoring**: Weekly access control audit, monthly third-party security review

---

### 3. Cost Ceiling and Quota Enforcement

**Rules**:
- Per-document cost cap: $0.030
- Per-customer monthly quota: $50,000
- Per-tenant monthly quota: $500,000

**Evaluation Tests**:

**Test Cases**:
- Customer hits monthly quota → System rejects new extractions with clear error message
- Single document extraction costs exceed $0.030 (e.g., 100-page contract) → System logs alert and charges max of $0.030
- High-volume customer hitting quota mid-month → Customer notified with 48-hour advance notice

**Acceptance Criteria**:
- 100% quota enforcement (zero overages in production)
- Clear error messages and customer notifications
- Audit trail documents all quota decisions

**Monitoring**: Daily cost tracking, monthly billing reconciliation

---

### 4. Output Format and Schema Compliance

**Schema Definition**:
```json
{
  "document_type": "enum (invoice|po|contract)",
  "extracted_fields": {
    "vendor_name": "string (max 255 chars)",
    "invoice_number": "string (max 50 chars)",
    "invoice_date": "ISO 8601 date",
    "total_amount": "float, non-negative",
    "currency": "enum (USD|EUR|GBP|JPY)",
    "line_items": "array of { description, quantity, unit_price, line_total }",
    "tax_total": "float, non-negative",
    "payment_terms": "string (max 100 chars)"
  },
  "confidence_scores": "object with float values 0.0-1.0",
  "validation_status": "enum (PASSED|FAILED_VALIDATION|LOW_CONFIDENCE)",
  "extraction_time_ms": "integer, positive",
  "audit_trail": {
    "extracted_at": "ISO 8601 datetime",
    "model_version": "semantic version (e.g., v3.2.1)",
    "user_id": "string"
  }
}
```

**Evaluation Tests**:

**Test Cases**:
- All fields present and correctly typed
- No extra fields in output (schema drift detection)
- Currency enum enforced (e.g., rejects "Dollar" or "dollars")
- Amounts are non-negative (validation catches negative totals)
- Dates are valid ISO 8601 (e.g., rejects "04/10/2024")
- confidence_scores all between 0.0 and 1.0

**Acceptance Criteria**:
- 100% schema compliance (zero invalid JSON responses)
- Type validation catches all errors
- Downstream systems can parse response without custom error handling

**Monitoring**: Weekly JSON validation audit, alert on any schema violations

---

## Quality Gates

### Pre-Production Deployment Checklist

**Eval Dimension Performance** (All Must Pass):

```
Correctness:
  ☐ Invoice field accuracy ≥ 96%
  ☐ Contract obligation accuracy ≥ 92%
  ☐ Line-item extraction ≥ 94%
  Pass/Fail: [MUST PASS]

Latency:
  ☐ P95 latency < 3000ms
  ☐ P99 latency < 8000ms
  ☐ No document > 30 seconds
  Pass/Fail: [MUST PASS]

Cost:
  ☐ Average cost per document < $0.020
  ☐ 99th percentile cost < $0.025
  ☐ No single extraction > $0.030
  Pass/Fail: [MUST PASS]
```

**Safety & Compliance** (All Must Pass):

```
Guardrails:
  ☐ PII detection rate ≥ 99%
  ☐ Cross-tenant isolation: 100%
  ☐ Cost quota enforcement: 100%
  ☐ Schema compliance: 100%
  Pass/Fail: [MUST PASS]

Security:
  ☐ Third-party penetration test passed
  ☐ SOCS 2 compliance verified
  ☐ Data encryption in transit and at rest
  Pass/Fail: [MUST PASS]
```

**Operational Readiness** (All Must Pass):

```
Documentation:
  ☐ Runbook: How to handle extraction errors
  ☐ Rollback procedures documented and tested
  ☐ Customer communication templates ready
  ☐ Monitoring dashboard configured
  Pass/Fail: [MUST PASS]

Monitoring:
  ☐ Alerts configured for error rate > 1%
  ☐ Alert configured for correctness drop > 3%
  ☐ Cost overrun alerts configured
  ☐ On-call rotation assigned
  Pass/Fail: [MUST PASS]

Training:
  ☐ Support team trained on common issues
  ☐ Engineering team trained on rollback
  ☐ Product team trained on communicating limits
  Pass/Fail: [MUST PASS]
```

### Exceptions and Approval Workflow

**Metric Exception Request**:
- If any metric below threshold, submit exception request to:
  - VP Product (correctness/latency/cost)
  - Head of Security (safety/compliance)
  - Head of Engineering (operational readiness)
- Exception must include:
  - Metric value and threshold
  - Risk mitigation plan
  - Rollback trigger criteria
  - Justification (e.g., "Contract extraction is harder than invoices; 92% is acceptable given domain complexity")
- Approval required from all 3 stakeholders (unanimous)

**Accepted Exceptions** (pre-approved):
- Contract obligation accuracy can be 92% (vs. 96% for invoices) due to semantic complexity
- P99 latency for 10+ page contracts can be 10-12 seconds (vs. 8 second global limit)

---

## Dashboard & Monitoring

### Weekly Metrics Report

**Report Cadence**: Every Monday 9 AM (automated email to product, eng, safety)

**Core Metrics**:

| Metric | Current Week | Target | Status | Trend |
|---|---|---|---|---|
| Invoice extraction accuracy | 96.2% | ≥96.0% | PASS | ↑ +0.3% |
| Contract accuracy | 91.8% | ≥92.0% | FAIL | ↓ -0.7% |
| P95 latency | 2.1s | <3.0s | PASS | ↔ flat |
| Cost per extraction | $0.0187 | <$0.020 | PASS | ↓ -0.0015 |
| Error rate | 0.4% | <1.0% | PASS | ↑ +0.1% |
| Customer corrections | 247 | <500 | PASS | ↔ flat |
| Uptime | 99.97% | ≥99.95% | PASS | ↑ +0.01% |

**Vendor Performance** (Top 10 + worst 5):

| Vendor | Documents | Accuracy | Change | Notes |
|---|---|---|---|---|
| Acme Corp | 1,247 | 97.1% | ↑+1.2% | All formats performing well |
| Global Tech | 856 | 94.3% | ↓-2.1% | New vendor format Q2 needs tuning |
| DataCo | 634 | 92.1% | ↔ flat | Consistent, some edge cases |
| ... | | | | |
| SmallVendor A | 12 | 75.0% | - | Low volume, unique format (not in training set) |

---

### Alert Thresholds

**Critical** (Immediate Escalation to On-Call):
- Error rate > 2% (hourly check)
- Any PII leakage detected (real-time)
- Cost spike > 50% above rolling average (hourly)
- Correctness drops > 5% vs. baseline (hourly)
- System latency P95 > 5s for 15 minutes (real-time)

**Warning** (Logged, Daily Review):
- Error rate 1-2%
- Correctness 94-96%
- P95 latency 3-5s
- Cost trending 10% above target
- New vendor format with >20% error rate

**Info** (Weekly Report):
- New vendors detected in production
- Edge case patterns emerging
- Customer feedback themes

---

### Incident Response Procedures

**On-Call Escalation**:
1. Automated alert → On-call engineer (Slack + SMS)
2. Engineer acks within 15 minutes
3. If error rate continuing to rise, auto-rollback to previous model
4. Engineer investigates root cause while fallback model handles traffic

**Post-Incident**:
- Root cause analysis within 24 hours
- Fix or model retrain within 48-72 hours
- Communication to affected customers within 4 hours of detection

**Example Incident Scenario**:
- 9:15 AM: Alert: "Error rate 3.5%, up from 0.4%"
- 9:20 AM: Engineer confirms: New vendor format (previously unseen) causing failures
- 9:25 AM: Rollback triggered → Error rate drops to 0.5%
- 10:00 AM: Customer notification sent (email + in-app alert)
- 2:00 PM: RCA: Vendor invoice uses non-standard date format "dd-mm-yyyy" instead of "mm-dd-yyyy"
- Next Monday: Model retrained on updated training set including new format

---

## Success Criteria (6-Month Targets)

### Correctness

**Month 1-2 (Baseline)**:
- Invoice accuracy: 96.0%
- Contract accuracy: 91.5%
- Error rate: 0.5-1.0%

**Month 3-4 (Optimization)**:
- Invoice accuracy: 96.5%
- Contract accuracy: 92.5%
- Error rate: < 0.3%

**Month 5-6 (Excellence)**:
- Invoice accuracy: 97.0%
- Contract accuracy: 93.0%
- Error rate: < 0.2%

### Latency & Cost

**Month 1-2**:
- P95 latency: 2.5-3.0s
- Cost per doc: $0.018-0.020

**Month 3-4**:
- P95 latency: 2.0-2.5s
- Cost per doc: $0.015-0.018

**Month 5-6**:
- P95 latency: < 2.0s
- Cost per doc: < $0.015

### User Adoption & Satisfaction

**Month 1-2**:
- Customers onboarded: 15 pilot accounts
- Documents processed: 50,000
- Manual corrections: 2-3% of extractions
- NPS: +35 (feature-specific)

**Month 3-4**:
- Customers onboarded: 35 accounts
- Documents processed: 200,000
- Manual corrections: 1-2% of extractions
- NPS: +42

**Month 5-6**:
- Customers onboarded: 50+ accounts
- Documents processed: 400,000+
- Manual corrections: < 1% of extractions
- NPS: +50

### Safety & Compliance

**6-Month Target**:
- PII detection: 99.5%
- Cross-tenant isolation: 100%
- Uptime: 99.95%
- Security audit: Passed (SOCS 2 Type 2)
- Customer data loss incidents: 0

### Business Impact

**6-Month Target**:
- Total documents processed: 500,000+
- Manual AP processing time saved: 10,000+ hours
- Customer cost savings: $1.2M+ (at $450K per major customer)
- Revenue impact: $2.5M ARR (at $50K/customer, 50 customers)

---

## Approval Checklist

- [ ] VP Product: Metrics and targets approved
- [ ] Head of Engineering: Technical feasibility confirmed
- [ ] Head of Security: Safety and compliance requirements sign-off
- [ ] CFO: Cost model and revenue projections approved
- [ ] Legal: Data handling and customer contracts reviewed

**Approved By**: _______________  **Date**: _______________

