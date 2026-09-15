# Selecting Context-Appropriate Prompting Techniques for Given Test Tasks

**Syllabus Reference**
ISTQB GenAI — 2.2.5 Selecting Context-Appropriate Prompting Techniques for Software Testing

## Link to the Transfer Task File
https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/Chapter2/ISTQB-GenAI-2.2.5_Selecting_Context-Appropriate_Prompting_Techniques_for_Test_Tasks_20260807.md

---

# Outcome

## Task 1 – Price and Tax Calculation Test Cases

### Sample Prompt

```text
You are a software testing assistant. Use only the provided acceptance criteria and tax rules.
Example 1: EUR 100.00, tax rate 20% → expected total price EUR 120.00.
Example 2: USD 50.00, tax rate 10% → expected total price USD 55.00.
Following this pattern, create eight test cases covering different currencies and boundary values.
Return the results as valid JSON with the fields id, acceptance_criterion_id, currency, net_price, tax_rate, and expected_total.
Verify that all acceptance criteria are covered and that all calculations are rounded to two decimal places.
```

**Primary challenge:** High precision and machine-readable, structured results
**Chosen technique:** Few-shot prompting
**Model/settings & format:** Temperature 0.0, max_tokens 600, JSON
**Rationale:** Few-shot examples support the accurate and consistently structured generation of price and tax calculation test cases.
**Risk & Mitigation:** Risk – incorrect calculations or incorrectly applied tax rules → Mitigation – use only the provided acceptance criteria and tax rules and verify the calculations manually or with an independent calculator.

**Note:** Few-shot prompting is sufficient for this task. To further improve accuracy and manual verifiability, it could also be combined with prompt chaining, for example by generating and verifying the calculations step by step.

---

## Task 2 – Accessibility Exploratory Charters

### Sample Prompt

```text
You are a software testing assistant. Use only the provided WCAG criteria.
Example 1: ID: AC-01 | Page: Product details page | Focus: Color contrast | Mission: Check readability | Checks: Text and buttons | Expected behavior: The provided WCAG contrast criterion is met.
Example 2: ID: AC-02 | Page: Product details page | Focus: Keyboard accessibility | Mission: Check operability | Checks: Interactive elements and focus | Expected behavior: All interactive elements are reachable and the focus is visible.
Following this pattern, create one accessibility exploratory charter for each specified product page.
Return the result as valid JSON with the fields id, page, wcag_criterion, focus, mission, checks, and expected_behavior.
Verify that every product page and every provided WCAG criterion has been considered. Do not invent additional criteria.
```

**Primary challenge:** Repeatable results and a consistent structure
**Chosen technique:** Few-shot prompting
**Model/settings & format:** Temperature 0.0, max_tokens 600, JSON
**Rationale:** Few-shot examples enable the repeatable creation of accessibility charters in a consistent structure.
**Risk & Mitigation:** Risk – invented or incorrectly assigned WCAG criteria → Mitigation – use only the provided WCAG criteria and manually verify the assignments.

---

## Task 3 – User Review Analysis

### Prompt 1 – Partial Analysis

```text
You are a software testing assistant. Analyze only the provided subset of product reviews.
Identify positive, negative, and recurring topics.
For each topic, document the number of occurrences and the related review IDs.
Do not invent information. Repeat this analysis until all provided subsets have been processed.
```

### Prompt 2 – Overall Analysis

```text
Summarize only the results of the previous partial analyses.
Identify overarching trends and sort them by frequency.
Combine similar topics without losing important differences.
For each trend, reference the related review IDs.
```

### Prompt 3 – Output and Validation

```text
Return the final result as valid JSON with the fields product, trend, sentiment, frequency, and review_ids.
Verify that all provided reviews have been considered.
Also state the total number of reviews analyzed.
Mark trends without sufficient evidence as “insufficiently supported.”
```

**Primary challenge:** Processing a large context, summarization, and trend identification
**Chosen technique:** Prompt chaining
**Model/settings & format:** Temperature 0.0, max_tokens 1000, JSON
**Rationale:** Prompt chaining enables large numbers of reviews to be analyzed step by step and the intermediate results to be combined into overarching trends.
**Risk & Mitigation:** Risk – important information may be lost during summarization or trends may be invented → Mitigation – support every trend with review IDs and manually compare the intermediate and final results with the original reviews.

---

## Task 4 – Regression Test Matrix from User Stories

### Prompt 1 – Extract Acceptance Criteria

```text
You are a software testing assistant. Analyze only the provided user stories.
Extract all existing acceptance criteria for each user story without creating new criteria.
Include the related user story and acceptance criterion IDs.
Mark unclear or missing information as “Not clearly specified.”
```

### Prompt 2 – Map Acceptance Criteria to Regression Tests

```text
Use only the results from Prompt 1 and the provided list of regression tests.
Map each acceptance criterion to one or more suitable regression tests.
Document the regression test ID for each mapping.
If no suitable mapping is possible, mark it as a “Test gap.”
```

### Prompt 3 – Consistent Matrix and Validation

```text
Create the regression test matrix following these format examples:

| User Story | Acceptance Criterion | Regression Test | Coverage |
|---|---|---|---|
| US-EX-01 | AC-EX-01: A registered user can log in with valid credentials. | RT-EX-01: Login with valid credentials | Covered |
| US-EX-02 | AC-EX-02: The total price is displayed in the selected currency. | No mapping | Test gap |

The example rows are only formatting examples and must not be treated as source data. Verify that every acceptance criterion has been included and state the number of test gaps.
```

**Primary challenge:** Structured mapping and a consistent tabular output
**Chosen techniques:** Prompt chaining and few-shot prompting
**Model/settings & format:** Temperature 0.0, max_tokens 800, Markdown table
**Rationale:** Prompt chaining enables a step-by-step and verifiable mapping of acceptance criteria, while few-shot examples ensure a consistent structure for the regression test matrix.
**Risk & Mitigation:** Risk – invented acceptance criteria or incorrect mappings to regression tests → Mitigation – use only the provided sources and manually validate all mappings.

---

## Task 5 – Investigating an Intermittent CI Failure

### Prompt 1 – Extract Relevant Information

```text
You are a software testing assistant. Analyze only the provided flaky test logs and stack traces.
Extract error messages, timestamps, affected test steps, and recurring patterns.
Compare successful and failed test runs if both are available.
For each piece of information, document the related log or line reference and do not draw conclusions yet.
```

### Prompt 2 – Generate and Evaluate Failure Hypotheses

```text
Use only the results from Prompt 1 to generate possible failure hypotheses.
Rate each hypothesis as “high,” “medium,” or “low.”
Support each rating with specific log or line references.
Return the result as a Markdown table with the columns Hypothesis, Likelihood, Log Evidence, Rationale, and Validation Step.
```

### Prompt 3 – Propose Validation Steps

```text
Create one concrete validation step for each hypothesis.
Mark hypotheses without sufficient evidence as “insufficiently supported.”
Verify that every hypothesis refers to at least one piece of evidence from Prompt 1.
Do not present any hypothesis as a confirmed root cause before it has been validated.
```

**Primary challenge:** In-depth analysis and the generation and evaluation of possible failure hypotheses
**Chosen technique:** Prompt chaining
**Model/settings & format:** Temperature 0.2, max_tokens 1000, Markdown table
**Rationale:** Prompt chaining enables a step-by-step analysis of logs and stack traces as well as the transparent generation, evaluation, and validation of possible failure hypotheses.
**Risk & Mitigation:** Risk – unsupported or incorrect failure hypotheses → Mitigation – support every hypothesis with log evidence and validate it through targeted reproduction or manual analysis.

**Note:** The slightly increased temperature supports the generation of several possible failure hypotheses without causing excessive variation in the output.
