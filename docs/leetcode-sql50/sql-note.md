---
sidebar_position: 1
---

# SQL Note

## 1. Common syntax

### 1.1. SELECT

*   Dùng để chỉ định các field (cột) muốn truy vấn. Có thể chọn nhiều field, ngăn cách bằng dấu `,`. Thứ tự liệt kê trong `SELECT` chính là thứ tự dữ liệu trả về.
*   `*` để chọn toàn bộ cột trong bảng.
*   `AS` dùng để đặt tên mới (alias) cho cột.
*   `SELECT DISTINCT` để trả về các giá trị unique (không trùng).
*   Có thể kết hợp nhiều field với `DISTINCT` để lấy tổ hợp duy nhất.

**Câu lệnh kết hợp:** `SELECT DISTINCT [tên trường ban đầu] AS [tên mới]`

### 1.2. FROM

*   Dùng để chỉ định bảng dữ liệu sẽ truy vấn
*   Có thể kết hợp nhiều bảng bằng các loại `JOIN`.

### 1.3. WHERE

*   Lọc dữ liệu theo điều kiện
*   Hỗ trợ các toán tử:
    *   So sánh: `=`, `>`, `<`, `>=`, `<=`, `<>`
    *   Logic: `AND`, `OR`, `NOT`
    *   Tìm kiếm: `LIKE`, `IN`, `BETWEEN`
    *   Kiểm tra giá trị `NULL`: `IS NULL`, `IS NOT NULL`

### 1.4. ORDER BY

*   Sắp xếp kết quả theo một hoặc nhiều cột. `ASC` (tăng dần – mặc định), `DESC` (giảm dần).

### 1.5. GROUP BY

*   Gom nhóm dữ liệu theo 1 hoặc nhiều cột.
*   Thường kết hợp với các hàm tổng hợp (`SUM`, `COUNT`, `AVG`, `MIN`, `MAX`).

### 1.6. HAVING

*   Lọc dữ liệu sau khi đã `GROUP BY`.
*   Khác với `WHERE` (lọc trước khi nhóm).

### 1.7. LIMIT / TOP / OFFSET

*   Giới hạn số dòng trả về.
*   `LIMIT n` (MySQL, PostgreSQL), `TOP n` (SQL Server).
*   `OFFSET` dùng để bỏ qua số dòng đầu.

## 2. SQL Execution Order

SQL không chạy theo thứ tự mình viết mà theo logic nội bộ.

**Thứ tự xử lý:**
1.  **FROM** (xác định bảng)
2.  **ON** (điều kiện JOIN)
3.  **JOIN** (kết nối bảng)
4.  **WHERE** (lọc dữ liệu)
5.  **GROUP BY** (gom nhóm)
6.  **HAVING** (lọc nhóm)
7.  **SELECT** (chọn cột, hàm tính)
8.  **DISTINCT** (loại bỏ trùng lặp)
9.  **ORDER BY** (sắp xếp)
10. **LIMIT/OFFSET** (giới hạn số dòng)

## 3. Các loại joins

*   **INNER JOIN**: chỉ lấy dòng có dữ liệu khớp ở cả hai bảng
*   **LEFT JOIN**: lấy toàn bộ bảng bên trái, bảng phải có thể `NULL`
*   **RIGHT JOIN**: ngược lại `LEFT JOIN`
*   **FULL OUTER JOIN**: lấy hết cả hai bảng, kể cả không khớp
*   **CROSS JOIN**: nhân Cartesian, tất cả tổ hợp có thể

## 4. Union

*   Dùng để gộp kết quả từ nhiều query.
*   **UNION**: loại bỏ trùng lặp.
*   **UNION ALL**: giữ lại tất cả (nhanh hơn).
*   **Yêu cầu**: số lượng cột và kiểu dữ liệu phải giống nhau.

| | JOIN | UNION |
| :---: | :---: | :---: |
| **Mục đích** | gắn dữ liệu từ **nhiều bảng khác nhau** theo một **khóa chung** (common key). | kết hợp kết quả từ **nhiều query giống cấu trúc**. |
| **Kết quả** | thêm **cột mới** (mở rộng theo chiều ngang). | thêm **dòng mới** (mở rộng theo chiều dọc). |
| **Điều kiện** | cần có mối quan hệ giữa các bảng (thường là khóa chính – khóa ngoại). | số lượng cột và kiểu dữ liệu trong các query phải giống nhau. |

## 5. CTEs (Common Table Expressions)

*   CTE là bảng tạm, tạo bằng `WITH ... AS (...)`.
*   Khác với View: View lưu lại định nghĩa, CTE chỉ tồn tại trong query.
*   Giúp chia nhỏ query phức tạp, dễ đọc và tái sử dụng.
*   Có thể đệ quy để sinh dữ liệu.

## 6. Aggregations (SUM, COUNT, MIN, MAX, AVG)

*   `SUM()`: tính tổng
*   `COUNT()`: đếm số dòng
*   `MIN()`: giá trị nhỏ nhất
*   `MAX()`: giá trị lớn nhất
*   `AVG()`: tính trung bình
*   Dùng chung với `GROUP BY` để gom nhóm.
*   `AVG()` và `SUM()` chỉ sử dụng trên trường số (numberical field) còn `COUNT()`, `MIN()`, `MAX()` có thể sử dụng cho các trường không phải số. Thấp nhất (`MIN`) có thể là bắt đầu từ chữ cái A khi xử lý các chuỗi (string) hoặc là ngày sớm nhất khi xử lý dữ liệu thời gian.
*   `COUNT(*)` ≠ `COUNT(column)` (cột có `NULL` sẽ không tính).
*   Có thể kết hợp với `CASE WHEN` để tính conditional aggregation.

## 7. Case when

*   Dùng để viết điều kiện (if/else) trong SQL.
*   Có thể phân loại dữ liệu hoặc tính toán có điều kiện.
