# Luồng tổng quan nền tảng

Tóm lược vai **người đăng việc**, **người làm tự do**, **trọng tài chuyên môn**; các phần **web**, **server**, **dịch vụ chấm điểm CV**, **chuỗi khối**, **hệ thống**; và vòng đời **tin tuyển → hợp đồng → làm bài → tranh chấp nếu có**. Kiến trúc: [architecture](architecture.md).

---

## 1. Bảng vai và phần hệ thống

| Phần | Việc chính | Đọc thêm |
| ---- | ---------- | -------- |
| Người đăng việc | Đăng tin chấm CV chọn người duyệt tranh chấp khi cần | [poster](poster.md) |
| Người làm tự do | Xem điểm CV ứng tuyển ký làm nộp bài | [freelancer](freelancer.md) |
| Trọng tài chuyên môn | Chỉ xử lý tranh chấp | [trọng tài](trong-tai.md) |
| Hệ thống | Hết hạn thông báo gọi chuỗi theo lịch | [hệ thống](system.md) |
| Chấm điểm CV | So khớp CV với tin khi đang tuyển | [cv-ai-scoring](cv-ai-scoring.md) |
| Điểm uy tín | Ghi trên chuỗi database có bản đọc | [chuỗi khối](blockchain.md) |
| Chuỗi khối | Giữ tiền tranh chấp cập nhật điểm | [chuỗi khối](blockchain.md) |

Cột **Đọc thêm** dẫn vào tài liệu chi tiết. Chấm điểm chỉ trong giai đoạn tuyển.

---

## 2. Vòng đời một công việc

Luồng chính là các bước **theo thứ tự** trên sơ đồ. **Người đăng việc** và **người làm tự do** thay nhau làm các bước ghi trên ô.

```mermaid
flowchart TB
  A1[Người đăng việc tạo tin] --> A2[Người đăng việc mở nhận hồ sơ]
  A2 --> B0[Người làm tự do xem điểm CV và ứng tuyển]
  B0 --> A3[Người đăng việc chọn một người]
  A3 --> A4[Hai bên ký hợp đồng]
  A4 --> B3[Người làm tự do làm và nộp bài]
  B3 --> A5[Người đăng việc duyệt hoặc yêu cầu sửa]
  A5 --> Q{Có tranh chấp?}
  Q -->|Không| K[Kết thúc]
  Q -->|Có| B4[Người làm tự do phản hồi và chứng cứ]
  B4 --> T[Trọng tài xử lý]
  T --> K2[Kết thúc sau tranh chấp]
```

**Hệ thống** quét hạn nhận hồ sơ ký nộp duyệt chứng cứ phiếu song song với luồng trên. Chi tiết: [hệ thống](system.md).

**Thứ tự nghiệp vụ**

1. **Người đăng việc** tạo tin mở nhận hồ sơ.  
2. **Người làm tự do** dùng **dịch vụ chấm điểm** nếu cần rồi nộp đơn **người đăng việc** có thể chấm từng người hoặc cả bảng trước khi chọn.  
3. **Người đăng việc** chọn một người.  
4. Hai bên **ký hợp đồng**.  
5. **Người làm tự do** nộp bài **người đăng việc** duyệt hoặc yêu cầu sửa.  
6. Nếu **không** tranh chấp kết thúc bình thường.  
7. Nếu **có** tranh chấp **người làm tự do** phản hồi **trọng tài** xử lý **hệ thống** có thể tự áp hạn xem [hệ thống](system.md).

---

## 3. Ai

```mermaid
flowchart TB
  subgraph nguoi[Ba nhóm người]
    P[Người đăng việc]
    F[Người làm tự do]
    A[Trọng tài]
  end
  Web[Giao diện web]
  May[Server]
  Cham[Dịch vụ chấm điểm]
  DB[(Dữ liệu và tệp)]
  Chain[Chuỗi khối]
  Hen[Lịch quét]
  P --> Web
  F --> Web
  A --> Web
  Web --> May
  Web --> Cham
  May --> DB
  May --> Chain
  Hen --> May
```

1. Ba nhóm đều vào **web** rồi **server** cho tài khoản tin hồ sơ tiền giữ.  
2. Khi đang nhận hồ sơ **web** gọi **dịch vụ chấm điểm** địa chỉ cấu hình sẵn đơn và CV chính vẫn lưu qua **server**.  
3. **Server** lưu dữ liệu và xử lý tiền trên **chuỗi khối** khi có giao dịch.  
4. **Lịch quét** chạy nền đồng bộ hạn và chuỗi.

---

## 4. Tranh chấp

```mermaid
flowchart TB
  V[Vụ tranh chấp]
  P[Người đăng việc] --> V
  F[Người làm tự do] --> V
  V --> T[Trọng tài điều phối]
  V --> M[Hệ thống xử lý khi đến hạn]
```

1. Hai bên cùng tham gia một vụ tranh chấp.  
2. **Trọng tài chuyên môn** điều phối và phân xử hoặc phiếu.  
3. **Hệ thống** vẫn quét hạn song song.
