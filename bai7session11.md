#  BÀI TẬP: KHẮC PHỤC LỖI HEADING STYLES VÀ TỰ ĐỘNG HÓA MỤC LỤC (HỘI NGHỊ KHOA HỌC SINH VIÊN PTIT 2026)


## PHẦN 1: PHÂN TÍCH LỖI CẤU TRÚC VÀ CƠ CHẾ HOẠT ĐỘNG CỦA AUTO TOC

### 1. Xác định lỗi cụ thể của từng dòng tiêu đề và cơ chế quét của Auto TOC
* **Lỗi hiện tại trong file:** Toàn bộ 7 dòng tiêu đề trong kế hoạch tổ chức hội nghị đang sử dụng định dạng thủ công (Manual Formatting) như thay đổi cỡ chữ, in đậm, tô vàng, đổi màu thủ công trên nền kiểu chữ mặc định `Normal`. Không có dòng nào được gán nhãn cấu trúc hệ thống (Heading Styles: Heading 1, Heading 2, Heading 3).
* **Cơ chế hoạt động của tính năng Auto TOC (Table of Contents):** 
  * Tính năng tạo mục lục tự động trong Microsoft Word không quét văn bản dựa trên các đặc điểm trực quan (như chữ to, chữ in đậm hay màu sắc mà mắt người nhìn thấy).
  * Thay vào đó, bộ quét của Word hoạt động bằng cách tìm kiếm và tổng hợp các đoạn văn bản được **gán nhãn cấu trúc hệ thống (Heading Styles)**.
  * Do toàn bộ các tiêu đề đang ở dạng `Normal` hoặc định dạng thủ công, Word hoàn toàn "mù" trước các tiêu đề này, dẫn đến kết quả trả về bảng mục lục bị rỗng hoặc chỉ hiển thị 1 dòng trống (`No table of contents entries found`).

### 2. Phân tích lỗi nhầm cấp Heading (Dòng '2.1.1. Tiểu ban Kỹ thuật')
* **Vấn đề:** Dòng `2.1.1. Tiểu ban Kỹ thuật` là một mục con chi tiết cấp 3 (chi tiết nhỏ hơn của Ban Nội dung), nhưng lại bị gán nhầm cấp hoặc thiếu đồng bộ cấu trúc.
* **Hậu quả về mặt thụt đầu dòng (Indentation) trong mục lục:**
  * Trong bảng mục lục tự động, Word tự động phân cấp thụt lề dựa vào cấp độ Heading: `Heading 1` nằm sát lề trái, `Heading 2` thụt vào 1 mức tab, và `Heading 3` thụt sâu hơn nữa.
  * Nếu gán nhầm cấp độ (ví dụ đưa H3 lên H2), mục chi tiết này sẽ bị hiển thị ngang hàng với mục chính, làm sai lệch hoàn toàn hệ thống phân cấp thông tin, gây rối mắt và làm giảm tính chuyên nghiệp của tài liệu hội nghị cấp khoa/trường.

---

## PHẦN 2: HƯỚNG DẪN SỬA LỖI VÀ NGHIỆM THU

### 1. Các bước thao tác chuẩn để sửa cấu trúc Heading cho 7 dòng tiêu đề
Để khôi phục toàn bộ cấu trúc phân cấp và giúp Auto TOC hiển thị chuẩn 3 cấp (Chương → Mục → Chi tiết), ta thực hiện lần lượt các bước sau:

1. **Gán nhãn Cấp 1 (Heading 1) cho các Chương lớn:**
   - Chọn dòng *"1. Thông tin chung hội nghị"* và *"2. Cơ cấu ban tổ chức"*, nhấn phím tắt **`Ctrl + Alt + 1`** (hoặc chọn `Heading 1` trên tab Home).
   - Chọn dòng *"3. Lịch trình sự kiện"*, nhấn phím tắt **`Ctrl + Alt + 1`** để đưa lên cấp chương chính.
2. **Gán nhãn Cấp 2 (Heading 2) cho các mục phần con:**
   - Chọn các dòng *"1.1. Tên hội nghị"*, *"1.2. Thời gian & địa điểm"*, và *"2. Ban Nội dung"*, nhấn phím tắt **`Ctrl + Alt + 2`** (hoặc chọn `Heading 2` trên tab Home).
3. **Gán nhãn Cấp 3 (Heading 3) cho mục chi tiết nhỏ:**
   - Chọn dòng *"2.1.1. Tiểu ban Kỹ thuật"*, nhấn phím tắt **`Ctrl + Alt + 3`** (hoặc chọn `Heading 3` trên tab Home) để đưa về đúng cấp độ chi tiết con.
4. **Chèn lại Bảng Mục lục tự động (Auto TOC):**
   - Xóa bỏ mục lục cũ bị lỗi.
   - Đặt con trỏ chuột tại trang đầu tiên, vào tab **References** ➔ **Table of Contents** ➔ Chọn mẫu *Automatic Table 1*. Lúc này bảng mục lục sẽ hiển thị chính xác toàn bộ 3 cấp phân cấp rõ ràng.

### 2. Xử lý Bẫy dữ liệu: Cập nhật số trang khi nội dung bị thay đổi
* **Tình huống:** Sau khi viết thêm nội dung, mục *"3. Lịch trình sự kiện"* bị dịch chuyển từ trang 2 sang trang 4. Nếu không thực hiện thêm thao tác nào, số trang hiển thị trong bảng mục lục vẫn sẽ ghi nhận là trang 2 (sai lệch hoàn toàn so với thực tế).
* **Giải pháp & Thao tác khắc phục:**
  - Nhấp chuột phải trực tiếp vào bảng mục lục tự động (Auto TOC).
  - Chọn **Update Field** (Cập nhật trường).
  - Chọn **Update entire table** (Cập nhật toàn bộ bảng) để Word quét lại toàn bộ tài liệu và cập nhật chính xác số trang mới nhất cho từng mục.