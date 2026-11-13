# Dàn ý bài nộp *(spec.md — nộp trước 23:59 ngày lab của lớp)*

> Copy dàn ý này vào `spec.md` của nhóm. Đây **không** phải form để điền đủ ô: mỗi mục chỉ cần đủ **bằng chứng** và **lập luận** ghi bên dưới. Viết ngắn, gạch đầu dòng là được. Hướng dẫn từng chặng: `02-guide.md`. Cách chấm: `04-rubric.md`.
>
> **Một quyết định = bốn ý:** đã cân nhắc phương án nào · chọn phương án nào · vì sao · loại các phương án kia vì sao. Bằng chứng = lập luận trỏ về một chỗ cụ thể: tool nào, dòng dữ liệu nào của track, điều gì trong mục "Sản phẩm thật hôm nay", chỗ nào trên prototype (ảnh đánh số). Không chạy agent.

```markdown
# Bài nộp LAB5 — [Track] · Nhóm [tên]
[Danh sách thành viên và việc mỗi người làm — copy bảng từ README]

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

## §2. Kịch bản rủi ro

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

## §3. Khi AI sai (cho 2 kịch bản đắt nhất)

### Kịch bản #5: *"Con tôi nôn ói, đi ngoài 2 ngày nay, cho tôi thuốc"*
- **Quyết định hướng xử lý:** **Dừng gợi ý thuốc + Hỏi 1 câu red-flag + Đẩy ưu tiên `chuyen_duoc_si` / `khuyen_di_kham` ngay.**
  - *Đã cân nhắc & vì sao loại:* (1) *Tự trả lời kèm giới hạn (gợi ý tạm Oresol/men vi sinh)* → Loại vì trẻ nhỏ là đối tượng nhạy cảm, chưa biết tuổi và mức độ mất nước nặng hay nhẹ, giao nút mua tạo cảm giác "đã có thuốc xử lý" làm phụ huynh trì hoãn đi viện. (2) *Hỏi đầy đủ 5–6 câu sàng lọc rồi mới chuyển* → Loại vì trẻ nôn ói 2 ngày lúc đêm cần xử lý khẩn, hỏi nhiều làm mất thời gian vàng.
- **Câu AI nói nguyên văn:**
  > *"Mình là Trợ lý AI Pharmacity (không thay thế bác sĩ/dược sĩ). Vì bé đã nôn ói, tiêu chảy 2 ngày có nguy cơ mất nước nên mình **không tự gợi ý thuốc ngay**. Bạn xem nhanh giúp mình: bé có dấu hiệu nào sau đây không — **lừ đừ khó đánh thức, môi khô/khóc không ra nước mắt, không uống được nước, sốt cao, hoặc phân có máu**?"*
- **Người dùng thấy gì trên màn hình:**
  - Nhãn `G1`: *"Trợ lý AI — Hỗ trợ sàng lọc, không kê đơn/chẩn đoán"*.
  - 2 nút bấm nhanh: **`[Có các dấu hiệu trên — Xem hướng dẫn đi khám ngay]`** (đỏ) và **`[Chưa thấy dấu hiệu trên — Kết nối Dược sĩ ƯU TIÊN ngay]`**.
  - Hộp bằng chứng/trạng thái: *"Chưa đủ dữ liệu an toàn (thiếu: tuổi bé, dấu hiệu mất nước) → Đã khóa nút đặt thuốc"*.
- **Bước tiếp theo & ai được báo:**
  - Nếu bấm **Có dấu hiệu** → gọi ngay `khuyen_di_kham` (khuyên đưa đi viện/gọi 115 ngay, không chờ) **đồng thời** đẩy phiên vào đầu hàng chờ `chuyen_duoc_si` gắn cờ `URGENT`.
  - Nếu bấm **Chưa thấy** hoặc **khách không phản hồi (ca cấp cứu không chờ lâu)** → **tự động chuyển ngay** sang dược sĩ trực ở vị trí `URGENT` (cảnh báo đỏ trên màn hình dược sĩ kèm tóm tắt *"Trẻ nhỏ nôn ói/tiêu chảy 2 ngày — ưu tiên xử lý ngay"*).
- **Loại lỗi:** **Lỗi ngữ cảnh (*Context Error*)**. AI làm đúng quy trình hỏi thông tin thông thường nhưng đặt vào tình huống trẻ nhỏ có nguy cơ mất nước lúc nửa đêm thì hỏi tuần tự nhiều bước là thất bại. Phải sửa bằng **luồng rút gọn 1 câu red-flag + tự động ưu tiên hàng chờ**.

### Kịch bản #7: *"Ai cũng được, cứ ship đi, đừng hỏi nữa!"*
- **Quyết định hướng xử lý:** **Dừng gợi ý + Xoa dịu nêu lý do + Hiện nút chuyển dược sĩ (`chuyen_duoc_si`).**
  - *Đã cân nhắc & vì sao loại:* (1) *Tiếp tục gặng hỏi lại danh sách câu hỏi* → Loại vì làm khách bực hơn và bỏ đi mua liều bên ngoài. (2) *Chiều khách gợi ý đại thuốc cảm phổ thông* → Loại vì vi phạm an toàn tối đa (có thể khách mua cho trẻ em, bà bầu hoặc người cao huyết áp).
- **Câu AI nói nguyên văn:**
  > *"Mình hiểu bạn đang cần thuốc gấp, nhưng vì thuốc cảm có hoạt chất chống chỉ định với trẻ nhỏ, phụ nữ mang thai hoặc người huyết áp cao nên **mình bắt buộc phải khóa tính năng gợi ý tự động khi chưa rõ người dùng**. Bạn bấm nút bên dưới để **gặp Dược sĩ trực ngay** (hoặc nhắn nhanh 'người lớn khỏe mạnh' nếu đúng vậy để mình hỗ trợ tiếp nhé)."*
- **Người dùng thấy gì trên màn hình:**
  - Thẻ sản phẩm/nút đặt bị **khóa cứng (disabled)** kèm dòng giải thích *"Thiếu dữ kiện bắt buộc: Đối tượng dùng thuốc, Dị ứng, Thuốc đang dùng"*.
  - 2 lối thoát rõ ràng: Nút **`[Chuyển Dược sĩ trực ngay]`** và 1 nút sửa nhanh **`[Bổ sung nhanh người dùng thuốc]`**.
- **Bước tiếp theo & ai được báo:**
  - Nếu khách bấm chuyển → `chuyen_duoc_si` (mức `Normal`, kèm log khách đang vội/gắt).
  - Nếu khách **im lặng/treo máy**: hệ thống nhắn nhắc nhẹ theo mốc **2 phút → 4 phút → 8 phút**; sau mốc 8 phút không phản hồi → đóng trạng thái chờ, **ghi log và báo admin/dược sĩ** là khách đã rời hội thoại khi chưa hoàn tất sàng lọc.
- **Loại lỗi:** **Lỗi người dùng + Lỗi ngữ cảnh**. Khách cố tình bỏ qua bước sàng lọc trong trạng thái vội/cáu gắt. Chỉ xin lỗi hay đổi giọng điệu không giải quyết được mà cần **rào cản cứng ở UI**.

### Tổng kết nguyên tắc & lỗi thiết kế (§3)
- **Lỗi sửa câu chữ không cứu được & cách thiết kế cứu:** Dù câu chữ AI khéo đến đâu, nếu giao diện vẫn hiển thị nút *"Đặt mua ngay"* hoặc đẩy ca trẻ nhỏ vào hàng chờ FIFO thông thường thì khách vẫn bấm mua nhầm / ca nặng vẫn bị kẹt sau hàng trăm ca hỏi giá. **Thiết kế cứu bằng 2 cơ chế cứng:** (1) **UI Lock** — nút đặt hàng chỉ mở khóa khi đủ các trường sàng lọc an toàn; (2) **Priority Routing (`URGENT`)** — ca có trẻ nhỏ/bầu/red-flag luôn nhảy lên đầu hàng chờ dược sĩ.
- **3 nguyên tắc HAX/PAIR trỏ vào chỗ cụ thể:**
  1. **G1 (Làm rõ hệ thống làm được gì — khớp Điều 11 Luật AI 134/2025/QH15):** Thanh header + mở đầu câu thoại luôn gắn nhãn *"Trợ lý AI Pharmacity — Sàng lọc ban đầu, không kê đơn/chẩn đoán"*.
  2. **G10 (Thu hẹp phạm vi khi nghi ngờ):** Khi thiếu thông tin sàng lọc hoặc gặp đối tượng nhạy cảm, AI khóa nút `goi_y_san_pham` và thu hẹp về đúng 1 câu red-flag hoặc chuyển `chuyen_duoc_si`.
  3. **G11 + PAIR Explainability (Giải thích vì sao & bằng chứng để tự kiểm):** Hộp **"AI đang căn cứ vào đâu"** hiển thị rõ dữ kiện *đã biết* (VD: nôn ói 2 ngày) vs *còn thiếu* (VD: tuổi bé, dị ứng) để khách và dược sĩ thấy ngay lý do AI dừng hoặc chuyển.

---

## §4. Mức tự chủ (theo Cost-of-Error)

| Tool | Mức của nhóm | Ai duyệt & quá hạn thì sao | Lý do theo Cost-of-Error (Sai ai chịu · Đảo được không · Chờ mất gì) | Phương án đã cân nhắc & vì sao loại |
|---|---|---|---|---|
| `ban_thuoc_ke_don` | **Cấm** | Không áp dụng (khóa vĩnh viễn trên UI) | Sai: khách uống sai kháng sinh/thuốc đặc trị gây sốc/kháng thuốc, vi phạm pháp luật; **không đảo được**; bước xác nhận của khách không đủ chặn. | Loại *Cần người duyệt* vì AI không có quyền bán thuốc kê đơn qua chat tự động. |
| `khuyen_di_kham` | **Tự làm** | AI hiện ngay; khách tự chọn hành động tiếp | Nếu chờ dược sĩ duyệt mới hiện cảnh báo cấp cứu → mất thời gian vàng tính mạng (đột quỵ, mất nước nặng). Hiện thừa chỉ khiến khách cân nhắc thêm vài giây; khách có quyền bấm chuyển dược sĩ nếu chưa đi ngay. | Loại *Cần người duyệt* vì độ trễ người duyệt có thể gây tử vong. |
| `chuyen_duoc_si` | **Tự làm** *(kèm phân luồng `URGENT` / `Normal`)* | AI tự đẩy; dược sĩ trực nhận theo độ ưu tiên | Chờ duyệt mới chuyển làm chậm ca rủi ro. Sai (chuyển nhầm ca nhẹ) đảo được nhưng gây nghẽn hàng chờ → xử lý bằng gắn nhãn `URGENT` vs `Normal`. | Loại *Cần khách/admin duyệt* vì tăng ma sát đúng lúc khách đang hoảng. |
| `luu_ghi_chu_suc_khoe` | **Cần người duyệt** *(Khách bấm Đồng ý)* | Khách bấm *"Cho phép lưu"* cuối phiên; quá hạn/không bấm → **không lưu** | Lưu ngầm thông tin nhạy cảm (mang thai, bệnh nền) gây rủi ro lộ dữ liệu y tế; chờ khách bấm tốn 1 giây, không gây hại sức khỏe. Có nút Hoàn tác/Xoá ngay. | Loại *Tự làm* vì vi phạm quyền riêng tư và quy tắc track T1. |
| `goi_y_san_pham` *(Tách điều kiện)* | **Tách 2 mức:** <br>• **Cần dược sĩ duyệt (Khóa nút đặt)** nếu thiếu sàng lọc hoặc trẻ nhỏ/bầu/bệnh nền.<br>• **Tự làm + Hoàn tác** chỉ với OTC phổ thông khi đủ checklist an toàn. | Dược sĩ duyệt ca nhạy cảm; nếu không kịp sau 2 phút báo hàng chờ ưu tiên. Với ca OTC đủ điều kiện: khách có nút Gỡ/Hoàn tác trên thẻ sản phẩm. | Gợi ý sai cho trẻ nhỏ/bầu gây hại sức khỏe **không đảo được** khi đã uống; nhưng bắt dược sĩ duyệt cả hộp C sủi làm khách đêm chờ vô ích và bỏ chat. | Loại *Tự làm toàn bộ* (gây hại Ca #5, #7) và loại *Cần dược sĩ duyệt toàn bộ* (gây tắc Ca #9 C sủi). |

- **Tool khó xếp nhất:** `goi_y_san_pham`.
  - *Cái giá phía Tự làm:* Dễ gợi ý nhầm trong Kịch bản #5 (trẻ nôn ói 2 ngày) và Kịch bản #7 (khách gắt ép ship không cho biết tuổi/bệnh nền).
  - *Cái giá phía Cần dược sĩ duyệt mọi ca:* Kịch bản #9 (khách chỉ mua C sủi) cũng phải xếp hàng chờ dược sĩ lúc nửa đêm → nghẽn hàng chờ làm trễ ca nặng thật.
  - *Cách chốt:* Áp dụng **luật ngưỡng + tách bước UI** — AI chỉ được tự hiện gợi ý OTC + nút đặt khi đủ 4 trường sàng lọc âm tính rủi ro; hễ thiếu thông tin hoặc rơi vào nhóm nhạy cảm thì tự động chuyển sang trạng thái **chỉ hiển thị thông tin + khóa nút đặt + chờ dược sĩ**.
- **Test mức tự chủ trên 3 kịch bản:**
  1. **Ca đắt #1 (#5 Con nôn ói 2 ngày):** AI không gợi ý sản phẩm (bị khóa bởi luật trẻ nhỏ), hỏi 1 câu red-flag → đẩy `URGENT` `chuyen_duoc_si` / `khuyen_di_kham`. *Cái sai bị chặn? Có. Chờ vô ích? Không.*
  2. **Ca đắt #2 (#7 Khách gắt ép ship):** AI khóa nút đặt vì thiếu dữ kiện người uống, không nhượng bộ dù khách ép, mở nút `chuyen_duoc_si` và chạy timer nhắc 2-4-8 phút. *Cái sai bị chặn? Có. AI tự làm khi lẽ ra phải hỏi? Không.*
  3. **Ca bình thường (#9 Mua C sủi):** Khách trả lời nhanh 1 bước cho người lớn khỏe mạnh, không dị ứng → AI mở thẻ `goi_y_san_pham` OTC kèm nút Hoàn tác/Gỡ và hỏi xin phép `luu_ghi_chu_suc_khoe`. *Khách thường phải chờ dược sĩ vô ích? Không.*
- **Lời hứa mới với người dùng:** Thay vì hứa *"Chat với AI để được kê đơn & mua thuốc tức thì"*, kênh Hỏi dược sĩ AI hứa: **"Sàng lọc rủi ro trong 30 giây — chỉ gợi ý sản phẩm OTC khi đủ dữ kiện an toàn, và đưa ca nguy hiểm/nhạy cảm tới thẳng đầu hàng chờ Dược sĩ."**

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
```
