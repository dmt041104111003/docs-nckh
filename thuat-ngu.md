# Bảng thuật ngữ chung



---

<a id="aptos"></a>

## Aptos

**Mạng blockchain** công khai mà nền tảng dùng để triển khai **hợp đồng thông minh** (tiền giữ, tranh chấp, điểm uy tín). Chi tiết triển khai: [blockchain](blockchain.md).

---

<a id="ai-scoring"></a>

## AI (chấm điểm CV)

**Trí tuệ nhân tạo** ở đây chỉ phần **suy luận mô hình**: đọc văn bản CV và mô tả việc, trả về **điểm** và **nhãn** hỗ trợ người xem — **không** thay người quyết định tuyển hay ký hợp đồng. Pipeline kỹ thuật: [cv-ai-scoring](cv-ai-scoring.md).

---

<a id="blockchain"></a>

## Blockchain

**Sổ ghi giao dịch phân tán**, nhiều người có thể **kiểm tra** mà không phụ thuộc một máy chủ đơn lẻ. Trên nền tảng này, blockchain dùng để ghi **tiền đã cam kết**, **bước tranh chấp**, **điểm uy tín** theo **quy tắc đã cài** (hợp đồng on-chain). Xem thêm: [blockchain](blockchain.md).

---

<a id="cv-scoring-service"></a>

## Bộ chấm điểm CV

**Phần mềm chạy riêng** (tách **runtime** khỏi server chính) nhận CV và mô tả việc, trả **điểm khớp** và giải thích ngắn. Chỉ dùng khi **đang tuyển**. Chi tiết: [cv-ai-scoring](cv-ai-scoring.md).

---

<a id="cosine"></a>

## Cosine similarity

**Độ giống nhau** giữa hai **vector** (số đo trong không gian nhiều chiều): giá trị càng cao thì hai văn bản (đã **embedding**) càng gần nghĩa theo cách mô hình hiểu. Dùng ở bước lọc nhanh trong pipeline chấm điểm.

---

<a id="cron"></a>

## Cron, cron job, cron expression

- **Cron**: kiểu **lịch tự động** trên server — đến giờ thì chạy một việc đã đăng ký.  
- **Cron job**: **một tác vụ** cụ thể trong lịch đó (ví dụ: quét hết hạn tin).  
- **Cron expression**: **chuỗi cấu hình** quy định *lúc nào* chạy (phút, giờ, ngày, …) — tùy môi trường (Linux cron, Spring `@Scheduled`, v.v.).  

Chi tiết vận hành: [hệ thống](system.md).

---

<a id="crossencoder"></a>

## CrossEncoder (rerank)

**Mô hình AI** đọc **cùng lúc** CV và mô tả việc trong **một lần suy luận** để xếp lại thứ tự hoặc tinh chỉnh điểm so với bước chỉ so **vector** đơn giản. Trong tài liệu gọi là bước **rerank** (sau bước embedding / lọc nhanh).

---

<a id="database"></a>

## Database (cơ sở dữ liệu)

**Kho lưu** tin, đơn, trạng thái việc, tin nhắn… để **tra cứu nhanh** và hiển thị trên web. **Không** thay thế blockchain cho phần **tiền và điểm** đã cam kết on-chain — hai lớp cần **khớp** khi thiết kế yêu cầu.

---

<a id="embedding"></a>

## Embedding (vector hóa)

Đưa **đoạn văn** (CV, mô tả việc) thành **dãy số** (**vector**) trong không gian nhiều chiều để máy so **mức gần nghĩa** với nhau. Là bước đầu trước khi **lọc** hoặc **rerank**.

---

<a id="transaction"></a>

## Giao dịch (transaction)

**Gói thao tác** gửi lên **blockchain** (chuyển tiền, đổi trạng thái hợp đồng, cập nhật điểm, …). Phải được **ký** bằng **ví** và **xác thực** bởi mạng thì mới có hiệu lực trên chuỗi.

---

<a id="smart-contract"></a>

## Hợp đồng thông minh (smart contract)

**Chương trình** chạy trên blockchain, chứa **luật** (khi nào giữ tiền, khi nào trả, khi nào mở tranh chấp, cộng điểm uy tín…). Người dùng **không sửa tay** từng dòng; họ **ký giao dịch** theo luồng ứng dụng.

---

<a id="escrow"></a>

## Ký quỹ / tiền giữ (escrow)

**Tiền khóa** trong hợp đồng cho đến khi **điều kiện** đúng (nghiệm thu, hết hạn tuyển, chuyển sang tranh chấp…). Giảm rủi ro một bên nhận tiền mà chưa làm việc hoặc bên kia không trả sau khi có bàn giao.

---

<a id="move"></a>

## Move

**Ngôn ngữ lập trình** dùng viết **hợp đồng thông minh** trên **Aptos**. Người đọc luồng chỉ cần biết: luật tiền và điển nằm trong code đã triển khai trên mạng.

---

<a id="on-chain"></a>

## On-chain

**Trên blockchain** — dữ liệu hoặc trạng thái đã ghi vào **giao dịch** được mạng chấp nhận, có thể **đối chiếu** công khai theo thiết kế. Trái lại, chỉ lưu trong **database** thường gọi là off-chain (trừ khi đồng bộ từ on-chain).

---

<a id="rerank"></a>

## Rerank

Bước **xếp lại** hoặc **tinh chỉnh điểm** sau khi đã có danh sách / điểm sơ bộ từ **embedding**; dùng mô hình mạnh hơn (**CrossEncoder**) để bắt ngữ cảnh hai chiều giữa CV và tin tuyển.

---

<a id="server"></a>

## Server

**Máy chủ** chạy logic nghiệp vụ: lưu **database**, file **tệp**, xử lý **API**, đăng nhập, gọi blockchain và **bộ chấm điểm CV** khi cần. Khác với **ứng dụng web** (chỉ chạy trên trình duyệt phía người dùng).

---

<a id="sigmoid"></a>

## Sigmoid

**Hàm toán học** đưa đầu ra mô hình về khoảng (0, 1), rồi có thể **ánh xạ** sang thang điểm 0–100 trong pipeline rerank. Người đọc luồng chỉ cần hiểu: dùng để **chuẩn hóa điểm** trước khi tổng hợp.

---

<a id="reputation"></a>

## UT / KUT (điểm tin cậy / bất tin cậy)

**Điểm uy tín** gắn tài khoản theo **luật** trên chuỗi (ví dụ cộng khi hoàn thành tốt, trừ khi vi phạm hạn hoặc thua tranh chấp). **Không** do admin sửa tay tùy ý — cập nhật qua **giao dịch** khi đủ điều kiện. Bảng mẫu: [blockchain](blockchain.md) (mục bảng điểm).

---

<a id="web-app"></a>

## Ứng dụng web (web)

**Giao diện** người dùng mở trên **trình duyệt** (hoặc app bọc web): đăng tin, ứng tuyển, ký, xem điểm CV… Mọi thao tác đi qua **server** và (khi cần) **blockchain** phía sau.

---

<a id="wallet"></a>

## Ví (blockchain)

**Tài khoản** trên mạng blockchain, có **địa chỉ** và **khóa ký** để gửi **giao dịch**. Người dùng ký khi khóa tiền, duyệt bước hợp đồng; **ví vận hành** là ví do hệ thống dùng trong giới hạn cho phép (ví dụ cron gửi giao dịch hết hạn).

---

<a id="remote-work"></a>

## Remote work

**Làm việc không cần có mặt tại văn phòng** đối tác; phối hợp qua mạng. Bài toán tin cậy và thanh toán được mô tả trong [blockchain](blockchain.md) và [bai-toan](bai-toan.md).

---

<a id="runtime"></a>

## Runtime (tách runtime)

**Môi trường chạy** tách riêng: ví dụ **bộ chấm điểm CV** chạy trên process/server khác **server** chính để **không nghẽn** khi nhiều người chấm điểm cùng lúc.

---
