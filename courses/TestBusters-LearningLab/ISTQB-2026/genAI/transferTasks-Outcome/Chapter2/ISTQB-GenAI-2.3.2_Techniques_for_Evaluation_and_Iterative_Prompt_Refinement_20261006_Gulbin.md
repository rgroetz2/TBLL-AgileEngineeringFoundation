# Run a Structured 3-Cycle Prompt Refinement Loop

---

## Syllabus Reference
ISTQB GenAI – 2.3.2 Techniques for Evaluation and Iterative Prompt Refinement

---

### Link to the Transfer Task File
[Improve AI output quality through iterative evaluation and prompt refinement.](https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/Chapter2/ISTQB-GenAI-2.3.2_Techniques_for_Evaluation_and_Iterative_Prompt_Refinement_20260405.md)

---

# OUTCOME

## 1. Evaluation Framework

The [evaluation framework from G117](https://github.com/guelbin/RBI-AgileEngineeringFoundation/blob/feature_G117-GenAI-Metrics-for-Test-Artifacts/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks-Outcome/Chapter2/ISTQB-GenAI-2.3.1_Metrics_for_Evaluating_Generative_AI_Results_20260916_Guelbin.md) is used for all three iterations.

The five metrics **Correctness, Completeness, Clarity, Traceability, and Actionability**, as well as their **rating scales from 1 to 5**, remain unchanged. The detailed definitions and descriptions of the rating levels are documented in G117.

For each prompt version, the four generated negative login test cases are manually evaluated together as a single test artifact. Each evaluation includes a brief justification based on the respective output. The maximum total score is **25 points**.

The user story and the manual observations form the evaluation basis for all versions. These sources are added to the prompts step by step. Reviewing an AI-generated test case does not mean that this test case has already been executed.

## 2. Acceptance Threshold

The acceptance threshold defined in G117 is applied consistently:

- Total score of at least **18/25**
- No individual metric below **3/5**
- Correctness of at least **4/5**

## 3. Selected Test Artifacts

The four negative ToolShop login test cases generated with each of the three prompt versions are evaluated.

The objective remains the same throughout the refinement:

1. Incorrect password for a registered customer
2. Invalid email format
3. Unregistered email address
4. Both mandatory fields empty

**Test object:** [ToolShop login page](https://practicesoftwaretesting.com/auth/login)

The complete prompts V1–V3 and their unchanged AI outputs are documented in the supplementary file [G118_Login_Prompts_and_Outputs_V1-V3_Guelbin.md](G118_Login_Prompts_and_Outputs_V1-V3_Guelbin.md).


### 3.1 User Story

The supplied Login user story describes successful login with valid credentials as well as error cases for mandatory fields, an invalid email format, and incorrect email/password details.

The case of a registered customer with an incorrect password is not explicitly described. Manual observation M02 provides evidence of the current behavior for this case. The complete user story is included in the supplementary file as part of Prompt V2.

### 3.2 Manual Observations

These observations describe the current system behavior. They supplement the user story and do not replace its acceptance criteria.

**Common preconditions:** The user is logged out and the login page is open.

| ID | Test scenario | Inputs and action | Observed result |
|---|---|---|---|
| M02 | Incorrect password | Enter a registered email address and an incorrect password. Click “Einloggen”. | “Invalid email or password” is displayed. |
| M03 | Invalid email format | Enter `sara.d.hotmail.com` and a nonempty password. Click “Einloggen”. | “Email Format ist ungültig” is displayed. |
| M04 | Unregistered email address | Enter `customer5@practicesoftwaretesting.com`, confirmed as unregistered during the manual check, and a nonempty password. Click “Einloggen”. | “Invalid email or password” is displayed. |
| M05 | Empty mandatory fields | Leave both fields empty. Click “Einloggen”. | “Email ist verpflichtend” and “Passwort ist verpflichtend” are displayed. |

---

## 4. Evaluation of the AI-Generated Test Cases

### 4.1 Evaluation of V1
V1 uses a simple prompt with the test objective and the website, without providing the user story or the manual observations.

| Metric | Score | Brief justification |
|---|---:|---|
| **Correctness** | **4/5** | The main expected results of all four test cases align with the user story and the manual observations. The additional expectation regarding access to protected account information in TC01 is not confirmed by these sources. The output explicitly states that the proposed expectations must be verified. |
| **Completeness** | **5/5** | All four selected negative login scenarios are included. Each test case describes preconditions, test data, steps, and expected results. The common preconditions apply to all four test cases. |
| **Clarity** | **4/5** | The test cases are understandable and structured. Some expected results remain general. Statements about unsuccessful login and the unchanged login state can be combined. |
| **Traceability** | **3/5** | The basic scenarios can be mapped to the user story, but explicit source references are missing. The basis for additional expectations, particularly regarding access to protected account information in TC01, remains unclear. |
| **Actionability** | **4/5** | “Attempt to submit the login form” does not specify a concrete user action. In TC02, the validation timing also remains unspecified. Minor clarifications are required for unambiguous execution. |
| **Total score** | **20/25** | The test artifact meets the acceptance threshold. |

The weakest dimension in V1 is **Traceability with 3/5**.

The output does not explicitly identify the source scenarios on which the test cases are based. Some additional expectations, such as checking access to protected account information in TC01, have no confirmed basis in the sources.

Although V1 meets the acceptance threshold, the source references and execution details can still be improved.

### 4.2 Evaluation of V2
| Metric | Score | Brief justification |
|---|---:|---|
| **Correctness** | **5/5** | The expected results align with the available requirements and manual observations. Expectations that are not explicitly supported by the supplied user story are transparently labeled as assumptions. |
| **Completeness** | **5/5** | All four selected negative login scenarios include preconditions, test data, steps, and expected results. |
| **Clarity** | **4/5** | The test cases are understandable and well structured. However, repeated labels such as “Assumption requiring confirmation” make the presentation unnecessarily lengthy. |
| **Traceability** | **4/5** | Each test case identifies a relevant user-story scenario. However, TC01 is linked only to the closest scenario because the case of a registered customer with an incorrect password is not explicitly described. |
| **Actionability** | **4/5** | The test cases are generally executable. In TC02, “Click the Login button if submission is available” leaves it unclear how to proceed if the form cannot be submitted. |
| **Total score** | **22/25** | The test artifact meets the acceptance threshold. |

### 4.3 Evaluation of V3
| Metric | Score | Brief justification |
|---|---:|---|
| **Correctness** | **5/5** | The expected results align with the supplied sources. Requirements and observed system behavior are correctly distinguished. |
| **Completeness** | **4/5** | All four scenarios are included. However, the expected results check only the error messages; explicit confirmation that the user remains logged out is missing. |
| **Clarity** | **4/5** | The test cases are understandable and structured. In TC02–TC04, the separate expected-results sections could be combined more concisely while retaining the source references. |
| **Traceability** | **5/5** | Each test case identifies the relevant user-story scenario and the manual observation. The requirement gap in TC01 is disclosed; the expected behavior is supported by M02. |
| **Actionability** | **4/5** | The user actions and error messages are concrete. For TC01, the email address of a registered test account still needs to be provided. |
| **Total score** | **22/25** | The test artifact meets the acceptance threshold. |

---

## 5. Improvement Actions and Iteration Log

| Version | Prompt Context and Change | Purpose |
|---|---|---|
| **V1** | Basic prompt with the test objective and website. | Establish an initial result for evaluation. |
| **V2** | Add the user story, request scenario references, and label unsupported expectations as assumptions. | Improve traceability and clarify the basis of expected results. |
| **V3** | Add observations M02–M05 and request concrete actions, exact observed messages, and shared notes. | Make the test cases more specific and reduce repeated explanations. |

V3 adds both source context and presentation instructions. Their individual effects therefore cannot be assessed separately.

---
## 6. Comparison of Scores and Quality Trend

| Metric | V1 | V2 | V3 |
|---|---:|---:|---:|
| Correctness | 4/5 | 5/5 | 5/5 |
| Completeness | 5/5 | 5/5 | 4/5 |
| Clarity | 4/5 | 4/5 | 4/5 |
| Traceability | 3/5 | 4/5 | 5/5 |
| Actionability | 4/5 | 4/5 | 4/5 |
| **Total score** | **20/25** | **22/25** | **22/25** |
| **Acceptance decision** | **Accepted** | **Accepted** | **Accepted** |

All three versions meet the acceptance threshold. The total score increases from V1 to V2 and remains unchanged in V3.

Traceability improves from **3/5 to 5/5**. V3 also includes more concrete actions and error messages. However, Completeness receives a lower score because the explicit check of the login state is missing.

The comparison shows that refinement can improve individual quality dimensions without increasing the total score. Each complete output must be reviewed again to identify new gaps.

---

## 7. Final Prompt Recommendation and Further Improvement Opportunities
Of the three evaluated versions, Prompt V3 is recommended as the basis for further use. It contains more concrete actions, exact observed error messages, and clear source references. The complete prompt is documented in [G118_Login_Prompts_and_Outputs_V1-V3_Guelbin](https://github.com/guelbin/RBI-AgileEngineeringFoundation/blob/feature_G118-Iterative-Prompt-Refinement/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks-Outcome/Chapter2/G118_Login_Prompts_and_Outputs_V1-V3_Guelbin.md).

The following improvements are recommended for future refinement:

- Combine each test case’s expected results more concisely while preserving the source references.
- Add an explicit check of the user’s login state. Label this additional expectation as an assumption unless it is supported by the sources.
- Provide concrete test account data.

These improvements are outside the three iterations documented here. No additional evaluation of them is included in this report.