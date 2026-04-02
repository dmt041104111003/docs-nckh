# Chuỗi khối: Tiền giữ hộ, Tranh chấp & Điểm uy tín

**Vấn đề:** Hai bên làm việc từ xa cần **tin tưởng về tiền** (ai giữ, khi nào trả, khi nào hoàn), **cách xử lý khi bất đồng** (tranh chấp có luật rõ), và **học được từ lịch sử** (điểm tin cậy / bất tin cậy để giảm rủi ro lần sau). Chỉ lưu trên máy chủ thường khiến người dùng khó kiểm chứng “ai được tiền” và “điểm có bị sửa tay hay không”.

**Cách xử lý:** Dự án dùng **chuỗi khối** để **khóa tiền trong hợp đồng giữ hộ**, chạy **luồng tranh chấp** khi có khiếu nại, và ghi **bảng uy tín** (quy tắc cộng trừ trong hợp đồng triển khai) sau các sự kiện như hoàn việc, hết hạn, kết thúc tranh chấp. **Máy chủ nghiệp vụ** đồng bộ trạng thái tin, gửi giao dịch khi cần (kể cả bước **khóa vận hành** cho hết hạn / hoàn tiền), nhận **mã giao dịch** từ người dùng (**ví**). Trên máy chủ có thể giữ **bản sao** điểm để hiển thị nhanh — cần **khớp** với chuỗi khi coi chuỗi là chuẩn.

---

## 1. Sơ đồ tổng: Ba lớp

```mermaid
%%{init: {"flowchart": {"curve": "linear", "padding": 12, "nodeSpacing": 40, "rankSpacing": 44}}}%%
flowchart TB
  subgraph tren[Tầng người dùng — ví]
    direction LR
    N1[Người đăng việc]
    N2[Người nhận việc]
  end
  subgraph giua[Hợp đồng trên chuỗi — thứ tự nghiệp vụ]
    direction TB
    HD1[Giữ tiền hộ — tiền & trạng thái công việc]
    HD2[Tranh chấp — chứng cứ / phiếu]
    HD3[Bảng uy tín — điểm tin cậy / bất tin cậy]
    HD2 --> HD1
    HD1 --> HD3
  end
  subgraph duoi[Hậu tầng]
    direction TB
    SVC[Máy chủ — gọi chuỗi & lưu trạng thái]
    DB[(Cơ sở dữ liệu — có thể sao chép điểm)]
  end
  N1 --> HD1
  N2 --> HD1
  N1 --> HD2
  N2 --> HD2
  SVC --> HD1
  SVC --> HD2
  HD3 -->|đọc điểm / nhật ký| SVC
  SVC --> DB
```

**Các bước luồng nghiệp vụ**

1. **Người đăng việc** khóa tiền vào **hợp đồng giữ hộ** khi đăng tin (theo điều khoản và bước ký).  
2. Theo tiến độ, tiền **trả cho người làm**, **hoàn cho chủ tin**, hoặc chuyển sang **tranh chấp** — tùy trạng thái, chữ ký và **hạn tự động**.  
3. **Máy chủ** lưu trạng thái nghiệp vụ, mã khóa ký quỹ, **mã giao dịch**, mã tranh chấp trên chuỗi; có thể gửi giao dịch bổ sung khi hết hạn hoặc sau phân xử.  
4. **Điểm tin cậy / bất tin cậy (UT/KUT)** được **cập nhật trong phần hợp đồng “uy tín”** khi các giao dịch **giữ tiền hộ** (và nhánh **tranh chấp** gắn với đó) chạy xong — lưu **theo địa chỉ ví**; ai cũng có thể **đọc lại điểm** qua hàm chỉ đọc trên chuỗi. **Cơ sở dữ liệu** có thể lưu bản sao; nếu coi chuỗi là chuẩn thì bản sao phải **khớp** sau mỗi giao dịch liên quan.

---

## 2. Vòng đời ký quỹ (nghiệp vụ)

**Luồng chính (một hàng, trên xuống):**

```mermaid
%%{init: {"flowchart": {"curve": "linear", "padding": 10, "rankSpacing": 48}}}%%
flowchart TB
  S1[1. Khóa tiền — mở tuyển] --> S2[2. Chọn người — chờ ký]
  S2 --> S3[3. Hai bên đã ký — đang làm]
  S3 --> S4[4. Đã nộp bài — chờ duyệt]
  S4 --> S5{Kết thúc thế nào?}
  S5 -->|Duyệt / hết hạn nghiệm thu| P1[Trả tiền người làm]
  S5 -->|Không tuyển được đến hạn| P2[Hoàn tiền chủ tin]
  S5 -->|Hủy / vận hành| P3[Hủy giữ hộ]
  S5 -->|Có khiếu nại| DIS[Sang luồng tranh chấp]
```

**Hết hạn do máy quét (song song với các bước trên, không nằm trong hàng chính):**

```mermaid
%%{init: {"flowchart": {"curve": "linear", "padding": 10}}}%%
flowchart TB
  M[Phát hiện quá hạn] --> Q1{Giai đoạn?}
  Q1 -->|Chờ ký| H1[Bỏ người chưa ký — tin mở lại nếu được]
  Q1 -->|Đang làm| H2[Bỏ người quá hạn nộp — chia tiền theo luật]
```

**Các bước luồng nghiệp vụ**

1. **Mở ký quỹ:** số tiền công việc (và phí nền tảng nếu có) được **khóa** trong hợp đồng.  
2. **Trong lúc làm:** trạng thái tin thay đổi trên **cơ sở dữ liệu**; chuỗi phản ánh bước cần **chữ ký** (chọn người, ký hợp đồng, duyệt bài…).  
3. **Kết thúc tốt:** trả tiền người làm (ví dụ khi hết hạn duyệt mà hệ thống tự trả).  
4. **Hết hạn không tuyển được:** hoàn cho người đăng việc.  
5. **Quá hạn ký / quá hạn nộp:** máy chủ hoặc lịch có thể gửi giao dịch **bỏ người quá hạn ký** / **bỏ người quá hạn nộp** cho khớp nghiệp vụ.  
6. **Tranh chấp:** ký quỹ gắn vụ tranh chấp cho đến khi có kết quả.

**Bước ký thường do máy chủ gửi** (khi hết hạn nhận hồ sơ, hủy giữ hộ, bỏ người quá hạn ký/nộp, tự duyệt khi hết hạn nghiệm thu…) tương ứng các **thao tác công khai** trong phần hợp đồng giữ tiền hộ — chi tiết kỹ thuật nằm trong mã nguồn phía máy chủ và hợp đồng.

---

## 3. Tranh chấp (chuỗi và nền tảng)

```mermaid
%%{init: {"flowchart": {"curve": "linear", "padding": 10, "rankSpacing": 44}}}%%
flowchart TB
  F1[Mở tranh chấp] --> F2[Lưu vụ — có mã trên chuỗi nếu cần]
  F2 --> F3[Người nhận việc phản hồi / chứng cứ]
  F3 --> F4{Hết hạn chứng cứ?}
  F4 -->|Có, bên kia im lặng| ON1[Máy / luật xử: thường phía thuê]
  F4 -->|Chưa| F5[Mở vòng bỏ phiếu]
  F5 --> F6[Gửi giao dịch mở phiếu]
  F6 --> F7[Trọng tài bỏ phiếu]
  F7 --> F8[Kết thúc — cập nhật + chia tiền]
  ON1 --> Ket[(Ghi nhận mã giao dịch & trạng thái)]
  F8 --> Ket
```

**Các bước luồng nghiệp vụ**

1. **Mở tranh chấp:** kèm mô tả, chứng cứ; giao diện có thể tạo **mã tranh chấp trên chuỗi** và gửi về máy chủ.  
2. **Giai đoạn chứng cứ:** nếu hết hạn mà người nhận việc không phản hồi, hệ thống có thể gửi giao dịch **xử lý hết hạn và chia tiền** (máy chủ ký) theo luật hợp đồng.  
3. **Bỏ phiếu:** khi đủ điều kiện, máy chủ có thể gửi giao dịch **mở phiếu** trên chuỗi.  
4. **Kết thúc:** ghi nhận bên thắng, cập nhật tin và ký quỹ; có thể còn bước **nhận tiền** bổ sung tùy thiết kế hợp đồng.

Chi tiết trọng tài: [tài liệu trọng tài](admin.md). Hết hạn tự động: [hệ thống tự động](system.md).

---

## 4. Điểm uy tín trên chuỗi

**Cấu trúc nghiệp vụ:** Một **kho lưu trữ chung** trên chuỗi giữ bảng **địa chỉ ví → bản ghi uy tín** (điểm **tin cậy**, **bất tin cậy**, số việc đã làm, số việc làm chủ tin, số tranh chấp thắng/thua). Mỗi lần đổi điểm có **nhật ký sự kiện** (ai, thao tác, thay đổi điểm, điểm mới, thời điểm). Chỉ **phần giữ tiền hộ** được phép gọi **cập nhật nội bộ** trong cùng giao dịch — tránh ai tự ý sửa điểm. Vẫn có thể có **lối vào công khai** (ký riêng) cho một số tình huống vận hành nếu cấu hình cho phép.

```mermaid
%%{init: {"flowchart": {"curve": "linear", "padding": 10, "rankSpacing": 38}}}%%
flowchart TB
  subgraph onchain[Trên chuỗi khối]
    ESC[Giữ tiền hộ] --> EVT[Sự kiện đủ điều kiện] --> REP[Bảng uy tín] --> LOG[Nhật ký] --> DOC[Đọc công khai]
  end
  DOC --> SVC[Máy chủ & giao diện]
```

**Quy tắc cộng trừ (theo bản hợp đồng đang triển khai)**

| Tình huống | Ai chịu tác động | Thay đổi |
| ---------- | ---------------- | -------- |
| Hoàn việc | Người nhận việc | **+10** điểm tin cậy; tăng đếm việc đã hoàn thành |
| Duyệt đúng hạn | Người đăng việc (khi nghiệm thu đúng hạn) | **+5** điểm tin cậy; tăng đếm việc làm chủ tin |
| Thắng tranh chấp | Bên thắng | **+5** điểm tin cậy |
| Thua tranh chấp | Bên thua | **+20** bất tin cậy; **−10** tin cậy (không âm) |
| Quá hạn nộp bài | Người nhận việc | **+10** bất tin cậy; **−5** tin cậy (không âm) |
| Quá hạn duyệt | Người đăng việc | **+10** bất tin cậy; **−5** tin cậy (không âm) |

**Các bước luồng nghiệp vụ**

1. **Tiền và trạng thái công việc** do **giữ tiền hộ / tranh chấp** điều phối; khi điều kiện trong hợp đồng thỏa, **điểm uy tín được cập nhật ngay trên chuỗi** trong cùng luồng giao dịch.  
2. **Trọng tài / máy chủ** không tự “nhập điểm tay” trong phần này — họ thay đổi **luồng tranh chấp và tiền**; điểm **bám theo kết quả thực thi** đã ghi trong hợp đồng.  
3. **Điều khoản trên giao diện** giải thích cho người dùng; **con số cụ thể** cần khớp bảng trên khi hệ thống dùng đúng phiên bản hợp đồng đã triển khai.

Luồng theo vai: [người đăng việc](poster.md), [người nhận việc](freelancer.md), [trọng tài](admin.md), [máy tự động](system.md) (mục 4).

---

## 5. Các thao tác máy chủ thường ký (ý nghĩa nghiệp vụ)

| Chủ đề | Việc làm (lời văn) | Ghi chú |
| ------ | ------------------ | ------- |
| Giữ hộ | Hết hạn ký — bỏ người làm | Trong phần hợp đồng giữ tiền |
| Giữ hộ | Hết hạn nộp — bỏ người làm | |
| Giữ hộ | Hết hạn duyệt — trả tiền người làm | |
| Giữ hộ | Hết hạn nhận hồ sơ — hoàn chủ tin | |
| Giữ hộ | Hủy ký quỹ (vận hành / khôi phục) | |
| Tranh chấp | Hết hạn chứng cứ / xử lý quá hạn | |
| Tranh chấp | Mở vòng bỏ phiếu | |
| Uy tín | Thường **không** gọi riêng — **đi kèm** giao dịch giữ hộ khi luật trong hợp đồng kích hoạt | Hoàn việc, hết hạn, xong tranh chấp |
| Uy tín | **Đọc** điểm để hiển thị / đối soát | Hàm chỉ đọc trên chuỗi |

Người dùng ký các bước tạo tin, chọn người, duyệt bài, mở tranh chấp… bằng **ví**; **mã giao dịch** lưu kèm tin / tranh chấp trên máy chủ. **Cập nhật uy tín** đi kèm các giao dịch **giữ tiền hộ** khi điều kiện trong hợp đồng thỏa (xem mục 4).

---

