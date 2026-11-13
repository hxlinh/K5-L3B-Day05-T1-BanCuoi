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

## §3. Khi AI sai  (chặng 3)
Với mỗi kịch bản đắt nhất:
- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

## §4. Mức tự chủ  (chặng 4)
Với mỗi tool:
- Mức của nhóm (tự làm / cần người duyệt / cấm). Nếu cần người duyệt: ai duyệt · người duyệt thấy gì · quá hạn thì sao.
- Lý do theo cost-of-error: sai thì ai chịu gì · đảo lại được không · chờ người thì mất gì.
- Phương án đã cân nhắc và vì sao loại.
Thêm:
- Tool khó xếp nhất: mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản ở §2), nhóm chốt thế nào.
- Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường: cái sai có bị chặn không? còn việc nào làm khi lẽ ra phải hỏi? người dùng bình thường có phải chờ vô ích không?
- Luật thêm hoặc tool tách bước (nếu có), và vì sao.
- Một mức tự chủ đã đổi lời hứa nào với người dùng.

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
```
