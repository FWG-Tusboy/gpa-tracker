# CLAUDE.md – GPA Tracker

## Quy trình phát triển: Waterfall model

Dự án tuân theo mô hình **Waterfall**: hoàn thành từng stage theo thứ tự, không nhảy sang stage sau khi stage trước chưa xong.

### Stage 1 – Requirements ✅ (đã xong)
- Tài liệu yêu cầu: [`PRD.md`](PRD.md). Đây là nguồn sự thật cho mọi chức năng (F1, F2, ...).

### Stage 2 – Thiết kế luồng nghiệp vụ
- Vẽ flow chart cho từng chức năng trong PRD bằng cú pháp **Mermaid**.
- Mỗi chức năng là **1 file** riêng, lưu trong thư mục `design/flows/`
  (ví dụ: `design/flows/F1-them-mon-hoc.md`).
- Flow chart phải bám sát Flow và Validation Rules trong PRD.

### Stage 3 – Thiết kế & lập trình chức năng
- **Trước khi bắt đầu Stage 3:** cài đặt các tools và thư viện cần thiết cho project.
- Làm lần lượt **từng function một**, với mỗi function:
  1. Thiết kế và code chức năng.
  2. Viết unit test cho chức năng đó.
  3. Chạy test và đảm bảo **tất cả test pass**.
  4. Yêu cầu người dùng kiểm tra trên **browser**. Chỉ khi người dùng xác nhận thành công mới chuyển sang function tiếp theo.
  5. Sau khi xong function, kể **1 computer joke** liên quan đến function vừa làm.

## Xử lý lỗi
- **Đầu mỗi session:** đọc file `common_errors.md` (nếu có) để tránh lặp lại lỗi cũ.
- **Mỗi khi gặp lỗi:** sau khi sửa xong, báo cho người dùng chạy command `/learn-by-mistake` để lưu lỗi vào `common_errors.md`.
