# Người đăng việc

Người đăng việc **đăng tin**, **xem ứng viên**, **chấm điểm CV** khi bật tính năng, **ký hợp đồng**, **duyệt bài**, và **mở tranh chấp** nếu sau bàn giao có bất đồng.

---

## Đăng ký vai và tạo tin

```mermaid
flowchart TD
  A[Đã đăng nhập] --> B[Xin vai người đăng việc]
  B --> C[Được phép đăng tin]
  C --> D[Tạo tin nháp hoặc đăng ngay]
  D --> E[Điền ngân sách tiền giữ hạn và nội dung]
  E --> F{Xuất bản}
  F -->|Lưu nháp| G[Sửa sau]
  F -->|Đăng| H[Tin mở nhận hồ sơ và khóa tiền nếu bật chuỗi khối]
```

1. Có tài khoản, xin thêm vai **người đăng việc** nếu nền tảng yêu cầu.  
2. Tạo tin, có thể lưu nháp hoặc đăng ngay.  
3. Điền ngân sách, thời hạn, nội dung, và **tiền giữ trên chuỗi** nếu bật.  
4. Đăng tin: ứng viên thấy tin và có thể ứng tuyển.

---

## Ứng viên và chấm điểm CV

```mermaid
flowchart TD
  A[Tin đang nhận hồ sơ] --> B[Danh sách đơn ứng tuyển]
  B --> S[Chấm điểm CV so với mô tả tin]
  S --> B2[Sắp theo độ phù hợp]
  B2 --> C{Lựa chọn}
  C --> D[Chọn một người và ký bước chọn người]
  C --> E[Từ chối một hoặc nhiều người]
  C --> F[Đóng tin nếu cần]
  D --> G[Chờ hai bên ký hợp đồng]
```

1. Xem danh sách người đã ứng tuyển.  
2. **Chấm điểm CV:** hệ thống lấy CV, so với tiêu đề mô tả và yêu cầu của tin, hiển thị điểm và thứ tự gợi ý.  
3. Có thể từ chối hồ sơ hoặc đóng tin.  
4. **Chọn một người** rồi chờ ký hợp đồng, thường kèm bước xác nhận trên chuỗi.  

Kỹ thuật chấm điểm: [cv-ai-scoring](cv-ai-scoring.md).

---

## Hợp đồng

```mermaid
flowchart TD
  A[Đã chọn người] --> B[Xem điều khoản]
  B --> C[Hai bên ký theo thứ tự quy định]
  C --> D{Trước khi ký xong}
  D -->|Dừng an toàn| E[Hủy trước khi đủ ký kèm bước chuỗi nếu có]
  D -->|Tiếp tục| F[Đủ chữ ký — chuyển sang đang làm]
```

1. Hai bên đọc điều khoản.  
2. Ký lần lượt theo quy định.  
3. Có thể **hủy an toàn** nếu chưa ký xong.  
4. Đủ ký: công việc chuyển sang **đang thực hiện**.

---

## Duyệt bài và tranh chấp

```mermaid
flowchart TD
  A[Người làm tự do đã nộp bài] --> B{Người đăng việc}
  B -->|Đồng ý| C[Duyệt kèm bước chuỗi nếu có]
  B -->|Cần sửa| D[Yêu cầu làm lại và ghi chú]
  B -->|Tranh chấp| E[Mở vụ tranh chấp kèm chứng cứ]
  C --> F[Hoàn thành]
  E --> G[Trọng tài và hai bên xử lý tiếp]
```

1. Xem bài nộp.  
2. **Duyệt** hoặc **yêu cầu sửa** hoặc **mở tranh chấp** theo quy định.  
3. Nếu tranh chấp: **trọng tài chuyên môn** điều phối; xem [trọng tài](trong-tai.md).

---

## Hủy tin và đăng lại

```mermaid
flowchart TD
  A[Tin đã hủy hoặc cần đăng lại] --> B[Đăng lại với nháp hoặc tiền và ký lại]
  A --> C[Xin hủy phía người đăng việc kèm lý do]
  A --> D[Xóa tin hoặc đổi nháp và đã đăng]
```

---

## Điểm uy tín phía người đăng việc

Điểm **tin cậy** và **bất tin cậy** hiển thị trên hồ sơ và tin. Cách cộng trừ nằm trong **điều khoản** trên ứng dụng.

| Tình huống | Tin cậy | Bất tin cậy |
| --- | --- | --- |
| Nghiệm thu đúng hạn theo điều khoản | +5 | — |
| Thắng tranh chấp | +5 | — |
| Thua tranh chấp | −10 | +20 |
| Quá hạn nghiệm thu | −5 | +10 |

Sau nghiệm thu, kết quả tranh chấp, hoặc **hệ thống** xử lý hết hạn, điểm có thể đổi.

Phía **người làm tự do** xem [freelancer](freelancer.md).

---

Một tài khoản có thể vừa đăng việc vừa làm tự do nếu được gán đủ vai. Tiền giữ và hoàn tiền phụ thuộc cấu hình chuỗi và trạng thái việc; hạn tự động xem [hệ thống](system.md).
