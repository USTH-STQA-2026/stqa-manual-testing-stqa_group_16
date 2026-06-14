# Test Execution Results

| Information          |                 |
|----------------------|-----------------|
| **Group**            | STQA_Group_16   |
| **Execution Date**   | 04/05/2026      |
| **Browser**          | Chromium/Chrome |
| **Operating System** | Linux           |

---

## Detailed Results

| TC ID | Functional Area | Expected Result (Summary) | Actual Result | Verdict | Evidence | Bug |
|-------|-----------------|---------------------------|---------------|---------|----------|-----|
| TC-1 | REQ-01 — Login | Redirect to home. AppBar shows the name + "Thủ thư" |  | Pass |  |  |
| TC-2 | REQ-01 — Login | Show "Mật khẩu không đúng" |  | Pass |  |  |
| TC-3 | REQ-01 — Login | Show "Không tìm thấy thành viên" |  | Pass |  |  |
| TC-4 | REQ-01 — Login | Show "Vui lòng nhập email và mật khẩu" |  | Pass |  |  |
| TC-5 | REQ-01 — Login | Show "Vui lòng nhập email và mật khẩu" |  | Pass |  |  |
| TC-6 | REQ-01 — Login | Show "Vui lòng nhập email và mật khẩu" |  | Pass |  |  |
| TC-7 | REQ-02 — View Book List | Show the list of 20 books |  | Pass |  |  |
| TC-8 | REQ-02 — View Book List | Show the list of 20 books |  | Pass |  |  |
| TC-9 | REQ-02 — View Book List | Display status "Có sẵn" |  | Pass |  |  |
| TC-11 | REQ-03 — Search Books | Show BOOK001 "Lập trình Flutter cơ bản" |  | Pass |  |  |
| TC-12 | REQ-03 — Search Books | Show BOOK001 + BOOK009 |  | Pass |  |  |
| TC-13 | REQ-03 — Search Books | Show "Không tìm thấy sách" |  | Pass |  |  |
| TC-15 | REQ-03 — Search Books | Show an error message or no data |  | Pass |  |  |
| TC-17 | REQ-04 — Borrow Book | Borrow succeeds. BOOK001 -> "Đã mượn". Due date = today + 14 days |  | Pass |  |  |
| TC-18 | REQ-04 — Borrow Book | Borrow succeeds for the second slot |  | Pass |  |  |
| TC-19 | REQ-04 — Borrow Book | Borrow succeeds for the third slot |  | Pass |  |  |
| TC-20 | REQ-04 — Borrow Book | Reject. The "Mượn" button is disabled or a borrow rejection message is shown |  | Pass |  |  |
| TC-21 | REQ-04 — Borrow Book | Reject the borrow request |  | Pass |  |  |
| TC-23 | REQ-04 — Borrow Book | Reject. Show the expired-account message instead of the suspended-account message |  | Pass |  |  |
| TC-25 | REQ-04 — Borrow Book | Reject |  | Pass |  |  |
| TC-26 | REQ-04 — Borrow Book | Reject |  | Pass |  |  |
| TC-27 | REQ-04 — Borrow Book | Displayed due date = today + 14 days |  | Pass |  |  |
| TC-28 | REQ-05 — Return Book | Return succeeds. BOOK013 -> "Có sẵn". No warning |  | Pass |  |  |
| TC-29 | REQ-05 — Return Book | Return succeeds. No warning |  | Pass |  |  |
| TC-31 | REQ-05 — Return Book | BOOK003 displays status "Có sẵn" |  | Pass |  |  |
| TC-32 | REQ-05 — Return Book | Reject and show a missing borrow record error |  | Pass |  |  |
| TC-33 | REQ-05 — Return Book | BR003 does not appear in MEM002's list |  | Pass |  |  |
| TC-34 | REQ-05 — Return Book | Reject the return |  | Pass |  |  |
| TC-35 | REQ-05 — Return Book | Reject |  | Pass |  |  |
| TC-37 | REQ-06 — Overdue Handling | BR001 changes to status "Quá hạn" |  | Pass |  |  |
| TC-38 | REQ-06 — Overdue Handling | Show all records with dueDate <= 19/05/2026 |  | Pass |  |  |
| TC-40 | REQ-06 — Overdue Handling | BR001 does not appear in MEM006's interface |  | Pass |  |  |
| TC-41 | REQ-06 — Overdue Handling | Show no overdue records | | Pass |  | |
| TC-42 | REQ-06 — Overdue Handling | BR001 is marked "Quá hạn" |  | Pass |  |  |
| TC-43 | REQ-06 — Overdue Handling | The record is marked "Quá hạn" (SRS: <= currentDate) |  | Pass |  | |
| TC-44 | REQ-06 — Overdue Handling | No "Kiểm tra quá hạn" button exists in the interface |  | Pass |  |  |
| TC-46 | REQ-07 — Member Management | The "Thành viên" tab is not visible or access is denied |  | Pass |  |  |
| TC-47 | REQ-07 — Member Management | Reject and show a full-name error |  | Pass |  |  |
| TC-48 | REQ-07 — Member Management | Reject and show an invalid email message |  | Pass |  |  |z
| TC-50 | REQ-07 — Member Management | Reject and show an invalid email message |  | Pass |  |  |
| TC-51 | REQ-07 — Member Management | Reject and show a duplicate email message |  | Pass |  |  |
| TC-53 | REQ-08 — Borrow Record Lookup | Show all 5 records: BR001, BR002, BR003, BR004, BR005 |  | Pass |  |  |
| TC-54 | REQ-08 — Borrow Record Lookup | Show only BR001 and BR004 for MEM002. Do not show BR002, BR003, or BR005 |  | Pass |  |  |
| TC-56 | REQ-08 — Borrow Record Lookup | Show a borrow record not found message |  | Pass |  |  |
| TC-57 | REQ-08 — Borrow Record Lookup | Display status "Đang mượn" |  | Pass |  |  |
| TC-58 | REQ-08 — Borrow Record Lookup | Display status "Đã trả" |  | Pass |  |  |
| TC-59 | REQ-08 — Borrow Record Lookup | Display status "Quá hạn" |  | Pass |  |  |
| TC-60 | REQ-08 — Borrow Record Lookup | Show the record list (BR001, BR004) |  | Pass |  |  |
| TC-61 | REQ-08 — Borrow Record Lookup | Show Record ID, Book, Borrow Date, Due Date, and Status |  | Pass |  |  |
| TC-24 | REQ-04 — Borrow Book | Reject. Show a message for exceeding the 3-book limit | Borrow succeeds; MEM006 can borrow three more books | Fail | ![TC-24](Screenshot/TC-24.png)| BUG-05 |
| TC-39 | REQ-06 — Overdue Handling | BR001 displays status "Quá hạn" for MEM002 | BR001 does not display status "Quá hạn" | Fail | ![TC-39](Screenshot/TC-39.png) ![TC-39(2)](Screenshot/TC-39(2).png)| BUG-08 |
| TC-41 (Bonus) | REQ-07 — Member Management | BR003 of MEM006 show "Đang mượn" ( no overdate )  | BR003 of MEM006 show status "Quá hạn" | Fail | ![TC-41](Screenshot/TC-41.png) | BUG-13 |
| TC-55 | REQ-08 — Borrow Record Lookup | BR004 does not appear in MEM006's interface | Can view another member's borrow record | Fail | ![TC-55](Screenshot/TC-55.png)| BUG-12 |
| TC-10 | REQ-02 — View Book List | Display status "Đã mượn" | Display status "Đang mượn" | Fail | ![TC-10](Screenshot/TC-10.png) | BUG-01 |
| TC-16 (Bonus) | REQ-03 — Search Books | Show book information for keywords: Technology, Economics | Does not show book information when searching in Filter by Category | Fail | ![TC-16](Screenshot/TC-16.png) ![TC-16(2)](Screenshot/TC-16(2).png) | BUG-03 |
| TC-14 | REQ-03 — Search Books | Show books in category "Công Nghệ" | Does not show the list for the searched category | Fail | ![TC-14](Screenshot/TC-14.png)  ![TC-14(2)](Screenshot/TC-14(2).png)    | BUG-02 |
| TC-49 | REQ-07 — Member Management | Reject and show an invalid email message | Member creation succeeds | Fail |![TC-49](Screenshot/TC-49.png) ![TC-49(2)](Screenshot/TC-49(2).png)| BUG-10 |
| TC-45 | REQ-07 — Member Management | Creation succeeds. The member appears in the list | Rejects with an invalid email message | Fail |![TC-45](Screenshot/TC-45.png) | BUG-09 |
| TC-52 | REQ-07 — Member Management | Reject and show a phone-number error | Rejects with an invalid email message | Fail | ![TC-52](Screenshot/TC-52.png) | BUG-11 |
| TC-36 | REQ-05 — Return Book | Reject the return | Return succeeds | Fail | ![TC-36](Screenshot/TC-36.png)  ![TC-36(2)](Screenshot/TC-36(2).png)| BUG-07 |
| TC-30 | REQ-05 — Return Book | Return succeeds and an overdue warning is shown | Return succeeds without a warning | Fail | ![TC-30](Screenshot/TC-30.png) | BUG-06 |
| TC-22 | REQ-04 — Borrow Book | Reject. Show the suspended-account message instead of the expired-account message | Shows the expired-account message instead of the suspended-account message | Fail |![TC-22](Screenshot/TC-22.png)| BUG-04 |
| TC-43 (Bonus) | REQ-06 — Overdue Handling | Display correct number, status of books | System:  convert status into “Đang mượn: 0 sách” when book isn't returned | Fail | ![TC-43](Screenshot/TC-43.png) | BUG-14 |

---

## Result Summary

| Metric            | Value   |
|-------------------|---------|
| Total Test Cases  | 63      |
| Pass              | 49      |
| Fail              | 14      |
| Blocked           | 0       |
| Not Run           | 0       |
| **Pass Rate**     | 77.8%  |

## Notes

- TestCase (Bonus-Bug): TC-16,TC-41,TC-43
- These bugs were not covered by the 8 specific requirements; they were detected during live testing while cross-checking with the SRS. Consequently, we proposed 3 extra test cases

### Results by Functional Area

| Area | Total TC | Pass | Fail | Pass Rate |
|------|---------|------|------|------------|
| REQ-01 — Login | 6 | 6 | 0 | 100% |
| REQ-02 — View Book List | 4 | 3 | 1 | 75% |
| REQ-03 — Search Books | 6 | 4 | 2 | 66.67% |
| REQ-04 — Borrow Book | 11 | 9 | 2 | 81.82% |
| REQ-05 — Return Book | 9 | 7 | 2 | 77.78% |
| REQ-06 — Overdue Handling | 9 | 7 | 2 | 77.8% |
| REQ-07 — Member Management | 9 | 5 | 4 | 55.5% |
| REQ-08 — Borrow Record Lookup | 9 | 8 | 1 | 88.89% |
