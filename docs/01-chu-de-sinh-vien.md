## Danh sách chủ đề sinh viên lựa chọn

Ngày tổng kết Phase 1 - Mini Project 'Enterprise Readiness Review'

## Hướng dẫn sử dụng file này

File này dùng để xác nhận chủ đề mà mỗi nhóm sẽ theo đuổi trong cả ngày . Giảng viên có thể cho nhóm chọn một trong hai hướng:

- dùng chính sản phẩm/agent đã làm ở Phase 1;
- dùng một scenario card mẫu nếu nhóm chưa có dự án đủ rõ để phân tích production.

Phần cuối file có bảng đăng ký chủ đề để lớp điền trực tiếp.

## 1. Chủ đề trung tâm của ngày tổng kết

Mọi nhóm đều xoay quanh cùng một đề bài lớn:

## Đề bài chung

## Enterprise Readiness Review cho AI Agent của nhóm

Nhóm bạn đóng vai một team chuẩn bị đưa AI agent vào môi trường thực tế. Hãy trả lời:

1. Bài toán thật sự mà agent giải quyết là gì?
2. Nên triển khai theo Cloud, On-prem hay Hybrid?
3. Có ràng buộc enterprise nào phải xử lý trước?
4. Cost ở mức MVP và Growth là bao nhiêu?
5. Nên tối ưu chi phí bằng 3 chiến lược nào?
6. Nếu cần scale, nhóm sẽ xử lý reliability và vận hành ra sao?
7. Dự án phù hợp với track nào ở Phase 2?

## 2. Nhóm chủ đề A - Dùng sản phẩm đã làm ở Phase 1

## 2.1. Khi nào nên chọn hướng này?

Nhóm nên chọn chính sản phẩm cũ nếu đã có đủ 4 điều sau:

1. đã có use case rõ ràng;
2. có mô tả người dùng và luồng sử dụng;

3. có thể ước lượng tương đối traffic, dữ liệu và cost;
4. có đủ hiểu biết để tranh luận về deployment, optimization và scaling.

## 2.2. Mẫu chủ đề có thể chọn từ sản phẩm Phase 1

- AI trợ lý nội bộ tra cứu tài liệu/quy trình
- AI agent hỗ trợ tuyển sinh/học vụ
- AI copilot cho chăm sóc khách hàng hoặc hỗ trợ ticket
- AI workflow assistant cho team vận hành
- Agent RAG cho tài liệu doanh nghiệp
- Multi-agent xử lý quy trình nội bộ
- AI evaluator/monitoring assistant cho sản phẩm đã deploy

## 2.3. Những gì nhóm phải xác nhận trước khi chốt chủ đề

1. Tên dự án/agent hiện tại là gì?
2. Người dùng cuối là ai?
3. Dữ liệu mà agent sẽ dùng thuộc loại nào?
4. Nếu trả lời sai hoặc chậm thì ảnh hưởng gì?
5. Hệ thống này có khả năng được triển khai thật trong bối cảnh nào?

## 3. Nhóm chủ đề B - Scenario cards mẫu

## 3.1. Cách dùng scenario cards

Mỗi nhóm chọn một scenario card để làm bài phân tích cả ngày. Scenario card giúp nhóm có cùng chất lượng đầu vào để thảo luận deployment, cost, reliability và track selection.

## 3.2. Scenario 1 - Bank Operations Copilot

Bối cảnh: Một ngân hàng muốn triển khai AI assistant hỗ trợ nhân viên tra cứu quy định nội bộ, KYC, SOP tín dụng và các bước xử lý hồ sơ. Người dùng chính: giao dịch viên, back-office, compliance. Dữ liệu liên quan:

policy nội bộ, biểu mẫu tín dụng, hồ sơ khách hàng, tài liệu compliance.

## Ràng buộc enterprise:

- dữ liệu nhạy cảm rất cao;

- cần audit trail;
- có thể không được đưa toàn bộ dữ liệu lên public cloud;
- cần tích hợp với hệ thống cũ.

## Điểm tranh luận chính:

- Cloud, On-prem hay Hybrid?
- Phần nào được phép đưa vào prompt?
- Ở đâu bắt buộc phải có human review?

## 3.3. Scenario 2 - Hospital Clinical Support Assistant

Bối cảnh: Bệnh viện muốn dùng AI để hỗ trợ tóm tắt hồ sơ, hướng dẫn quy trình nội bộ và trả lời câu hỏi hành chính. Người dùng chính: bác sĩ, điều dưỡng, nhân viên tiếp nhận. Dữ liệu liên quan: bệnh án, lịch khám, quy trình nội bộ, biểu mẫu.

## Ràng buộc enterprise:

- dữ liệu sức khỏe rất nhạy cảm;
- cần phân quyền truy cập;
- độ chính xác ảnh hưởng mạnh tới vận hành;
- phụ thuộc vào HIS/EMR hoặc hệ thống cũ.

## Điểm tranh luận chính:

- AI được hỗ trợ ở mức hành chính hay chuyên môn?
- Nếu có hallucination, hậu quả là gì?
- Cần approval của con người ở bước nào?

## 3.4. Scenario 3 - University Admissions &amp; Student Support Agent

Bối cảnh: giải đáp học vụ và đồng hành với sinh viên mới.

Người dùng chính:

Trường đại học muốn triển khai AI assistant hỗ trợ tư vấn tuyển sinh, thí sinh, sinh viên năm nhất, cán bộ tuyển sinh.

Dữ liệu liên quan: FAQ tuyển sinh, quy định đào tạo, biểu mẫu, dữ liệu hồ sơ ứng viên.

## Ràng buộc enterprise:

- traffic mùa cao điểm tăng rất mạnh;
- nội dung cần cập nhật theo từng đợt;
- dữ liệu cá nhân ở mức trung bình;

- cần trải nghiệm phản hồi tốt.

## Điểm tranh luận chính:

- Cost driver lớn nhất nằm ở đâu?
- Có cần queue/fallback trong mùa tuyển sinh không?
- Cloud có đủ hay cần Hybrid?

## 3.5. Scenario 4 - Manufacturing Maintenance Copilot

Bối cảnh: Một nhà máy muốn AI assistant hỗ trợ kỹ thuật viên tra cứu SOP , log bảo trì, lỗi máy móc và checklist an toàn.

Người dùng chính:

kỹ thuật viên bảo trì, quản đốc xưởng, team vận hành.

Dữ liệu liên quan:

SOP, manual máy móc, log bảo trì, lịch vận hành.

## Ràng buộc enterprise:

- nhiều hệ thống legacy;
- có khu vực mạng nội bộ không cho internet;
- cần độ ổn định cao;
- thiết bị và mạng có thể không đồng đều.

## Điểm tranh luận chính:

- On-prem có đáng cân nhắc không?
- Nếu mạng yếu thì hệ thống vận hành ra sao?
- Edge deployment có cần thiết không?

## 3.6. Scenario 5 - Logistics Operations Agent

Bối cảnh: Một công ty logistics muốn AI assistant hỗ trợ điều phối đơn hàng, giải thích trạng thái giao hàng, xử lý ticket và tra cứu SOP .

Người dùng chính:

điều phối viên, CS team, quản lý vận hành.

Dữ liệu liên quan: đơn hàng, trạng thái giao vận, ticket, lịch giao hàng.

## Ràng buộc enterprise:

- cần kết nối nhiều hệ thống;
- giờ cao điểm rõ rệt;
- yêu cầu phản hồi nhanh;
- sai thông tin dễ gây khiếu nại.

## Điểm tranh luận chính:

- Nên cache ở lớp nào?
- Provider lỗi thì fallback ra sao?
- Cost lớn nhất nằm ở xử lý nào?

## 4. Nguyên tắc chọn chủ đề

1. Chủ đề phải đủ rõ để tranh luận deployment, cost, optimization và reliability .
2. Không chọn chủ đề quá mơ hồ chỉ dừng ở mức ý tưởng tính năng.
3. Không chọn chủ đề mà nhóm hoàn toàn không thể ước lượng user, data hay risk.
4. Nếu sản phẩm cũ chưa đủ rõ, chuyển sang scenario card để bảo đảm chất lượng thảo luận.

## 5. Bảng đăng ký chủ đề cho các nhóm

|   STT | Tên nhóm   | Chủ đề chọn   | Loại chủ đề        | Ghi chú nhanh   |
|-------|------------|---------------|--------------------|-----------------|
|     1 |            |               | Phase 1 / Scenario |                 |
|     2 |            |               | Phase 1 / Scenario |                 |
|     3 |            |               | Phase 1 / Scenario |                 |
|     4 |            |               | Phase 1 / Scenario |                 |
|     5 |            |               | Phase 1 / Scenario |                 |
|     6 |            |               | Phase 1 / Scenario |                 |
|     7 |            |               | Phase 1 / Scenario |                 |
|     8 |            |               | Phase 1 / Scenario |                 |
|     9 |            |               | Phase 1 / Scenario |                 |
|    10 |            |               | Phase 1 / Scenario |                 |

## 6. Phiếu chốt chủ đề của từng nhóm

Tên nhóm:

Chủ đề được chọn:

Nguồn chủ đề: □ Sản phẩm Phase 1 □ Scenario card Lý do chọn chủ đề này:

## 3 từ khóa mô tả bối cảnh enterprise của chủ đề:

Người xác nhận:

Thời gian: