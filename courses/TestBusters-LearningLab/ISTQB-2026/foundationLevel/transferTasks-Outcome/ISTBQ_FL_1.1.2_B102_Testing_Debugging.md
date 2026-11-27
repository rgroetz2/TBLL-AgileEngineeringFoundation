# Transfer Task Outcome Template

## Description
This template should be used to document the outcome of a transfer task. The objective is that each ourcome has the same structure that makes it easier for a reviewer to review the ourcome. 

---

# Title of the transfer task
1.1.2 - Apply Testing and Debugging on Toolshop

## Reference to to ISTQB Syllabus chapter
ISTQB FL – 1.1.2 Testing and Debugging

## Link to the transfer task file
https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%201/ISTQB-FL-1.1.2_Testing_and_Debugging_20260405.md

## Outcome

### 1. Introduction

During User Acceptance Testing (UAT) of the Toolshop webshop, several defects were identified.

For this task, one defect was selected and investigated. The objective was to reproduce the defect, document the observed behavior, debug the implementation to identify the root cause, and propose a possible solution.

The investigation demonstrates the difference between testing, which reveals failures, and debugging, which identifies and investigates their causes.

---

### 2. Defect – Maximum Price of Price Range Filter Not Working

**Defect description**

The price range filter in the Toolshop webshop does not correctly apply the selected maximum price. When a user selects a minimum and maximum price, the application does not consider that when sending the filtering request to the backend.

As a result, products outside the selected price range can still appear in the product listing.

#### A. Testing – Defect Reproduction

| Item | Description |
|---|---|
| Test case ID | TC-FLT-001 |
| Preconditions | User opens the webshop and navigates to the product overview page containing products with different prices. |
| Test data | Minimum price: `50`; Maximum price: `100` |
| Test steps | 1. Open the webshop. 2. Navigate to the product overview page. 3. Locate the Price Range filter. 4. Set the minimum price to 50. 5. Set the maximum price to 100. 6. Observe the displayed products. |
| Expected result | Only products with prices between 50 and 100 should be displayed. |
| Actual result | The price range is not applied. Products outside the selected price range can still appear in the product listing. |
| Status | Failed |

#### B. Debugging – Root Cause Analysis

The relevant product filtering implementation was investigated in the frontend source code.

The investigation identified the root cause in the `getProductsNew()` method of the `ProductService` class.

**Root cause:**

When constructing the HTTP request for the price range filter, the implementation only includes the minimum price (`minPrice`).

Although both `minPrice` and `maxPrice` are passed to the method, the maximum price is never included in the API request.

The original implementation is:

```typescript
if (minPrice && maxPrice) {
  params = params.set('between', `price,${minPrice}`);
}
```

The condition checks whether both values exist, but the actual request parameter only contains the minimum price.

Consequently, the backend does not receive the complete selected price range, which causes the maximum price restriction to be missing.

**Relevant code location:**

- File: [`src/app/_services/product.service.ts`](https://github.com/testsmith-io/practice-software-testing/blob/main/sprint5-with-bugs/UI/src/app/_services/product.service.ts)
- Class: `ProductService`
- Method: `getProductsNew()`
- Defective implementation: Construction of the `between` HTTP query parameter.

#### C. Possible Solution

Correct the HTTP request construction to include both the minimum and maximum price values.

**Proposed correction:**

```typescript
if (minPrice && maxPrice) {
  params = params.set('between', `price,${minPrice},${maxPrice}`);
}
```

This change ensures that both selected price boundaries are included in the request sent to the backend.

The backend implementation must also be checked to confirm that it supports this parameter format and correctly applies both boundaries.

The expected behavior after the correction is that only products within the selected price range are returned.

#### D. Verification After Fix

After implementing the correction, the original test case should be executed again to verify that the price range filter is correctly applied. Original testcase => see [A Testing – Defect Reproduction](#a-testing--defect-reproduction)

---

### 3. Learning Summary

| Concept | One-line takeaway |
|---|---|
| Testing | Testing reveals failures by comparing actual results with expected results. |
| Defect reproduction | A defect must be reproducible and documented with clear steps and test data. |
| Debugging | Debugging investigates the implementation to identify the root cause of a failure. |
| Root cause analysis | The observed failure is a symptom; debugging identifies the underlying technical reason. |
| Testing vs. Debugging | Testing identifies that something is wrong; debugging investigates why it is wrong. |
| Verification | After fixing a defect, tests must be executed again to verify the correction. |
| Regression testing | Related functionality should also be tested to ensure that the fix has not introduced new defects. |

**Conclusion**

This task demonstrated the practical difference between testing and debugging in software development.

Testing was used to reproduce the price range filter defect and document the difference between the expected and actual behavior.

Debugging was then used to investigate the frontend implementation and identify the root cause: the maximum price was not included when constructing the API request in the `getProductsNew()` method.

A possible correction was proposed to include both the minimum and maximum price values in the request.

The key learning is that testing reveals the existence of a defect, while debugging helps identify its technical cause. Both activities are necessary to understand, correct and verify software defects effectively.
