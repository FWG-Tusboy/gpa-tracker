# F4 – Sửa và xoá môn học

Nguồn: PRD §2 F4. Phụ thuộc: F1 (dùng chung form và validation).

Mỗi dòng trong bảng có 2 nút: **Sửa** và **Xoá**. Trạng thái "đang sửa" được lưu bằng `editingId` (id của môn đang sửa, hoặc rỗng).

```mermaid
flowchart TD
    Start(["Người dùng bấm nút trên một dòng"]) --> Which{"Bấm nút nào?"}

    %% ---------- Sửa ----------
    Which -->|Sửa| Fill["Đưa Tên môn, Số tín chỉ, Điểm của dòng lên form"]
    Fill --> SetEdit["Ghi nhớ editingId = id của dòng; đổi nút 'Thêm môn' thành 'Lưu'"]
    SetEdit --> Highlight["Đánh dấu dòng đang sửa trong bảng; con trỏ vào ô Tên môn"]
    Highlight --> EditInput[/"Người dùng chỉnh sửa dữ liệu"/]
    EditInput --> Save{"Nhấn 'Lưu' hoặc Enter?"}
    Save -->|Chưa| EditInput
    Save -->|Có| Validate[["Validation giống F1"]]
    Validate --> Valid{"Hợp lệ?"}
    Valid -->|Không| ShowErr["Hiển thị lỗi dưới ô tương ứng, vẫn ở chế độ Sửa"]
    ShowErr --> EditInput
    Valid -->|Có| Update["Làm tròn điểm; cập nhật môn có id = editingId trong danh sách"]
    Update --> ExitEdit["Thoát chế độ Sửa: editingId rỗng, nút trở lại 'Thêm môn', bỏ đánh dấu dòng"]
    ExitEdit --> ResetForm["Xoá trắng form, con trỏ vào ô Tên môn"]
    ResetForm --> Refresh

    %% ---------- Xoá ----------
    Which -->|Xoá| Confirm{"Hộp xác nhận: 'Xoá môn ... ?'"}
    Confirm -->|Huỷ| End(["Kết thúc"])
    Confirm -->|Đồng ý| Remove["Xoá môn khỏi danh sách theo id"]
    Remove --> WasEditing{"Môn bị xoá đang được sửa?"}
    WasEditing -->|Có| ExitEdit2["Thoát chế độ Sửa và xoá trắng form"]
    WasEditing -->|Không| Refresh
    ExitEdit2 --> Refresh

    %% ---------- Cập nhật chung ----------
    Refresh[["F2: Quy đổi điểm cho bảng"]] --> Persist[["F5: Lưu danh sách vào localStorage"]]
    Persist --> Recalc[["F3: Tính lại GPA và xếp loại"]]
    Recalc --> Render["Vẽ lại bảng môn học"]
    Render --> End
```

## Ghi chú thiết kế
- **Dùng chung một form** cho Thêm và Sửa; `editingId` quyết định khi submit sẽ thêm mới (F1) hay cập nhật (F4).
- **Bấm "Sửa" ở dòng khác khi đang sửa**: dữ liệu dòng mới ghi đè lên form, `editingId` chuyển sang dòng mới (thay đổi chưa lưu của dòng cũ bị bỏ).
- **Xoá đúng dòng đang sửa**: thoát chế độ Sửa để form không trỏ tới môn đã không còn.
- Hộp xác nhận xoá dùng `window.confirm` (đơn giản, có sẵn trên mọi trình duyệt), nội dung có tên môn để tránh xoá nhầm.
- Xoá theo **id**, không theo vị trí hay tên, nên an toàn khi trùng tên môn.

## Câu hỏi mở
- PRD không nhắc nút **"Huỷ"** khi đang sửa. Đề xuất: thêm nút "Huỷ" (hoặc phím Esc) để thoát chế độ Sửa mà không lưu. Cần người dùng xác nhận trước khi làm ở Stage 3.
