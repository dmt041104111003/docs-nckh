# Bài toán nền tảng và cách hệ thống xử lý

Tài liệu nối **vấn đề thực tế** (remote freelance) với **cách thiết kế** trong repo: mỗi mục dẫn sang file luồng tương ứng. Thuật ngữ kỹ thuật: [bảng thuật ngữ](thuat-ngu.md).

---

## 1. Hiện trạng — những vấn đề cần giải quyết

### 1.1 Tin cậy và tiền khi làm việc remote

Hai bên không gặp trực tiếp: **người đăng việc** lo **trả nhầm / bị “ôm” tiền**; **người làm tự do** lo **làm xong không nhận đúng phần** hoặc **tranh chấp không có chứng từ rõ**. Cần **ký quỹ**, **điều kiện giải ngân** và **lịch sử giao dịch có thể kiểm tra** thay vì chỉ ghi trong một database đóng.

### 1.2 Tranh chấp sau bàn giao

Khi hai bên **bất đồng** sau khi nộp bài, cần **quy trình có thứ tự**, **trung lập**, **đúng hạn** — không xử kiểu tin nhắn tùy ý hay một người quyết định mãi không rõ luật.

### 1.3 Sàng lọc ứng viên và khối lượng đơn

**Người đăng việc** nhận nhiều CV; **xem tay** tốn thời gian, **tiêu chí không đồng nhất**. **Người làm tự do** cần **đánh giá nhanh** mức khớp với tin trước khi nộp đơn. Cần **hỗ trợ có giải thích**, nhưng **không thay** quyết định tuyển hay pháp lý.

### 1.4 Hạn chót và vận hành nền

Nhiều mốc (**nhận hồ sơ**, **ký**, **nộp bài**, **duyệt**, **chứng cứ**, **phiếu trọng tài**) nếu chỉ nhờ người nhớ sẽ **trễ hạn** hoặc **không đồng bộ** với **tiền trên blockchain**. Cần **máy quét theo lịch** (cron) và **thông báo**.


---

## 2. Hệ thống đã giải quyết như thế nào

| Vấn đề (mục 1) | Cách xử lý trong thiết kế | Tài liệu |
| -------------- | ------------------------- | -------- |
| **1.1** Tiền và tin cậy | **Blockchain** (Aptos / Move): ký quỹ, trạng thái việc, điểm tin cậy / bất tin cậy theo **sự kiện** và luật hợp đồng; có thể **đối chiếu** giao dịch công khai. Server lưu tin nhắn / hồ sơ để tra cứu nhanh; **tiền và điểm** theo chuẩn on-chain khi lệch với DB. | [blockchain](blockchain.md) |
| **1.2** Tranh chấp | **Ba vòng** cố định: hệ thống **gán ngẫu nhiên trọng tài** mỗi vòng → hai bên phản hồi theo thứ tự → **phiếu trọng tài**; hết hạn có xử lý thay trọng tài / chốt theo luật; sau vòng 3 **cập nhật kết quả**, **hoàn tiền / giao dịch** theo **điều kiện hợp đồng**. | [trọng tài](trong-tai.md) |
| **1.3** Sàng lọc CV | **Bộ chấm điểm CV** (AI): pipeline **B1–B5** (embedding, lọc nhanh, rerank, điểm tổng hợp), trả **điểm + nhãn + explanation**; chỉ **hỗ trợ**, không tự loại / tự trúng tuyển. | [cv-ai-scoring](cv-ai-scoring.md) |
| **1.4** Hạn và nền | **Cron jobs** trên server (**cron expression** cấu hình): quét hạn **tin** và **tranh chấp**, đổi trạng thái, **gửi thông báo**, **gọi giao dịch blockchain** khi đủ điều kiện (kèm ví vận hành nếu thiết kế có). | [hệ thống](system.md) |
| **1.5** Kiến trúc | **Web** → **Server** (DB + tệp) → **Blockchain**; **bộ chấm điểm CV** tách runtime; **cron** xử lý hết hạn và giao dịch nền. | [architecture](architecture.md) |

Luồng nghiệp vụ end-to-end (tin → hợp đồng → làm bài → tranh chấp nếu có): [flow-tổng-quan](flow-tổng-quan.md).

---

## 3. Theo vai

| Góc nhìn | File |
| -------- | ---- |
| Người đăng tin, chọn người, duyệt, tranh chấp | [poster](poster.md) |
| Người ứng tuyển, ký, nộp bài, tham gia tranh chấp | [freelancer](freelancer.md) |
| Trọng tài (phạm vi vụ / vòng) | [trọng tài](trong-tai.md) |
| Cron, thông báo, tích hợp blockchain phía máy | [hệ thống](system.md) |
| Chi tiết kỹ thuật chấm điểm AI | [cv-ai-scoring](cv-ai-scoring.md) |

---

## 4. Sơ đồ

```
Hiện trạng                          Giải pháp trong tài liệu
─────────────────────────────────────────────────────────────
Tiền & uy tín cần minh bạch    →    blockchain.md
Tranh chấp cần quy trình      →    trong-tai.md
CV đông, cần hỗ trợ có điểm  →    cv-ai-scoring.md
Nhiều deadline cần tự động    →    system.md
Tổng thể thành phần           →    architecture.md
Vòng đời việc                  →    flow-tổng-quan.md, poster, freelancer
```
