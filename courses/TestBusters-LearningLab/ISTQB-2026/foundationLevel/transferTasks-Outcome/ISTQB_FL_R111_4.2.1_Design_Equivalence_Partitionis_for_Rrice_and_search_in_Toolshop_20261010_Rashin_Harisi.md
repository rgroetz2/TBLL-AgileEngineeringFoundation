# Design Equivalence Partitions for Price Range and Search in Toolshop

## Reference to ISTQB Syllabus Chapter
ISTQB Foundation Level – **4.2.1 Equivalence Partitioning**

## Link to the Transfer Task
[Design Equivalence Partitions for Price Range and Search in the Toolshop](https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/foundationLevel/transferTasks/Chapter%204/ISTQB-FL-4.2.1_Equivalence_Partitioning_20260405.md)

## System Under Test
[Toolshop – Practice Software Testing](https://practicesoftwaretesting.com/)

## Related Acceptance Criteria
- **AC4 – Search:** A valid search query contains 3–40 characters. Submitting it updates the product grid to show matching products and resets all active filters.
- **AC10 – Price Range Slider:** The default range is $1–$100, with a maximum of $200.
- **AC11 – Adjusting the Price Range:** The product grid shows only products within the selected range.

---

## Outcome

### 1. Test Objective
Apply Equivalence Partitioning (EP) to Toolshop Search and Price Range, choose representative values, execute tests, and compare observed UI behavior with acceptance criteria.

### 2. Search – Equivalence Partitions

| Partition ID | Partition Definition | Classification | Representative Input | Expected Result | Coverage Contribution |
|---|---|---|---|---|---|
| S-EP1 | Length 0–2 characters | Invalid | `ab` | Search is not executed | 25% |
| S-EP2 | Length 3–40, matching products exist | Valid | `pliers` | Matching products displayed; active filters reset | 25% |
| S-EP3 | Length 3–40, no matching products | Valid | `paper` | No matching products displayed; active filters reset | 25% |
| S-EP4 | Length greater than 40 | Invalid | 41-character string | Search is not executed | 25% |

**Additional observations:** Empty input did not initiate a new search; `@@@` produced a no-products-found result. AC4 does not specify a validation-message requirement.

### 3. Price Range – Equivalence Partitions

The slider supports integer endpoints, including 0 and 200. Both handles can be placed at the same value. Product prices may contain decimals.

| Partition ID | Partition Definition | Classification | Representative Range | Expected Result | Coverage Contribution | Executed |
|---|---|---|---|---|---|---|
| P-EP1 | Positive-width range with matching products | Valid | $48–$49 | Only matching products within the selected range are displayed | 25% | Yes |
| P-EP2 | Positive-width range with no matching products | Valid | $0–$1 | No products displayed | 25% | Yes |
| P-EP3 | Single-value range with no matching products | Valid | $0–$0 | No products displayed | 25% | Yes |
| P-EP4 | Single-value range with matching products | Valid condition; not verified | Not identified | Products priced exactly at the selected value are displayed | 25% | No |

**Price Range EP Coverage: 3 / 4 = 75%.** Each partition contributes 25 percentage points when exercised; P-EP4 remains uncovered. The no-match classes are based on observed UI results, not an independent verification of the full product dataset.

**Additional checks:** Default $1–$100; highest endpoint $200–$200 (no products found); $40–$50 (Bolt Cutters, $48.41, displayed).

### 4. Six Formal Test Cases

| Test Case ID | Related AC | Partition | Representative Input | Expected Result |
|---|---|---|---|---|
| TC-S01 | AC4 | S-EP1 | `ab` | Search is not executed |
| TC-S02 | AC4 | S-EP2 | `pliers`, with Pliers category selected | Matching products displayed and active filters reset |
| TC-S03 | AC4 | S-EP3 | `paper` | No matching products displayed |
| TC-P01 | AC11 | P-EP1 | $48–$49 | Only products priced within $48–$49 displayed |
| TC-P02 | AC11 | P-EP2 | $0–$1 | No products displayed if no prices fall within range |
| TC-P03 | AC11 | P-EP3 | $0–$0 | No products displayed if no products are priced at $0 |

**Precondition:** Start each independent test from a freshly loaded product overview page with no unintended active filters.

### 5. Test Execution Log

| Test Case ID | Actual Result | Status |
|---|---|---|
| TC-S01 | Input shorter than 3 characters did not trigger search; no validation message appeared | PASS |
| TC-S02 | Four products matching `pliers` displayed; Pliers category filter cleared | PASS |
| TC-S03 | No-products-found result displayed for `paper` | PASS |
| TC-P01 | Bolt Cutters ($48.41) displayed in $48–$49 | PASS |
| TC-P02 | `There are no products found.` displayed for $0–$1 | PASS* |
| TC-P03 | Both handles set to $0; `There are no products found.` displayed | PASS* |

**\*Qualification:** The empty UI state was observed, but the complete product dataset was not independently verified to confirm that no matching product exists in these ranges.

**Other executed checks:** A search longer than 40 characters did not execute; `@@@` returned no products; $40–$50 included Bolt Cutters; $200–$200 returned no products.

### 6. Equivalence Partition Coverage

**EP Coverage = (Exercised partitions / Total identified partitions) × 100**

| Feature | Exercised | Identified | Coverage |
|---|---:|---:|---:|
| Search | 4 | 4 | 100% |
| Price Range | 3 | 4 | 75% |
| **Overall** | **7** | **8** | **87.5%** |

Default-range and maximum-value checks are additional requirement/boundary checks, not separate EP partitions. Coverage reflects the identified partitions, not defect-detection completeness.

### 7. Partition Refinement After Execution

Initially, price range classes were considered as default range, matching products, no matching products, and maximum range. During execution, the slider was found to allow single-value ranges and only integer endpoints, while product prices can be decimal (e.g., Bolt Cutters at $48.41).

The partitions were refined using **range width** (positive width versus single value) and **whether matching products exist**. This prevents counting overlapping boundary checks as separate equivalence classes. A single-value range containing a matching product remains unverified.

### 8. Observations and Limitations

- AC4 does not require a particular validation message for invalid search lengths.
- The observed minimum slider value was $0; AC10 does not explicitly specify a minimum selectable value.
- Integer slider endpoints can include products with decimal prices, such as $48.41 within $48–$49.
- Empty price-range results were not independently cross-checked against the complete product dataset.

### Learning Summary ###

Through this task, I learned to derive equivalence partitions from acceptance criteria, select representative values, and distinguish input partitions from boundary checks and expected system behavior.

For Search, I checked both input-length validity and the presence or absence of matching products. I also verified that submitting a valid search resets an active category filter.

For Price Range, I learned that the slider accepts integer endpoints while product prices may include decimals, and that it permits single-value ranges. These observations led me to refine the initial partitions after execution.

Finally, I practiced calculating EP coverage and documenting limitations rather than treating every unexpected behavior or empty result as a confirmed defect.
