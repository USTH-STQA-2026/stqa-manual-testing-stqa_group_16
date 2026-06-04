# Bug Reports

| Information      |               |
|------------------|---------------|
| **Group**        | STQA_Group_16 |
| **Report Date**  | 04/06/2026    |

---

## BUG-01

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-01         |
| **Related TC**      | TC-10          |
| **Related REQ**     | REQ-02         |
| **Severity**        | Low — the displayed status label violates REQ-02 but does not lose data or block a core flow |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
BOOK003 displays status "Đang mượn" instead of "Đã mượn"

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Reset. Log in with a valid account

**Steps to Reproduce:**
1. Open the Sách tab.
2. Find `BOOK003`.
3. Observe the book status.

**Expected Result:**
Status displays "Đã mượn"

**Actual Result:**
Status displays "Đang mượn"

**Impact:**
The book status does not match the label required by REQ-02, making the interface inconsistent and potentially confusing during status checks.

**Evidence:**
![TC-10](Screenshot/TC-10.png)

**Proposed Fix:**
Standardize the displayed status label for borrowed books to "Đã mượn" in the book list as required by REQ-02.

---

## BUG-02

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-02         |
| **Related TC**      | TC-14          |
| **Related REQ**     | REQ-03         |
| **Severity**        | Medium — the category filter returns no results for valid input with different letter case |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
Category filter returns no Công Nghệ books when the keyword uses different letter case

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Reset. Log in with a valid account

**Steps to Reproduce:**
1. Open the book search/filter function.
2. Select or enter a category in "Lọc".
3. Enter keyword `công nghệ` or `CÔNG NGHỆ`.
4. Observe the result list.

**Expected Result:**
Show books in category "Công Nghệ"

**Actual Result:**
Does not show the list for the searched category

**Impact:**
Users cannot filter books by category when valid input uses a different letter case from the displayed data, reducing book lookup usefulness.

**Evidence:**
![TC-14](Screenshot/TC-14.png) 
![TC-14(2)](Screenshot/TC-14(2).png)

**Proposed Fix:**
Check the category filtering logic and normalize matching for case-insensitive comparison before querying or filtering data.

---

## BUG-03

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-03         |
| **Related TC**      | TC-16          |
| **Related REQ**     | REQ-03         |
| **Severity**        | Medium — category filtering does not work correctly after switching the interface to English |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
Filter by Category returns no results for the suggested English keywords

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: English after switching from Vietnamese

**Preconditions:**
Reset. Log in with a valid account

**Steps to Reproduce:**
1. Switch the interface from Vietnamese to English.
2. Select the Book tab.
3. Open Filter.
4. Enter a Filter by Category suggestion: `Technology` or `Economics`.
5. Observe the result.

**Expected Result:**
Show book information for keyword : Technology , Economics

**Actual Result:**
Does not show book information when searching in Filter by Category

**Impact:**
Users of the English interface get no category-filter results from keywords suggested by the interface, making the filter difficult to use.

**Evidence:**
![TC-16](Screenshot/TC-16.png) 
![TC-16(2)](Screenshot/TC-16(2).png)

**Proposed Fix:**
Align category filter values with translated labels and map English keywords to the corresponding category data.

---

## BUG-04

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-04         |
| **Related TC**      | TC-22          |
| **Related REQ**     | REQ-04         |
| **Severity**        | Medium — borrowing is rejected but the account-status reason is wrong |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
Suspended account is reported as expired when borrowing a book

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Reset. Log in as MEM004

**Steps to Reproduce:**
1. Log in as `MEM004`.
2. Open the Sách tab.
3. Select `BOOK001`.
4. Click "Mượn".

**Expected Result:**
Reject. Show the suspended-account message instead of the expired-account message

**Actual Result:**
Shows the expired-account message instead of the suspended-account message

**Impact:**
The user receives the wrong account-status message, making the borrow rejection reason unclear and likely to be handled incorrectly.

**Evidence:**
![TC-22](Screenshot/TC-22.png)

**Proposed Fix:**
Check the member-status branch used for borrow rejection and show the suspended-account message for `MEM004`.

---

## BUG-05

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-05         |
| **Related TC**      | TC-24          |
| **Related REQ**     | REQ-04         |
| **Severity**        | High — violates the maximum 3-book borrow limit in REQ-04 |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
MEM006 can still borrow after reaching the 3-book limit

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Reset. Log in as MEM006. Borrow two more books to reach 3

**Steps to Reproduce:**
1. Log in as `MEM006`.
2. Borrow two more books so the account has 3 borrowed books.
3. Select `BOOK001`.
4. Click "Mượn".

**Expected Result:**
Reject. Show a message for exceeding the 3-book limit

**Actual Result:**
Borrow succeeds; MEM006 can borrow three more books

**Impact:**
The borrowed-book limit is not enforced, violating borrowing rules and allowing a member to hold more books than permitted.

**Evidence:**
![TC-24](Screenshot/TC-24.png)

**Proposed Fix:**
Check active borrow record count before creating a new record; when the count reaches 3, reject and show the limit-exceeded message.

---

## BUG-06

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-06         |
| **Related TC**      | TC-30          |
| **Related REQ**     | REQ-05         |
| **Severity**        | Medium — overdue return succeeds without the REQ-05 warning |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
Returning overdue BR001 shows no overdue warning

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Reset. Log in as MEM002

**Steps to Reproduce:**
1. Log in as `MEM002`.
2. Open the Mượn/Trả tab.
3. Select `BR001` with a return date after due date `15/09/2024`.
4. Click "Trả".

**Expected Result:**
Return succeeds and an overdue warning is shown

**Actual Result:**
Return succeeds without a warning

**Impact:**
The user is not told that the record is returned after due date, leaving important information out of the overdue return flow.

**Evidence:**
![TC-30](Screenshot/TC-30.png)

**Proposed Fix:**
After detecting a return date after the due date, keep processing the return but show the overdue warning required by REQ-05.

---

## BUG-07

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-07         |
| **Related TC**      | TC-36          |
| **Related REQ**     | REQ-05         |
| **Severity**        | High — a member can return another member's book, affecting return authorization and borrow record data |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
MEM006 can return a book for MEM002 through borrow record lookup

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Reset. Log in as MEM006

**Steps to Reproduce:**
1. Log in as `MEM006`.
2. Open Tra cứu phiếu mượn.
3. Look up active member `MEM002`.
4. Return a book from the found record.

**Expected Result:**
Reject the return

**Actual Result:**
Return succeeds

**Impact:**
A member can change another member's borrow/return status, causing record inconsistency and violating the rule that members return only their own books.

**Evidence:**
![TC-36](Screenshot/TC-36.png)
![TC-36(2)](Screenshot/TC-36(2).png)

**Proposed Fix:**
Check record ownership before the return action and reject the action when the record does not belong to the logged-in member.

---

## BUG-08

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-08         |
| **Related TC**      | TC-39          |
| **Related REQ**     | REQ-06         |
| **Severity**        | Medium — a member does not see the correct overdue status for an own record |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
MEM002 does not see BR001 with status "Quá hạn"

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Run after TC-35

**Steps to Reproduce:**
1. Log in as `MEM002`.
2. Open the Mượn/Trả tab.
3. Observe `BR001`.

**Expected Result:**
BR001 displays status "Quá hạn" for MEM002

**Actual Result:**
BR001 does not display status "Quá hạn"

**Impact:**
The member cannot recognize that an own record is overdue, making displayed status information inconsistent with the overdue handling flow.

**Evidence:**
![TC-39](Screenshot/TC-39.png)
![TC-39(2)](Screenshot/TC-39(2).png)

**Proposed Fix:**
Synchronize the updated overdue status into the record list shown to the owning member.

---

## BUG-09

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-09         |
| **Related TC**      | TC-45          |
| **Related REQ**     | REQ-07         |
| **Severity**        | Medium — valid data is rejected, so the standard member-creation flow cannot complete |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
Adding a member with valid data is rejected as invalid email

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Reset. Log in as LIB001

**Steps to Reproduce:**
1. Log in as `LIB001`.
2. Open the Thành viên tab and click "Thêm".
3. Enter Name `Nguyen Van A`, Email `newuser@domain.com`, Phone `0987654321`.
4. Submit.

**Expected Result:**
Creation succeeds. The member appears in the list

**Actual Result:**
Rejects with an invalid email message

**Impact:**
The librarian cannot create a member with the valid test-case data, interrupting member management.

**Evidence:**
![TC-45](Screenshot/TC-45.png)

**Proposed Fix:**
Check the email validator for valid cases with `@` and `.` in the domain before rejecting member creation.

---

## BUG-10

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-10         |
| **Related TC**      | TC-49          |
| **Related REQ**     | REQ-07         |
| **Severity**        | Medium — invalid email format can still be saved, affecting member data quality |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
Email missing a dot in the domain can still create a member

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Reset. Log in as LIB001

**Steps to Reproduce:**
1. Log in as `LIB001`.
2. Open the add member function.
3. Enter Name `Nguyen Van A`, Email `newuser@domain`, Phone `0987654321`.
4. Submit.

**Expected Result:**
Reject with an invalid email message

**Actual Result:**
Member creation succeeds

**Impact:**
The system stores an account with an email that is invalid under REQ-07, reducing contact-data quality and potentially affecting email-based flows.

**Evidence:**
![TC-49](Screenshot/TC-49.png)
![TC-49(2)](Screenshot/TC-49(2).png)

**Proposed Fix:**
Require email to contain `@` and a `.` in the domain before allowing member creation.

---

## BUG-11

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-11         |
| **Related TC**      | TC-52          |
| **Related REQ**     | REQ-07         |
| **Severity**        | Low — the action is rejected but the error message points to the wrong input field |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
Empty phone number reports invalid email

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Reset. Log in as LIB001

**Steps to Reproduce:**
1. Log in as `LIB001`.
2. Open the add member function.
3. Enter Name `Nguyen Van A`, Email `newuser@domain.com`, and leave Phone empty.
4. Submit.

**Expected Result:**
Reject and show a phone-number error

**Actual Result:**
Rejects with an invalid email message

**Impact:**
The librarian does not receive the correct error for the empty phone-number field, making the input harder to fix.

**Evidence:**
![TC-52](Screenshot/TC-52.png)

**Proposed Fix:**
Separate empty phone-number validation from email validation and show the phone-number error when Phone is invalid.

---

## BUG-12

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-12         |
| **Related TC**      | TC-55          |
| **Related REQ**     | REQ-08         |
| **Severity**        | High — a member can view another member's borrow record, violating REQ-08 visibility limits |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 20/05/2026     |
| **Status**          | Open           |

**Title:**
MEM006 can view BR004 owned by another member

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Reset. Log in as MEM006

**Steps to Reproduce:**
1. Log in as `MEM006`.
2. Open the Mượn/Trả tab.
3. Check whether the list contains `BR004` owned by `MEM002`.

**Expected Result:**
BR004 does not appear in MEM006's interface

**Actual Result:**
Can view another member's borrow record

**Impact:**
A member can view a borrow record not owned by that member, exposing borrowing information and violating role-based access control.

**Evidence:**
![TC-55](Screenshot/TC-55.png)

**Proposed Fix:**
Filter borrow records by the logged-in member in both data retrieval and display; only librarians should view records for all members.

## BUG-13

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-13         |
| **Related TC**      | TC-41 (Bonus)  |
| **Related REQ**     | REQ-07         |
| **Severity**        | Medium         |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 01/06/2026     |
| **Status**          | Open           |

**Title:**
BR003 of MEM006 show status "Quá hạn"

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
Reset. Login LIB001

**Steps to Reproduce:** <br>
1.Tab Sách. <br>
2 Tab Mượn/Trả. <br>
3 Check book overdate.

**Expected Result:**
BR003 of MEM006 show "Đang mượn" ( not overdate ) 

**Actual Result:**
BR003 of MEM006 show status "Quá hạn"

**Impact:**
Affects data accuracy of book status but does not crash the system or block the main return workflow.

**Evidence:**
![TC-41](Screenshot/TC-41.png)

**Proposed Fix:**

## BUG-14

| Attribute           | Details        |
|---------------------|----------------|
| **Bug ID**          | BUG-14         |
| **Related TC**      | TC-43 (Bonus)  |
| **Related REQ**     | REQ-06         |
| **Severity**        | Medium         |
| **Reporter**        | STQA_Group_16  |
| **Found On**        | 01/06/2026     |
| **Status**          | Open           |

**Title:**
System:  convert status into “Đang mượn: 0 sách” when book isn't returned

**Environment:**
- Browser: Chromium/Chrome
- Operating System: Linux
- Interface Language: Vietnamese

**Preconditions:**
MEM002 borrowing, not returned, overdate

**Steps to Reproduce:** <br>
1. Login into account Librarian <br>
2. Tab “Mượn/Trả” <br>
3. Click “Kiểm tra sách quá hạn” <br>
4. Tab “Thành viên” <br>
5. Lookup MEM002 and check status/quantity of book borrowing

**Expected Result:**
System still show true quantity of book borrowing of MEM002 is 1 book. Not return into “Đang mượn": 0 sách” when book isn't returned.

**Actual Result:**
System:  convert status into “Đang mượn: 0 sách” when book isn't returned

**Impact:**
Causes data inconsistency in member records, directly impacting the librarian's tracking ability, but a workaround (checking book history) exists.

**Evidence:**
![TC-43](Screenshot/TC-43.png)

**Proposed Fix:**