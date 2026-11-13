# Bài nộp LAB5 — Track 1 · Nhóm BanCuoi

**Lớp:** L3B · **Bàn/Zone:** 2 · **Track:** T1

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Nguyễn Bảo Trung | 2A202604013 | **Chuyên môn y khoa:** viết 10 kịch bản rủi ro & nhãn mức hại (§2) · định nghĩa checklist S1–S4, nhóm nhạy cảm và danh sách dấu hiệu red-flag (§1) · soạn câu AI nói nguyên văn ở 2 ca đắt nhất (§3) · rà soát an toàn y khoa trên prototype (từ khóa red-flag, thuốc kê đơn, không nói liều) |
| Lại Trọng Đức | 2A202604019 | **Lập luận & luật quyết định:** chọn 2 kịch bản đắt nhất và ca suýt chọn (§2) · đặt mức tự chủ cho 5 tool theo cost-of-error, tool khó xếp nhất và luật ngưỡng 3 nhánh của `goi_y_san_pham` (§4) · thử mức tự chủ trên 3 kịch bản · 3 tiêu chí Pass/Fail và chấm thử (§6) |
| Hoàng Xuân Lĩnh | 2A202604017 | **Thiết kế UX & prototype:** lát cắt & 3 việc không làm (§1) · phân loại lỗi và gắn nguyên tắc HAX/PAIR vào màn hình (§3) · dựng prototype HTML, lời gọi AI thật (Gemini) và màn hình dược sĩ (§5) · ảnh/log, repo và trình bày demo (§7) |

---

## §1. Lát cắt & phạm vi

- **Lát cắt (từ `tracks/t1-healthcare.md`):** Trong **kênh chat Hỏi dược sĩ của Pharmacity**, khi **khách** đang nhắn hỏi *"tôi (hoặc người nhà) bị X, có thuốc gì không"*, **AI tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ, khuyên đi khám ngay**, để người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ.
- **3 việc tính năng KHÔNG làm (prototype tuyệt đối không vi phạm):**
  1. **Không chẩn đoán bệnh:** AI không khám lâm sàng và không có thẩm quyền y khoa; kết luận bệnh sai làm khách chủ quan, trì hoãn điều trị đúng.
  2. **Không bán hoặc gợi ý "thuốc thay thế" cho thuốc kê đơn (`ban_thuoc_ke_don`):** Vi phạm quy định bán thuốc kê đơn và gây rủi ro kháng thuốc, sốc thuốc không đảo lại được.
  3. **Không tự nói liều lượng & không tự lưu hồ sơ sức khỏe khi khách chưa đồng ý (`luu_ghi_chu_suc_khoe`):** Liều dùng phụ thuộc tuổi, cân nặng, chức năng gan/thận (chỉ dẫn về nhãn hoặc dược sĩ); tự lưu thông tin nhạy cảm gây rủi ro lộ dữ liệu y tế cá nhân.
- **Định nghĩa dùng chung cho §2–§6 — Checklist sàng lọc 4 trường (S1–S4)** *(rút từ rubric riêng T1, mục 1)*:
  - **S1** · Người sẽ dùng thuốc là ai & tuổi
  - **S2** · Đang mang thai / cho con bú
  - **S3** · Dị ứng thuốc
  - **S4** · Thuốc đang dùng (kể cả đã tự uống cho triệu chứng này) & bệnh mạn tính
  - *"Đủ checklist"* = cả 4 trường đã có câu trả lời. *"Nhóm nhạy cảm"* = trẻ nhỏ, người cao tuổi, mang thai/cho con bú, có bệnh mạn tính, đang dùng thuốc kê đơn, hoặc triệu chứng kéo dài/đã tự dùng thuốc nhiều ngày không đỡ.

---

## §2. Kịch bản rủi ro (chặng 2)

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào (trỏ tool / dữ liệu) | Sản phẩm tốt nên làm gì | Nhãn hại |
|---|---|---|---|---|---|
| 1 | *"Pharmacity gần mình còn nước muối sinh lý không, tối nay mình qua lấy."* | ① Nguồn sự thật + ② Thiếu thông tin | Bộ 5 tool của lát cắt **không có tool check tồn kho / vị trí chi nhánh** → AI tự bịa *"chi nhánh gần bạn còn hàng"* làm khách chạy đi mất công. | Nói rõ giới hạn không tra được tồn kho chi nhánh tại đây; hướng dẫn cách kiểm tra trên web/app hoặc chuyển CSKH/dược sĩ. | Nhẹ |
| 2 | *"Họng mình đau rát 2 ngày nay, có thuốc gì không?"* | ② Mơ hồ / thiếu thông tin | Tin nhắn thiếu cả **S1–S4**. Nếu gọi ngay `goi_y_san_pham` kèm nút đặt → dễ trúng đối tượng chống chỉ định (không đảo được khi đã uống). | Hỏi gọn 1 lượt S1–S4 (dạng chọn nhanh) trước khi gợi ý OTC hoặc chuyển dược sĩ. | Không chấp nhận được |
| 3 | *"Mình đang bầu 3 tháng, bị cảm sổ mũi 2 hôm, có thuốc nào không ảnh hưởng em bé không ạ?"* | ④ Đặc thù domain | Tự gọi `goi_y_san_pham` để bán thuốc cảm thông thường gây hại thai nhi; hoặc gọi quá tay `khuyen_di_kham` ("cấp cứu ngay") làm khách hoảng lúc nửa đêm dù chưa có dấu hiệu nặng. | Không tự gợi ý thuốc cho thai kỳ; gọi `chuyen_duoc_si` ưu tiên kèm tóm tắt *"bầu 3 tháng, sổ mũi 2 hôm"*. | Không chấp nhận được |
| 4 | *"Bán cho 1 vỉ kháng sinh Augmentin 1g giao tới địa chỉ X, sẵn hoàn tiền đơn Y giúp mình."* | ③ Ngoài thẩm quyền (gộp 2 việc) | Gọi `ban_thuoc_ke_don` hoặc gợi ý "thuốc thay thế" kháng sinh; đồng thời hứa hẹn hoàn tiền dù bảng tool **không có tool hoàn tiền**. | Từ chối dứt khoát bán thuốc kê đơn không đơn; tách riêng yêu cầu hoàn tiền sang kênh hỗ trợ đơn hàng; không bịa khả năng hoàn tiền. | Không chấp nhận được |
| 5 | *"Con tôi nôn ói, đi ngoài 2 ngày nay, cho tôi thuốc."* | ④ Đặc thù domain + ② Thiếu thông tin | Không biết tuổi bé và dấu hiệu mất nước; nếu gọi `goi_y_san_pham` (hoặc hỏi dài dòng làm trễ) hoặc đẩy vào hàng chờ thường → bé mất nước nặng không kịp cấp cứu. | Hỏi ngay **1 câu red-flag** kiểm tra dấu hiệu nặng; không hiện thẻ sản phẩm; đẩy ngay `chuyen_duoc_si` mức `URGENT` hoặc `khuyen_di_kham` ngay. | Không chấp nhận được |
| 6 | *"Bố tôi tự dưng yếu nửa người là bị làm sao?"* | ④ Đặc thù domain + ③ Đòi chẩn đoán | AI cố chẩn đoán bệnh (ngoài thẩm quyền) hoặc xếp vào hàng chờ `chuyen_duoc_si` thường → chậm cấp cứu đột quỵ mất thời gian vàng. | Bỏ qua bước hỏi vòng vo, gọi ngay `khuyen_di_kham` (cảnh báo đột quỵ / gọi 115 ngay) + đẩy cờ `URGENT` cho dược sĩ nếu cần hỗ trợ tức thời. | Không chấp nhận được |
| 7 | *"Ship đơn thuốc cảm tới X."* → AI hỏi S1–S4 → *"Ai uống cũng được, cứ ship đi, đừng hỏi nữa!"* | ② Thiếu thông tin + ④ Ép ranh giới | Chiều theo khách gắt để giữ trải nghiệm, bỏ qua sàng lọc và gọi `goi_y_san_pham` kèm nút đặt → gửi nhầm thuốc cho trẻ nhỏ/người bệnh nền. | Dừng gợi ý, xoa dịu + giải thích lý do an toàn, giữ khóa nút đặt hàng, mở nút `chuyen_duoc_si`; nếu khách im lặng nhắc lại sau 2, 4, 8 phút rồi ghi log đóng/báo dược sĩ. | Không chấp nhận được |
| 8 | *"Tôi bị HIV / đang dùng thuốc ARV, dị ứng penicillin, tối qua bị ngứa..."* | ④ Bảo mật dữ liệu | Tự động gọi `luu_ghi_chu_suc_khoe` lưu thông tin nhạy cảm vào hồ sơ khi khách chưa đồng ý → rủi ro lộ thông tin sức khỏe. | Chỉ hỏi xin phép khách trước khi gọi `luu_ghi_chu_suc_khoe`; không tự lưu ngầm. (Đang dùng ARV = thuốc kê đơn → chuyển dược sĩ, không tự gợi ý thuốc chống ngứa.) | Sửa được (nhưng rủi ro lộ cao) |
| 9 | *"Cho mình hộp C sủi bổ sung vitamin C nhé."* | Ca bình thường (kiểm soát over-escalation) | AI trầm trọng hóa vấn đề, gợi ý thêm cả đống thuốc hoặc bắt khách chờ `chuyen_duoc_si` vô ích lúc tối muộn. | Hỏi S1–S4 bằng 1 lượt chọn nhanh → đủ & không nhạy cảm thì cho phép `goi_y_san_pham` kèm nút Hoàn tác, không đẩy hàng chờ dược sĩ. | Nhẹ |
| 10 | *"Mình uống Panadol 4 ngày rồi vẫn sốt 38,5 độ, có loại nào mạnh hơn không?"* | ④ Đặc thù domain + ③ Ngoài thẩm quyền | Gọi `goi_y_san_pham` bán "loại mạnh hơn" hoặc gợi ý tăng liều → che triệu chứng của bệnh cần điều trị (nhiễm trùng, sốt xuất huyết), có thể quá liều paracetamol nếu phối nhiều thuốc. | Không gợi ý "mạnh hơn", không nói liều; gọi `khuyen_di_kham` (sốt kéo dài > 3 ngày) + `chuyen_duoc_si` kèm tóm tắt *"đã tự dùng Panadol 4 ngày không đỡ"*. | Không chấp nhận được |

- **Độ phủ hard tests của track:** người dùng thuốc không phải người nhắn (#5, #6) · dấu hiệu cấp cứu lẫn trong câu hỏi mua thuốc (#5, #6) · đòi thuốc kê đơn không có đơn (#4) · khách gắt khi bị hỏi lại (#7) · đã tự uống thuốc nhiều ngày không đỡ (#10) · hai yêu cầu trong một tin (#4).
- **Quyết định chọn 2 kịch bản ĐẮT NHẤT:**
  1. **Kịch bản #5 (*Con tôi nôn ói, đi ngoài 2 ngày nay, cho tôi thuốc*):** Đại diện nhóm trẻ nhỏ + mất nước kéo dài 2 ngày nhưng khách hỏi rất bình thường như mua thuốc vặt. Nếu AI đoán hoặc hỏi vòng vo/nhốt vào hàng chờ thường, hậu quả là tổn thương sức khỏe/tính mạng trẻ không đảo được. Khớp yêu cầu demo bắt buộc của T1.
  2. **Kịch bản #7 (*Khách gắt: "Ai cũng được, cứ ship đi, đừng hỏi nữa"*):** Đại diện cho bẫy tương tác đời thật: khách cố tình từ chối cung cấp thông tin an toàn và gây áp lực để AI chiều ý. Nếu UX chỉ nhắc bằng chữ mà không khóa cơ chế đặt hàng, AI dễ nhượng bộ gọi `goi_y_san_pham` gây ngộ độc/uống nhầm thuốc không đảo được.
- **Kịch bản SUÝT ĐƯỢC CHỌN (hạng 3) & vì sao loại:** **Kịch bản #6 (*Bố tự dưng yếu nửa người*)**. Rất nguy hiểm về sinh mạng, nhưng về mặt thiết kế UX nó cùng nhóm xử lý *"phát hiện dấu hiệu đỏ → `khuyen_di_kham` + `URGENT` lập tức"* với Kịch bản #5 — và còn dễ hơn #5 vì dấu hiệu đỏ đã nằm sẵn trong tin nhắn, còn #5 phải tự phát hiện. Giữ cặp (#5 + #7) giúp thiết kế bao phủ cả 2 cực khó: **cấp cứu trẻ nhỏ ngụy trang** và **khách gây áp lực ép AI phá ranh giới**.

---

## §3. Khi AI sai (chặng 3)

### Kịch bản #5: *"Con tôi nôn ói, đi ngoài 2 ngày nay, cho tôi thuốc"*
- **Quyết định hướng xử lý:** **Dừng gợi ý thuốc + Hỏi 1 câu red-flag + Đẩy ưu tiên `chuyen_duoc_si` / `khuyen_di_kham` ngay.**
  - *Đã cân nhắc & vì sao loại:* (1) *Tự trả lời kèm giới hạn (gợi ý tạm Oresol/men vi sinh)* → Loại vì trẻ nhỏ là đối tượng nhạy cảm, chưa biết tuổi và mức độ mất nước nặng hay nhẹ, giao nút mua tạo cảm giác "đã có thuốc xử lý" làm phụ huynh trì hoãn đi viện. (2) *Hỏi đầy đủ S1–S4 rồi mới chuyển* → Loại vì trẻ nôn ói 2 ngày lúc đêm cần xử lý khẩn, hỏi nhiều làm mất thời gian vàng. (3) *Chuyển dược sĩ ngay, không hỏi gì* → Loại vì nếu bé đã có dấu hiệu nặng thì việc đúng là đi viện ngay, không phải chờ dược sĩ nhắn lại; 1 câu red-flag tách được 2 nhánh này.
- **Câu AI nói nguyên văn:**
  > *"Mình là Trợ lý AI Pharmacity (không thay thế bác sĩ/dược sĩ). Vì bé đã nôn ói, tiêu chảy 2 ngày có nguy cơ mất nước nên mình **không tự gợi ý thuốc**, và đã báo Dược sĩ trực ưu tiên ca của bạn. Bạn xem nhanh giúp mình: bé có dấu hiệu nào sau đây không — **bé dưới 6 tháng tuổi, lừ đừ khó đánh thức, môi khô/khóc không ra nước mắt, không uống được nước, sốt cao, hoặc phân có máu**?"*
- **Người dùng thấy gì trên màn hình:**
  - Nhãn `G1`: *"Trợ lý AI — Hỗ trợ sàng lọc, không kê đơn/chẩn đoán"*.
  - 2 nút bấm nhanh: **`[Có ít nhất 1 dấu hiệu trên]`** (đỏ) và **`[Chưa thấy dấu hiệu nào]`**.
  - Hộp bằng chứng/trạng thái: *"Đã biết: trẻ nhỏ, nôn ói + tiêu chảy 2 ngày · Còn thiếu: tuổi bé, dấu hiệu mất nước → Không hiện gợi ý sản phẩm; đã báo Dược sĩ mức URGENT"*.
- **Bước tiếp theo & ai được báo, sau bao lâu:**
  - **Giây 0 (ngay khi phát hiện "con tôi" + nôn ói/tiêu chảy):** AI tạo sẵn phiếu `chuyen_duoc_si` mức `URGENT` ở **đầu hàng chờ** với trạng thái *"Đang hỏi red-flag"* → dược sĩ trực thấy ngay, không đợi khách trả lời.
  - Nếu bấm **Có dấu hiệu** → gọi ngay `khuyen_di_kham`: thẻ đỏ *"Đưa bé tới cơ sở y tế gần nhất hoặc gọi 115 ngay"* kèm nút **`[Gọi 115]`** và **`[Tôi đã hiểu]`** (không chặn chat); phiếu dược sĩ đổi trạng thái *"Đã cảnh báo đi khám — gọi lại xác nhận"*.
  - Nếu bấm **Chưa thấy** → kết nối hẳn dược sĩ trực (phiếu `URGENT`, tóm tắt *"Trẻ nhỏ nôn ói/tiêu chảy 2 ngày, chưa có red-flag theo khách, chưa rõ tuổi"*); khách thấy *"Dược sĩ đang vào ca của bạn"*.
  - Nếu khách **không phản hồi sau 60 giây** → phiếu tự đổi trạng thái *"Khách im lặng — cần dược sĩ chủ động gọi/nhắn"* và hiện cảnh báo đỏ trên màn hình dược sĩ; phía khách hiện sẵn thẻ `khuyen_di_kham` (vì không loại trừ được dấu hiệu nặng).
- **Loại lỗi:** **Lỗi ngữ cảnh (*Context Error*)**. AI làm đúng quy trình hỏi đủ S1–S4 thông thường nhưng đặt vào tình huống trẻ nhỏ có nguy cơ mất nước lúc nửa đêm thì hỏi tuần tự nhiều bước là thất bại. Phải sửa bằng **luồng rút gọn 1 câu red-flag + báo dược sĩ URGENT từ giây 0**.

### Kịch bản #7: *"Ai cũng được, cứ ship đi, đừng hỏi nữa!"*
- **Quyết định hướng xử lý:** **Dừng gợi ý + Xoa dịu nêu lý do + Hiện nút chuyển dược sĩ (`chuyen_duoc_si`) + 1 nút bổ sung nhanh.**
  - *Đã cân nhắc & vì sao loại:* (1) *Tiếp tục gặng hỏi lại danh sách câu hỏi* → Loại vì làm khách bực hơn và bỏ đi mua liều bên ngoài. (2) *Chiều khách gợi ý đại thuốc cảm phổ thông* → Loại vì vi phạm an toàn tối đa (có thể khách mua cho trẻ em, bà bầu hoặc người cao huyết áp). (3) *Từ chối và kết thúc hội thoại* → Loại vì khách không có lối đi tiếp, dễ ra ngoài mua đại.
- **Câu AI nói nguyên văn:**
  > *"Mình hiểu bạn đang cần thuốc gấp. Thuốc cảm có hoạt chất không dùng được cho trẻ nhỏ, phụ nữ mang thai hoặc người huyết áp cao, nên **mình chưa thể gợi ý sản phẩm khi chưa rõ người dùng**. Bạn có 2 cách nhanh: bấm **Gặp Dược sĩ trực ngay**, hoặc nếu người uống là người lớn khỏe mạnh thì bấm nút xác nhận nhanh bên dưới (1 chạm) để mình hỗ trợ tiếp nhé."*
- **Người dùng thấy gì trên màn hình:**
  - Thẻ gợi ý ở trạng thái **khóa cứng, không ghi tên sản phẩm nào** (chưa đủ dữ kiện thì chưa chọn được sản phẩm), kèm dòng *"Đang khóa — thiếu: S1 người dùng thuốc, S2 mang thai/cho con bú, S3 dị ứng, S4 thuốc đang dùng/bệnh nền"*.
  - 2 lối thoát rõ ràng: Nút **`[Chuyển Dược sĩ trực ngay]`** và nút **`[Xác nhận nhanh: Người lớn, không mang thai, không dị ứng, không dùng thuốc/bệnh nền khác]`** (bấm vào là điền đủ S1–S4; nếu không đúng, khách chọn "Khác" để mở form chọn nhanh từng trường).
- **Bước tiếp theo & ai được báo, sau bao lâu:**
  - Nếu bấm **Xác nhận nhanh** → đủ S1–S4, không thuộc nhóm nhạy cảm → AI mở thẻ OTC `goi_y_san_pham` có nút **`[Đặt]`**; sau khi đặt có nút **`[Hoàn tác / Hủy đơn]`** đến lúc đơn được đóng gói. Không ai cần được báo.
  - Nếu bấm **Chuyển dược sĩ** → `chuyen_duoc_si` mức `Normal`, kèm ghi chú *"Khách đang vội, đã từ chối sàng lọc"* để dược sĩ không hỏi lại từ đầu. Khách có nút **`[Hủy chuyển, quay lại chat]`**.
  - Nếu khách **im lặng**: hệ thống nhắn nhắc nhẹ theo mốc **2 phút → 4 phút → 8 phút**; sau mốc 8 phút không phản hồi → đóng phiên chờ, **ghi log và báo dược sĩ trực** *"Khách đã rời hội thoại khi chưa hoàn tất sàng lọc"* (để dược sĩ quyết định có nhắn lại không).
- **Loại lỗi:** **Lỗi người dùng + Lỗi ngữ cảnh**. Khách cố tình bỏ qua bước sàng lọc trong trạng thái vội/cáu gắt. Chỉ xin lỗi hay đổi giọng điệu không giải quyết được mà cần **rào cản cứng ở UI**.

### Tổng kết nguyên tắc & lỗi thiết kế (§3)
- **Loại lỗi quyết định cách nói & cách sửa:**

| Loại lỗi | Ví dụ trong bài | Cách AI nói | Cách sửa |
|---|---|---|---|
| **Hệ thống** (AI/dữ liệu sai) | #1 AI bịa "chi nhánh còn hàng" | Nhận lỗi, xin lỗi rõ, nói giới hạn: *"Mình không tra được tồn kho chi nhánh"* | Sửa hệ thống: cấm AI trả lời tồn kho khi không có tool; đưa link tra cứu |
| **Người dùng** (khách bỏ qua/nhập thiếu) | #7 khách từ chối sàng lọc | Không xin lỗi, không trách khách; giải thích *vì sao* cần thông tin + đưa lối đi 1 chạm | Rào cứng ở UI (khóa nút đặt) + giảm ma sát (xác nhận nhanh 1 chạm) |
| **Ngữ cảnh** (hệ thống chạy đúng thiết kế nhưng sai tình huống) | #5 hỏi đủ S1–S4 với ca trẻ mất nước lúc nửa đêm | Ngắn, nói thẳng việc đã làm: *"đã báo Dược sĩ ưu tiên"* | Sửa luồng: đường tắt red-flag + URGENT từ giây 0 |

- **Lỗi sửa câu chữ không cứu được & cách thiết kế cứu:** Dù câu chữ AI khéo đến đâu, nếu giao diện vẫn hiển thị nút *"Đặt mua ngay"* hoặc đẩy ca trẻ nhỏ vào hàng chờ FIFO thông thường thì khách vẫn bấm mua nhầm / ca nặng vẫn bị kẹt sau hàng trăm ca hỏi giá. **Thiết kế cứu bằng 2 cơ chế cứng:** (1) **UI Lock** — nút đặt hàng chỉ mở khóa khi đủ S1–S4 và không thuộc nhóm nhạy cảm, luật này chạy bằng code chứ không phụ thuộc câu AI nói; (2) **Priority Routing (`URGENT`)** — ca có trẻ nhỏ/bầu/red-flag luôn nhảy lên đầu hàng chờ dược sĩ.
- **3 nguyên tắc HAX/PAIR trỏ vào chỗ cụ thể:**
  1. **G1 (Làm rõ hệ thống làm được gì — khớp Điều 11 khoản 1 Luật AI 134/2025/QH15):** Thanh header + mở đầu câu thoại luôn gắn nhãn *"Trợ lý AI Pharmacity — Sàng lọc ban đầu, không kê đơn/chẩn đoán"* (câu mở đầu ở #5).
  2. **G10 (Thu hẹp phạm vi khi nghi ngờ):** Khi thiếu S1–S4 hoặc gặp nhóm nhạy cảm, AI khóa nút đặt của `goi_y_san_pham` và thu hẹp về đúng 1 câu red-flag (#5) hoặc 1 nút xác nhận nhanh / chuyển `chuyen_duoc_si` (#7).
  3. **G11 + PAIR Explainability (Giải thích vì sao & bằng chứng để tự kiểm):** Hộp **"AI đang căn cứ vào đâu"** hiển thị rõ dữ kiện *đã biết* (VD: nôn ói 2 ngày) vs *còn thiếu* (VD: tuổi bé, dị ứng) để khách và dược sĩ thấy ngay lý do AI dừng hoặc chuyển; dòng *"Đang khóa — thiếu: …"* trên thẻ gợi ý ở #7.

---

## §4. Mức tự chủ (chặng 4)

| Tool | Mức của nhóm | Ai duyệt · người duyệt thấy gì · quá hạn thì sao | Lý do theo Cost-of-Error (Sai ai chịu · Đảo được không · Chờ mất gì) | Phương án đã cân nhắc & vì sao loại |
|---|---|---|---|---|
| `ban_thuoc_ke_don` | **Cấm** | Không áp dụng — không có nút nào gọi tool này trên UI | Sai: khách uống sai kháng sinh/thuốc đặc trị gây sốc/kháng thuốc, vi phạm pháp luật; **không đảo được**; bước xác nhận của khách không đủ chặn. | Loại *Cần người duyệt* vì bán thuốc kê đơn cần đơn của bác sĩ và kiểm tra tại quầy, không phải việc của chat tự động. |
| `khuyen_di_kham` | **Tự làm** *(có đường lui: khách bấm `[Tôi đã hiểu]` để đóng)* | AI hiện ngay; khách tự chọn hành động tiếp | Hiện thừa: khách mất vài giây cân nhắc, bấm đóng được (track: "khách bỏ qua được"). Chờ dược sĩ duyệt mới hiện cảnh báo cấp cứu → mất thời gian vàng (đột quỵ #6, mất nước nặng #5). | Loại *Cần người duyệt* vì độ trễ người duyệt có thể gây tử vong. |
| `chuyen_duoc_si` | **Tự làm** *(kèm phân luồng `URGENT` / `Normal`)* | AI tự đẩy; dược sĩ trực nhận theo độ ưu tiên, thấy phiếu tóm tắt (đối tượng, triệu chứng, S1–S4 đã biết/còn thiếu). Ca `Normal` khách có nút `[Hủy chuyển]` | Chuyển nhầm ca nhẹ: đảo được, tốn vài phút của dược sĩ → giảm bằng nhãn `URGENT` vs `Normal`. Chờ duyệt mới chuyển làm chậm đúng những ca rủi ro nhất. | Loại *Cần khách/admin duyệt* vì tăng ma sát đúng lúc khách đang hoảng. |
| `luu_ghi_chu_suc_khoe` | **Cần người duyệt** *(khách bấm Đồng ý)* | **Khách** duyệt; thấy đúng nội dung sẽ lưu (VD: *"Dị ứng penicillin · đang dùng ARV"*) + 2 nút `[Đồng ý lưu]` / `[Không lưu]`. Quá hạn/không bấm → **không lưu**. Đã lưu vẫn có `[Hoàn tác (Xóa ghi chú)]` | Lưu ngầm thông tin nhạy cảm (mang thai, HIV, bệnh nền) gây rủi ro lộ dữ liệu y tế — xóa được nhưng đã lộ. Chờ khách bấm tốn 1 giây, không gây hại sức khỏe. | Loại *Tự làm* vì vi phạm quyền riêng tư và rule 7 của track T1. Loại *Cấm* vì ghi chú giúp dược sĩ lần sau không hỏi lại dị ứng. |
| `goi_y_san_pham` *(tách theo điều kiện)* | **3 nhánh theo S1–S4:**<br>• **Tự làm + Hoàn tác:** OTC phổ thông khi đủ S1–S4 và không thuộc nhóm nhạy cảm.<br>• **Khóa nút đặt, chờ bổ sung:** thiếu ≥1 trường S1–S4 → thẻ khóa, không tên sản phẩm.<br>• **AI không gợi ý — dược sĩ gợi ý:** nhóm nhạy cảm hoặc có red-flag. | Nhánh 1: không cần duyệt; khách có `[Hoàn tác / Hủy đơn]` đến lúc đơn được đóng gói.<br>Nhánh 2: **khách** tự mở khóa bằng cách bổ sung S1–S4; không bao giờ tự mở theo thời gian.<br>Nhánh 3: **dược sĩ** thấy phiếu tóm tắt và tự gợi ý; quá 5 phút chưa tiếp nhận → khách thấy vị trí trong hàng chờ + dấu hiệu nào cần đi khám ngay; nút đặt vẫn khóa. | Gợi ý sai cho trẻ nhỏ/bầu gây hại sức khỏe **không đảo được** khi đã uống; nhưng bắt dược sĩ duyệt cả hộp C sủi làm khách đêm chờ vô ích và bỏ chat, đồng thời làm nghẽn hàng chờ của ca nặng. | Loại *Tự làm toàn bộ* (gây hại Ca #5, #7) và loại *Cần dược sĩ duyệt toàn bộ* (gây tắc Ca #9 C sủi). |

- **Tool khó xếp nhất:** `goi_y_san_pham`.
  - *Mỗi người từng chọn gì:* **Lĩnh** chọn **Tự làm + Hoàn tác** (để khách mua nhanh, giống trải nghiệm app hiện nay) · **Trung** chọn **Cần dược sĩ duyệt mọi ca** (từ góc nhìn lâm sàng: thuốc đã uống là không đảo được) · **Đức** đề xuất **tách theo điều kiện bằng luật ngưỡng S1–S4** — nhóm chốt theo phương án này.
  - *Cái giá phía Tự làm:* Dễ gợi ý nhầm trong Kịch bản #5 (trẻ nôn ói 2 ngày) và Kịch bản #7 (khách gắt ép ship không cho biết người dùng/bệnh nền). Nút "Hoàn tác" vô nghĩa khi thuốc đã uống.
  - *Cái giá phía Cần dược sĩ duyệt mọi ca:* Kịch bản #9 (khách chỉ mua C sủi) cũng phải xếp hàng chờ dược sĩ lúc nửa đêm → nghẽn hàng chờ làm trễ ca nặng thật.
  - *Cách chốt:* **luật ngưỡng S1–S4 + tách bước tool** (xem dưới).
- **Luật thêm & tool tách bước:**
  - Tách `goi_y_san_pham` thành 2 bước: **(a) hiện thẻ gợi ý** (chỉ thông tin) và **(b) bật nút đặt**. Bước (b) chỉ chạy khi đủ S1–S4 và không thuộc nhóm nhạy cảm. Lý do: tách để khóa đúng phần không đảo được (đặt → giao → uống) mà vẫn cho khách thấy AI đang làm gì.
  - Luật mở khóa chạy bằng code (đếm trường S1–S4 và cờ nhạy cảm), **không phụ thuộc AI phân loại hay câu chữ** — khách gắt đến đâu cũng không mở được.
- **Test mức tự chủ trên 3 kịch bản:**
  1. **Ca đắt #1 (#5 Con nôn ói 2 ngày):** AI không hiện thẻ sản phẩm (nhóm nhạy cảm → nhánh 3), hỏi 1 câu red-flag, phiếu `URGENT` được tạo từ giây 0, có dấu hiệu thì `khuyen_di_kham` ngay. *Cái sai bị chặn? Có. Còn tự làm khi lẽ ra phải hỏi? Không. Chờ vô ích? Không.*
  2. **Ca đắt #2 (#7 Khách gắt ép ship):** AI giữ thẻ khóa vì thiếu S1–S4 (nhánh 2), không nhượng bộ dù khách ép, mở nút `chuyen_duoc_si` + xác nhận nhanh 1 chạm, chạy nhắc 2-4-8 phút. *Cái sai bị chặn? Có. AI tự làm khi lẽ ra phải hỏi? Không. Chờ vô ích? Không — có lối 1 chạm.*
  3. **Ca bình thường (#9 Mua C sủi):** Khách xác nhận nhanh S1–S4 trong 1 lượt → AI mở thẻ `goi_y_san_pham` OTC kèm nút Đặt và Hoàn tác (nhánh 1), cuối phiên hỏi xin phép `luu_ghi_chu_suc_khoe`. *Khách thường phải chờ dược sĩ vô ích? Không.*
- **Lời hứa mới với người dùng:** Thay vì hứa *"Chat với AI để được tư vấn & mua thuốc tức thì"*, kênh Hỏi dược sĩ AI hứa: **"AI chỉ bật nút mua OTC khi đã biết rõ ai sẽ dùng thuốc; ca trẻ nhỏ, mang thai hay có dấu hiệu nguy hiểm được đưa thẳng tới đầu hàng chờ Dược sĩ."** Mức *Cần dược sĩ* cho nhóm nhạy cảm đổi lời hứa "nhanh" thành lời hứa "đúng người".

---

## §5. Prototype (chặng 5)

- **File / Link:**
  - Repo: <https://github.com/hxlinh/K5-L3B-Day05-T1-BanCuoi>
  - File: [`prototype/index.html`](prototype/index.html)
  - GitHub Pages: <https://hxlinh.github.io/K5-L3B-Day05-T1-BanCuoi/prototype/>
- **Công cụ & phần mock:** 1 file HTML/CSS/JS thuần (mở trực tiếp, không cần server), dựng với hỗ trợ của coding agent (Claude Code); mock danh mục OTC mẫu, 4 kịch bản test, ô nhập tự do cho ca lạ (phân loại bằng luật từ khóa), nút mô phỏng khách im lặng (60 giây ở #5; 2 → 4 → 8 phút ở #7), và hàng chờ Dược sĩ trực.
- **Có lời gọi AI thật:** **Có** — Google Gemini (`gemini-3.5-flash-lite`) ở ô nhập tự do. AI **chỉ phân loại rủi ro** (đối tượng, nhóm nhạy cảm, red-flag, thuốc kê đơn, S1–S4 còn thiếu) và trả về JSON; kết quả được gộp với luật từ khóa theo kiểu *bên nào cảnh báo thì nghe bên đó*. AI không bao giờ tự mở nút Đặt — việc khóa/mở do code quyết định. API key chỉ nằm trong `sessionStorage`, không ghi vào file; lỗi/quá 15 giây thì tự quay về luật từ khóa.
- **Chú thích 3 ảnh chụp màn hình (khớp số ①–⑥ trên prototype):**

  | Ca #5 — bấm "Có dấu hiệu" (① ② ④ ⑥) | Ca #7 — khách gắt, thẻ khóa (① ② ③ ⑥) | Ca #7 — xác nhận nhanh → Đặt (② ⑤) |
  |---|---|---|
  | ![Ca 5](prototype/screenshot-1-ca5-canh-bao.png) | ![Ca 7 khóa](prototype/screenshot-2-ca7-khoa-nut.png) | ![Ca 7 đặt](prototype/screenshot-3-ca7-dat-hoan-tac.png) |

- **Ca ngoài kịch bản + bằng chứng lời gọi AI thật** (khách gõ *"Tôi bị ho bán cho tôi siro ho đi"*):

  | Thiếu S1–S4 → hỏi form 1 chạm (① ②) | Xác nhận nhanh → Đặt + Hoàn tác + xin phép lưu (② ⑤) | Log Gemini: input & phân loại | Log Gemini: S1–S4 còn thiếu & lý do |
  |---|---|---|---|
  | ![Ca lạ ho 1](prototype/screenshot-4-ca-la-ho-sang-loc.png) | ![Ca lạ ho 2](prototype/screenshot-5-ca-la-ho-dat-hoan-tac.png) | ![Log AI 1](prototype/ai-call-log-1-input.png) | ![Log AI 2](prototype/ai-call-log-2-output.png) |

  - **Số ① (`G1` — Minh bạch AI):** Banner đầu khung chat ghi rõ *"Trợ lý AI Pharmacity · Hỗ trợ sàng lọc, không thay Dược sĩ/Bác sĩ"*.
  - **Số ② (`G11` + `PAIR Explainability` — Bằng chứng):** Hộp *"AI đang căn cứ vào đâu"* (Đã biết vs. Còn thiếu S1–S4) + bảng trạng thái 5 tool, cập nhật trực tiếp theo hội thoại.
  - **Số ③ (`G10` — Thu hẹp phạm vi & Rào cản cứng):** Thẻ `goi_y_san_pham` bị **khóa nút Đặt hàng** khi thiếu S1–S4; với `ban_thuoc_ke_don` (*Cấm*) **không có nút mua nào**.
  - **Số ④ (`Mức Tự làm` — Cấp cứu không chờ):** Thẻ đỏ `khuyen_di_kham` + nút `[Gọi 115]` / `[Tôi đã hiểu]` + nút chọn nhanh 1 câu red-flag.
  - **Số ⑤ (`Mức Cần duyệt` + `Hoàn tác`):** Thẻ xin phép `luu_ghi_chu_suc_khoe` (khách bấm Đồng ý/Không lưu) và nút **Hoàn tác** trên thẻ sản phẩm OTC / xóa ghi chú.
  - **Số ⑥ (`Bonus T1` — Góc nhìn Dược sĩ):** Bảng hàng chờ bên phải hiển thị ca chuyển sang kèm mức `URGENT` / `Normal` và phiếu tóm tắt triệu chứng, dữ kiện còn thiếu để khách không phải kể lại.
- **Quyết định thiết kế hiện độ chắc & mức tự chủ:** Chọn hiển thị **hộp checklist dữ kiện (Đã biết / Còn thiếu) + trạng thái khóa nút** thay vì hiện con số `% độ tin cậy` (ví dụ "AI chắc 65%") — vì khách đang lo sức khỏe không hiểu ý nghĩa con số %, và con số không cho họ biết phải làm gì tiếp; trong khi nhìn *"Thiếu: tuổi bé, dị ứng"* giúp họ biết ngay phải cung cấp gì hoặc vì sao AI chưa dám gợi ý. Cũng đã cân nhắc *chỉ ẩn nút đặt* (không hiện thẻ khóa) → loại vì khách không hiểu vì sao không mua được và sẽ hỏi lại/bỏ đi; thẻ khóa kèm lý do vừa là rào cứng vừa là lời giải thích.

---

## §6. Tốt là gì

1. **Tiêu chí 1 (Chặn ca nguy hiểm / nhạy cảm):** 100% tin nhắn có red-flag hoặc thuộc nhóm nhạy cảm (§1) **không hiện nút đặt** và được gọi `khuyen_di_kham` hoặc `chuyen_duoc_si` mức `URGENT` trong lượt trả lời đầu tiên (0 ca vào hàng chờ `Normal`).
   - *Chấm thử Ca #5:* **Đạt** (không hiện thẻ sản phẩm, phiếu URGENT tạo ở lượt đầu, có dấu hiệu thì cảnh báo đi khám). *Ca #7:* **Đạt** (không có red-flag/nhóm nhạy cảm đã biết; nút đặt vẫn khóa).
2. **Tiêu chí 2 (Không làm liều khi thiếu dữ kiện):** 100% ca thiếu ≥1 trường S1–S4 không được bật nút đặt của `goi_y_san_pham`, bất kể giọng điệu khách gắt.
   - *Chấm thử Ca #5 & #7:* **Đạt**.
3. **Tiêu chí 3 (Quyền riêng tư):** 0 lần gọi `luu_ghi_chu_suc_khoe` khi chưa có thao tác bấm "Đồng ý lưu" của khách.
   - *Chấm thử Ca #5 & #7:* **Đạt**.
- **Lỗi còn lại chưa giải quyết hết bằng thiết kế này:** Nếu khách **nói dối** khi bấm xác nhận nhanh (ví dụ chọn *"người lớn khỏe mạnh"* nhưng thực tế mua cho trẻ nhỏ), AI không có cách xác minh và vẫn mở gợi ý OTC — cần dược sĩ/nhãn thuốc cảnh báo lại khi giao nhận. Ngoài ra luật từ khóa có thể bỏ sót cách nói lạ của red-flag (VD: "bé cứ nằm im không chơi") → cần bổ sung từ điển từ ca thật do dược sĩ gắn nhãn.

---

## §7. Demo
- **Đã trình bày:** 4 ca có sẵn trên prototype (#5 trẻ nôn ói, #7 khách gắt, #4 kháng sinh + hoàn tiền, #9 C sủi) và **1 ca ngoài kịch bản** gõ vào ô nhập tự do: *"Tôi bị ho bán cho tôi siro ho đi"*.
- **Prototype đã làm gì với ca ngoài kịch bản:** gửi tin cho Gemini phân loại (kết quả: `doi_tuong: chua_ro`, không red-flag, không kê đơn, thiếu S1–S4) → AI không gợi ý ngay mà hiện form S1–S4 + thẻ gợi ý bị khóa → khách bấm *Xác nhận nhanh* (người lớn, không mang thai, không dị ứng, không dùng thuốc khác) → mở thẻ OTC kèm **Hoàn tác / Hủy đơn** và thẻ xin phép lưu ghi chú. Đúng mức tự chủ ở §4 (nhánh 2 → nhánh 1 của `goi_y_san_pham`). *(Ảnh 4–5 và log ở §5.)*
- **Câu hỏi nhận được & cách nhóm trả lời:**
  1. *"Nếu bệnh/triệu chứng nằm ngoài bộ dữ liệu (VD: ho) thì sao?"* → Dùng AI xử lý: AI thật phân loại rủi ro cho mọi tin nhắn, kể cả triệu chứng không có trong từ điển. Làm rõ thêm: AI chỉ **phân loại**, không tự quyết; nếu không nhận ra được gì thì mặc định an toàn là hỏi S1–S4 và giữ khóa nút Đặt.
  2. *"Nếu người dùng nghi ngờ câu trả lời của AI và tranh luận thì sao?"* → Hướng phát triển: áp dụng **Human-in-the-loop** ngay cả khi khách không bấm *"Tôi muốn hỏi Dược sĩ"* → `chuyen_duoc_si` kèm tóm tắt hội thoại, dược sĩ vào thẳng cuộc trò chuyện.
- **Nhóm có đổi ý gì không:** Không đổi mức tự chủ của tool nào. Bổ sung một hướng cho bản sau từ câu hỏi 2: hiện tại khách chỉ chuyển được sang dược sĩ ở những bước AI chủ động đưa nút; bản sau cần AI nhận ra khách hàng đang không tin tưởng mình để kích hoạt HITL ngay lập tức.
