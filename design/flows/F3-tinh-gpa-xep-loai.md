# F3 – Tính GPA và xếp loại

Nguồn: PRD §2 F3. Phụ thuộc: F2. Được gọi mỗi khi danh sách môn thay đổi: thêm (F1), sửa/xoá (F4), tải trang, xoá tất cả (F5).

```mermaid
flowchart TD
    Start(["Danh sách môn thay đổi"]) --> Empty{"Danh sách rỗng?"}
    Empty -->|Có| ShowEmpty["Hiển thị '—' cho tổng số môn, tổng tín chỉ, tín chỉ đạt, GPA hệ 4, GPA hệ 10, xếp loại"]
    ShowEmpty --> Hint["Hiển thị dòng hướng dẫn 'Hãy thêm môn học đầu tiên'"]
    Hint --> End(["Kết thúc"])

    Empty -->|Không| Init["Khởi tạo: tổng tín chỉ = 0, tín chỉ đạt = 0, tổng hệ 4 = 0, tổng hệ 10 = 0"]
    Init --> Next{"Còn môn chưa duyệt?"}
    Next -->|Có| Convert[["F2: Lấy điểm hệ 4 của môn"]]
    Convert --> Accum["Cộng dồn: tổng tín chỉ += tín chỉ; tổng hệ 4 += hệ 4 × tín chỉ; tổng hệ 10 += hệ 10 × tín chỉ"]
    Accum --> Passed{"Điểm hệ 10 ≥ 4.0?"}
    Passed -->|Có| AddPassed["Tín chỉ đạt += tín chỉ"]
    Passed -->|Không| Next
    AddPassed --> Next

    Next -->|Không| Calc["GPA hệ 4 = tổng hệ 4 / tổng tín chỉ; GPA hệ 10 = tổng hệ 10 / tổng tín chỉ; làm tròn 2 chữ số"]
    Calc --> X{"GPA hệ 4 ≥ 3.6?"}
    X -->|Có| RX["Xuất sắc"]
    X -->|Không| G{"GPA hệ 4 ≥ 3.2?"}
    G -->|Có| RG["Giỏi"]
    G -->|Không| K{"GPA hệ 4 ≥ 2.5?"}
    K -->|Có| RK["Khá"]
    K -->|Không| TB{"GPA hệ 4 ≥ 2.0?"}
    TB -->|Có| RTB["Trung bình"]
    TB -->|Không| RY["Yếu"]

    RX --> Show
    RG --> Show
    RK --> Show
    RTB --> Show
    RY --> Show
    Show["Hiển thị: tổng số môn, tổng tín chỉ, tín chỉ đạt, GPA hệ 4, GPA hệ 10, xếp loại; ẩn dòng hướng dẫn"] --> End
```

## Ghi chú thiết kế
- **Tín chỉ đạt** = tổng tín chỉ các môn có điểm hệ 10 ≥ 4.0 (tức không phải F).
- **GPA tính trên tất cả môn**, kể cả môn F (PRD: Σ trên toàn bộ danh sách).
- **Xếp loại dựa trên GPA hệ 4 đã làm tròn 2 chữ số**, tức đúng con số người dùng nhìn thấy, tránh trường hợp hiển thị 3.60 mà xếp loại "Giỏi".
- Hiển thị GPA luôn đủ 2 chữ số thập phân (vd `3.50`, không phải `3.5`).
- Tách phần **tính toán** (hàm thuần nhận danh sách, trả về số liệu) khỏi phần **hiển thị** (cập nhật DOM) để unit test được phần tính toán.
- Tổng tín chỉ luôn > 0 khi danh sách không rỗng (tín chỉ ≥ 1 theo F1), nên không có chia cho 0.
