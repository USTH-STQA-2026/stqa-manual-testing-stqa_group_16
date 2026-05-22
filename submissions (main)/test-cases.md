# Test Cases

| Information    |                     |
|----------------|---------------------|
| **Group**      | STQA_Group_16       |
| **Created On** |                     |
| **System**     | https://stqa.rbc.vn |
| **Reference**  | SRS v1.0            |

---

## Step 1: Input Domain Modeling (IDM)

### IDM — Login (REQ-01)

| Characteristic | Block | Representative Value | Expected Result |
|---------------------------|-------------------|--------------------------|------------------|
| Email exists | Yes | librarian@library.com | Login succeeds |
| Email exists | No | nobody@test.com | Show "Khong tim thay thanh vien" |
| Password | Correct | admin123, password123 | Login succeeds |
| Password | Incorrect | wrongpassword | Show "Mat khau khong dung" |
| Missing input | Empty | | Show "Vui long nhap email va mat khau" |

### IDM — View Book List (REQ-02)

| Characteristic | Block | Representative Value | Expected Result |
|---------------------------|-------------------|--------------------------|------------------|
| User role | Librarian | librarian@library.com | Can view the full list of 20 books |
| User role | Member | dam.tran@email.com | Can view the full list of 20 books |
| Displayed book status | Có sẵn | BOOK001 "Lập trình Flutter cơ bản" | Display status "Có sẵn" |
| Displayed book status | Đã mượn | BOOK003 "Kiểm thử phần mềm nhập môn" | Display status "Đã mượn" |
| Displayed book status | Thất lạc | BOOK007 "Kinh tế vi mô" | Display status "Thất lạc" |

### IDM — Search Books (REQ-03)

| Characteristic | Block | Representative Value | Expected Result |
|---------------------------|-------------------|--------------------------|------------------|
| Keyword exists in DB | Yes, book title | Flutter | Show BOOK001 "Lập trình Flutter cơ bản" |
| Keyword exists in DB | Yes, author name | Nguyễn Minh Đức | Show BOOK001 + BOOK009 |
| Keyword exists in DB | Does not exist | XYZ123 | Show message "Không tìm thấy sách" |
| Letter case | Lowercase | flutter | Same result as searching "Flutter" |
| Letter case | Uppercase | FLUTTER | Same result as searching "Flutter" |

### IDM — Borrow Book (REQ-04)

| Characteristic | Block | Representative Value | Expected Result |
|---------------------------|-------------------|--------------------------|------------------|
| Book status | Có sẵn | BOOK001 "Lập trình Flutter cơ bản" | Allow borrowing |
| Book status | Đang mượn | BOOK003 "Kiểm thử phần mềm nhập môn" (borrowed by MEM002) | Reject borrowing |
| Book status | Thất lạc | BOOK020 "Dẫn luận ngôn ngữ học" | Reject borrowing |
| Member status | Hoạt động | MEM002 ba.nguyen@email.com | Allow borrowing if the member has fewer than 3 books |
| Member status | Tạm ngưng | MEM004 cu.le@email.com | Reject and show the suspended-account message |
| Member status | Hết hạn | MEM005 binh.pham@email.com | Reject and show the expired-account message |
| Number of borrowed books (BVA) | 0 books (BVA min) | MEM003 dam.tran@email.com — 0 active borrow records | Allow borrowing |
| Number of borrowed books (BVA) | 1 book (BVA middle) | MEM002 ba.nguyen@email.com — BR001 is active | Allow borrowing |
| Number of borrowed books (BVA) | 2 books (BVA limit - 1) | MEM006 biet.hoang@email.com — borrow one more book to reach 2 | Allow borrowing |
| Number of borrowed books (BVA) | 3 books (BVA at limit - mutant) | MEM006 after borrowing 3 books | Reject and show a limit-exceeded message |
| Borrow duration | Automatically calculated, 14 days | Borrow date + 14 days | Due date = borrow date + 14 days |

### IDM — Return Book (REQ-05)

| Characteristic | Block | Representative Value | Expected Result |
|---------------------------|-------------------|--------------------------|------------------|
| Borrow record exists | Yes | BR003 (MEM006 is borrowing BOOK013) | Process the return |
| Borrow record exists | No | BR0038888 | Reject and show an error |
| Correct returning member? | Same member who borrowed | MEM006 returns BR003 | Allow return |
| Correct returning member? | Another member | MEM002 tries to return BR003 owned by MEM006 | Reject return |
| Member borrowed this book? | Yes | MEM006 with BR003 (BOOK013) | Allow return |
| Member borrowed this book? | No | MEM006 tries to return BR002 already returned by MEM003 | Reject return |
| Return time versus due date | Before due date (returnDate < dueDate) | MEM006 returns BR003 before 15/10/2024 | Return succeeds without a warning |
| Return time versus due date | On due date (= dueDate) | returnDate = 15/10/2024 | Return succeeds without a warning |
| Return time versus due date | After due date (returnDate > dueDate) | MEM002 returns BR001 after the 15/09/2024 due date | Return succeeds and shows an overdue warning |
| Book status after return | Update to "Có sẵn" | BOOK013 after MEM006 returns BR003 | BOOK013 -> "Có sẵn" |

### IDM — Overdue Handling (REQ-06)

| Characteristic | Block | Representative Value | Expected Result |
|---------------------------|-------------------|--------------------------|------------------|
| User who runs the check | Librarian | librarian@library.com (LIB001) | Can click "Kiểm tra quá hạn" |
| User who runs the check | Member | ba.nguyen@email.com (MEM002) | No button or access denied |
| dueDate versus currentDate | dueDate < currentDate | BR001: due 15/09/2024 < 19/05/2026 | Mark as "Quá hạn" |
| dueDate versus currentDate | dueDate = currentDate | Assumed record due today | Mark as "Quá hạn" (SRS: <= today) |
| dueDate versus currentDate | dueDate > currentDate | New record created after reset with a future due date | Do not mark overdue |
| Permission to view overdue list | Librarian | LIB001 | View all overdue records |
| Permission to view overdue list | Member with an overdue record | MEM002 (BR001 overdue) | View only own record |
| Permission to view overdue list | Member without an overdue record | MEM003 dam.tran@email.com | Show no overdue records |
| Overdue record owner | Current member | MEM006 views own BR003 | Allow viewing |
| Overdue record owner | Another member | MEM006 tries to view BR001 owned by MEM002 | Do not allow viewing |

### IDM — Member Management (REQ-07)

| Characteristic | Block | Representative Value | Expected Result |
|---------------------------|-------------------|--------------------------|------------------|
| User role | Librarian | librarian@library.com | Allow adding a member |
| User role | Member | ba.nguyen@email.com | Deny access to the "Thành viên" tab |
| Full name | Valid | Nguyen Van A | Accept |
| Full name | Empty | | Reject and show an error |
| Email format | Valid (@ + . domain) | newuser@domain.com | Accept |
| Email format | Missing @ | newuserdomain.com | Reject with an invalid email message |
| Email format | Missing . in domain | newuser@domain | Reject with an invalid email message |
| Email format | Empty | | Reject with an invalid email message |
| Email existence | Not yet used | newuser@domain.com | Allow creation |
| Email existence | Already used | dam.tran@email.com | Reject with a duplicate email message |
| Phone number | Valid | 0987654321 | Accept |
| Phone number | Empty | | Reject and show an error |

### IDM — Borrow Record Lookup (REQ-08)

| Characteristic | Block | Representative Value | Expected Result |
|---------------------------|-------------------|--------------------------|------------------|
| User role | Librarian | librarian@library.com | View all 5 records (BR001 -> BR005) |
| User role | Member | ba.nguyen@email.com (MEM002) | View only BR001 and BR004 |
| Record ownership | Own record | MEM006 views BR003 | Allow viewing |
| Record ownership | Another member's record | MEM006 tries to view BR004 owned by MEM002 | Deny access |
| Record exists | Exists | BR003 | Display record details |
| Record exists | Does not exist | BR999 | Show a not-found message |
| Record status | Đang mượn | BR003 (MEM006, BOOK013) | Display "Đang mượn" |
| Record status | Đã trả | BR002 (MEM003, BOOK001, returned 20/08) | Display "Đã trả" |
| Record status | Quá hạn | BR001 (MEM002, due 15/09/2024) | Display "Quá hạn" after the librarian check |
| Member borrowing history | Has records | MEM002 (BR001 active, BR004 returned) | Show the list |
| Member borrowing history | No records | Newly created member from TC-43 | Show an empty list |

### Decision Table — Borrow Book (REQ-04)

| Condition / Action | R1 | R2 | R3 | R4 | R5 |
|-----------------------|----|----|----|----|----|
| Book is available? | Y | N | Y | Y | Y |
| Member is active? | Y | Y | N | N | Y |
| Member status is suspended? | N | N | Y | N | N |
| Member status is expired? | N | N | N | Y | N |
| Number of borrowed books < 3? | Y | Y | Y | Y | N |
| Allow borrowing | X | | | | |
| Reject: book unavailable | | X | | | |
| Reject: member suspended | | | X | | |
| Reject: member expired | | | | X | |
| Reject: limit exceeded | | | | | X |
| Create due date +14 days | X | | | | |

### Decision Table — Return Book (REQ-05)

| Condition / Rule | R1 | R2 | R3 | R4 | R5 |
|------------------|----|----|----|----|----|
| Borrow record exists? | Y | N | Y | Y | Y |
| Correct borrower? | Y | Y | N | Y | Y |
| Member borrowed this book? | Y | Y | Y | N | Y |
| Returned on time? | Y | Y | Y | Y | N |
| Book status changes to "Có sẵn" | X | | | | X |
| System shows overdue warning | | | | | X |

### Decision Table — Overdue Handling (REQ-06)

| Condition / Rule | R1 | R2 | R3 | R4 | R5 | R6 | R7 |
|------------------|----|----|----|----|----|----|----|
| User is a librarian? | Y | N | N | N | N | Y | N |
| Overdue record exists? | Y | Y | Y | N | Y | Y | Y |
| User owns the record? | - | Y | N | Y | Y | - | N |
| dueDate ≤ currentDate? | Y | Y | Y | N | Y | N | Y |
| Result | ✔ | ✔ | ✖ | ✖ | ✔ | ✖ | ✖ |

### Decision Table — Member Management (REQ-07)

| Condition / Rule | R1 | R2 | R3 | R4 | R5 | R6 |
|------------------|----|----|----|----|----|----|
| User is a librarian? | Y | N | Y | Y | Y | Y |
| Full name is valid? | Y | Y | N | Y | Y | Y |
| Email format is valid? | Y | Y | Y | N | Y | Y |
| Email is not already used? | Y | Y | Y | Y | N | Y |
| Phone number is valid? | Y | Y | Y | Y | Y | N |
| Result | ✔ | ✖ | ✖ | ✖ | ✖ | ✖ |

### Decision Table — Borrow Record Lookup (REQ-08)

| Condition / Rule | R1 | R2 | R3 | R4 |
|------------------|----|----|----|----|
| User is a librarian? | Y | N | N | N |
| User owns the record? | - | Y | N | Y |
| Record exists? | Y | Y | Y | N |
| Result | ✔ | ✔ | ✖ | ✖ |

---

## Step 2: Test Cases

| TC ID | Test Objective | Preconditions | Steps | Input Data | Expected Result | REQ | Technique |
|-------|-------------------|----------------|----------------|-----------------|------------------|-----|----------|
| TC-1 | Successful login for librarian | Login page is open | 1. Enter email -> 2. Enter password -> 3. Login | email: librarian@library.com / pw: admin123 | Redirect to home. AppBar shows the name + "Thủ thư" | REQ-01 | EP |
| TC-2 | Incorrect password | Login page is open | 1. Enter a valid email -> 2. Enter incorrect pw -> 3. Login | email: librarian@library.com / pw: wrongpass | Show "Mật khẩu không đúng" | REQ-01 | EP |
| TC-3 | Email does not exist | Login page is open | 1. Enter an unknown email -> 2. Enter pw -> 3. Login | email: noone@email.com / pw: admin123 | Show "Không tìm thấy thành viên" | REQ-01 | EP |
| TC-4 | Email only is empty | Login page is open | 1. Leave email empty -> 2. Enter pw -> 3. Login | email: "" / pw: admin123 | Show "Vui lòng nhập email và mật khẩu" | REQ-01 | EP |
| TC-5 | Password only is empty | Login page is open | 1. Enter email -> 2. Leave pw empty -> 3. Login | email: librarian@library.com / pw: "" | Show "Vui lòng nhập email và mật khẩu" | REQ-01 | EP |
| TC-6 | Email and password are empty | Login page is open | 1. Leave both fields empty -> 2. Login | email: "" / pw: "" | Show "Vui lòng nhập email và mật khẩu" | REQ-01 | EP |
| TC-7 | Librarian can view the book list | Reset. Log in as LIB001 | 1. Open the Sách tab -> 2. Observe | librarian@library.com | Show the list of 20 books | REQ-02 | EP |
| TC-8 | Member can view the book list | Reset. Log in as MEM003 | 1. Open the Sách tab -> 2. Observe | dam.tran@email.com | Show the list of 20 books | REQ-02 | EP |
| TC-9 | Book displays status "Có sẵn" | Reset. Log in with a valid account | 1. Open the Sách tab -> 2. Find BOOK001 | BOOK001 "Lập trình Flutter cơ bản" | Display status "Có sẵn" | REQ-02 | EP |
| TC-10 | Book displays status "Đã mượn" | Reset. Log in with a valid account | 1. Open the Sách tab -> 2. Find BOOK003 | BOOK003 "Kiểm thử phần mềm nhập môn" | Display status "Đã mượn" | REQ-02 | EP |
| TC-11 | Search by book title is case-insensitive | Reset. Log in with a valid account | 1. Enter a keyword in "Tìm kiếm" -> 2. Press Enter or the search button | keyword: "flutter", "Flutter", "FLUTTER" | Show BOOK001 "Lập trình Flutter cơ bản" | REQ-03 | EP |
| TC-12 | Search by author name is case-insensitive | Reset. Log in with a valid account | 1. Enter an author name in "Tìm kiếm" -> 2. Press Enter or the search button | keyword: "Nguyễn Minh Đức", "nguyễn minh đức" | Show BOOK001 + BOOK009 | REQ-03 | EP |
| TC-13 | Search with a keyword missing from DB | Reset. Log in with a valid account | 1. Enter a keyword in "Tìm kiếm" -> 2. Press Enter or the search button | keyword: "XYZ123" | Show message "Không tìm thấy sách" | REQ-03 | EP |
| TC-14 | Category filter is case-insensitive | Reset. Log in with a valid account | 1. Select or enter a category in "Lọc" | keyword: "công nghệ", "CÔNG NGHỆ" | Show books in category "Công Nghệ" | REQ-03 | EP |
| TC-15 | Filter with a category missing from DB | Reset. Log in with a valid account | 1. Select or enter a category in "Lọc" | keyword: "Sức Khỏe" | Show an error message or no data | REQ-03 | EP |
| TC-16 | Filter after changing language from Vietnamese to English | Reset. Log in with a valid account | 1. Select the Book tab -> 2. Click Filter -> 3. Enter a Filter by Category suggestion | keyword : Technology , Economics | Show book information for keyword : Technology , Economics | REQ-03 | EP |
| TC-17 | Successful borrow with 0 books, an available book, and an active account (DT-R1, BVA min) | Reset. Log in as MEM003 | 1. Open the Sách tab -> 2. Select BOOK001 -> 3. Click "Mượn" | Book: BOOK001 (Có sẵn) / Member: MEM003 dam.tran@email.com (0 books) | Borrow succeeds. BOOK001 -> "Đã mượn". Due date = today + 14 days | REQ-04 | DT, BVA, EP |
| TC-18 | Successful borrow with 1 borrowed book (BVA middle) | Reset. Log in as MEM002 | 1. Open the Sách tab -> 2. Select BOOK001 -> 3. Click "Mượn" | Book: BOOK001 (Có sẵn) / Member: MEM002 ba.nguyen@email.com (1 book) | Borrow succeeds for the second slot | REQ-04 | BVA |
| TC-19 | Successful borrow with 2 borrowed books (BVA limit - 1) | Reset. Log in as MEM006. Borrow any one book to reach 2 | 1. Select BOOK001 -> 2. Click "Mượn" | Book: BOOK001 (Có sẵn) / Member: MEM006 biet.hoang@email.com (2 books) | Borrow succeeds for the third slot | REQ-04 | BVA |
| TC-20 | Reject a book that is already on loan (DT-R2) | Reset. Log in as MEM003 | 1. Open the Sách tab -> 2. Select BOOK003 -> 3. Click "Mượn" | Book: BOOK003 (Đang mượn by MEM002) / Member: MEM003 | Reject. The "Mượn" button is disabled or a borrow rejection message is shown | REQ-04 | DT, EP |
| TC-21 | Reject a lost book | Reset. Log in as MEM002 | 1. Open the Sách tab -> 2. Select BOOK020 -> 3. Click "Mượn" | Book: BOOK020 "Dẫn luận ngôn ngữ học" (Thất lạc) / Member: MEM002 | Reject the borrow request | REQ-04 | EP |
| TC-22 | Reject a suspended account (DT-R3) | Reset. Log in as MEM004 | 1. Open the Sách tab -> 2. Select BOOK001 -> 3. Click "Mượn" | Book: BOOK001 (Có sẵn) / Member: MEM004 cu.le@email.com (Tạm ngưng) | Reject. Show the suspended-account message instead of the expired-account message | REQ-04 | DT, EP |
| TC-23 | Reject an expired account (DT-R3) | Reset. Log in as MEM005 | 1. Open the Sách tab -> 2. Select BOOK001 -> 3. Click "Mượn" | Book: BOOK001 (Có sẵn) / Member: MEM005 binh.pham@email.com (Hết hạn) | Reject. Show the expired-account message instead of the suspended-account message | REQ-04 | DT, EP |
| TC-24 | Reject after reaching the 3-book limit (DT-R4, BVA max) | Reset. Log in as MEM006. Borrow two more books to reach 3 | 1. Select BOOK001 -> 2. Click "Mượn" | Book: BOOK001 (Có sẵn) / Member: MEM006 (3 borrowed books) | Reject. Show a message for exceeding the 3-book limit | REQ-04 | DT, BVA |
| TC-25 | Reject an on-loan book for a valid account (DT-R2 confirm) | Reset. Log in as MEM002 | 1. Select BOOK013 -> 2. Click "Mượn" | Book: BOOK013 (Đang mượn by MEM006) / MEM002 (1 book, active) | Reject | REQ-04 | DT |
| TC-26 | Reject when multiple conditions are invalid (DT combo) | Reset. Log in as MEM004 | 1. Select BOOK003 -> 2. Click "Mượn" | Book: BOOK003 (Đang mượn) / MEM004 (Tạm ngưng) | Reject | REQ-04 | DT |
| TC-27 | Verify due date = borrow date + 14 days | Reset. Log in as MEM003 | 1. Borrow BOOK001 -> 2. View record information | Borrow date = today | Displayed due date = today + 14 days | REQ-04 | EP |
| TC-28 | Successful return before due date (DT-R6, BVA) | Reset. Log in as MEM006 | 1. Open the Mượn/Trả tab -> 2. Select BR003 -> 3. Click "Trả" | Record: BR003 / MEM006 biet.hoang@email.com / returnDate < 15/10/2024 | Return succeeds. BOOK013 -> "Có sẵn". No warning | REQ-05 | DT, BVA |
| TC-29 | Successful return on due date (BVA boundary) | Reset. Log in as MEM006 | 1. Open the Mượn/Trả tab -> 2. Select BR003 -> 3. Click "Trả" | Record: BR003 / returnDate = dueDate (15/10/2024) | Return succeeds. No warning | REQ-05 | BVA |
| TC-30 | Overdue return warning (DT-R5, BVA) | Reset. Log in as MEM002 | 1. Open the Mượn/Trả tab -> 2. Select BR001 -> 3. Click "Trả" | Record: BR001 / MEM002 ba.nguyen@email.com / returnDate > 15/09/2024 | Return succeeds and an overdue warning is shown | REQ-05 | DT, BVA |
| TC-31 | Update book status after return to "Có sẵn" | Run TC-27. Log in with any account | 1. Open the Sách tab -> 2. Find BOOK003 | BOOK003 after MEM006 returns BR003 | BOOK003 displays status "Có sẵn" | REQ-05 | EP |
| TC-32 | Reject a missing borrow record (DT-R2) | Reset. Log in as MEM006 | 1. Try to return BR0038888 -> 2. Observe | Record: BR0038888 (does not exist) / MEM006 | Reject and show a missing borrow record error | REQ-05 | DT, EP |
| TC-33 | Reject the wrong returning member (DT-R3) | Reset. Log in as MEM002 | 1. Find BR003 in MEM002's Mượn/Trả tab -> 2. Observe | Record: BR003 (owned by MEM006) / Returning member: MEM002 | BR003 does not appear in MEM002's list | REQ-05 | DT, EP |
| TC-34 | Reject when the member did not borrow that book (DT-R4) | Reset. Log in as MEM006 | 1. Try to return BR002 -> 2. Observe | Record: BR002 (already returned by MEM003 and no longer active) / MEM006 | Reject the return | REQ-05 | DT, EP |
| TC-35 | Reject when multiple conditions are invalid | Reset. Log in as MEM002 | 1. Try to return BR0038888 -> 2. Observe | Record: BR0038888 / Member: MEM002 | Reject | REQ-05 | DT |
| TC-36 | Reject when a borrowing member tries to return another member's book | Reset. Log in as MEM006 | 1. Open Tra cứu phiếu mượn -> 2. Enter active member information -> 3. Return the book | Lookup: MEM002 | Reject the return | REQ-05 | DT |
| TC-37 | Librarian overdue check changes BR001 to "Quá hạn" | Reset. Log in as LIB001 | 1. Open the Mượn/Trả tab -> 2. Click "Kiểm tra quá hạn" -> 3. Observe BR001 | LIB001 / BR001 (due 15/09/2024 < 19/05/2026) | BR001 changes to status "Quá hạn" | REQ-06 | DT |
| TC-38 | Librarian views all overdue records (DT-R1) | Run after TC-35 | 1. Log in as LIB001 -> 2. View overdue list | LIB001 | Show all records with dueDate <= 19/05/2026 | REQ-06 | DT |
| TC-39 | Member sees own overdue record (DT-R2) | Run after TC-35 | 1. Log in as MEM002 -> 2. Open the Mượn/Trả tab -> 3. Observe BR001 | MEM002 / BR001 | BR001 displays status "Quá hạn" for MEM002 | REQ-06 | DT |
| TC-40 | Member cannot view another member's overdue record (DT-R3) | Reset. Log in as MEM006 | 1. Open the Mượn/Trả tab -> 2. Observe overdue list | MEM006 / BR001 (owned by MEM002) | BR001 does not appear in MEM006's interface | REQ-06 | DT |
| TC-41 | Member without overdue records sees none (DT-R4) | Reset. Log in as MEM003 | 1. Open the Mượn/Trả tab -> 2. View overdue list | MEM003 dam.tran@email.com — no overdue records | Show no overdue records | REQ-06 | DT |
| TC-42 | BVA: dueDate < currentDate marks overdue | Reset. Log in as LIB001 | 1. Click "Kiểm tra quá hạn" -> 2. Observe BR001 | BR001: dueDate=15/09/2024, currentDate=19/05/2026 (< ) | BR001 is marked "Quá hạn" | REQ-06 | BVA |
| TC-43 | BVA: dueDate = currentDate marks overdue | Reset. Log in as LIB001 | 1. Click "Kiểm tra quá hạn" -> 2. Observe the record due today | Assumed record: dueDate = today (19/05/2026) | The record is marked "Quá hạn" (SRS: <= currentDate) | REQ-06 | BVA |
| TC-44 | Member cannot access the overdue check function | Reset. Log in as MEM002 | 1. Open the Mượn/Trả tab -> 2. Find the "Kiểm tra quá hạn" button | MEM002 | No "Kiểm tra quá hạn" button exists in the interface | REQ-06 | EP |
| TC-45 | Create member successfully with all valid data (DT-R1) | Reset. Log in as LIB001 | 1. Open the Thành viên tab -> 2. Click "Thêm" -> 3. Fill in the form -> 4. Submit | Name: Nguyen Van A / Email: newuser@domain.com / Phone: 0987654321 | Creation succeeds. The member appears in the list | REQ-07 | DT |
| TC-46 | Member cannot add a member (DT-R2) | Reset. Log in as MEM002 | 1. Find the Thành viên tab or add function -> 2. Observe | ba.nguyen@email.com (MEM002) | The "Thành viên" tab is not visible or access is denied | REQ-07 | DT, EP |
| TC-47 | Empty full name (DT-R3) | Reset. Log in as LIB001 | 1. Add a member -> 2. Leave name empty -> 3. Submit | Name: "" / Email: newuser@domain.com / Phone: 0987654321 | Reject and show a full-name error | REQ-07 | DT, EP |
| TC-48 | Email missing @ (DT-R4) | Reset. Log in as LIB001 | 1. Add a member -> 2. Enter invalid email format -> 3. Submit | Name: Nguyen Van A / Email: newuserdomain.com / Phone: 0987654321 | Reject with an invalid email message | REQ-07 | DT, EP |
| TC-49 | Email missing . in domain (DT-R4) | Reset. Log in as LIB001 | 1. Add a member -> 2. Enter invalid email format -> 3. Submit | Name: Nguyen Van A / Email: newuser@domain / Phone: 0987654321 | Reject with an invalid email message | REQ-07 | DT, EP |
| TC-50 | Empty email (DT-R4) | Reset. Log in as LIB001 | 1. Add a member -> 2. Leave email empty -> 3. Submit | Name: Nguyen Van A / Email: "" / Phone: 0987654321 | Reject with an invalid email message | REQ-07 | EP |
| TC-51 | Email already exists (DT-R5) | Reset. Log in as LIB001 | 1. Add a member -> 2. Enter an existing email -> 3. Submit | Name: Nguyen Van A / Email: dam.tran@email.com (MEM003 already exists) / Phone: 0987654321 | Reject with a duplicate email message | REQ-07 | DT, EP |
| TC-52 | Empty phone number (DT-R6) | Reset. Log in as LIB001 | 1. Add a member -> 2. Leave SĐT empty -> 3. Submit | Name: Nguyen Van A / Email: newuser@domain.com / Phone: "" | Reject and show a phone-number error | REQ-07 | DT, EP |
| TC-53 | Librarian views all borrow records (DT-R1) | Reset. Log in as LIB001 | 1. Open the Mượn/Trả tab -> 2. Observe all records | librarian@library.com | Show all 5 records: BR001, BR002, BR003, BR004, BR005 | REQ-08 | DT |
| TC-54 | Member views only own records (DT-R2) | Reset. Log in as MEM002 | 1. Open the Mượn/Trả tab -> 2. Observe | ba.nguyen@email.com (MEM002) | Show only BR001 and BR004 for MEM002. Do not show BR002, BR003, or BR005 | REQ-08 | DT |
| TC-55 | Member cannot view another member's record (DT-R3) | Reset. Log in as MEM006 | 1. Open the Mượn/Trả tab -> 2. Check whether BR004 appears | MEM006 biet.hoang@email.com / BR004 (owned by MEM002) | BR004 does not appear in MEM006's interface | REQ-08 | DT |
| TC-56 | Borrow record does not exist (DT-R4) | Reset. Log in as LIB001 | 1. Search by record ID -> 2. Enter BR999 | Record ID: BR999 | Show a borrow record not found message | REQ-08 | EP |
| TC-57 | Display status "Đang mượn" | Reset. Log in as LIB001 | 1. Open the Mượn/Trả tab -> 2. View BR003 | BR003 (MEM006, BOOK013, Đang mượn) | Display status "Đang mượn" | REQ-08 | EP |
| TC-58 | Display status "Đã trả" | Reset. Log in as LIB001 | 1. Open the Mượn/Trả tab -> 2. View BR002 | BR002 (MEM003, BOOK001, Đã trả 20/08/2024) | Display status "Đã trả" | REQ-08 | EP |
| TC-59 | Display status "Quá hạn" after librarian check | Run after TC-35. Log in as LIB001 | 1. Open the Mượn/Trả tab -> 2. View BR001 | BR001 (MEM002, BOOK003, due 15/09/2024) | Display status "Quá hạn" | REQ-08 | EP |
| TC-60 | Member with borrowing history sees record list | Reset. Log in as MEM002 | 1. Open the Mượn/Trả tab -> 2. Observe | MEM002 (BR001 Đang mượn + BR004 Đã trả) | Show the record list (BR001, BR004) | REQ-08 | EP |
| TC-61 | Record information shows all required fields | Reset. Log in as LIB001 | 1. Open the Mượn/Trả tab -> 2. View BR003 details | BR003 (MEM006, BOOK013, borrowed 01/10/2024, due 15/10/2024) | Show Record ID, Book, Borrow Date, Due Date, and Status | REQ-08 | EP |

---

## Summary

| Functional Area | TC Count | Covered REQ | Applied IDM Technique |
|----------------|-------|---------|----------------------|
| Login | 6 | REQ-01 | EP |
| View Book List | 4 | REQ-02 | EP |
| Search Books | 6 | REQ-03 | EP |
| Borrow Book | 11 | REQ-04 | DT, BVA, EP |
| Return Book | 9 | REQ-05 | DT, BVA, EP |
| Overdue Handling | 8 | REQ-06 | DT, BVA, EP |
| Member Management | 8 | REQ-07 | DT, EP |
| Borrow Record Lookup | 9 | REQ-08 | DT, EP |
| **Total** | **61** | REQ-01 -> REQ-08 | EP, BVA, DT |
