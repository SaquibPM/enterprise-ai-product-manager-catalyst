---
name: ai-evaluation
description: >
  Master AI evaluation frameworks that catch regressions before they hit production. Learn to design eval datasets, implement LLM-as-judge scorers with proven rubrics, set SLA thresholds, and orchestrate regression testing across model upgrades. Build confidence in your AI features with deterministic checks, human calibration, and continuous monitoring. This skill covers the complete evaluation lifecycle: from defining success metrics for enterprise AI to shipping model upgrades with zero surprises. Includes worked example for spend classification in procurement analytics—real dimensions, real rubrics, real numbers.
---

## Purpose

Evaluation is how enterprise AI products survive first contact with real data. A strong evaluation framework tells you exactly what changed when you upgrade a model, catches feature degradation before customers do, and gives you confidence to ship faster. This skill teaches you to design evals that match your business requirements (accuracy matters for classification, latency matters for user-facing features, cost matters when you're scaling), implement scoring mechanisms you can actually trust, and build regression testing that makes model upgrades routine instead of terrifying.

---

## Key Concepts

### Eval Dimensions Taxonomy

Every AI feature is multi-dimensional. You can't just measure accuracy and ship. Here are the dimensions that matter in enterprise settings:

#### 1. Correctness
**Definition:** The output accurately completes the intended task.

For enterprise AI, correctness breaks into two sub-types:
- **Binary correctness:** The answer is either right or wrong (e.g., is this email spam? Is this invoice valid?). Score as 0 or 1.
- **Graduated correctness:** Partial credit exists (e.g., wrong category but right parent category; correct answer but incomplete explanation). Use a scale like 0-5:
  - 0: Completely wrong direction
  - 1: Wrong answer, but reasoning shows partial understanding
  - 2: Partially correct (e.g., right category at L1, wrong at L2)
  - 3: Correct with minor gaps
  - 4: Correct with complete reasoning
  - 5: Perfect

**Example:** Spend classification in procurement—if the system predicts "Office Supplies > Printer Paper" when the correct category is "Office Supplies > Toner," that's a graduated score of 3 (right L1, wrong L2) not a 0.

**Enterprise context:** You need this because customers accept "close but not perfect" in some domains. A finance analyst can quickly override a slightly wrong spend category, but a completely wrong one wastes their time. Graduated scoring reflects real business impact.

#### 2. Faithfulness
**Definition:** The output is grounded in provided context and doesn't invent facts (hallucination detection).

This is critical for enterprise features—if your AI makes up compliance details, creates fake customer data, or invents pricing information that doesn't exist in your knowledge base, you're exposed.

Scoring:
- 0: Hallucinated critical facts not in source material
- 1: Minor hallucinations or extrapolations beyond source
- 2: Grounded in source, with acceptable inferences
- 3: Completely faithful to provided context

**Example:** Given a contract document, if the AI says "this contract includes a 30-day termination clause" when no such clause exists, that's a 0. If it infers "likely 2-3 weeks for implementation" based on "similar projects took 2-3 weeks," that might be a 2 depending on context.

#### 3. Relevance
**Definition:** The output addresses what the user actually wanted, even if the answer is technically accurate.

This catches a common trap: your system is correct but answers the wrong question.

Scoring:
- 0: Completely off-topic
- 1: Tangentially related but misses main intent
- 2: Addresses intent with some extraneous content
- 3: Directly addresses what was asked

**Example:** "What are my top 10 highest-spend vendors?" If the system returns the top 10 vendors by transaction count (not spend), it's accurate but gets a 0 for relevance. If it returns vendors ranked by total annual spend but also includes ones below the top 10, that's a 2.

#### 4. Safety
**Definition:** No harmful content, no PII leakage, no compliance violations.

This is binary: safe or not safe.

Sub-checks:
- **PII detection:** Does the output expose customer names, email addresses, payment details, SSNs, etc. that weren't in the source material?
- **Harmful content:** Does the system produce instructions for illegal activity, hate speech, abuse?
- **Compliance violations:** Does the output reveal information that violates GDPR, HIPAA, SOX, etc.?

Score as 0 (unsafe) or 1 (safe).

**Example:** User asks "list all customers in California." If the system responds with names and email addresses, that's a 0 (PII leakage). If it responds with a count and aggregate metrics only, that's a 1.

#### 5. Latency
**Definition:** Response time meets SLA requirements.

Enterprise SLAs matter:
- **p50:** Median response time (50% of requests complete within this time)
- **p95:** 95% of requests complete within this time (the tail)
- **p99:** 99% of requests complete within this time (extreme outliers)

Example SLA: "Spend classification must return within p95 of 500ms for the UI, p99 of 2 seconds."

Scoring depends on your SLA:
- 0: Exceeds p99 timeout
- 1: Between p95 and p99
- 2: Between p50 and p95
- 3: Within p50 or better

**Enterprise context:** A classification that takes 10 seconds might be 100% accurate but useless if your dashboard needs responses in 200ms. Latency is a hard constraint.

#### 6. Cost
**Definition:** Token usage, API costs, infrastructure spend per request.

This matters as you scale. If your AI feature costs $0.50 per request and you're running 1M requests/month, that's $500K/month in API costs.

Scoring framework:
- Define your cost budget per request: "We can afford $0.01 per spend classification"
- Measure actual cost
- Score as:
  - 0: Costs more than 2x budget
  - 1: Costs 1-2x budget
  - 2: Costs 1-1.2x budget
  - 3: Costs at or below budget

**Example:** GPT-4 spend classification costs $0.025/request. Your budget is $0.01. That's a cost score of 0. Switching to GPT-4o Mini brings it to $0.002. That's a 3.

#### 7. Consistency
**Definition:** Same input produces acceptably similar outputs across multiple runs.

This is about variance. If you ask the same question 10 times and get 10 different answers, that's a reliability problem even if each individual answer is correct.

Scoring:
- 0: Outputs wildly differ (e.g., "safe" vs. "unsafe")
- 1: Significant variation but same general direction (e.g., "high confidence" vs. "low confidence" on the same answer)
- 2: Minor variations in explanation, same core answer
- 3: Identical or effectively identical outputs

**Example:** Spend classification for the same PO. Run 1: "Travel > Hotels." Run 2: "Travel > Lodging." Run 3: "Travel > Hotels." Two out of three match—this might be a 2 (acceptable) if "Hotels" and "Lodging" are treated as equivalent in your taxonomy.

---

### Evaluation Methods Matrix

You have three primary mechanisms. Use the right one for the right dimension.

#### 1. Deterministic Checks
**What:** Exact match, regex patterns, schema validation, code execution.

**When to use:**
- Correctness where the answer is unambiguous (is this output valid JSON? Does it match the schema?)
- Safety checks (does output contain PII like SSN patterns?)
- Latency measurements (does response come back in <500ms?)
- Cost tracking (sum the tokens used)

**Advantages:** Fast, no hallucinations, reproducible, zero variance.

**Disadvantages:** Can't handle nuance. "Hotels" vs. "Lodging" as spend categories won't match with regex.

**Example rubric for spend classification:**
```
Deterministic Spend Category Check:
- Extract predicted category from output
- Check if predicted category is in valid taxonomy (UNSPSC or custom)
  → If yes: pass
  → If no: fail
- Compare exact match to ground truth
  → If exact match: +1 point
  → If parent category matches (L1): +0.5 points
  → If no match: 0 points
- Measure latency
  → If <500ms: pass
  → If 500-2000ms: pass but flag
  → If >2000ms: fail
```

#### 2. LLM-as-Judge
**What:** Use a separate model (usually stronger/more expensive) to score outputs from your main model.

**When to use:**
- Correctness with nuance (is this explanation good? Does this capture the intent?)
- Relevance (does this answer what was asked?)
- Faithfulness (is this grounded or hallucinated?)
- When deterministic checks can't capture the dimension

**Advantages:** Can handle subtlety, scales to complex outputs, fast iteration (no human in loop).

**Disadvantages:** The judge model can have its own biases. Known issues:

**Known LLM-as-Judge Biases:**

1. **Position Bias:** Judge favors output A simply because it appeared first.
   - Mitigation: Randomize order of outputs being compared. Use A vs. B and B vs. A in separate runs.

2. **Verbosity Bias:** Judge assumes longer outputs are more thorough, even if they contain fluff.
   - Mitigation: Normalize output length in the rubric. Explicitly instruct: "Length does not indicate quality."

3. **Self-Preference Bias:** If you use GPT-4 to judge GPT-4 outputs, it shows preference for its own outputs.
   - Mitigation: Use a different model as judge. GPT-4o judges GPT-4 outputs. Or use Claude 3 to judge both.

4. **Instruction Following:** The judge follows your rubric instructions to the degree you've specified them. Vague rubrics → inconsistent scoring.
   - Mitigation: Hyperspecific rubrics with examples (see below).

**LLM-as-Judge Rubric Template:**

```
You are an expert procurement analyst evaluating spend category classification.

TASK: A system classified a purchase transaction into a spend category. 
You must score this classification on correctness (0-5 scale).

GROUND TRUTH CATEGORY: {actual_category}
PREDICTED CATEGORY: {predicted_category}
TRANSACTION DETAILS: {transaction_text}

SCORING RUBRIC:

5 = Exact match. Predicted category matches ground truth exactly.
    Example: Predicted "Office Supplies > Paper Products" for a ream of copy paper order.
    Ground Truth: "Office Supplies > Paper Products"
    Score: 5

4 = Right L2 (sub-category). Same parent category, matching sub-category but minor taxonomy difference.
    Example: Predicted "Office Supplies > Stationery" (close to Paper Products)
    Ground Truth: "Office Supplies > Paper Products"
    Score: 4

3 = Right L1 (parent category) only. Correct high-level category, wrong subcategory.
    Example: Predicted "Office Supplies > Furniture" 
    Ground Truth: "Office Supplies > Paper Products"
    Score: 3

2 = Reasonable but wrong. Prediction is logical given transaction details but not the correct category.
    Example: Predicted "Travel > Hotels" for an office chair order (wrong, but at least attempted a category)
    Ground Truth: "Office Supplies > Furniture"
    Score: 2

1 = Misses intent entirely. Prediction shows the system didn't understand the transaction type.
    Example: Predicted "IT > Software" for a paper supply order
    Ground Truth: "Office Supplies > Paper Products"
    Score: 1

0 = Nonsensical or off-topic. The prediction isn't even a real category or completely unrelated.
    Example: Predicted "xyz123" or "purple"
    Ground Truth: "Office Supplies > Paper Products"
    Score: 0

INSTRUCTIONS:
- Do not consider frequency. A rare but correct classification should score the same as a common one.
- Do not reward verbosity. Judge only the category selection, not explanation length.
- If the transaction is genuinely ambiguous, note it but still assign the closest score.
- Output only the numeric score (0-5) followed by a one-line explanation.

SCORE:
```

#### 3. Human Evaluation
**What:** Expert annotators (procurement analysts, compliance officers, domain experts) review outputs and score them.

**When to use:**
- Gold-standard ground truth (seed your eval dataset)
- Ambiguous cases where even LLM-as-judge varies
- Safety-critical decisions (compliance, PII, legal risk)
- Monthly calibration (are my automated evals still tracking reality?)

**Workflow:**
1. **Define the task:** "You are evaluating spend category classifications. Score each as 0-5 using the rubric above."
2. **Select annotators:** 2-3 domain experts minimum (increases inter-annotator agreement).
3. **Provide clear rubric:** Use the same rubric as your LLM-as-Judge for consistency.
4. **Calibration round:** Have all annotators score the same 10 examples, discuss disagreements, align on interpretation.
5. **Production annotation:** Each expert scores independently, then you aggregate (majority vote for binary, average for scaled).
6. **Measure inter-annotator agreement:** Use Krippendorff's alpha (α) or Cohen's kappa:
   - α > 0.8: Excellent agreement
   - α 0.67-0.8: Good agreement
   - α < 0.67: Need rubric refinement
7. **Review disagreements:** When annotators diverge, update the rubric with a new example.

**Advantages:** Ground truth you can trust. Catches bias in automated methods. Builds domain intuition.

**Disadvantages:** Slow (1-2 weeks to label 100 items), expensive (analyst time), doesn't scale to every eval.

**Decision Matrix: When to Use Which Method**

| Dimension | Best Method | Why | Fallback |
|-----------|------------|-----|----------|
| Correctness (binary) | Deterministic | Unambiguous, fast | LLM-as-Judge if ambiguity exists |
| Correctness (graduated) | LLM-as-Judge | Captures nuance | Human eval for gold standard |
| Faithfulness | LLM-as-Judge | Hallucination is nuanced | Deterministic regex for PII |
| Relevance | LLM-as-Judge | Requires understanding intent | Human eval for calibration |
| Safety (PII) | Deterministic | Regex + entity extraction | LLM-as-Judge for context |
| Safety (compliance) | Human eval | Too risky to automate | LLM-as-Judge + human review |
| Latency | Deterministic | Measure wall-clock time | Infrastructure monitoring |
| Cost | Deterministic | Sum tokens + API pricing | Billing system integration |
| Consistency | Deterministic | Run 5 times, measure variance | LLM-as-Judge on outputs |

---

### Eval Dataset Design

Your eval is only as good as your dataset. A 20-item dataset where everything is a "happy path" won't catch regressions.

#### Size Guidelines

**Start small, grow systematically:**
- **Phase 1 (MVP):** 20-30 items. Covers main use cases. Good enough for first model selection.
- **Phase 2 (Baseline):** 50-100 items. Add edge cases, failure modes, adversarial examples. Use for regression testing.
- **Phase 3 (Production):** 200-500 items. Stratified by category/feature. Enough to detect 5-10% regressions with statistical confidence.
- **Phase 4 (Mature):** 1000+ items. Continuous growth from production logs. Detects <1% regressions.

**Rule of thumb:** For a dimension you care about, you need at least 30 examples to measure it reliably.

#### Coverage Strategy

**Happy path (40%):** Normal transactions, clear categories, no ambiguity.
- Example: "Dell monitor, office supplies"
- Example: "Airline ticket, travel"

**Edge cases (30%):** Borderline categorizations, multi-line items, foreign currency, date corner cases.
- Example: "Monitor + keyboard combo. Which category?" (Could be IT Equipment, Office Supplies, or bundled as one)
- Example: "PO in CNY (Chinese Yuan). System trained on USD." (Does it handle currency conversion?)
- Example: "IT contractor invoice with multiple service lines." (Is this IT > Contracts or IT > Services or split?)

**Adversarial (20%):** Deliberately tricky cases to catch system weaknesses.
- Example: "Invoice for 'consulting' with no company name." (Could be management consulting, IT consulting, HR consulting)
- Example: "Item description: 'miscellaneous.'" (Maximum ambiguity)
- Example: "Misspelled vendor, mangled PO number." (Does fuzzy matching work?)

**Failure modes (10%):** Cases where the system previously failed. Production bugs surfaced by customers.
- Example: "Last month, system misclassified 'Zoom license' as Office Supplies instead of IT > Software"
- Example: "Contract showed 5% price reduction month-over-month. System classified as 'Discount' category that doesn't exist in hierarchy."

#### Data Sources

1. **Production traces:** Sample actual transactions from your system logs. Most representative.
   - Sample 1-2% of daily traffic (100-500 items/week if you see 1M tx/week)
   - Label these with ground truth (human review or trusted vendor classification)

2. **Synthetic generation:** Use LLM to create realistic but diverse examples.
   - Prompt: "Generate 20 realistic procurement transactions for 'Office Supplies' category with variations in amount, vendor, description."
   - Then have a human validate each one

3. **Customer-reported issues:** Every support ticket mentioning "misclassification" or "wrong category" becomes an eval item.
   - These are gold—they represent real business impact

4. **Taxonomy deep dives:** For each category in your taxonomy, create 5-10 examples.
   - Forces coverage of all categories, catches underrepresented ones

#### Labeling Process

**LLM-bootstrap approach:**
1. Use your current best model to pre-label 100 items
2. Have 2-3 domain experts review 50 of them, correct errors, resolve ambiguities
3. Train a labeling guide from their corrections
4. Have the domain experts label the remaining 50 items using the guide
5. Measure inter-annotator agreement (target: α > 0.75)
6. All 100 items now have human-validated ground truth

This is faster than pure human labeling but more rigorous than LLM-only.

#### Version Control for Datasets

Store datasets in git. Example structure:
```
eval_datasets/
├── spend_classification_v1.0.jsonl
│   └── 100 items, basic categories
├── spend_classification_v1.1.jsonl
│   └── 150 items, added edge cases
├── spend_classification_v2.0.jsonl
│   └── 250 items, split happy/edge/adversarial with stratification
├── CHANGELOG.md
│   └── "v2.0: Added 50 adversarial items based on Q1 production failures"
└── labeling_guide.md
    └── Rubric, inter-annotator agreement scores, known ambiguities
```

**Why version control matters:** When you upgrade models and see accuracy drop 3%, you need to know whether it's model degradation or dataset shift. Versioned datasets let you compare like-for-like.

---

### Regression Testing Across Model Upgrades

This is where evaluation becomes operational. Your eval framework must answer: "If I upgrade from GPT-4 to GPT-4o, what breaks, what improves, and is it worth it?"

#### Golden Dataset Maintenance

Your golden dataset is the set of eval items you use consistently to measure progress. This should be stable (changes rarely) but evolve monthly.

**Maintenance cadence:**
- **Weekly:** Run evals on golden dataset. Track metrics (accuracy, latency, cost). Plot trends.
- **Monthly:** Review production failures from the past month. Add 5-10 new items to golden dataset representing those failures. This dataset now has ~110 items (100 original + 10 monthly).
- **Quarterly:** Audit golden dataset for coverage. Are there underrepresented categories? Add them. Remove obvious duplicates.
- **Annually:** Refresh labeling. Get domain experts to re-label 30% of golden dataset to ensure labels haven't drifted with changed business context.

#### Baseline Establishment

Before you upgrade a model, measure the current model's performance on golden dataset. This is your baseline.

**For spend classification (100-item golden dataset):**

| Dimension | Metric | Baseline |
|-----------|--------|----------|
| Correctness | Accuracy (exact match) | 89% |
| Correctness | Accuracy (right L1) | 95% |
| Latency | p50 | 150ms |
| Latency | p95 | 420ms |
| Latency | p99 | 890ms |
| Cost | $/request | $0.012 |
| Consistency | % runs with identical output | 98.5% |
| Safety | % with PII detected | 0% |

This becomes your comparison point.

#### Threshold Setting (Absolute vs. Relative)

**Absolute thresholds:** "Accuracy must be >= 88%"
- Use when you have external SLA or business requirement
- Example: "We promised customers 90% accuracy in the contract"

**Relative thresholds:** "Accuracy must not drop >2% from baseline"
- Use when you're optimizing cost/latency and acceptable trade-offs exist
- Example: "New model costs 10x less but accuracy drops 1%—that's fine"

**Recommended thresholds for spend classification:**

| Dimension | Threshold Type | Threshold |
|-----------|----------------|-----------|
| Correctness | Absolute | >= 88% (exact match) |
| Correctness | Relative | Don't drop >3% |
| Latency p95 | Absolute | <= 500ms |
| Cost | Relative | Don't increase >20% |
| Safety | Absolute | 0% PII leakage |

#### CI/CD Integration Pattern

**Your eval should run in your deployment pipeline:**

```
commit → build → unit tests → eval tests → integration tests → deploy

Eval tests:
1. Run golden dataset on new model
2. Compare to baseline
3. If any absolute threshold violated: FAIL
4. If any relative threshold violated by >threshold: WARN
5. Block deployment if FAIL, allow with override if WARN
```

**Actual GitHub Actions workflow example:**

```yaml
name: AI Model Eval

on: [pull_request]

jobs:
  evaluate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Run eval on golden dataset
        run: |
          python eval/run_eval.py \
            --model-id ${{ github.event.pull_request.head.sha }} \
            --dataset eval_datasets/spend_classification_v2.0.jsonl \
            --output eval_results.json
      
      - name: Compare to baseline
        run: |
          python eval/compare_baseline.py \
            --current eval_results.json \
            --baseline eval_baselines/baseline_v2.0.json \
            --thresholds eval/thresholds.yaml \
            --output comparison.json
      
      - name: Report results
        if: always()
        run: |
          python eval/report.py comparison.json \
            --format github-comment > comment.txt
          
      - name: Add comment to PR
        uses: actions/github-script@v6
        with:
          script: |
            const fs = require('fs');
            const comment = fs.readFileSync('comment.txt', 'utf8');
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: comment
            })
      
      - name: Fail if regressions
        run: python eval/fail_if_needed.py comparison.json
```

**What the PR comment looks like:**

```
AI Evaluation Results

Spend Classification Model

✅ Correctness (exact match): 89% (baseline 89%, change +0%)
✅ Correctness (L1 match): 95% (baseline 95%, change +0%)
✅ Latency p95: 418ms (baseline 420ms, change -2ms)
⚠️  Latency p99: 2100ms (baseline 890ms, change +1210ms)
✅ Cost: $0.011/req (baseline $0.012, change -8%)
✅ Safety: 0% PII (baseline 0%, change +0%)

THRESHOLDS:
- Correctness threshold: >= 88% ✅
- Latency p95 threshold: <= 500ms ✅
- Latency p99 threshold: <= 2000ms ❌ (exceeded by 100ms)

DECISION: ⚠️ WARN - Latency p99 regression detected. Review required.
Override with: --skip-eval-thresholds
```

#### Model Upgrade Playbook

**When you want to upgrade from Model A to Model B:**

**Stage 1: Offline Evaluation (1-2 days)**
- Run Model B on golden dataset
- Compare all dimensions to baseline
- Document changes (what improved, what regressed, by how much)
- Run on 100 random production transactions for sanity check
- Decision: "Does this look promising?" or "Does this look risky?"

**Example: GPT-4 → GPT-4o**
- Correctness: 89% → 91% (+2%) ✅
- Latency p95: 420ms → 280ms (-33%) ✅
- Cost: $0.012 → $0.003 (-75%) ✅
- Decision: Looks good. Proceed.

**Stage 2: Shadow Deploy (3-7 days)**
- Deploy Model B alongside Model A in production
- Model B processes every transaction but responses aren't shown to users
- Log outputs from both models
- Compare on same production data
- Can you detect any safety issues or PII problems?

**Example: 50,000 production transactions in shadow mode**
- Correctness divergence: 2.1% of transactions scored differently
- Safety: 0 PII issues in either model
- Latency: Both models sustain p95 < 500ms under load
- Decision: Proceed to canary.

**Stage 3: Canary Deploy (7-14 days)**
- Route 5-10% of real traffic to Model B
- 90% still uses Model A
- Monitor for user complaints, support tickets
- Measure actual production metrics (user satisfaction, downstream accuracy if B feeds another system)
- If no issues, increase to 25%

**Example: Spend classification canary**
- 5% of users see Model B classifications
- Day 3: 2 support tickets about misclassification vs Model A
- Investigate: Both were edge cases (multi-line items). Model B got 1 right, Model A got 0 right. User preference for Model B.
- Day 7: No further issues
- Proceed to 25%.

**Stage 4: Full Rollout**
- Route 100% of traffic to Model B
- Keep Model A running in shadow for 1 week (rollback path)
- After 1 week with no issues, decommission Model A

**Total time: 2-4 weeks. Risk: Minimal. Confidence: High.**

---

## Application

**Step-by-step process for building an eval framework:**

### Step 1: Define Success Metrics
What does success look like for your feature? List all dimensions that matter.
- For spend classification: Correctness (#1), Cost (#2), Latency (#3), Safety (#4)
- For contract summarization: Faithfulness (#1), Completeness (#2), Cost (#3), Safety (#4)
- For fraud detection: Safety (#1), Correctness (#2), Latency (#3)

**Deliverable:** 1-page document listing dimensions, their relative importance (rank 1-5), and business impact of failure.

### Step 2: Choose Evaluation Methods
For each dimension, decide: Deterministic, LLM-as-Judge, or Human.

**Deliverable:** Decision matrix showing method per dimension + rationale.

### Step 3: Design Scoring Rubrics
For each dimension, write the detailed rubric (not just the name).

**Deliverable:** Rubrics document with examples for each dimension.

### Step 4: Build Eval Dataset
Collect 20-30 items covering happy path, edge cases, adversarial, failure modes.

**Deliverable:** `eval_data_v1.0.jsonl` with ground truth labels and inter-annotator agreement scores.

### Step 5: Implement Eval Code
Write Python code that:
1. Loads the eval dataset
2. Runs your model on each item
3. Scores each output using deterministic checks, LLM-as-Judge, or human labels
4. Aggregates scores per dimension
5. Outputs metrics + detailed per-item analysis

**Deliverable:** `eval/run_eval.py` that takes `--model-id`, `--dataset`, and outputs `eval_results.json`.

### Step 6: Establish Baselines
Run evals on your current best model. Document all metrics. This is your baseline.

**Deliverable:** `eval_baselines/baseline_v1.0.json` with all metrics + timestamp.

### Step 7: Set Thresholds
For each dimension, decide absolute or relative thresholds.

**Deliverable:** `eval/thresholds.yaml` with all thresholds + rationale.

### Step 8: Integrate into CI/CD
Add eval step to your deployment pipeline. On every PR, run evals automatically.

**Deliverable:** GitHub Actions workflow that blocks deployment if thresholds violated.

### Step 9: Build Dashboard
Track metrics over time. Weekly updates. Monthly deep-dives.

**Deliverable:** Dashboard (Grafana, Looker, or even Google Sheets) showing accuracy, latency, cost trends + alerts.

### Step 10: Establish Calibration Cadence
Monthly: Review production failures, add to eval dataset. Quarterly: Audit coverage. Annually: Re-label for drift.

**Deliverable:** Calendar event + process doc ("How we evolve the eval dataset").

---

## Worked Example: Spend Classification Eval Framework

You're building an AI-powered spend classification feature for a procurement analytics platform. Here's how you'd build the complete eval framework.

### What the Feature Does

Users upload purchase transactions (POs, invoices, credit card statements). The system classifies each line item into the UNSPSC (United Nations Standard Products and Services Code) taxonomy plus custom company categories. Example:

- Input: "Dell monitor $299, qty 1, vendor TechCorp"
- Output: "IT Equipment > Computer Components > Monitors" (UNSPSC 30191000)

This feeds into spend analytics: "How much did we spend on IT Equipment last quarter?"

### Eval Dimensions Chosen (and Why)

| Dimension | Chosen? | Why |
|-----------|---------|-----|
| Correctness | YES | Primary metric. Business can't analyze spend if categories are wrong. |
| Faithfulness | YES | Must ground category in transaction details, not hallucinate. |
| Relevance | NO | Transaction + category is unambiguous. No relevance dimension. |
| Safety | YES | Must not leak vendor details. Must not expose PII. |
| Latency | YES | Feature used in dashboard. p95 SLA is 500ms. |
| Cost | YES | Running 2M+ classifications/month. Cost directly impacts unit economics. |
| Consistency | YES | Users won't trust feature if same PO categorizes differently each time. |

### Eval Dataset: 200 Transactions

**Source breakdown:**
- 100 items from production logs (sampled randomly from last month)
- 40 items synthetically generated (cover underrepresented categories)
- 30 items from customer-reported misclassifications (failures we know happened)
- 30 items adversarial (edge cases)

**Coverage:**
- 15 major UNSPSC categories represented
- Size range: $10 to $500K
- Vendors: 45 unique vendors (some repeat)
- Currencies: 80% USD, 15% EUR, 5% other (GBP, CNY, INR)
- Date range: Q1-Q3 2025 + Q1 2026 (seasonal variation)

**Stratification:**
- Happy path (100 items): Clear descriptions, single-line, no ambiguity
- Edge cases (60 items): Multi-line items, bundles, ambiguous descriptions
- Adversarial (30 items): Misspelled vendors, missing PO numbers, currency mismatches
- Failure modes (10 items): Exact transactions that the system failed on in the past

**Ground truth labeling:**
- Procurement team (2 analysts) + finance team (1 manager) labeled all 200 items
- Used 5-point correctness scale (5 = perfect, 0 = nonsensical)
- Inter-annotator agreement: Cohen's kappa = 0.82 (good agreement)
- Disagreements (18 items) resolved through discussion. Final labels agreed unanimously.

### Deterministic Checks

```python
def check_valid_category(output: str, taxonomy: dict) -> bool:
    """Check if predicted category exists in valid taxonomy."""
    return output in taxonomy.keys()

def check_hierarchy_match(predicted: str, ground_truth: str, taxonomy: dict) -> float:
    """Score based on matching hierarchy levels."""
    pred_levels = predicted.split(" > ")
    truth_levels = ground_truth.split(" > ")
    
    matches = 0
    for i in range(min(len(pred_levels), len(truth_levels))):
        if pred_levels[i] == truth_levels[i]:
            matches += 1
        else:
            break
    
    if matches == len(truth_levels):
        return 5.0  # Exact match
    elif matches == 1 and len(truth_levels) > 1:
        return 3.0  # Right L1 only
    elif matches == 2 and len(truth_levels) > 2:
        return 4.0  # Right L1 and L2
    else:
        return 0.0

def check_latency(response_time_ms: float, sla_p50: float, sla_p95: float, sla_p99: float) -> dict:
    """Score based on SLA compliance."""
    return {
        "response_time_ms": response_time_ms,
        "within_p50": response_time_ms <= sla_p50,
        "within_p95": response_time_ms <= sla_p95,
        "within_p99": response_time_ms <= sla_p99,
        "score": 3 if response_time_ms <= sla_p50 else 
                 2 if response_time_ms <= sla_p95 else
                 1 if response_time_ms <= sla_p99 else 0
    }

def check_pii_leakage(output: str) -> dict:
    """Detect PII in output."""
    pii_patterns = {
        "email": r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b',
        "phone": r'\b\d{3}[-.]?\d{3}[-.]?\d{4}\b',
        "ssn": r'\b\d{3}-\d{2}-\d{4}\b',
        "credit_card": r'\b\d{4}[\s\-]?\d{4}[\s\-]?\d{4}[\s\-]?\d{4}\b'
    }
    
    found = {}
    for pii_type, pattern in pii_patterns.items():
        matches = re.findall(pattern, output)
        if matches:
            found[pii_type] = len(matches)
    
    return {
        "pii_detected": bool(found),
        "pii_types": found,
        "safe": 1 if not found else 0
    }

def check_consistency(outputs: list, tolerance: float = 0.8) -> dict:
    """Run same input 5 times, measure consistency."""
    # outputs = list of 5 outputs for same input
    unique_outputs = set(outputs)
    consistency_score = (1 - (len(unique_outputs) - 1) / 4) if len(outputs) == 5 else 1.0
    
    return {
        "runs": len(outputs),
        "unique_outputs": len(unique_outputs),
        "consistency_score": consistency_score,  # 1.0 = identical, 0.0 = all different
        "score": 3 if consistency_score >= 0.9 else 
                 2 if consistency_score >= 0.7 else
                 1 if consistency_score >= 0.5 else 0
    }
```

### LLM-as-Judge Rubric

```
You are an expert procurement analyst evaluating spend category classification.

TASK: A system classified a purchase transaction into a spend category. 
You must score this classification on correctness using the rubric below.

GROUND TRUTH CATEGORY: {ground_truth_category}
PREDICTED CATEGORY: {predicted_category}
TRANSACTION DETAILS:
- Description: {description}
- Amount: {amount} {currency}
- Vendor: {vendor}
- Date: {date}
- PO Number: {po_number}

UNSPSC TAXONOMY (Abbreviated):
30: IT & Telecom
  30.10: Computer Hardware
    30.10.10: Monitors
    30.10.15: Keyboards & Mice
    30.10.20: CPUs & Components
  30.20: Software
    30.20.10: Operating Systems
    30.20.15: Business Applications
45: Office Supplies
  45.10: Paper Products
    45.10.10: Copy Paper
    45.10.15: Printer Ink
  45.20: Furniture
    45.20.10: Desks
    45.20.15: Chairs
80: Travel & Lodging
  80.10: Flights
  80.20: Hotels
    80.20.10: Business Travel

SCORING RUBRIC:

5 = PERFECT MATCH
Predicted category exactly matches ground truth. Demonstrates clear understanding.
Example: 
- Transaction: "HP LaserJet printer, toner cartridges $350"
- Ground truth: "45.10.15 Printer Ink"
- Prediction: "45.10.15 Printer Ink"
- Score: 5

4 = MINOR VARIATION
Predicted matches ground truth at L1 and L2, but minor L3 difference or equivalent category.
Example:
- Transaction: "Dell printer cartridge $80"
- Ground truth: "45.10.15 Printer Ink"
- Prediction: "45.10.15 Printer Supplies" (if your taxonomy treats these as equivalent)
- Score: 4

3 = CORRECT PARENT CATEGORY
Predicted matches ground truth at L1 (major category) and L2, but wrong at L3.
Example:
- Transaction: "Dell monitor $299"
- Ground truth: "30.10.10 Monitors"
- Prediction: "30.10.20 CPUs & Components" (right IT > Computer Hardware, wrong subcategory)
- Score: 3

2 = PLAUSIBLE BUT WRONG
Prediction is a reasonable category but not correct. Shows some domain understanding.
Example:
- Transaction: "Samsung monitor $299"
- Ground truth: "30.10.10 Monitors"
- Prediction: "30.20.10 Operating Systems" (wrong, but at least IT category)
- Score: 2

1 = MAJOR CATEGORY WRONG
Predicted is in wrong high-level category. Misses the mark significantly.
Example:
- Transaction: "Stapler $20"
- Ground truth: "45.10.10 Copy Paper" (assume it was a bulk office supply order)
- Prediction: "80.10 Flights" (completely wrong category)
- Score: 1

0 = NONSENSICAL
Prediction is not a valid category, is gibberish, or completely off-topic.
Example:
- Prediction: "xyz123" or "purple" or "hamburgers"
- Score: 0

SPECIAL CASES:

MULTI-LINE ITEMS:
If the transaction contains multiple items (e.g., "Monitor $300 + Keyboard $100"), 
the system should categorize the dominant item. If it splits them, great. 
If it picks the wrong one, score accordingly.

AMBIGUOUS TRANSACTIONS:
Some transactions are inherently ambiguous (e.g., "consulting services" could be 
IT consulting or management consulting). If the prediction is in the right category 
group but wrong subtype, score as 3. If it's a different major category, score as 2.
Note the ambiguity in your comment.

CURRENCY & VENDOR ISSUES:
Never penalize the system for currency or vendor variations (these are just metadata). 
Score only the category prediction. If vendor name is misspelled or PO is missing, 
that doesn't affect the category score.

INSTRUCTIONS:
1. Read the transaction details carefully.
2. Identify what the transaction actually is (monitor, software license, hotel booking, etc.)
3. Compare predicted category to ground truth category
4. Assign score using rubric above
5. Output ONLY: "{score}\n{one_line_justification}"

Example outputs:
5
Perfect match - Dell monitor correctly classified as IT > Computer Hardware > Monitors

3
Right parent category (Office Supplies) but wrong subcategory. Monitor categorized as Paper Products instead of being recognized as IT equipment.

0
Invalid category - prediction "xyz123" is not in the UNSPSC taxonomy.

SCORE:
```

Run this rubric on all 200 eval items. Expected distribution:
- 5s: ~45% (perfect match on common items)
- 4s: ~20% (minor variations)
- 3s: ~20% (right parent, wrong child)
- 2s: ~10% (plausible but wrong)
- 1s: ~4% (major category wrong)
- 0s: ~1% (nonsensical)

**Average: 4.2/5 = 84% accuracy (weighted)**

### Human Eval Workflow

Every month, pull 50 random items from the eval dataset. Have 2-3 procurement analysts score them independently using the same rubric. Compare to LLM-as-Judge scores.

**Monthly Calibration Meeting (1 hour):**
1. Analysts score 50 items (takes ~2 hours, done async)
2. Analyst 1 and Analyst 2 compare scores
3. Compute inter-annotator agreement (target: > 0.75)
4. Review 5-10 items with major disagreements (score gap > 2)
5. Update labeling guide with new examples of ambiguity
6. Compare human scores to LLM-as-Judge scores (did the judge track human judgment?)

**Example output from January calibration:**
- Analyst 1 vs Analyst 2 agreement: 0.79 (good)
- Analyst 1 vs LLM-as-Judge agreement: 0.73 (acceptable)
- Analyst 2 vs LLM-as-Judge agreement: 0.71 (acceptable)
- Items with human-LLM divergence: 6 items
  - 3 of them: LLM was actually right, human disagreed due to outdated taxonomy knowledge
  - 2 of them: Human was right, LLM had position bias (scored first option higher)
  - 1 of them: Genuinely ambiguous, split the difference

**Action:** Update LLM-as-Judge rubric to clarify these edge cases.

### Regression Testing: Model Upgrade Case Study

**Current state (Jan 2026):**
- Model: GPT-4
- Accuracy (exact match): 89%
- Latency p95: 420ms
- Cost: $0.012/request
- Consistency: 98.5%
- Safety: 0% PII

**Opportunity:** GPT-4o is 10x cheaper and faster. Test it.

**Stage 1: Offline Evaluation (2 days)**

Run both models on golden dataset:

| Metric | GPT-4 (Baseline) | GPT-4o (New) | Change | Status |
|--------|-----------------|-------------|--------|--------|
| Accuracy (exact) | 89% | 87% | -2% | ⚠️ Regress |
| Accuracy (L1+) | 95% | 96% | +1% | ✅ Improve |
| Latency p50 | 150ms | 80ms | -47% | ✅ Improve |
| Latency p95 | 420ms | 220ms | -48% | ✅ Improve |
| Cost | $0.012 | $0.0015 | -87.5% | ✅ Improve |
| Consistency | 98.5% | 98.1% | -0.4% | ✅ Same |
| Safety | 0% | 0% | 0% | ✅ Same |

**Analysis:**
- Exact accuracy drops 2% (89% → 87%). Is this acceptable? 
  - 2% of 200 items = 4 items changed from right to wrong
  - Review those 4 items: 3 are edge cases, 1 is a real regression
  - Net: -1% business impact (on total 200-item dataset)
- Accuracy at L1+ (right parent category) actually improves 1%
- Cost drops 87.5%. Annual savings: $2M per million classifications.
- Latency more than halves

**Decision:** The 1% net regression is acceptable given 87.5% cost reduction and 2x latency improvement. Proceed to Stage 2.

**Stage 2: Shadow Deploy (5 days)**

Deploy GPT-4o alongside GPT-4 for production transactions:

```
50,000 production transactions, both models score them:

Metric                      GPT-4    GPT-4o    Divergence
Exact match accuracy        89.2%    87.1%     2.1% diverge
Right parent category       94.8%    96.1%     1.3% diverge
Avg response time           387ms    198ms     (expected)
Cost per request            $0.0122  $0.00146  (expected)
Safety (PII detected)       0        0         0% diverge
Consistency (5 runs)        98.3%    97.9%     0.4% diverge

Items with high divergence (score diff > 2):
- 234 items where GPT-4o scored 5 but GPT-4 scored 3 (GPT-4o improved)
- 156 items where GPT-4 scored 5 but GPT-4o scored 3 (GPT-4 was better)
- 89 items where GPT-4o scored 0 (nonsensical)
- 12 items where GPT-4o was flagged as unsafe (PII) that GPT-4 missed

Action items:
- Review the 89 "nonsensical" outputs from GPT-4o. Pattern: multi-line items with >3 components
- Review the 12 unsafe outputs. Pattern: GPT-4o includes vendor email addresses GPT-4 omits
- Update prompt to say "never include email addresses in output"
```

After fixing the prompt, re-run shadow deploy on 50K items:

```
Improvements:
- Nonsensical outputs: 89 → 3 (97% reduction)
- Unsafe outputs: 12 → 0 (100% reduction)
- Exact match accuracy: 87.1% → 88.2% (better)
- Right parent accuracy: 96.1% → 96.5% (better)

New decision: Proceed to Stage 3 (Canary).
```

**Stage 3: Canary Deploy (10 days)**

Roll out GPT-4o to 5% of production traffic:

```
Day 1: 5% of users see GPT-4o
- Monitor support tickets related to categorization
- Compare user satisfaction (thumbs up/down on classifications)

Day 3: 1 support ticket
- User: "Why did my office supplies get classified as IT Equipment?"
- Investigation: Multi-line PO (monitor + paper). GPT-4o classified by dominant item (monitor).
  GPT-4 had also misclassified the same PO. User is actually okay with GPT-4o.
- Not a regression, just noise.

Day 7: Zero additional tickets
- Exact match accuracy on canary cohort: 87.9% (on 50K items)
- Right parent accuracy: 96.2%
- Latency p95: 215ms (vs 420ms GPT-4, target was 500ms)
- User satisfaction: 92% thumbs up (same as GPT-4)

Decision: Increase to 25% traffic.

Day 10-14: Canary at 25%
- No issues
- Metrics stable
- User satisfaction: 92% thumbs up
- Cost per transaction: $0.00146 (confirmed)

Decision: Proceed to full rollout.
```

**Stage 4: Full Rollout**

```
Day 1: 100% of traffic on GPT-4o
- Keep GPT-4 running in shadow (rollback path)
- Monitor dashboards

Day 7: GPT-4 still running in shadow
- No user complaints
- No downstream issues (categorizations feed other analytics)
- Metrics stable
- Cost savings: $154K for the week (vs GPT-4)

Decision: Decommission GPT-4. Full rollout complete.

Final metrics (post-rollout, 100K classifications):
- Exact match accuracy: 88.1% (vs 89% baseline, -0.9% acceptable)
- Right parent accuracy: 96.2% (vs 95% baseline, +1.2%)
- Latency p95: 218ms (vs 420ms baseline, -48%)
- Cost: $0.00146/req (vs $0.012, -87.8%)
- Annual savings: ~$1.9M for 1.5B annual classifications

Net: -0.9% accuracy for -87.8% cost = worth it.
```

### Dashboard: Metrics Tracked Weekly

```
Spend Classification Dashboard

Weekly Metrics (Latest week):
┌─────────────────────────────────────────────────┐
│ Correctness (Exact Match)            87.8%      │
│ 4-week trend: 88.1% → 88.0% → 87.9% → 87.8%   │
│ Threshold: >= 85%. Status: ✅ PASS              │
├─────────────────────────────────────────────────┤
│ Correctness (Right L1+)               96.1%     │
│ 4-week trend: 96.2% → 96.2% → 96.1% → 96.1%  │
│ Threshold: >= 95%. Status: ✅ PASS              │
├─────────────────────────────────────────────────┤
│ Latency p95                           218ms     │
│ 4-week trend: 220ms → 219ms → 218ms → 218ms  │
│ Threshold: <= 500ms. Status: ✅ PASS            │
├─────────────────────────────────────────────────┤
│ Cost ($/req)                          $0.00146  │
│ 4-week trend: $0.00146 → $0.00146 → $0.00146  │
│ Threshold: <= $0.002. Status: ✅ PASS           │
├─────────────────────────────────────────────────┤
│ Safety (PII Leakage)                 0%         │
│ 4-week trend: 0% → 0% → 0% → 0%                │
│ Threshold: 0%. Status: ✅ PASS                  │
├─────────────────────────────────────────────────┤
│ Consistency                          97.8%      │
│ 4-week trend: 98.1% → 98.0% → 97.9% → 97.8%  │
│ Threshold: >= 97%. Status: ✅ PASS              │
└─────────────────────────────────────────────────┘

Volume: 1.2M classifications this week

Top categories by volume:
1. Office Supplies (320K)     - 89% accuracy
2. IT Equipment (280K)        - 85% accuracy ⚠️ below target
3. Travel (150K)              - 92% accuracy
4. Consulting Services (120K) - 81% accuracy ⚠️ below target

Items with recurring issues:
- Multi-line POs: 2.3% lower accuracy. Investigate prompt clarity.
- "Consulting" descriptions: 19% lower accuracy. Need clearer taxonomy.

Action items:
- Update taxonomy for consulting services subcategories
- Add 20 multi-line items to eval dataset
- Re-run LLM-as-Judge on consulting category
```

---

## Common Pitfalls

### Pitfall 1: Evaluating Only Happy Path

**What Happens:** Your eval dataset is 100% clear transactions. When you ship, you hit edge cases (multi-line items, ambiguous descriptions, misspelled vendors, foreign currency) and the model fails 5x more often than your eval predicted.

**How to Avoid:**
- 40% happy path, 30% edge cases, 20% adversarial, 10% failure modes
- Before shipping, run your model on 1,000 production transactions and spot-check 50 random ones. Did the eval predict performance?
- If there's divergence, add those divergent cases to your eval dataset

### Pitfall 2: Using the Same Model as Judge and Generator

**What Happens:** You use GPT-4 to generate spend classifications, then use GPT-4 as the judge to score those classifications. The judge shows preference for outputs from its own reasoning style, inflating accuracy scores. You think you're at 92% accuracy when you're really at 87%.

**How to Avoid:**
- If your main model is GPT-4, use Claude 3.5 Sonnet or GPT-4o as judge
- Measure judge bias: Compare LLM-as-Judge scores to human scores monthly. If they diverge >5%, investigate
- Randomize output order in the judge prompt to avoid position bias

### Pitfall 3: Eval Dataset Doesn't Evolve with Production Data

**What Happens:** You build an eval dataset in Q1 2026 based on Q4 2025 data. By Q3 2026, spending patterns have changed (e.g., new vendor, new product category, economic shift affects expense mix). Your eval dataset is now stale. Model upgraded in Q3 shows "good" eval numbers but actually fails on new patterns.

**How to Avoid:**
- Monthly: Sample 10-20 items from production. Add to eval dataset.
- Monthly: Review support tickets mentioning misclassification. Add those cases to eval.
- Quarterly: Audit eval dataset for category coverage. Add underrepresented categories.
- Annually: Re-label 30% of eval dataset with human experts. Measure label drift.

### Pitfall 4: Over-Indexing on Accuracy, Ignoring Cost/Latency

**What Happens:** Your eval framework shows Model A is 2% more accurate than Model B. You ship Model A. But Model B is 5x cheaper and 3x faster. You spend $2M more per year for 2% accuracy improvement. Your business grinds to a halt waiting for responses.

**How to Avoid:**
- Define trade-off curves. "Is 2% accuracy worth 5x cost?" Measure willingness to trade.
- In eval results, always show a decision matrix:
  - Model A: 89% accuracy, $0.01/req, 300ms p95
  - Model B: 87% accuracy, $0.002/req, 100ms p95
  - Decision: "Which do we ship?"
- Let business stakeholders decide, don't optimize for accuracy alone

### Pitfall 5: Not Calibrating LLM Judge Against Human Judgment

**What Happens:** Your LLM judge gives a transaction a score of 5 (perfect) but your procurement analyst says it's a 2 (wrong). You ship based on eval numbers. In production, users complain the classifications are wrong.

**How to Avoid:**
- Monthly: Have humans score 50 items
- Compare LLM-as-Judge scores to human scores
- Compute inter-rater agreement (target: > 0.75)
- If LLM scores diverge from humans, update the rubric with new examples
- If divergence persists, use human scores for final decision instead of LLM scores

### Pitfall 6: Treating Eval as One-Time, Not Continuous

**What Happens:** You build an eval framework, ship the model, and never touch eval again. Production data drifts. Model degrades silently. Customers notice issues before you do.

**How to Avoid:**
- Weekly: Run evals on golden dataset. Track trends.
- Monthly: Evolve eval dataset (add production failures).
- Quarterly: Audit coverage and baseline thresholds.
- Annually: Re-calibrate human judgment.
- CI/CD: Eval runs automatically on every model update.

---

## References

**Frameworks & Theory:**
- "Towards Explainable Evaluation Metrics for Natural Language Generation" (Chen et al., 2023) — on designing eval metrics
- "Scaling Language Models: Challenges & Solutions" (OpenAI, 2024) — regression testing at scale
- "Evaluating AI Systems: A Practical Guide" (Anthropic, 2025) — comprehensive eval design

**Tools & Infrastructure:**
- MLflow: Tracking eval metrics across model versions
- Hugging Face Evaluate: Pre-built eval metrics library
- Weights & Biases: Dashboard for eval metrics over time
- GitHub Actions: CI/CD integration for eval automation

**Enterprise AI Operations:**
- "The ML Ops Handbook" (Databricks) — model evaluation in production
- "Responsible AI" (Microsoft) — safety eval frameworks
- "Model Card" (Google) — documenting model performance + limitations

**Case Studies:**
- Duolingo: How they eval language model translations (monthly human calibration)
- Figma: AI-assisted feature eval (accuracy + latency trade-offs)
- Stripe: Fraud detection eval (safety-critical evaluation, SLA enforcement)
