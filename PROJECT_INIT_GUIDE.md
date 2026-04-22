# Hướng Dẫn Khởi Động Dự Án
## LexAI — Hệ Thống Đa Tác Tử Thẩm Định Hợp Đồng Pháp Lý
### Đánh Giá Sẵn Sàng Doanh Nghiệp | Ngày Tổng Kết Phase 1

---

## 0. Thẻ Nhận Dạng Dự Án

| Thông tin | Chi tiết |
|---|---|
| **Tên dự án** | LexAI — Hệ Thống Trí Tuệ Nhân Tạo Thẩm Định Hợp Đồng |
| **Loại chủ đề** | Sản phẩm Phase 1 (Tình huống B — Pháp lý / Doanh nghiệp) |
| **Kiến trúc tác tử** | Đa tác tử Cấp độ 3 |
| **Người dùng mục tiêu** | Luật sư trẻ, cộng sự pháp lý, bộ phận pháp chế nội bộ |
| **Vấn đề cốt lõi** | 3–8 giờ cho mỗi hợp đồng khi thẩm định sơ bộ; rủi ro bịa đặt thông tin; $166 triệu/năm lãng phí vào lao động pháp lý lặp đi lặp lại |

---

## 1. Phiếu Bài 0 — Dòng Thời Gian Học Tập & Chốt Dự Án

**Hệ thống tác tử là gì:**
Một chuỗi 4 tác tử tự động hóa toàn bộ quy trình thẩm định sơ bộ lần đầu của một hợp đồng pháp lý.

**Danh sách tác tử:**

| Tác tử | Vai trò | Cấp độ mô hình |
|---|---|---|
| Tác tử Trích xuất | Phân tích tệp hợp đồng, bóc tách ngày tháng, số tiền, các bên liên quan | Claude Haiku (chi phí thấp) |
| Tác tử Chấm điểm Rủi ro | So sánh điều khoản với Bộ quy tắc công ty, gắn cờ vi phạm | Claude Sonnet |
| Tác tử Soạn thảo Phản hồi | Viết lại điều khoản rủi ro bằng ngôn ngữ bảo vệ quyền lợi | Claude Opus (chi phí cao, quan trọng) |
| Tác tử Kiểm chứng Trích dẫn | Đối chiếu mọi trích dẫn pháp lý qua thư viện luật chính thống | Claude Haiku + giao diện lập trình bên ngoài |

**Vì sao chủ đề này đủ điều kiện triển khai thực tế:**
- Người dùng rõ (luật sư trẻ), đầu vào rõ (hợp đồng dạng tệp), đầu ra rõ (báo cáo rủi ro + tài liệu phản hồi)
- Nỗi đau đo được: tiết kiệm 3–8 giờ mỗi hợp đồng, mục tiêu thay thế $166 triệu/năm chi phí nhân sự
- Rủi ro bịa đặt thực sự = buộc phải thảo luận thật về kiểm soát con người và độ tin cậy

---

## 2. Phiếu Bài 1 — Phòng Khám Triển Khai Doanh Nghiệp

### Đánh Giá Mức Độ Nhạy Cảm Dữ Liệu

| Loại dữ liệu | Mức độ nhạy cảm | Có thể rời khỏi tổ chức? |
|---|---|---|
| Nội dung hợp đồng (giao dịch khách hàng) | **Rất cao** | Không |
| Bộ quy tắc của công ty | Cao | Không |
| Trích dẫn pháp lý (án lệ) | Thấp đến trung bình | Có |
| Tài liệu phản hồi đã soạn thảo | Cao | Có kiểm soát |

### 3 Ràng Buộc Doanh Nghiệp Lớn Nhất

1. **Chủ quyền dữ liệu** — Nội dung hợp đồng chứa bí mật thương mại, chi tiết mua bán sáp nhập, điều khoản vay vốn. Không thể gửi đến điểm cuối của mô hình ngôn ngữ lớn trên đám mây công cộng nếu không có rủi ro pháp lý.
2. **Trách nhiệm pháp lý khi bịa đặt** — Một điều khoản bịa đặt hoặc một vụ kiện trích dẫn không có thật = luật sư bị kỷ luật + tổn thất hàng triệu đô. Mọi kết quả đầu ra của trí tuệ nhân tạo đều cần nguồn trích dẫn có thể kiểm chứng.
3. **Bắt buộc kiểm soát bởi con người** — Với hợp đồng giá trị >$500 nghìn, không có kết quả nào của trí tuệ nhân tạo được coi là cuối cùng nếu chưa có chữ ký phê duyệt của luật sư được cấp phép.

### Quyết định Triển khai: **Kết hợp Nội bộ và Đám mây**

| Tầng | Vị trí | Lý do |
|---|---|---|
| Tiếp nhận và cắt nhỏ tài liệu | Máy chủ nội bộ | Hợp đồng không bao giờ rời khỏi mạng nội bộ |
| Trích xuất & Chấm điểm rủi ro | Đám mây riêng (Khu vực Chính phủ / Bảo mật nâng cao) | Thỏa thuận xử lý dữ liệu, mạng riêng biệt |
| Soạn thảo phản hồi (Opus) | Đám mây riêng, phiên mã hóa | Lý luận giá trị cao, cần toàn bộ ngữ cảnh |
| Kiểm chứng trích dẫn | Đám mây công cộng + Giao diện thư viện luật | Án lệ là hồ sơ công khai |
| Giao diện xem xét của con người | Máy chủ nội bộ | Nhật ký kiểm tra ở lại nội bộ |

**Hai lý do chọn Kết hợp thay vì thuần Đám mây:**
1. Đặc quyền pháp lý — Tài liệu được bảo mật bởi quan hệ luật sư-khách hàng không thể đi qua điểm cuối công khai mà không từ bỏ đặc quyền đó
2. Nhật ký kiểm tra — Cơ quan quản lý (Đoàn Luật sư, Ủy ban Chứng khoán) yêu cầu nhật ký không thể thay đổi lưu tại chỗ

---

## 3. Phiếu Bài 2 — Phòng Thí Nghiệm Giải Phẫu Chi Phí

### Giả Định Lưu Lượng (Sản phẩm Khả Thi Tối Thiểu)

| Chỉ số | Ước lượng |
|---|---|
| Người dùng/ngày | 50 luật sư |
| Hợp đồng xử lý/ngày | 30 hợp đồng |
| Độ dài hợp đồng trung bình | 80 trang ≈ 120 nghìn đơn vị từ |
| Cao điểm (mùa mua bán sáp nhập) | 3 lần bình thường = 90 hợp đồng/ngày |

### Phân Tích Chi Phí Mỗi Hợp Đồng

| Tầng | Yếu tố chi phí | Chi phí ước tính/hợp đồng |
|---|---|---|
| Trích xuất (Haiku, 120 nghìn đơn vị từ đầu vào) | Chi phí đơn vị từ | ~$0.04 |
| Chấm điểm rủi ro (Sonnet, 120K vào + 2K ra) | Chi phí đơn vị từ | ~$0.43 |
| Soạn thảo phản hồi (Opus, 40K vào + 8K ra) | Chi phí đơn vị từ | ~$2.10 |
| Kiểm chứng trích dẫn (Haiku + Giao diện thư viện luật) | Đơn vị từ + lần gọi giao diện | ~$0.80 |
| Lưu trữ (hợp đồng + nhật ký kiểm tra) | Lưu trữ đám mây, lưu 3 năm | ~$0.01 |
| Nền tảng xem xét của con người (phân bổ) | Chi phí kỹ thuật | ~$0.15 |
| **Tổng mỗi hợp đồng** | | **~$3.53** |

### Chi Phí Hàng Tháng — Sản phẩm Khả Thi Tối Thiểu (30 hợp đồng/ngày × 22 ngày làm việc)

| Giai đoạn | Hợp đồng/tháng | Chi phí mô hình ngôn ngữ | Hạ tầng | Tổng cộng |
|---|---|---|---|---|
| **Sản phẩm tối thiểu** | 660 | ~$2,330 | ~$1,200 | **~$3,530/tháng** |
| **Tăng trưởng (5 lần)** | 3,300 | ~$11,650 | ~$3,500 | **~$15,150/tháng** |
| **Mở rộng quy mô (10 lần)** | 6,600 | ~$18,000* | ~$6,000 | **~$24,000/tháng** |

*Giai đoạn 10 lần được hưởng lợi từ chiết khấu theo khối lượng và tiết kiệm từ định tuyến mô hình.

**Chi phí ẩn nhóm hay quên nhất:**
- Phí truy cập thư viện luật: ~$2,000/tháng phí cơ bản
- Lương người xem xét (kiểm soát con người): 1 luật sư cấp cao × 20% thời gian = ~$8,000/tháng
- Bảo trì thiết kế câu lệnh khi cập nhật phiên bản mô hình
- Kiểm toán bảo mật (tuân thủ tiêu chuẩn an ninh): $15–30 nghìn một lần + $5 nghìn/năm

---

## 4. Phiếu Bài 3 — Tranh Luận Tối Ưu Chi Phí

### 3 Chiến Lược Được Chọn

**Chiến lược 1: Định tuyến mô hình (Thực hiện ngay)**
- Định tuyến theo độ phức tạp tác vụ: Trích xuất → Haiku, Chấm điểm rủi ro → Sonnet, Soạn thảo phản hồi → Opus chỉ khi điểm rủi ro > ngưỡng 7/10
- Tiết kiệm: **Giảm 65–70% chi phí đơn vị từ** (đã xác nhận trong đặc tả dự án)
- Đánh đổi: Chuỗi xử lý dài hơn một chút, cần logic định tuyến
- Thời điểm: Ngay từ ngày đầu, trước khi có lưu lượng thực

**Chiến lược 2: Bộ nhớ đệm ngữ nghĩa (Thực hiện ở giai đoạn tăng trưởng)**
- Lưu kết quả trích xuất cho các điều khoản hợp đồng giống nhau hoặc gần giống nhau (điều khoản mẫu xuất hiện trong hơn 60% hợp đồng)
- Dùng độ tương đồng véc-tơ (cosine > 0.95) để phục vụ điểm rủi ro từ bộ nhớ đệm mà không cần gọi lại mô hình ngôn ngữ
- Tiết kiệm: ~30% lần gọi Sonnet bị loại bỏ khi mở rộng quy mô
- Đánh đổi: Bộ nhớ đệm mất hiệu lực khi Bộ quy tắc được cập nhật
- Thời điểm: Sau khi xử lý hơn 1.000 hợp đồng (đủ dữ liệu để biện minh cho hạ tầng bộ nhớ đệm)

**Chiến lược 3: Cắt nhỏ theo cấu trúc + Suy luận có chọn lọc (Thực hiện ngay)**
- Cắt hợp đồng theo cấu trúc điều khoản/chương mục, không phải theo cửa sổ đơn vị từ cố định
- Bỏ qua các phần được gắn cờ là "điều khoản mẫu tiêu chuẩn" sau 100 hợp đồng đầu tiên (học xem phần nào an toàn để xử lý nhanh)
- Tiết kiệm: Giảm trung bình ~40% đơn vị từ đầu vào mỗi hợp đồng
- Đánh đổi: Rủi ro bỏ sót điều khoản phi tiêu chuẩn ẩn trong phần mẫu — giảm thiểu bằng kiểm tra ngẫu nhiên (5%)
- Thời điểm: Từ khi ra mắt sản phẩm tối thiểu

---

## 5. Phiếu Bài 4 — Diễn Tập Mở Rộng Quy Mô & Độ Tin Cậy

### 3 Tình Huống Sự Cố

| Tình huống | Tác động đến người dùng | Phản ứng ngắn hạn | Giải pháp dài hạn |
|---|---|---|---|
| **Lưu lượng tăng đột biến** (mùa mua bán sáp nhập, tải 10 lần) | Hàng đợi chất đống, luật sư chờ >5 phút | Trả về mã công việc bất đồng bộ, thông báo qua thư điện tử khi xong | Hàng đợi công việc, tự động mở rộng cụm suy luận Opus |
| **Hết thời gian chờ / nhà cung cấp mô hình gián đoạn** | Chuỗi xử lý đình trệ giữa chừng | Thử lại 3 lần với thời gian chờ tăng dần; chuyển dự phòng sang Sonnet cho soạn thảo nếu Opus không khả dụng | Dự phòng đa nhà cung cấp, cơ chế ngắt mạch tự động |
| **Trích dẫn bịa đặt lọt qua** | Luật sư nộp hồ sơ với án lệ giả → bị kỷ luật | Tác tử kiểm chứng chặn đầu ra cho đến khi thư viện luật xác nhận mọi trích dẫn | Cổng cứng: chuỗi xử lý không được phát tài liệu phản hồi nếu chưa qua 100% kiểm chứng trích dẫn |

### Các Chỉ Số Theo Dõi

- `độ trễ p99 mỗi lượt thẩm định` — Cam kết dịch vụ: <3 phút với hợp đồng <50 trang
- `tỷ lệ chặn do bịa đặt` — Số lần Tác tử Kiểm chứng từ chối mỗi ngày (cảnh báo nếu >5%)
- `chiều sâu hàng đợi xem xét con người` — Hợp đồng đang chờ phê duyệt (cảnh báo nếu >10)
- `chi phí mô hình mỗi hợp đồng` — Cảnh báo nếu vượt $6 (2 lần mức nền = lỗi định tuyến mô hình)
- `tỷ lệ trúng bộ nhớ đệm` — Nên >25% ở giai đoạn tăng trưởng

### Phương Án Dự Phòng

```
Nếu Opus không khả dụng:
  → Dùng Sonnet cho soạn thảo + bắt buộc kiểm soát con người với mọi hợp đồng (không chỉ >$500K)

Nếu giao diện thư viện luật gián đoạn:
  → Chặn đầu ra trích dẫn, gắn cờ hợp đồng là "Chờ Kiểm Chứng Trích Dẫn"
  → Thông báo cho luật sư, có thể tiếp tục với rủi ro tự chịu (được ghi nhật ký)

Nếu toàn bộ mô hình ngôn ngữ gián đoạn >15 phút:
  → Chuyển sang hàng đợi bất đồng bộ, cam kết dịch vụ 4 giờ
  → Không được hạ cấp xuống quy tắc cứng cho chấm điểm rủi ro pháp lý (cược quá cao)
```

---

## 6. Phiếu Bài 5 — Bản Đồ Kỹ Năng & Định Hướng Giai Đoạn 2

### Bảng Tự Đánh Giá Nhóm

| Hướng đi | Kỹ năng yêu cầu | Mức độ phù hợp với LexAI |
|---|---|---|
| **Kỹ thuật Trí tuệ Nhân tạo** | Điều phối đa tác tử, truy vấn tăng cường bằng truy xuất, thiết kế câu lệnh, đường ống đánh giá | **Phù hợp chính** — Chuỗi 4 tác tử cần kỹ thuật mô hình ngôn ngữ chuyên sâu |
| **Hạ tầng / Dữ liệu / Vận hành** | Cơ sở dữ liệu véc-tơ, triển khai, theo dõi, quản lý tài chính đám mây | **Phù hợp phụ** — Độ phức tạp của triển khai kết hợp |
| **Kinh doanh / Sản phẩm** | Lập mô hình lợi nhuận đầu tư, bán hàng doanh nghiệp, tuân thủ | Vai trò hỗ trợ mạnh |

**Hướng đi đề xuất cho Giai đoạn 2: Kỹ thuật Trí tuệ Nhân tạo** với vai trò hỗ trợ về Hạ tầng.

**Kỹ năng cần bổ sung:**
1. Khung điều phối đa tác tử (LangGraph hoặc tự xây dựng) để quản lý trạng thái
2. Truy vấn tăng cường cho lĩnh vực pháp lý — cắt nhỏ văn bản luật khác với văn bản thông thường
3. Khung đánh giá phát hiện bịa đặt (không chỉ dừng ở kiểm chứng trích dẫn)

---

## 7. Đợt Nước Rút Buổi Chiều — Đề Xuất 7 Khối Nội Dung (Cấu Trúc Slide)

### Slide 1: Tổng Quan Dự Án
- **Tên:** LexAI — Thẩm Định Hợp Đồng Pháp Lý bằng Trí Tuệ Nhân Tạo
- **Người dùng:** Luật sư trẻ tại các công ty luật quy mô vừa (50–200 luật sư)
- **Vấn đề:** $166 triệu/năm toàn ngành lãng phí vào thẩm định sơ bộ; 3–8 giờ mỗi hợp đồng
- **Giải pháp:** Chuỗi 4 tác tử thẩm định, chấm điểm rủi ro, soạn thảo phản hồi và kiểm chứng trích dẫn trong <3 phút
- **Luận điểm thu hút nhà đầu tư:** Chúng tôi là giải pháp duy nhất có Tác tử Kiểm chứng tạo ra nhật ký kiểm tra pháp lý cứng — loại bỏ rủi ro hàng đầu của trí tuệ nhân tạo trong ngành luật

### Slide 2: Bối Cảnh Doanh Nghiệp & Ràng Buộc
- Dữ liệu: Hợp đồng được bảo mật bởi quan hệ luật sư-khách hàng, tài liệu mua bán sáp nhập, thỏa thuận vay vốn
- Ràng buộc chính: Chủ quyền dữ liệu, trách nhiệm khi bịa đặt, bắt buộc kiểm soát con người với hợp đồng >$500K
- Vì sao khó: Không phải trợ lý trả lời câu hỏi thông thường — nó cần phải **đúng**, không chỉ hữu ích

### Slide 3: Kiến Trúc — Triển Khai Kết Hợp

```
[Máy Chủ Nội Bộ Công Ty Luật]     [Đám Mây Riêng Bảo Mật Cao]
   Tiếp nhận tệp hợp đồng   →     Tác tử Trích xuất (Haiku)
   Kho Bộ quy tắc            →     Tác tử Chấm điểm Rủi ro (Sonnet)
   Giao diện Xem xét Con người ←  Tác tử Soạn thảo Phản hồi (Opus) [chỉ khi rủi ro > 7]
   Cơ sở dữ liệu Nhật ký     ←   Tác tử Kiểm chứng Trích dẫn (Haiku + Thư viện Luật)
```

### Slide 4: Giải Phẫu Chi Phí
- Sản phẩm tối thiểu: **$3,530/tháng** (660 hợp đồng, 50 người dùng)
- Tăng trưởng (5 lần): **$15,150/tháng**
- Mô hình doanh thu: $150/hợp đồng → hòa vốn ở 24 hợp đồng/tháng
- **Lợi nhuận đầu tư cho công ty luật: 3–6 lần** (thay thế 2–3 giờ luật sư trẻ mỗi hợp đồng @ $300/giờ)

### Slide 5: Kế Hoạch Tối Ưu Chi Phí

| Chiến lược | Tiết kiệm | Thời điểm |
|---|---|---|
| Định tuyến mô hình | 65–70% | Ngay từ đầu |
| Cắt nhỏ theo cấu trúc | Giảm 40% đơn vị từ | Ngay từ đầu |
| Bộ nhớ đệm ngữ nghĩa | 30% lần gọi Sonnet | Sau 1.000 hợp đồng |

### Slide 6: Độ Tin Cậy & Mở Rộng Quy Mô
- Hàng đợi bất đồng bộ cho lưu lượng tăng đột biến (mùa mua bán sáp nhập)
- Cổng kiểm chứng cứng — không phát đầu ra nếu chưa có xác nhận từ thư viện luật
- Chuỗi leo thang đến con người cho mọi hợp đồng >$500K
- Dự phòng đa nhà cung cấp: Anthropic → nhà cung cấp thay thế

### Slide 7: Khuyến Nghị Hướng Đi & Bước Tiếp Theo
- **Hướng đi Giai đoạn 2: Kỹ thuật Trí tuệ Nhân tạo**
- 3 cột mốc tiếp theo:
  1. Xây dựng bộ đánh giá phát hiện bịa đặt (Tuần 1–2)
  2. Tích hợp môi trường thử nghiệm giao diện thư viện luật (Tuần 3–4)
  3. Triển khai sản phẩm tối thiểu cho 1 công ty luật thí điểm với 5 hợp đồng thực (Tháng 2)
- **Kêu gọi đầu tư:** $250 nghìn vòng hạt giống để xây dựng sản phẩm tối thiểu sẵn sàng triển khai + hợp tác thí điểm doanh nghiệp đầu tiên

---

## 8. Danh Sách Kiểm Tra Chấm Điểm (Kiểm Toán Trước Khi Trình Bày)

Trước khi trình bày, xác nhận nhóm có thể tự tin trả lời:

- [ ] **Bối cảnh doanh nghiệp (15đ):** Có thể kể tên 3 ràng buộc lớn nhất và giải thích vì sao mỗi cái không thể thương lượng?
- [ ] **Kiến trúc (20đ):** Có thể bảo vệ Kết hợp thay vì thuần Đám mây với 2 lý do pháp lý cụ thể?
- [ ] **Phân tích chi phí (20đ):** Có thể trình bày chi phí theo thành phần ở cả mức tối thiểu lẫn tăng trưởng? Có bao gồm chi phí lao động kiểm soát con người không?
- [ ] **Tối ưu hóa (15đ):** Có thể giải thích đánh đổi của bộ nhớ đệm ngữ nghĩa (mất hiệu lực khi Bộ quy tắc cập nhật)?
- [ ] **Độ tin cậy (15đ):** Điều gì xảy ra khi giao diện thư viện luật gián đoạn? Điều gì xảy ra trong cao điểm mùa mua bán sáp nhập?
- [ ] **Hướng đi Giai đoạn 2 (10đ):** Vì sao Kỹ thuật Trí tuệ Nhân tạo chứ không phải Hạ tầng? Kỹ năng cụ thể nào nhóm cần học thêm?
- [ ] **Trình bày (5đ):** Trong 5 phút, 1 câu hỏi phản biện nhóm dự đoán và cách trả lời

---

## 9. Khung Thuyết Phục Nhà Đầu Tư (Cho Kế Hoạch Kinh Doanh)

> "Chúng tôi không xây dựng một trợ lý trả lời câu hỏi pháp lý thông thường. Chúng tôi đang xây dựng **hạ tầng tuân thủ** mà mọi công ty luật sẽ buộc phải có nhật ký kiểm tra khi quy định về trí tuệ nhân tạo siết chặt. Tác tử Kiểm chứng Trích dẫn là lợi thế cạnh tranh — không có đối thủ nào có cơ chế chặn cứng bịa đặt với xác nhận nguồn pháp lý bên ngoài. Đặt chân vào 5 công ty luật quy mô vừa đầu tiên cho chúng tôi dữ liệu huấn luyện để lợi thế đó ngày càng rộng thêm mỗi quý."

**Các chỉ số chính cần sẵn sàng trình bày với nhà đầu tư:**

| Chỉ số | Giá trị |
|---|---|
| Tổng thị trường tiềm năng | $23 tỷ thị trường công nghệ pháp lý toàn cầu, $8 tỷ phân khúc thẩm định hợp đồng |
| Thời gian hoàn vốn cho công ty luật | 4–8 tháng |
| Lợi nhuận đầu tư ròng cho khách hàng | 3–6 lần trong Năm 1 |
| Kinh tế đơn vị | Chi phí $3.53 → thu $150 → **biên lợi nhuận gộp 42 lần mỗi hợp đồng** |
| Thời gian tiết kiệm | 2–3 giờ mỗi luật sư mỗi tuần |
| Chi phí lao động được thay thế | Lên đến $166 triệu/năm khi mở rộng quy mô |
