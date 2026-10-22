# F5 – Lưu dữ liệu

Nguồn: PRD §2 F5. Phụ thuộc: F1, F4.

Key localStorage: `gpa-tracker:v1`. F5 gồm 3 luồng: **tải khi mở trang**, **lưu khi có thay đổi**, và **xoá tất cả**.

## 5.1 Tải dữ liệu khi mở trang

```mermaid
flowchart TD
    Start(["Mở hoặc tải lại trang"]) --> Read["Đọc localStorage key 'gpa-tracker:v1'"]
    Read --> ReadOk{"Đọc được, không báo lỗi?"}
    ReadOk -->|Không| Empty["Khởi tạo danh sách rỗng"]
    ReadOk -->|Có| HasData{"Có dữ liệu?"}
    HasData -->|Không| Empty
    HasData -->|Có| Parse{"Phân tích JSON thành công?"}
    Parse -->|Không| Warn["Ghi cảnh báo vào console, bỏ qua dữ liệu hỏng"]
    Parse -->|Có| IsArray{"Là một mảng?"}
    IsArray -->|Không| Warn
    IsArray -->|Có| Items{"Mọi môn đều hợp lệ: có id, tên 1–100 ký tự, tín chỉ nguyên 1–10, điểm 0–10?"}
    Items -->|Không| Warn
    Items -->|Có| Load["Dùng danh sách đã đọc"]
    Warn --> Empty
    Empty --> Render
    Load --> Render
    Render["Vẽ bảng môn học"] --> Calc[["F3: Tính GPA và xếp loại"]]
    Calc --> End(["Trang sẵn sàng"])
```

## 5.2 Lưu khi danh sách thay đổi

Được gọi sau mỗi lần thêm (F1), sửa, xoá (F4), xoá tất cả (5.3).

```mermaid
flowchart TD
    Start(["Danh sách vừa thay đổi"]) --> Serialize["Chuyển danh sách thành JSON: id, tên, tín chỉ, điểm hệ 10"]
    Serialize --> Write["Ghi vào localStorage key 'gpa-tracker:v1'"]
    Write --> WriteOk{"Ghi thành công?"}
    WriteOk -->|Có| End(["Kết thúc"])
    WriteOk -->|Không| Warn["Ghi cảnh báo vào console; ứng dụng vẫn chạy với dữ liệu trong bộ nhớ"]
    Warn --> End
```

## 5.3 Xoá tất cả

```mermaid
flowchart TD
    Start(["Bấm nút 'Xoá tất cả'"]) --> HasItems{"Danh sách có môn nào?"}
    HasItems -->|Không| End(["Kết thúc"])
    HasItems -->|Có| Confirm{"Hộp xác nhận: 'Xoá tất cả môn học?'"}
    Confirm -->|Huỷ| End
    Confirm -->|Đồng ý| Clear["Danh sách = rỗng"]
    Clear --> ExitEdit["Nếu đang sửa: thoát chế độ Sửa, xoá trắng form"]
    ExitEdit --> Save[["5.2: Lưu danh sách rỗng"]]
    Save --> Calc[["F3: Hiển thị '—' và dòng hướng dẫn"]]
    Calc --> Render["Vẽ lại bảng rỗng"]
    Render --> End
```

## Ghi chú thiết kế
- **Chỉ lưu dữ liệu gốc** (id, tên, tín chỉ, điểm hệ 10). Điểm chữ, hệ 4, GPA luôn tính lại (F2, F3).
- **Dữ liệu hỏng thì bỏ cả danh sách**, đúng PRD "bỏ qua, khởi tạo danh sách rỗng". Không cố cứu từng môn, vì làm vậy có thể ra GPA sai mà người dùng không biết.
- Mọi thao tác với localStorage nằm trong `try/catch`: trình duyệt chặn storage (chế độ riêng tư, hết dung lượng) không được làm trắng trang.
- Lỗi khi đọc/ghi chỉ ghi `console.warn`, không hiện thông báo cho người dùng (PRD không yêu cầu).
- Nút "Xoá tất cả" khi danh sách rỗng: không hỏi xác nhận, không làm gì (có thể làm mờ nút).
