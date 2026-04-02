# Người đăng việc / bên thuê

**Vấn đề:** Bên thuê cần **thu hút ứng viên đủ chất**, **chọn đúng người**, **giữ tiền an toàn** đến khi việc xong, **duyệt bài đúng hạn**, và khi bất đồng phải có **lối tranh chấp** rõ — nếu không, rủi ro lộn xộn tiền, chậm phản hồi, hoặc mất uy tín.

**Cách xử lý:** Vai **người đăng việc** đi theo lộ trình **đăng tin → mở nhận hồ sơ → có thể chấm CV hỗ trợ → chọn người → hai bên ký → duyệt hoặc yêu cầu sửa → kết thúc hoặc tranh chấp**; tiền và điểm uy tín gắn với **chuỗi khối** và điều khoản — chi tiết từng sơ đồ dưới đây.

---

## Trở thành người đăng việc và tạo tin

```mermaid
%%{init: {"flowchart": {"curve": "linear", "padding": 8, "rankSpacing": 36}}}%%
flowchart TB
  A[Người dùng đã đăng nhập] --> B[Đăng ký vai trò người thuê]
  B --> C[Có quyền đăng việc]
  C --> D[Tạo tin: nháp hoặc đăng ngay]
  D --> E[Điền ngân sách / tiền giữ hộ / hạn chót / danh mục]
  E --> F{Xuất bản}
  F -->|Lưu nháp| G[Chỉnh sửa sau]
  F -->|Đăng tin| H[Tin mở nhận hồ sơ + tiền giữ trên chuỗi nếu bật]
```

**Các bước luồng nghiệp vụ**

1. Người dùng đã có tài khoản; **xin thêm vai trò** người thuê nếu nền tảng yêu cầu.  
2. Tạo tin mới: có thể **lưu nháp** để chỉnh sau hoặc **đăng ngay**.  
3. Điền đủ thông tin nghiệp vụ: ngân sách, thời hạn, hạng mục, và (nếu có) **đặt cọc / tiền giữ hộ** trên chuỗi khối.  
4. Xuất bản: tin chuyển sang trạng thái **mở nhận hồ sơ**; ứng viên có thể thấy và ứng tuyển.

---

## Quản lý ứng viên và chọn người nhận việc

```mermaid
%%{init: {"flowchart": {"curve": "linear", "padding": 8, "rankSpacing": 36}}}%%
flowchart TB
  A[Tin đang mở nhận hồ sơ] --> B[Xem danh sách ứng tuyển]
  B --> S[Chấm điểm CV tự động — so khớp với mô tả tin]
  S --> B2[Cột điểm / sắp theo độ phù hợp]
  B2 --> C{Lựa chọn}
  C --> D[Chấp nhận một người + ký mã giao dịch]
  C --> E[Từ chối một hoặc nhiều người]
  C --> F[Đóng tin nếu cần]
  D --> G[Chờ hai bên ký hợp đồng]
```

**Các bước luồng nghiệp vụ**

1. Tin đang **mở nhận hồ sơ**; người thuê xem danh sách người đã ứng tuyển.  
2. **Chấm điểm CV** (phần nền tảng): với ứng viên có file đính kèm, hệ thống tải CV, so với **tiêu đề + mô tả + yêu cầu** của tin, hiển thị điểm và **sắp danh sách** — bước **sàng lọc** trước khi quyết định.  
3. **Từ chối** một hoặc nhiều hồ sơ hoặc **đóng tin** nếu không còn tuyển.  
4. **Chấp nhận một người** → **chờ ký hợp đồng**; thường kèm xác nhận / ký trên chuỗi (tiền giữ hộ).  
5. Ứng viên không được chọn nhận trạng thái từ chối theo quy định nền tảng.

Chi tiết màn hình và tích hợp: [luồng chấm điểm CV](cv-ai-scoring.md).

---

## Hợp đồng (phía người thuê)

```mermaid
%%{init: {"flowchart": {"curve": "linear", "padding": 8, "rankSpacing": 36}}}%%
flowchart TB
  A[Đã chọn người nhận việc] --> B[Tạo hoặc xem điều khoản]
  B --> C[Hai bên ký theo thứ tự hệ thống]
  C --> D{Trước khi ký xong}
  D -->|Hủy an toàn| E[Hủy trước khi ký + mã giao dịch]
  D -->|Tiếp tục| F[Sau khi ký đủ — chuyển sang đang làm]
```

**Các bước luồng nghiệp vụ**

1. Sau khi chọn ứng viên, hai bên xem **điều khoản hợp đồng** do hệ thống hoặc template quy định.  
2. Mỗi bên **ký theo thứ tự** quy định (ví dụ chủ tin trước hoặc người nhận việc trước).  
3. Nếu **chưa ký xong** mà chủ tin muốn dừng: dùng **hủy an toàn** (hoàn tác có kiểm soát, kèm giao dịch nếu cần).  
4. Khi **đủ chữ ký**, công việc chuyển sang **đang thực hiện** — bắt đầu đếm hạn nộp / duyệt theo tin đã thỏa thuận.

---

## Nhận sản phẩm và tranh chấp

```mermaid
%%{init: {"flowchart": {"curve": "linear", "padding": 8, "rankSpacing": 36}}}%%
flowchart TB
  A[Người nhận việc đã nộp bài] --> B{Người thuê}
  B -->|Đồng ý| C[Duyệt bài + mã giao dịch]
  B -->|Cần sửa| D[Yêu cầu làm lại + ghi chú + mã giao dịch]
  B -->|Tranh chấp| E[Tạo vụ tranh chấp + chứng cứ + mã giao dịch]
  C --> F[Hoàn thành]
  E --> G[Đang tranh chấp — trọng tài và hai bên xử lý tiếp]
```

**Các bước luồng nghiệp vụ**

1. Người nhận việc **nộp sản phẩm** (link, tệp, ghi chú theo quy định).  
2. Người thuê xem xét: **duyệt** → tiến tới hoàn thành và thanh toán / giải phóng tiền giữ hộ; **yêu cầu sửa** → người nhận việc phải nộp lại bản chỉnh; **mở tranh chấp** → gửi mô tả và chứng cứ, chuyển sang quy trình tranh chấp.  
3. Nếu tranh chấp: **trọng tài** điều phối / phân xử; hai bên phản hồi và ký các bước trên chuỗi nếu có.

---

## Hủy tin, đăng lại, rút lui

```mermaid
%%{init: {"flowchart": {"curve": "linear", "padding": 8, "rankSpacing": 36}}}%%
flowchart TB
  A[Tin đã hủy hoặc muốn đăng lại] --> B[Đăng lại tin — nháp hoặc giữ tiền / ký giao dịch]
  A --> C[Xin hủy phía người thuê + lý do]
  A --> D[Xóa tin hoặc chuyển nháp ↔ đăng]
```

**Các bước luồng nghiệp vụ**

1. Tin **đã hủy** hoặc cần **đăng lại**: chủ tin chọn đăng lại (có thể lưu nháp hoặc nộp lại tiền giữ hộ / ký giao dịch tùy trạng thái).  
2. **Xin hủy phía người thuê** (rút lui có lý do) theo nút / màn hình nền tảng — áp quy tắc hoàn tiền hoặc phạt.  
3. **Xóa tin** hoặc chuyển **nháp ↔ đã đăng** khi còn trong phạm vi cho phép.

---

## Điểm uy tín (phía người đăng việc)

**Điểm tín nhiệm (UT)** và **bất tín nhiệm (KUT)** hiển thị trên **hồ sơ** và **tin tuyển** (người nhận việc xem uy tín chủ tin). Chi tiết trong **màn điều khoản hệ thống** trên ứng dụng.

**Lưu ý triển khai:** Quy tắc cộng trừ **đã có trong hợp đồng trên chuỗi** và có thể được sao chép trên máy chủ — bảng dưới là **chuẩn nghiệp vụ**; xem thêm [chuỗi khối, mục 4](blockchain.md).

| Tình huống (người thuê) | UT | KUT |
| --- | --- | --- |
| Nghiệm thu / phản hồi **đúng hạn** (theo điều khoản) | +5 | — |
| **Thắng** tranh chấp | +5 | — |
| **Thua** tranh chấp | −10 | +20 |
| **Quá hạn nghiệm thu** | −5 | +10 |

```mermaid
%%{init: {"flowchart": {"curve": "linear", "padding": 10, "rankSpacing": 28}}}%%
flowchart TB
  subgraph cong[Tăng tin cậy]
    A[Duyệt đúng hạn hoặc thắng tranh chấp] --> B[UT chủ tin tăng]
  end
  subgraph tru[Giảm tin / tăng bất tin]
    C[Quá hạn nghiệm thu hoặc thua tranh chấp] --> D[UT giảm — KUT tăng]
  end
```

**Các bước luồng nghiệp vụ**

1. Sau **nghiệm thu**, **kết quả tranh chấp**, hoặc **máy quét hết hạn** (xem [hệ thống tự động](system.md)), hệ thống cập nhật UT/KUT cho chủ tin (trên chuỗi và/hoặc bản sao máy chủ).  
2. Bạn đối tác dùng điểm làm **tín hiệu nhanh** khi chọn tin.  

Điểm cho **người nhận việc** (quá hạn ký, nộp bài, rút…) — xem [người nhận việc](freelancer.md).

---

## Ghi chú

- Một tài khoản có thể vừa **đăng việc** vừa **nhận việc** nếu được gán nhiều vai trò.
- Bước liên quan **tiền giữ hộ / hoàn tiền** phụ thuộc cấu hình chuỗi khối và trạng thái công việc; hạn tự động xem [hệ thống tự động](system.md).
