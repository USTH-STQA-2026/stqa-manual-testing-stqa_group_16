z# Test Cases — Bảng trường hợp kiểm thử

| Thông tin      |                       |
|----------------|-----------------------|
| **Nhóm**       | `STQA_Group_16`       |
| **Ngày tạo**   | `01/05/2026`          |
| **Hệ thống**   | <https://stqa.rbc.vn>   |
| **Tham chiếu** | SRS v1.0              |

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

### IDM — Đăng nhập (REQ-01)

| Đặc tính (Characteristic)  | Phân vùng (Block) | Giá trị đại diện (Value)                         | Kết quả mong đợi                              |
|----------------------------|-------------------|--------------------------------------------------|-----------------------------------------------|
| Email có tồn tại trong DB? | Có (Thủ thư)      | `librarian@library.com`                          | Đăng nhập thành công, chuyển trang chủ        |
|                            | Có (Thành viên)   | `ba.nguyen@email.com`                            | Đăng nhập thành công, chuyển trang chủ        |
|                            | Không tồn tại     | `noone@email.com`                                | **"Không tìm thấy thành viên"**               |
| Mật khẩu có đúng?          | Đúng              | `admin123` (cho `librarian@library.com`)         | Đăng nhập thành công                          |
|                            | Sai               | `wrongpass`                                      | **"Mật khẩu không đúng"**                     |
| Ô nhập có rỗng?            | Chỉ email rỗng    | email=`""` / pw=`admin123`                       | **"Vui lòng nhập email và mật khẩu"**         |
|                            | Chỉ password rỗng | email=`librarian@library.com` / pw=`""`          | **"Vui lòng nhập email và mật khẩu"**         |
|                            | Cả hai rỗng       | email=`""` / pw=`""`                             | **"Vui lòng nhập email và mật khẩu"**         |

### IDM — Xem danh sách sách (REQ-02)

| Đặc tính (Characteristic) | Phân vùng (Block)    | Giá trị đại diện (Value)                      | Kết quả mong đợi                    |
|---------------------------|----------------------|-----------------------------------------------|-------------------------------------|
| Vai trò người dùng?       | Thủ thư              | `librarian@library.com`                       | Xem được danh sách toàn bộ 20 sách  |
|                           | Thành viên           | `dam.tran@email.com`                          | Xem được danh sách toàn bộ 20 sách  |
| Trạng thái sách hiển thị? | Có sẵn               | BOOK001 "Lập trình Flutter cơ bản"            | Hiển thị trạng thái **"Có sẵn"**    |
|                           | Đã mượn              | BOOK003 "Kiểm thử phần mềm nhập môn"         | Hiển thị trạng thái **"Đã mượn"**   |
|                           | Thất lạc             | BOOK007 "Kinh tế vi mô"                      | Hiển thị trạng thái **"Thất lạc"**  |

### IDM — Tìm kiếm sách (REQ-03)

| Đặc tính (Characteristic)                 | Phân vùng (Block)                             | Giá trị đại diện (Value) | Kết quả mong đợi                                   |
| ----------------------------------------- | --------------------------------------------- | ------------------------ | -------------------------------------------------- |
| "Tìm kiếm" – Từ khóa có tồn tại trong DB? | Có (Tên sách) + Không phân biệt hoa thường    | `"flutter"`              | Hiển thị BOOK001 – "Lập trình Flutter cơ bản"      |
|                                           | Có (Tên tác giả) + Không phân biệt hoa thường | `"nguyễn minh đức"`      | Hiển thị BOOK001 + BOOK009                         |
|                                           | Không tồn tại                                 | `"XYZ123"`               | Hiển thị thông báo: **"Không tìm thấy sách"**      |
| "Lọc" – Theo thể loại                     | Có + Không phân biệt hoa thường               | `"công Nghệ"`            | Hiển thị danh sách sách thuộc thể loại "Công Nghệ" |
|                                           | Không tồn tại                                 | `"Sức Khỏe"`             | Hiển thị thông báo lỗi / không có dữ liệu          |

### IDM — Mượn sách (REQ-04)

| Đặc tính (Characteristic) | Phân vùng (Block)        | Giá trị đại diện (Value)                                         | Kết quả mong đợi                               |
|---------------------------|--------------------------|------------------------------------------------------------------|------------------------------------------------|
| Trạng thái sách?          | Có sẵn                   | **BOOK001** "Lập trình Flutter cơ bản"                           | Cho phép mượn                                  |
|                           | Đang mượn                | **BOOK003** "Kiểm thử phần mềm nhập môn" (đang mượn bởi MEM002) | Từ chối, không cho mượn                        |
|                           | Thất lạc                 | **BOOK020** "Dẫn luận ngôn ngữ học"                              | Từ chối, không cho mượn                        |
| Trạng thái thành viên?    | Hoạt động                | MEM002 `ba.nguyen@email.com`                                     | Cho phép mượn (nếu < 3 sách)                   |
|                           | Tạm ngưng                | MEM004 `cu.le@email.com`                                         | Từ chối + thông báo **tạm ngưng**              |
|                           | Hết hạn                  | MEM005 `binh.pham@email.com`                                     | Từ chối + thông báo **hết hạn** (≠ tạm ngưng) |
| Số sách đang mượn? (BVA)  | 0 sách (BVA min)         | MEM003 `dam.tran@email.com` — 0 phiếu đang mượn                 | Cho phép mượn                                  |
|                           | 1 sách (BVA giữa)        | MEM002 `ba.nguyen@email.com` — đang mượn BR001 (1 sách)         | Cho phép mượn                                  |
|                           | 2 sách (BVA giới hạn -1) | MEM006 `biet.hoang@email.com` — mượn thêm 1 để có 2 sách        | Cho phép mượn                                  |
|                           | 3 sách (BVA đúng giới hạn)| MEM006 sau khi đã mượn đủ 3 sách                               | Từ chối + thông báo vượt giới hạn              |
| Thời hạn mượn             | Tự động tính (14 ngày)   | Ngày mượn + 14 ngày                                              | Hạn trả = ngày mượn + 14 ngày                  |

### IDM — Trả sách (REQ-05)

| Đặc tính (Characteristic)    | Phân vùng (Block)              | Giá trị đại diện (Value)                              | Kết quả mong đợi                       |
|------------------------------|--------------------------------|-------------------------------------------------------|----------------------------------------|
| Phiếu mượn có tồn tại?       | Có                             | BR003 (MEM006 đang mượn BOOK013)                      | Xử lý trả sách                         |
|                              | Không tồn tại                  | `BR0038888`                                           | Từ chối + thông báo lỗi                |
| Đúng người trả?              | Đúng thành viên đã mượn        | MEM006 trả BR003                                      | Cho phép trả                           |
|                              | Thành viên khác                | **MEM002 cố trả BR003** (của MEM006)                  | Từ chối trả sách                       |
| Thành viên có mượn sách đó?  | Có mượn sách đó                | MEM006 với BR003 (BOOK013)                            | Cho phép trả                           |
|                              | Không mượn sách đó             | **MEM006 cố trả BR002** (của MEM003, đã trả rồi)     | Từ chối trả sách                       |
| Thời gian trả so với hạn?    | Trả trước hạn (returnDate < dueDate) | MEM006 trả BR003 trước 15/10/2024               | Trả thành công, không cảnh báo         |
|                              | Trả đúng ngày hạn (= dueDate)  | returnDate = 15/10/2024                               | Trả thành công, không cảnh báo         |
|                              | Trả quá hạn (returnDate > dueDate) | MEM002 trả BR001, hạn 15/09/2024 đã qua          | Trả thành công + **cảnh báo quá hạn**  |
| Trạng thái sách sau khi trả? | Cập nhật về "Có sẵn"           | BOOK013 sau khi MEM006 trả BR003                      | BOOK013 → **"Có sẵn"**                 |

### IDM — Xử lý quá hạn (REQ-06)

| Đặc tính (Characteristic)    | Phân vùng (Block)                  | Giá trị đại diện (Value)                          | Kết quả mong đợi                          |
|------------------------------|------------------------------------|---------------------------------------------------|-------------------------------------------|
| Người thực hiện kiểm tra?    | Thủ thư                            | `librarian@library.com` (LIB001)                  | Cho phép bấm "Kiểm tra quá hạn"           |
|                              | Thành viên                         | `ba.nguyen@email.com` (MEM002)                    | Không có nút / từ chối truy cập           |
| dueDate so với currentDate?  | dueDate < currentDate              | BR001: hạn 15/09/2024 < 19/05/2026               | Đánh dấu **"Quá hạn"**                    |
|                              | dueDate = currentDate              | Phiếu giả định hạn đúng hôm nay                   | Đánh dấu **"Quá hạn"** (SRS: ≤ ngày nay) |
|                              | dueDate > currentDate              | Phiếu tạo mới sau khi reset, hạn > hôm nay        | Không đánh dấu quá hạn                    |
| Quyền xem danh sách quá hạn? | Thủ thư                            | LIB001                                            | Xem tất cả phiếu quá hạn                  |
|                              | Thành viên có phiếu quá hạn        | MEM002 (BR001 quá hạn)                            | Chỉ thấy phiếu **của mình**               |
|                              | Thành viên không có phiếu quá hạn  | MEM003 `dam.tran@email.com`                       | Không hiển thị phiếu quá hạn              |
| Phiếu quá hạn thuộc về ai?   | Chính thành viên hiện tại          | MEM006 xem BR003 (của mình)                       | Cho phép xem                              |
|                              | Thành viên khác                    | MEM006 cố xem BR001 (của MEM002)                  | Không cho phép                            |

### IDM — Quản lý thành viên (REQ-07)

| Đặc tính (Characteristic) | Phân vùng (Block)           | Giá trị đại diện (Value)  | Kết quả mong đợi              |
|---------------------------|-----------------------------|---------------------------|-------------------------------|
| Vai trò người dùng?       | Thủ thư                     | `librarian@library.com`   | Cho phép thêm thành viên      |
|                           | Thành viên                  | `ba.nguyen@email.com`     | Từ chối truy cập tab Thành viên |
| Họ tên?                   | Hợp lệ                      | `Nguyen Van A`            | Chấp nhận                     |
|                           | Rỗng                        | `""`                      | Từ chối + thông báo lỗi       |
| Email format?             | Hợp lệ (`@` + `.` domain)   | `newuser@domain.com`      | Chấp nhận                     |
|                           | Thiếu `@`                   | `newuserdomain.com`       | Từ chối + email không hợp lệ  |
|                           | Thiếu dấu `.` trong domain  | `newuser@domain`          | Từ chối + email không hợp lệ  |
|                           | Rỗng                        | `""`                      | Từ chối + email không hợp lệ  |
| Email đã tồn tại?         | Chưa tồn tại                | `newuser@domain.com`      | Cho phép tạo                  |
|                           | Đã tồn tại                  | `dam.tran@email.com`      | Từ chối + email đã tồn tại    |
| Số điện thoại?            | Hợp lệ                      | `0987654321`              | Chấp nhận                     |
|                           | Rỗng                        | `""`                      | Từ chối + thông báo lỗi       |

### IDM — Tra cứu phiếu mượn (REQ-08)

| Đặc tính (Characteristic)  | Phân vùng (Block)           | Giá trị đại diện (Value)              | Kết quả mong đợi                                          |
|----------------------------|-----------------------------|---------------------------------------|-----------------------------------------------------------|
| Vai trò người dùng?        | Thủ thư                     | `librarian@library.com`               | Xem tất cả 5 phiếu (BR001→BR005)                          |
|                            | Thành viên                  | `ba.nguyen@email.com` (MEM002)        | Chỉ thấy BR001, BR004                                     |
| Phiếu có thuộc người dùng? | Phiếu của chính mình        | MEM006 xem BR003                      | Cho phép xem                                              |
|                            | Phiếu của người khác        | MEM006 cố xem BR004 (của MEM002)      | Từ chối truy cập                                          |
| Phiếu có tồn tại?          | Tồn tại                     | BR003                                 | Hiển thị chi tiết phiếu                                   |
|                            | Không tồn tại               | `BR999`                               | Thông báo không tìm thấy                                  |
| Trạng thái phiếu?          | Đang mượn                   | BR003 (MEM006, BOOK013)               | Hiển thị **"Đang mượn"**                                  |
|                            | Đã trả                      | BR002 (MEM003, BOOK001, trả 20/08)    | Hiển thị **"Đã trả"**                                     |
|                            | Quá hạn                     | BR001 (MEM002, hạn 15/09/2024)        | Hiển thị **"Quá hạn"** (sau khi TT đã kiểm tra)          |
| Thành viên có lịch sử?     | Có phiếu mượn               | MEM002 (có BR001 đang mượn, BR004 đã trả) | Hiển thị danh sách                                    |
|                            | Không có phiếu nào          | Thành viên mới tạo (TC-43), chưa mượn gì | Hiển thị rỗng                                         |

## Bước 1b — Decision Tables

### DT — REQ-04 Mượn sách

| Điều kiện / Rule  | R1 | R2 | R3 | R4 | R5 |
|-------------------|----|----|----|----|----|
| Sách có sẵn?      | Y  | N  | Y  | Y  | Y  |
| Member hoạt động? | Y  | Y  | N  | Y  | Y  |
| Số sách < 3?      | Y  | Y  | Y  | N  | Y  |
| **Kết quả**       | ✔  | ✖  | ✖  | ✖  | ✔  |

### DT — REQ-05 Trả sách

| Điều kiện / Rule             | R1 | R2 | R3 | R4 | R5 | R6 | R7 |
|------------------------------|----|----|----|----|----|----|----|
| Phiếu mượn tồn tại?          | Y  | N  | Y  | Y  | Y  | Y  | Y  |
| Đúng người mượn?             | Y  | Y  | N  | Y  | Y  | Y  | Y  |
| Thành viên có mượn sách này? | Y  | Y  | Y  | N  | Y  | Y  | Y  |
| Trả đúng hạn?                | Y  | Y  | Y  | Y  | N  | Y  | N  |
| **Kết quả**                  | ✔  | ✖  | ✖  | ✖  | ✔* | ✔  | ✔* |

### DT — REQ-06 Overdue Handling

| Điều kiện / Rule        | R1 | R2 | R3 | R4 | R5 | R6 | R7 |
|-------------------------|----|----|----|----|----|----|----|
| User là Thủ thư?        | Y  | N  | N  | N  | N  | Y  | N  |
| Có phiếu quá hạn?       | Y  | Y  | Y  | N  | Y  | Y  | Y  |
| Là chủ phiếu?           | -  | Y  | N  | Y  | Y  | -  | N  |
| dueDate ≤ currentDate?  | Y  | Y  | Y  | N  | Y  | N  | Y  |
| **Kết quả**             | ✔  | ✔  | ✖  | ✖  | ✔  | ✖  | ✖  |

### DT — REQ-07 Member Management

| Điều kiện / Rule     | R1 | R2 | R3 | R4 | R5 | R6 |
|----------------------|----|----|----|----|----|-----|
| User là Thủ thư?     | Y  | N  | Y  | Y  | Y  | Y  |
| Họ tên hợp lệ?       | Y  | Y  | N  | Y  | Y  | Y  |
| Email format hợp lệ? | Y  | Y  | Y  | N  | Y  | Y  |
| Email chưa tồn tại?  | Y  | Y  | Y  | Y  | N  | Y  |
| SĐT hợp lệ?          | Y  | Y  | Y  | Y  | Y  | N  |
| **Kết quả**          | ✔  | ✖  | ✖  | ✖  | ✖  | ✖  |

### DT — REQ-08 Borrow Record Lookup

| Điều kiện / Rule      | R1 | R2 | R3 | R4 |
|-----------------------|----|----|----|-----|
| User là Thủ thư?      | Y  | N  | N  | N  |
| Là chủ phiếu?         | -  | Y  | N  | Y  |
| Phiếu tồn tại?        | Y  | Y  | Y  | N  |
| **Kết quả**           | ✔  | ✔  | ✖  | ✖  |

---

## Bước 2: Test Cases

> **Quy ước:** Reset dữ liệu bằng nút "Khôi phục dữ liệu" hoặc refresh trang trước mỗi TC có ghi "Reset data".

---

### REQ-01 — Đăng nhập (6 TC)

| Mã TC | Mục tiêu kiểm thử              | Tiền điều kiện    | Bước thực hiện                              | Dữ liệu đầu vào                                     | Kết quả mong đợi                                   | REQ    | Kỹ thuật |
|-------|--------------------------------|-------------------|---------------------------------------------|-----------------------------------------------------|----------------------------------------------------|--------|----------|
| TC-01 | Đăng nhập thành công (Thủ thư) | Trang login mở    | 1. Nhập email → 2. Nhập password → 3. Login | email: `librarian@library.com` / pw: `admin123`     | Chuyển trang chủ. AppBar hiển thị tên + "Thủ thư"  | REQ-01 | EP       |
| TC-02 | Sai mật khẩu                   | Trang login mở    | 1. Nhập email đúng → 2. Nhập pw sai → 3. Login | email: `librarian@library.com` / pw: `wrongpass` | Hiển thị **"Mật khẩu không đúng"**                | REQ-01 | EP       |
| TC-03 | Email không tồn tại            | Trang login mở    | 1. Nhập email không tồn tại → 2. Nhập pw → 3. Login | email: `noone@email.com` / pw: `admin123`     | Hiển thị **"Không tìm thấy thành viên"**           | REQ-01 | EP       |
| TC-04 | Chỉ email bị bỏ trống          | Trang login mở    | 1. Để trống email → 2. Nhập pw → 3. Login   | email: `""` / pw: `admin123`                        | Hiển thị **"Vui lòng nhập email và mật khẩu"**     | REQ-01 | EP       |
| TC-05 | Chỉ password bị bỏ trống       | Trang login mở    | 1. Nhập email → 2. Để trống pw → 3. Login   | email: `librarian@library.com` / pw: `""`           | Hiển thị **"Vui lòng nhập email và mật khẩu"**     | REQ-01 | EP       |
| TC-06 | Cả email và password đều trống | Trang login mở    | 1. Để trống cả hai → 2. Login               | email: `""` / pw: `""`                              | Hiển thị **"Vui lòng nhập email và mật khẩu"**     | REQ-01 | EP       |

---

### REQ-02 — Xem danh sách sách

| Mã TC | Mục tiêu kiểm thử                         | Tiền điều kiện          | Bước thực hiện                     | Dữ liệu đầu vào                            | Kết quả mong đợi                   | REQ    | Kỹ thuật |
|-------|-------------------------------------------|-------------------------|------------------------------------|--------------------------------------------|-------------------------------------|--------|----------|
| TC-07 | Thủ thư xem được danh sách sách           | Reset. Đăng nhập LIB001 | 1. Vào tab Sách → 2. Quan sát      | `librarian@library.com`                    | Hiển thị danh sách 20 sách          | REQ-02 | EP       |
| TC-08 | Thành viên xem được danh sách sách        | Reset. Đăng nhập MEM003 | 1. Vào tab Sách → 2. Quan sát      | `dam.tran@email.com`                       | Hiển thị danh sách 20 sách          | REQ-02 | EP       |
| TC-09 | Sách hiển thị đúng trạng thái "Có sẵn"    | Reset. Đăng nhập bất kỳ | 1. Vào tab Sách → 2. Tìm BOOK001   | BOOK001 "Lập trình Flutter cơ bản"         | Trạng thái hiển thị **"Có sẵn"**    | REQ-02 | EP       |
| TC-10 | Sách hiển thị đúng trạng thái "Đã mượn"   | Reset. Đăng nhập bất kỳ | 1. Vào tab Sách → 2. Tìm BOOK003   | BOOK003 "Kiểm thử phần mềm nhập môn"       | Trạng thái hiển thị **"Đã mượn"**   | REQ-02 | EP       |

---

### REQ-03 — Tìm kiếm sách (5 TC)

| Mã TC      | Mục tiêu kiểm thử                                             | Tiền điều kiện                            | Bước thực hiện                                                        | Dữ liệu đầu vào     | Kết quả mong đợi                                   | REQ  | Kỹ thuật |
| ---------- | ------------------------------------------------------------- | ----------------------------------------- | --------------------------------------------------------------------- | ------------------- | -------------------------------------------------- | ---- | -------- |
| TC_REQ3_01 | Kiểm tra tìm kiếm theo tên sách không phân biệt hoa thường    | Người dùng đang ở màn hình danh sách sách | 1. Nhập từ khóa vào ô "Tìm kiếm" 2. Nhấn Enter / 3.nút tìm kiếm     | `"flutter"`         | Hiển thị BOOK001 – "Lập trình Flutter cơ bản"      | REQ3 | EP       |
| TC_REQ3_02 | Kiểm tra tìm kiếm theo tên tác giả không phân biệt hoa thường | Người dùng đang ở màn hình danh sách sách | 1. Nhập tên tác giả vào ô "Tìm kiếm" 2. Nhấn Enter / 3.nút tìm kiếm | `"nguyễn minh đức"` | Hiển thị BOOK001 và BOOK009                        | REQ3 | EP       |
| TC_REQ3_03 | Kiểm tra tìm kiếm với từ khóa không tồn tại trong DB          | Người dùng đang ở màn hình danh sách sách | 1. Nhập từ khóa vào ô "Tìm kiếm" 2. Nhấn Enter / 3.nút tìm kiếm     | `"XYZ123"`          | Hiển thị thông báo: "Không tìm thấy sách"          | REQ3 | EP       |
| TC_REQ3_04 | Kiểm tra lọc theo thể loại không phân biệt hoa thường         | Người dùng đang ở màn hình danh sách sách | 1. Chọn / nhập thể loại tại mục "Lọc"                                 | `"công Nghệ"`       | Hiển thị danh sách sách thuộc thể loại "Công Nghệ" | REQ3 | EP       |
| TC_REQ3_05 | Kiểm tra lọc theo thể loại không tồn tại                      | Người dùng đang ở màn hình danh sách sách | 1. Chọn / nhập thể loại tại mục "Lọc"                                 | `"Sức Khỏe"`        | Hiển thị thông báo lỗi hoặc không có dữ liệu       | REQ3 | EP       |

---

### REQ-04 — Mượn sách (11 TC)

**Lưu ý seed data:** MEM002 đang mượn **1 sách** (BR001). MEM006 đang mượn **1 sách** (BR003). MEM003 đang mượn **0 sách**.

| Mã TC | Mục tiêu kiểm thử                                                      | Tiền điều kiện                                        | Bước thực hiện                                       | Dữ liệu đầu vào                                                               | Kết quả mong đợi                                                       | REQ    | Kỹ thuật    |
|-------|------------------------------------------------------------------------|-------------------------------------------------------|------------------------------------------------------|-------------------------------------------------------------------------------|------------------------------------------------------------------------|--------|-------------|
| TC-16 | Mượn thành công — 0 sách, sách có sẵn, TK hoạt động (DT-R1, BVA min)  | Reset. Đăng nhập MEM003                               | 1. Tab Sách → 2. Chọn BOOK001 → 3. Nhấn "Mượn"      | Sách: BOOK001 (Có sẵn) / Thành viên: MEM003 `dam.tran@email.com` (0 sách)    | Mượn thành công. BOOK001 → "Đã mượn". Hạn = hôm nay + 14 ngày        | REQ-04 | DT, BVA, EP |
| TC-17 | Mượn thành công — đang mượn 1 sách (BVA giữa)                          | Reset. Đăng nhập MEM002                               | 1. Tab Sách → 2. Chọn BOOK001 → 3. Nhấn "Mượn"      | Sách: BOOK001 (Có sẵn) / Thành viên: MEM002 `ba.nguyen@email.com` (1 sách)   | Mượn thành công (slot thứ 2)                                           | REQ-04 | BVA         |
| TC-18 | Mượn thành công — đang mượn 2 sách (BVA giới hạn - 1)                  | Reset. Đăng nhập MEM006. Mượn thêm 1 sách bất kỳ để có 2 | 1. Chọn BOOK001 → 2. Nhấn "Mượn"              | Sách: BOOK001 (Có sẵn) / Thành viên: MEM006 `biet.hoang@email.com` (2 sách)  | Mượn thành công (slot thứ 3)                                           | REQ-04 | BVA         |
| TC-19 | Không cho mượn — sách đang được mượn (DT-R2)                           | Reset. Đăng nhập MEM003                               | 1. Tab Sách → 2. Chọn BOOK003 → 3. Nhấn "Mượn"      | Sách: BOOK003 (Đang mượn bởi MEM002) / Thành viên: MEM003                    | Từ chối. Nút "Mượn" bị vô hiệu hoặc thông báo không cho mượn          | REQ-04 | DT, EP      |
| TC-20 | Không cho mượn — sách thất lạc                                         | Reset. Đăng nhập MEM002                               | 1. Tab Sách → 2. Chọn BOOK020 → 3. Nhấn "Mượn"      | Sách: BOOK020 "Dẫn luận ngôn ngữ học" (Thất lạc) / Thành viên: MEM002        | Từ chối, không cho mượn                                                | REQ-04 | EP          |
| TC-21 | Không cho mượn — TK bị tạm ngưng (DT-R3)                               | Reset. Đăng nhập MEM004                               | 1. Tab Sách → 2. Chọn BOOK001 → 3. Nhấn "Mượn"      | Sách: BOOK001 (Có sẵn) / Thành viên: MEM004 `cu.le@email.com` (Tạm ngưng)    | Từ chối. Thông báo **tạm ngưng** (không phải "hết hạn")               | REQ-04 | DT, EP      |
| TC-22 | Không cho mượn — TK hết hạn (DT-R3)                                    | Reset. Đăng nhập MEM005                               | 1. Tab Sách → 2. Chọn BOOK001 → 3. Nhấn "Mượn"      | Sách: BOOK001 (Có sẵn) / Thành viên: MEM005 `binh.pham@email.com` (Hết hạn)  | Từ chối. Thông báo **hết hạn** (không phải "tạm ngưng")               | REQ-04 | DT, EP      |
| TC-23 | Không cho mượn — đã đủ 3 sách (DT-R4, BVA max)                         | Reset. Đăng nhập MEM006. Mượn thêm 2 sách để đủ 3    | 1. Chọn BOOK001 → 2. Nhấn "Mượn"                    | Sách: BOOK001 (Có sẵn) / Thành viên: MEM006 (đang mượn 3 sách)               | Từ chối. Thông báo **vượt giới hạn 3 sách**                           | REQ-04 | DT, BVA     |
| TC-24 | Không cho mượn — sách đang mượn + TK hợp lệ (DT-R2 confirm)           | Reset. Đăng nhập MEM002                               | 1. Chọn BOOK013 → 2. Nhấn "Mượn"                    | Sách: BOOK013 (Đang mượn bởi MEM006) / MEM002 (1 sách, hoạt động)            | Từ chối                                                                | REQ-04 | DT          |
| TC-25 | Không cho mượn — nhiều điều kiện sai cùng lúc (DT combo)               | Reset. Đăng nhập MEM004                               | 1. Chọn BOOK003 → 2. Nhấn "Mượn"                    | Sách: BOOK003 (Đang mượn) / MEM004 (Tạm ngưng)                               | Từ chối                                                                | REQ-04 | DT          |
| TC-26 | Kiểm tra hạn trả = ngày mượn + 14 ngày                                 | Reset. Đăng nhập MEM003                               | 1. Mượn BOOK001 → 2. Xem thông tin phiếu             | Ngày mượn = hôm nay                                                           | Hạn trả hiển thị = hôm nay + 14 ngày                                  | REQ-04 | EP          |

### REQ-05 — Trả sách (8 TC)

| Mã TC | Mục tiêu kiểm thử                                      | Tiền điều kiện                 | Bước thực hiện                                        | Dữ liệu đầu vào                                                            | Kết quả mong đợi                                       | REQ    | Kỹ thuật    |
|-------|--------------------------------------------------------|--------------------------------|-------------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------|--------|-------------|
| TC-27 | Trả sách thành công — trước hạn (DT-R6, BVA)          | Reset. Đăng nhập MEM006        | 1. Tab Mượn/Trả → 2. Chọn BR003 → 3. Nhấn "Trả"      | Phiếu: BR003 / MEM006 `biet.hoang@email.com` / returnDate < 15/10/2024     | Trả thành công. BOOK013 → **"Có sẵn"**. Không cảnh báo | REQ-05 | DT, BVA     |
| TC-28 | Trả sách thành công — đúng ngày hạn (BVA boundary)    | Reset. Đăng nhập MEM006        | 1. Tab Mượn/Trả → 2. Chọn BR003 → 3. Nhấn "Trả"      | Phiếu: BR003 / returnDate = dueDate (15/10/2024)                           | Trả thành công. Không có cảnh báo                      | REQ-05 | BVA         |
| TC-29 | Trả sách quá hạn — cảnh báo quá hạn (DT-R5, BVA)     | Reset. Đăng nhập MEM002        | 1. Tab Mượn/Trả → 2. Chọn BR001 → 3. Nhấn "Trả"      | Phiếu: BR001 / MEM002 `ba.nguyen@email.com` / returnDate > 15/09/2024      | Trả thành công + **cảnh báo quá hạn** hiển thị        | REQ-05 | DT, BVA     |
| TC-30 | Cập nhật trạng thái sách sau khi trả → "Có sẵn"       | Chạy TC-27. Đăng nhập bất kỳ  | 1. Tab Sách → 2. Tìm BOOK013                          | BOOK013 sau khi MEM006 trả BR003                                           | BOOK013 hiển thị trạng thái **"Có sẵn"**               | REQ-05 | EP          |
| TC-31 | Từ chối — phiếu mượn không tồn tại (DT-R2)            | Reset. Đăng nhập MEM006        | 1. Cố trả phiếu BR0038888 → 2. Quan sát               | Phiếu: `BR0038888` (không tồn tại) / MEM006                                | Từ chối + **thông báo lỗi** phiếu không tồn tại        | REQ-05 | DT, EP      |
| TC-32 | Từ chối — sai người trả (DT-R3)                       | Reset. Đăng nhập MEM002        | 1. Tìm BR003 trong tab Mượn/Trả của MEM002 → 2. Quan sát | Phiếu: BR003 (của MEM006) / Người trả: MEM002                          | BR003 không xuất hiện trong danh sách của MEM002       | REQ-05 | DT, EP      |
| TC-33 | Từ chối — thành viên không mượn sách đó (DT-R4)       | Reset. Đăng nhập MEM006        | 1. Cố trả BR002 → 2. Quan sát                         | Phiếu: BR002 (MEM003 đã trả, không còn active) / MEM006                    | Từ chối trả sách                                       | REQ-05 | DT, EP      |
| TC-34 | Từ chối — nhiều điều kiện sai (phiếu sai + sai người) | Reset. Đăng nhập MEM002        | 1. Cố trả BR0038888 → 2. Quan sát                     | Phiếu: `BR0038888` / Thành viên: MEM002                                    | Từ chối                                                | REQ-05 | DT          |
| TC-35 | Từ chối - Mem Đang Mươn có thể trả sách hộ Mem khác | Reset . Đăng nhập MEM006 |1. Vào mục Tra cứu phiếu mượn -> 2. Nhập thông tin Member active -> 3. Trả sách | Tra cứu : MEM002| Từ chối trả sách |REQ-05|EP|
### REQ-06 — Xử lý quá hạn

| Mã TC | Mục tiêu kiểm thử                                       | Tiền điều kiện               | Bước thực hiện                                                   | Dữ liệu đầu vào                                          | Kết quả mong đợi                                          | REQ    | Kỹ thuật |
|-------|---------------------------------------------------------|------------------------------|------------------------------------------------------------------|----------------------------------------------------------|-----------------------------------------------------------|--------|----------|
| TC-36 | Thủ thư kiểm tra quá hạn — BR001 chuyển "Quá hạn"      | Reset. Đăng nhập LIB001      | 1. Tab Mượn/Trả → 2. Nhấn "Kiểm tra quá hạn" → 3. Quan sát BR001 | LIB001 / BR001 (hạn 15/09/2024 < 19/05/2026)           | BR001 chuyển sang trạng thái **"Quá hạn"**                | REQ-06 | DT       |
| TC-36 | Thủ thư xem tất cả phiếu quá hạn (DT-R1)               | Chạy sau TC-35               | 1. Đăng nhập LIB001 → 2. Xem danh sách overdue                   | LIB001                                                   | Hiển thị tất cả phiếu có dueDate ≤ 19/05/2026            | REQ-06 | DT       |
| TC-37 | Thành viên thấy phiếu quá hạn của mình (DT-R2)         | Chạy sau TC-35               | 1. Đăng nhập MEM002 → 2. Tab Mượn/Trả → 3. Quan sát BR001        | MEM002 / BR001                                           | BR001 hiển thị trạng thái **"Quá hạn"** cho MEM002        | REQ-06 | DT       |
| TC-38 | Thành viên không xem được phiếu quá hạn người khác (DT-R3) | Reset. Đăng nhập MEM006   | 1. Tab Mượn/Trả → 2. Quan sát danh sách overdue                  | MEM006 / BR001 (của MEM002)                              | BR001 **không hiển thị** trong giao diện MEM006            | REQ-06 | DT       |
| TC-39 | Thành viên không có phiếu quá hạn — không hiển thị (DT-R4) | Reset. Đăng nhập MEM003   | 1. Tab Mượn/Trả → 2. Xem overdue                                 | MEM003 `dam.tran@email.com` — không có phiếu quá hạn    | Không hiển thị phiếu quá hạn nào                          | REQ-06 | DT       |
| TC-40 | BVA: dueDate < currentDate → đánh dấu quá hạn           | Reset. Đăng nhập LIB001      | 1. Nhấn "Kiểm tra quá hạn" → 2. Quan sát BR001                   | BR001: dueDate=15/09/2024, currentDate=19/05/2026 (< )   | BR001 **đánh dấu "Quá hạn"**                              | REQ-06 | BVA      |
| TC-41 | BVA: dueDate = currentDate → đánh dấu quá hạn           | Reset. Đăng nhập LIB001      | 1. Nhấn "Kiểm tra quá hạn" → 2. Quan sát phiếu hạn hôm nay       | Phiếu giả định: dueDate = ngày hôm nay (19/05/2026)      | Phiếu **đánh dấu "Quá hạn"** (SRS: ≤ currentDate)        | REQ-06 | BVA      |
| TC-42 | Thành viên không có quyền truy cập chức năng kiểm tra   | Reset. Đăng nhập MEM002      | 1. Tab Mượn/Trả → 2. Tìm nút "Kiểm tra quá hạn"                 | MEM002                                                   | **Không tồn tại** nút "Kiểm tra quá hạn" trên giao diện  | REQ-06 | EP       |

### REQ-07 — Quản lý thành viên (8 TC)

| Mã TC | Mục tiêu kiểm thử                         | Tiền điều kiện               | Bước thực hiện                                                      | Dữ liệu đầu vào                                                                             | Kết quả mong đợi                           | REQ    | Kỹ thuật |
|-------|-------------------------------------------|------------------------------|---------------------------------------------------------------------|---------------------------------------------------------------------------------------------|---------------------------------------------|--------|----------|
| TC-43 | Tạo thành viên thành công — all valid (DT-R1) | Reset. Đăng nhập LIB001  | 1. Tab Thành viên → 2. Nhấn "Thêm" → 3. Điền thông tin → 4. Submit | Name: `Nguyen Van A` / Email: `newuser@domain.com` / Phone: `0987654321`                   | Tạo thành công. Thành viên xuất hiện trong danh sách | REQ-07 | DT  |
| TC-44 | Thành viên không được thêm member (DT-R2) | Reset. Đăng nhập MEM002      | 1. Tìm tab Thành viên hoặc chức năng thêm → 2. Quan sát            | `ba.nguyen@email.com` (MEM002)                                                              | Không thấy tab "Thành viên" / Từ chối truy cập | REQ-07 | DT, EP |
| TC-45 | Họ tên rỗng (DT-R3)                       | Reset. Đăng nhập LIB001      | 1. Thêm member → 2. Để trống tên → 3. Submit                       | Name: `""` / Email: `newuser@domain.com` / Phone: `0987654321`                              | Từ chối + **thông báo lỗi họ tên**           | REQ-07 | DT, EP |
| TC-46 | Email thiếu `@` (DT-R4)                   | Reset. Đăng nhập LIB001      | 1. Thêm member → 2. Nhập email sai format → 3. Submit              | Name: `Nguyen Van A` / Email: `newuserdomain.com` / Phone: `0987654321`                    | Từ chối + **email không hợp lệ**             | REQ-07 | DT, EP |
| TC-47 | Email thiếu dấu `.` trong domain (DT-R4)  | Reset. Đăng nhập LIB001      | 1. Thêm member → 2. Nhập email sai format → 3. Submit              | Name: `Nguyen Van A` / Email: `newuser@domain` / Phone: `0987654321`                        | Từ chối + **email không hợp lệ**             | REQ-07 | DT, EP |
| TC-48 | Email rỗng (DT-R4)                        | Reset. Đăng nhập LIB001      | 1. Thêm member → 2. Để trống email → 3. Submit                     | Name: `Nguyen Van A` / Email: `""` / Phone: `0987654321`                                    | Từ chối + **email không hợp lệ**             | REQ-07 | EP     |
| TC-49 | Email đã tồn tại (DT-R5)                  | Reset. Đăng nhập LIB001      | 1. Thêm member → 2. Nhập email đã có → 3. Submit                   | Name: `Nguyen Van A` / Email: `dam.tran@email.com` *(MEM003 đã tồn tại)* / Phone: `0987654321` | Từ chối + **email đã tồn tại**           | REQ-07 | DT, EP |
| TC-50 | Số điện thoại rỗng (DT-R6)                | Reset. Đăng nhập LIB001      | 1. Thêm member → 2. Để trống SĐT → 3. Submit                      | Name: `Nguyen Van A` / Email: `newuser@domain.com` / Phone: `""`                           | Từ chối + **thông báo lỗi SĐT**              | REQ-07 | DT, EP |

### REQ-08 — Tra cứu phiếu mượn (9 TC, giảm từ 12)

| Mã TC | Mục tiêu kiểm thử                                     | Tiền điều kiện               | Bước thực hiện                                          | Dữ liệu đầu vào                                        | Kết quả mong đợi                                                      | REQ    | Kỹ thuật |
|-------|-------------------------------------------------------|------------------------------|---------------------------------------------------------|--------------------------------------------------------|-----------------------------------------------------------------------|--------|----------|
| TC-51 | Thủ thư xem tất cả phiếu mượn (DT-R1)                | Reset. Đăng nhập LIB001      | 1. Tab Mượn/Trả → 2. Quan sát danh sách tất cả phiếu   | `librarian@library.com`                                | Hiển thị tất cả 5 phiếu: **BR001, BR002, BR003, BR004, BR005**        | REQ-08 | DT       |
| TC-52 | Thành viên chỉ xem phiếu của mình (DT-R2)            | Reset. Đăng nhập MEM002      | 1. Tab Mượn/Trả → 2. Quan sát                           | `ba.nguyen@email.com` (MEM002)                         | Chỉ thấy **BR001 và BR004** (phiếu của MEM002). Không thấy BR002, BR003, BR005 | REQ-08 | DT |
| TC-53 | Thành viên không xem được phiếu người khác (DT-R3)   | Reset. Đăng nhập MEM006      | 1. Tab Mượn/Trả → 2. Kiểm tra có MEM002 không  ?        | MEM006 `biet.hoang@email.com` / BR004 (của MEM002)     | BR004 **không xuất hiện** trong giao diện MEM006                      | REQ-08 | DT       |
| TC-54 | Phiếu không tồn tại (DT-R4)                          | Reset. Đăng nhập LIB001      | 1. Tra cứu theo mã phiếu → 2. Nhập BR999               | Mã phiếu: `BR999`                                      | Thông báo **không tìm thấy phiếu**                                    | REQ-08 | EP       |
| TC-55 | Hiển thị trạng thái "Đang mượn"                       | Reset. Đăng nhập LIB001      | 1. Tab Mượn/Trả → 2. Xem BR003                          | BR003 (MEM006, BOOK013, đang mượn)                     | Hiển thị trạng thái **"Đang mượn"**                                   | REQ-08 | EP       |
| TC-56 | Hiển thị trạng thái "Đã trả"                          | Reset. Đăng nhập LIB001      | 1. Tab Mượn/Trả → 2. Xem BR002                          | BR002 (MEM003, BOOK001, đã trả 20/08/2024)             | Hiển thị trạng thái **"Đã trả"**                                      | REQ-08 | EP       |
| TC-57 | Hiển thị trạng thái "Quá hạn" (sau khi TT kiểm tra)  | Chạy sau TC-35. Đăng nhập LIB001 | 1. Tab Mượn/Trả → 2. Xem BR001                     | BR001 (MEM002, BOOK003, hạn 15/09/2024)                | Hiển thị trạng thái **"Quá hạn"**                                     | REQ-08 | EP       |
| TC-58 | Thành viên có lịch sử mượn — hiển thị danh sách      | Reset. Đăng nhập MEM002      | 1. Tab Mượn/Trả → 2. Quan sát                           | MEM002 (có BR001 đang mượn + BR004 đã trả)             | Hiển thị danh sách phiếu **(BR001, BR004)**                           | REQ-08 | EP       |
| TC-59 | Kiểm tra thông tin phiếu hiển thị đầy đủ các trường  | Reset. Đăng nhập LIB001      | 1. Tab Mượn/Trả → 2. Xem chi tiết BR003                 | BR003 (MEM006, BOOK013, mượn 01/10/2024, hạn 15/10/2024) | Hiển thị đủ: **Mã phiếu, Sách, Ngày mượn, Ngày hết hạn, Trạng thái** | REQ-08 | EP       |

---

## Tổng hợp

| Nhóm chức năng               | Số TC sau tối ưu | REQ phủ | Kỹ thuật áp dụng |
|------------------------------|-----------------|---------|------------------|
| REQ-01: Đăng nhập            | **6**           | REQ-01  | EP               |
| REQ-02: Xem danh sách sách   | **4**         | REQ-02  | EP               |
| REQ-03: Tìm kiếm sách        | **5**           | REQ-03  | EP               |
| REQ-04: Mượn sách            | **11**        | REQ-04  | EP, BVA, DT      |
| REQ-05: Trả sách             | **8** | REQ-05 | EP, BVA, DT   |
| REQ-06: Xử lý quá hạn        | **8** | REQ-06  | EP, BVA, DT      |
| REQ-07: Quản lý thành viên   | **8**     | REQ-07  | EP, DT           |
| REQ-08: Tra cứu phiếu mượn   | **9** | REQ-08 | EP, DT      |
| **Tổng**                     | **59** |     REQ-01 → REQ-08 | EP, BVA, DT |
