# Test Summary

## 1. Group Information

| Item                  | Information                |
|-----------------------|----------------------------|
| **Group**             | STQA_Group_16              |
| **Class**             | 252ICT2012.L1              |
| **Report Date**       | 20/05/2026                 |
| **System Under Test** | https://stqa.rbc.vn — v1.0 |

---

## 2. Result Overview

| Metric               | Value   |
|----------------------|---------|
| Total Test Cases     | 61      |
| Pass                 | 49      |
| Fail                 | 12      |
| Blocked              | 0       |
| Not Run              | 0       |
| **Pass Rate**        | 80.33%  |
| **Bugs Found**       | 12      |

### Distribution by Functional Area

| Functional Area | TC | Pass | Fail | Bug | Assessment |
|----------------|----|------|------|-----|----------|
| REQ-01 — Login | 6 | 6 | 0 | 0 | All executed TCs pass |
| REQ-02 — View Book List | 4 | 3 | 1 | 1 | Core behavior works; a book status label is still wrong |
| REQ-03 — Search Books | 6 | 4 | 2 | 2 | Search cases pass; category filtering still has defects |
| REQ-04 — Borrow Book | 11 | 9 | 2 | 2 | Most cases pass; message and book-limit defects remain |
| REQ-05 — Return Book | 9 | 7 | 2 | 2 | Overdue warning and return authorization defects remain |
| REQ-06 — Overdue Handling | 8 | 7 | 1 | 1 | Librarian flow works; member status display still has a defect |
| REQ-07 — Member Management | 8 | 5 | 3 | 3 | Highest fail count; input validation defects remain |
| REQ-08 — Borrow Record Lookup | 9 | 8 | 1 | 1 | A member record-visibility defect remains |

### Bug Distribution by Severity

| Severity | Count | Bug IDs |
|--------|----------|---------|
| High   | 3        | BUG-05, BUG-07, BUG-12 |
| Medium | 7        | BUG-02, BUG-03, BUG-04, BUG-06, BUG-08, BUG-09, BUG-10 |
| Low    | 2        | BUG-01, BUG-11 |

---

## 3. Test Design Techniques Used

| Technique | Applied REQs | TC Count | Application Explanation |
|----------|----------------------|---------------|-------------------------|
| EP | REQ-01, REQ-02, REQ-03, REQ-04, REQ-05, REQ-06, REQ-07, REQ-08 | 40 | Splits data and roles into representative valid and invalid partitions, such as login success/failure, book status, valid email, and existing/non-existing borrow records. |
| BVA | REQ-04, REQ-05, REQ-06 | 9 | Checks business boundaries such as borrowed-book counts around the limit of 3, return timing before/on/after due date, and dueDate versus currentDate. |
| DT | REQ-04, REQ-05, REQ-06, REQ-07, REQ-08 | 29 | Combines permission, book/member status, record ownership, and valid-data conditions to check each decision rule. |

---

## 4. Software Quality Analysis

### 4.1. Strengths

- All 61 test cases were executed; no test case is Blocked or Not Run.
- REQ-01 passes 6/6 TCs. The main flows in REQ-04, REQ-06, and REQ-08 also pass most executed TCs.
- The test set covers valid data, invalid data, boundaries, and permission/status rules through EP, BVA, and Decision Tables.

### 4.2. Weaknesses

- 12 of 61 TCs fail. REQ-07 has the largest concentration with 3 member-data validation bugs.
- Authorization defects in borrow record and return flows need attention: a member can return another member's book and view another member's record.
- Several statuses or messages do not meet the requirements, including book status, borrow rejection reason, overdue warning, and overdue status shown to members.
- Category filtering still fails for different letter case and for English keywords after language switching.

---

## 5. Proposed Fix Priority

| Order | Bug | Severity | Priority Rationale |
|--------|-----|--------|---------------|
| 1 | BUG-07 | High | A member can return another member's book and change borrow record data without authorization. |
| 2 | BUG-12 | High | A member can view another member's borrow record, violating REQ-08 visibility rules. |
| 3 | BUG-05 | High | The system allows borrowing beyond the 3-book limit in REQ-04. |
| 4 | BUG-09 | Medium | Valid data is rejected in the standard member-creation flow. |
| 5 | BUG-10 | Medium | Invalid email data can be saved, affecting member data quality. |
| 6 | BUG-06 | Medium | An overdue return is missing the overdue warning. |
| 7 | BUG-08 | Medium | A member does not see the correct overdue status for an own record. |
| 8 | BUG-04 | Medium | The system reports the wrong reason when a suspended account is denied borrowing. |
| 9 | BUG-02 | Medium | Category filtering returns no results for valid keywords with different letter case. |
| 10 | BUG-03 | Medium | Filter by Category returns no results for suggested English keywords after language switching. |
| 11 | BUG-11 | Low | An empty phone number shows an error for the wrong field while the action is still rejected. |
| 12 | BUG-01 | Low | A book status label does not match REQ-02 but does not block a core flow. |

---

## 6. Conclusion

The test execution reaches an 80.33% pass rate with 49 of 61 TCs passing. The system satisfies most executed test cases for login, borrowing, overdue handling, and borrow record lookup, but the borrow/return and member-management flows should not be treated as stable until the authorization, borrow-limit, and validation defects above are addressed.

---

## 7. Lessons Learned (Optional)

- Comparing expected results with the SRS helps distinguish UI defects, business-rule defects, and authorization defects.
- EP, BVA, and Decision Tables complement one another: EP covers data classes, BVA exposes limit defects, and DT clarifies multi-condition rules.
- Recording actual results when a TC fails makes the bug report traceable from test case to defect.

---

## 8. AI Usage Declaration (Optional)

| AI Tool | Used For | Verification and Edits |
|------------|-------------------|-----------------------------------|
| Codex | Reviewing and completing `bug-reports.md` and `summary.md` from existing documents | Cross-checked against `test-cases.md`, `test-execution.md`, and the SRS; evidence fields remain blank for the group to add later |
