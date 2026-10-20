#  BÀI TẬP: KHẮC PHỤC LỖI TƯ DUY DATABASE VÀ PHỤC HỒI AUTO-FILTER TRONG GOOGLE SHEETS



## PHẦN 1: PHÂN TÍCH LỖI

### 1. Liệt kê đầy đủ từng lỗi trong bảng quản lý báo cáo viên

Dựa trên cấu trúc bảng hiện tại, hệ thống quản lý dữ liệu của Ban Nội dung đang vi phạm nghiêm trọng các Quy tắc Vàng trong thiết kế Cơ sở dữ liệu (Database) trên bảng tính:

* **Lỗi Ô A1 (Tiêu đề chung đặt liền trong bảng dữ liệu):** Dòng đầu tiên chứa tiêu đề lớn *"DANH SÁCH BÁO CÁO VIÊN — HỘI NGHỊ KHKH 2026"* nằm ngay liền kề với bảng dữ liệu mà không có dòng trống tách biệt.
* **Vi phạm:** **Quy tắc số (1)** – *Hàng đầu tiên của vùng dữ liệu bắt buộc phải là tên cột (Header), không gộp ô, không chứa tiêu đề văn bản chung ở dòng đầu bảng dữ liệu.*
* **Hậu quả với AutoFilter:** Bộ lọc tự động (`AutoFilter`) nhận diện nhầm dòng tiêu đề văn bản này làm tên cột, dẫn đến việc xác định sai phạm vi dữ liệu và không thể lọc chính xác theo các trường mong muốn.


* **Lỗi hàng trống và dòng tổng kết chen ngang giữa dữ liệu (Dòng chứa `"TỔNG SỐ CNTT: 45"` và các hàng trống):**
* **Vi phạm:** **Quy tắc số (4)** – *Tuyệt đối không có hàng trống hoặc cột trống ngắt quãng giữa dữ liệu* và **Quy tắc số (5)** – *Dòng tổng/average phải đặt ngoài bảng dữ liệu, tuyệt đối không nằm chèn giữa các bản ghi.*
* **Hậu quả với AutoFilter:** Theo cơ chế hoạt động của tính năng `AutoFilter`, hệ thống chỉ quét vùng dữ liệu liên tục cho đến khi gặp **hàng trống đầu tiên** hoặc dòng tổng kết xen kẽ. Do có hàng trống và dòng thống kê nằm ngay giữa bảng, bộ lọc bị ngắt quãng, chỉ quét được vài dòng ở phía trên và bỏ sót hoàn toàn ~115 bản ghi phía dưới. Đây là nguyên nhân chính khiến trưởng ban chỉ lọc được 2 kết quả ngẫu nhiên thay vì ~35 người dự kiến.



---

### 2. Tại sao ô B3 chứa `"Trần Thị Bình  "` (có khoảng trắng cuối) khiến Filter không khớp?

* **Cơ chế so sánh chuỗi ký tự của Filter:** Công cụ `AutoFilter` thực hiện việc so khớp dữ liệu theo **từng ký tự chính xác tuyệt đối (Exact Match)**.
* **Nguyên nhân:** Ký tự khoảng trắng (`" "`) cũng được máy tính tính là một ký tự hợp lệ trong bảng mã. Chuỗi `"Trần Thị Bình  "` (có khoảng trắng thừa ở cuối) có độ dài và mã định danh khác biệt hoàn toàn với chuỗi chuẩn `"Trần Thị Bình"`.
* Khi thực hiện lọc, bộ lọc không tự động cắt bỏ khoảng trắng thừa ẩn phía sau. Do đó, hệ thống xem đây là hai giá trị hoàn toàn khác nhau, dẫn đến việc dữ liệu bị bỏ sót hoặc không khớp khi lọc theo điều kiện.

---

## PHẦN 2: SỬA LỖI & NGHIỆM THU

### 3. Cấu trúc bảng đúng chuẩn (Flat Database)

Để khắc phục hoàn toàn các lỗi trên, cấu trúc bảng tính cần được chuẩn hóa lại theo mô hình cơ sở dữ liệu phẳng như sau:

* **Phần Tiêu đề chung:** Nên đặt ở các dòng phía trên (ví dụ dòng 1 và 2), sau đó **cách ra ít nhất 1 dòng trống** rồi mới bắt đầu bảng dữ liệu chính để tránh làm nhiễu bộ lọc.
* **Hàng tiêu đề cột (Header) - Đặt tại Dòng 4 (Cố định ở 1 dòng đơn, không gộp ô):**
* **Cột A:** `STT`
* **Cột B:** `Họ và Tên`
* **Cột C:** `Khoa / Viện`
* **Cột D:** `Điểm Xét Duyệt`
* **Cột E:** `Trạng Thái`


* **Vùng Dữ liệu chính (Data Rows) - Từ Dòng 5 đến Dòng 124:** Mỗi dòng là một bản ghi duy nhất, liên tục không có hàng trống ngắt quãng, không gộp ô, dữ liệu đồng nhất kiểu định dạng.
* **Dòng Tổng kết (Summary Row) - Đặt ở Dòng 126 (Nằm ngoài bảng):** Cách dòng dữ liệu cuối cùng ít nhất 1 dòng trống, đặt công thức tổng hợp (ví dụ: `=COUNTA(B5:B124)`).

**Mô tả cấu trúc bảng chuẩn (Bảng minh họa):**

| STT (Cột A) | Họ và Tên (Cột B) | Khoa / Viện (Cột C) | Điểm Xét Duyệt (Cột D) | Trạng Thái (Cột E) |
| --- | --- | --- | --- | --- |
| **1** | Nguyễn Văn An | CNTT | 8.5 | Đã duyệt |
| **2** | Trần Thị Bình | CNTT | 7.9 | Chờ duyệt |
| **3** | Lê Minh Châu | Điện tử | 8.2 | Đã duyệt |
| ... | ... | ... | ... | ... |

*(Lưu ý tuyệt đối: Không có dòng tổng kết hay hàng trống nào nằm xen kẽ từ dòng 5 đến dòng 124).*

---

### 4. Công thức hàm `TRIM()` và Thao tác lọc đúng

#### **a. Công thức hàm `TRIM()` làm sạch cột "Họ và Tên":**

Để loại bỏ triệt để các khoảng trắng thừa ở đầu, cuối hoặc khoảng trắng kép ở giữa chuỗi dữ liệu (như ô B3), ta thực hiện:

1. Chèn thêm một cột phụ tạm thời bên cạnh cột Họ và Tên (ví dụ cột F).
2. Tại ô F5, nhập công thức làm sạch:
`=TRIM(B5)`
3. Kéo công thức xuống cho toàn bộ các dòng dữ liệu (đến dòng 124).
4. **Cố định giá trị:** Sao chép toàn bộ cột F vừa xử lý, nhấp chuột phải vào cột B gốc (`Họ và Tên`), chọn **Paste special ➔ Paste values only** (Dán giá trị) để ghi đè dữ liệu sạch lên, sau đó xóa cột phụ F đi.

#### **b. Thao tác lọc đúng để lấy danh sách báo cáo viên CNTT có điểm từ 8.0 trở lên:**

1. Đảm bảo bảng dữ liệu đã được dọn sạch hàng trống, gộp ô và đặt tiêu đề cột chuẩn ở dòng 4.
2. Bôi đen toàn bộ vùng bảng dữ liệu (từ A4 đến E124) và nhấn tổ hợp phím **`Ctrl + Shift + L`** (trên Excel) hoặc vào *Data ➔ Create a filter* (trên Google Sheets) để bật tính năng `AutoFilter`.
3. **Lọc theo Khoa / Viện:** Nhấp vào biểu tượng bộ lọc ở cột **Khoa / Viện** (Cột C), chọn mục lọc và chỉ tích chọn giá trị `"CNTT"`.
4. **Lọc theo Điểm Xét Duyệt:** Nhấp vào biểu tượng bộ lọc ở cột **Điểm Xét Duyệt** (Cột D), chọn *Filter by condition* (hoặc *Number Filters*) ➔ Chọn điều kiện **Greater than or equal to** ($\ge$) và nhập số `8.0`.
5. Hệ thống sẽ tự động giao (AND) hai điều kiện trên và trả về chính xác danh sách ~35 báo cáo viên thuộc khoa CNTT có điểm xét duyệt từ 8.0 trở lên một cách hoàn hảo.