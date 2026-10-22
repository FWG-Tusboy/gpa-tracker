# F2 – Quy đổi điểm

Nguồn: PRD §2 F2. Phụ thuộc: F1. Được gọi bởi: F1, F4 (sau khi lưu), F3 (khi tính GPA hệ 4), hiển thị bảng.

Đầu vào: điểm hệ 10 **đã làm tròn 1 chữ số thập phân** (từ F1). Đầu ra: điểm chữ và điểm hệ 4.

```mermaid
flowchart TD
    Start(["Nhận điểm hệ 10 đã làm tròn"]) --> A{"Điểm ≥ 8.5?"}
    A -->|Có| RA["A – 4.0"]
    A -->|Không| BP{"Điểm ≥ 8.0?"}
    BP -->|Có| RBP["B+ – 3.5"]
    BP -->|Không| B{"Điểm ≥ 7.0?"}
    B -->|Có| RB["B – 3.0"]
    B -->|Không| CP{"Điểm ≥ 6.5?"}
    CP -->|Có| RCP["C+ – 2.5"]
    CP -->|Không| C{"Điểm ≥ 5.5?"}
    C -->|Có| RC["C – 2.0"]
    C -->|Không| DP{"Điểm ≥ 5.0?"}
    DP -->|Có| RDP["D+ – 1.5"]
    DP -->|Không| D{"Điểm ≥ 4.0?"}
    D -->|Có| RD["D – 1.0"]
    D -->|Không| RF["F – 0.0"]

    RA --> Out
    RBP --> Out
    RB --> Out
    RCP --> Out
    RC --> Out
    RDP --> Out
    RD --> Out
    RF --> Out
    Out[/"Trả về điểm chữ và điểm hệ 4"/] --> End(["Kết thúc"])
```

## Ghi chú thiết kế
- **Dùng ngưỡng dưới (≥)** thay vì so khoảng `8.0 – 8.4`. Vì điểm đã làm tròn 1 chữ số, hai cách cho cùng kết quả, nhưng dùng ngưỡng thì không có "khe hở" (vd 8.45) nếu sau này đổi cách làm tròn.
- Là **hàm thuần** (không đọc/ghi DOM hay localStorage), nên dễ viết unit test cho từng ngưỡng biên: 0, 3.9, 4.0, 4.9, 5.0, 5.4, 5.5, 6.4, 6.5, 6.9, 7.0, 7.9, 8.0, 8.4, 8.5, 10.
- Điểm chữ và hệ 4 **không lưu vào localStorage**; luôn tính lại từ điểm hệ 10 khi cần (tránh dữ liệu lệch nhau).
