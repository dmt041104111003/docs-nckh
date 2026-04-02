# Chuỗi khối: tiền giữ, tranh chấp, điểm uy tín

Làm việc từ xa cần **chỗ giữ tiền an toàn**, **cách xử khiếu nại có thể kiểm tra**, và **điểm uy tín** lặp lại theo thời gian. **Chuỗi khối** công khai phục vụ các phần này. **Server** vẫn lưu tin đơn và tin nhắn để tra cứu nhanh; **chuỗi** giữ phần **tiền đã khóa**, **bước tranh chấp**, và **điểm** theo quy tắc đã triển khai.

---

## Chuỗi khối

Chuỗi **không thay** toàn bộ cơ sở dữ liệu. Tin ứng tuyển chat và hồ sơ vẫn ở **database**. Chuỗi đảm nhiệm phần **sau khi đã cam kết** khó thay đổi một chiều: số tiền giữ, điều kiện trả, luồng tranh chấp, cập nhật điểm.

**Quy tắc chạy trên mạng** được gọi là hợp đồng thông minh. Nền tảng dùng mạng **Aptos** và ngôn ngữ **Move**. Người đọc tài liệu luồng chỉ cần biết: mọi thay đổi tiền và điểm trên chuỗi đều là **giao dịch** đã được **xác thực**.

---

## Giao dịch và ví

Một **giao dịch** là gói thao tác gửi lên mạng. **Ký bằng ví** chứng minh người đó đồng ý, ví dụ khóa tiền giữ chấp nhận nghiệm thu mở tranh chấp. **Địa chỉ ví** gắn với điểm uy tín và luồng tiền.

Một số việc **đến hạn** do **server** hoặc **lịch quét** thực hiện bằng **ví vận hành** đã được phép trong thiết kế, để tự động mà vẫn kiểm tra được trên chuỗi.

---

## Ký quỹ và trạng thái việc

**Tiền giữ** nằm trong hợp đồng cho đến khi thỏa điều kiện: nghiệm thu, hoàn cho người đăng việc nếu không tuyển được, hoặc chuyển sang tranh chấp. Trạng thái **đang tuyển đang làm chờ duyệt đã đóng** phải **khớp** giữa **database** và **chuỗi**. Nếu lệch, thường **chuỗi** là chuẩn cho **tiền và điểm**.

---

## Điểm tin cậy và bất tin cậy

Hai chỉ số nghiệp vụ được cộng trừ khi có **sự kiện** đúng luật. Cập nhật điểm **không** tùy tiện: chỉ luồng giữ tiền và tranh chấp hợp lệ mới kích hoạp. Ai cũng có thể **đọc** bảng điểm theo địa chỉ trên mạng.

---

## Ba lớp minh họa

```mermaid
flowchart TB
  subgraph tren[Người dùng và ví]
    N1[Người đăng việc]
    N2[Người làm tự do]
  end
  subgraph giua[Trên chuỗi]
    HD1[Giữ tiền và trạng thái việc]
    HD2[Tranh chấp]
    HD3[Bảng uy tín]
  end
  subgraph duoi[Hạ tầng]
    SVC[Server]
    DB[(Database)]
  end
  N1 --> HD1
  N2 --> HD1
  N1 --> HD2
  N2 --> HD2
  SVC --> HD1
  SVC --> HD2
  HD3 --> SVC
  SVC --> DB
```

1. **Người đăng việc** khóa tiền khi đăng tin theo điều khoản.  
2. Theo tiến độ tiền trả cho người làm hoàn cho người đăng việc hoặc vào nhánh tranh chấp tùy trạng thái chữ ký và hạn.  
3. **Server** lưu trạng thái và mã giao dịch; có thể gửi thêm giao dịch khi hết hạn.  
4. **Điểm tin cậy và bất tin cậy** cập nhật trong hợp đồng uy tín khi các bước giữ tiền và tranh chấp **kết thúc đúng luật**. Database có thể giữ bản sao để hiển thị và cần **khớp** với chuỗi.

---

## Vòng đời giữ tiền

```mermaid
flowchart TB
  S1[Khóa tiền mở tuyển] --> S2[Chọn người chờ ký]
  S2 --> S3[Hai bên đã ký đang làm]
  S3 --> S4[Đã nộp bài chờ duyệt]
  S4 --> S5{Kết thúc}
  S5 -->|Duyệt hoặc hết hạn duyệt| P1[Trả người làm]
  S5 -->|Hết hạn không tuyển| P2[Hoàn người đăng việc]
  S5 -->|Hủy theo vận hành| P3[Hủy giữ]
  S5 -->|Có khiếu nại| DIS[Sang tranh chấp]
```

**Hết hạn do máy quét** chạy song song: ví dụ quá hạn ký quá hạn nộp quá hạn nhận hồ sơ **hệ thống** cập nhật và có thể gửi giao dịch phù hợp. Chi tiết: [hệ thống](system.md).

---

## Tranh chấp

```mermaid
flowchart TB
  F1[Mở tranh chấp] --> F2[Lưu vụ có mã trên chuỗi nếu cần]
  F2 --> F3[Người làm tự do phản hồi]
  F3 --> F4{Hết hạn chứng cứ?}
  F4 -->|Có một bên im| ON1[Xử theo luật mặc định]
  F4 -->|Chưa| F5[Vòng bỏ phiếu]
  F5 --> F6[Trọng tài bỏ phiếu]
  F6 --> F8[Kết thúc chia tiền]
  ON1 --> Ket[Ghi nhận]
  F8 --> Ket
```

Quy trình **trọng tài chuyên môn**: [trọng tài](trong-tai.md).

---

## Bảng cộng trừ điểm mẫu

| Tình huống | Ai bị tác động | Thay đổi |
| ---------- | -------------- | -------- |
| Hoàn việc | Người làm tự do | +10 tin cậy |
| Duyệt đúng hạn | Người đăng việc | +5 tin cậy |
| Thắng tranh chấp | Bên thắng | +5 tin cậy |
| Thua tranh chấp | Bên thua | +20 bất tin cậy −10 tin cậy không âm |
| Quá hạn nộp | Người làm tự do | +10 bất tin cậy −5 tin cậy |
| Quá hạn duyệt | Người đăng việc | +10 bất tin cậy −5 tin cậy |

Số cụ thể phải khớp phiên bản hợp đồng đang chạy.

Luồng theo vai: [người đăng việc](poster.md), [người làm tự do](freelancer.md), [trọng tài](trong-tai.md), [hệ thống](system.md).

---

## Việc hệ thống thường làm trên chuỗi

| Chủ đề | Việc |
| ------ | ---- |
| Giữ tiền | Hết hạn ký bỏ người làm |
| Giữ tiền | Hết hạn nộp bỏ người làm |
| Giữ tiền | Hết hạn duyệt trả người làm |
| Giữ tiền | Hết hạn nhận hồ sơ hoàn người đăng việc |
| Giữ tiền | Hủy giữ |
| Tranh chấp | Hết hạn chứng cứ |
| Tranh chấp | Mở vòng bỏ phiếu |
| Uy tín | Thường đi cùng giao dịch giữ tiền khi luật kích hoạt |
| Uy tín | Đọc điểm để hiển thị |

Người dùng ký các bước đăng tin chọn người duyệt mở tranh chấp bằng **ví**. **Cập nhật uy tín** đi kèm giao dịch giữ tiền khi điều kiện trong hợp đồng thỏa.
