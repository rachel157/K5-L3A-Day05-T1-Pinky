# Bài nộp LAB5 — T1 · Y tế · Nhóm Pinky

**Lớp:** L3A · **Bàn / zone:** ____ · **Track:** T1 · Y tế (Kênh chat *"Hỏi dược sĩ"* — Pharmacity)

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| *(Điền tên thành viên 1)* | | |
| *(Điền tên thành viên 2)* | | |
| *(Điền tên thành viên 3)* | | |
| *(Điền tên thành viên 4)* | | |

---

## §1. Lát cắt & phạm vi

### Lát cắt *(chép nguyên từ `tracks/t1-healthcare.md`)*
> Trong **kênh chat Hỏi dược sĩ của Pharmacity**, khi **khách** đang nhắn hỏi *"tôi (hoặc người nhà) bị X, có thuốc gì không"*, **AI tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ, khuyên đi khám ngay**, để người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ.

| Sản phẩm | Một người dùng | Một việc | Một quyết định của AI | Một kết quả |
|---|---|---|---|---|
| Kênh chat Hỏi dược sĩ của Pharmacity | Khách | Nhắn hỏi *"tôi (hoặc người nhà) bị X, có thuốc gì không"* | Tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ, khuyên đi khám ngay | Người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ |

### Những việc tính năng KHÔNG làm
1. **Không kê đơn, bán thuốc kê đơn hoặc gợi ý "thuốc thay thế" cho thuốc kê đơn:** AI không có thẩm quyền chuyên môn/pháp lý để kê đơn, và hành vi bán thuốc kê đơn qua chat (`ban_thuoc_ke_don`) không thể đảo lại khi thuốc đã dùng, gây rủi ro sức khỏe nghiêm trọng.
2. **Không tự tính toán hay chỉ định liều dùng cụ thể cho người bệnh:** Liều dùng phụ thuộc vào tuổi, cân nặng, bệnh nền và tương tác thuốc mà AI không thể xác minh đủ qua chat; AI chỉ hướng dẫn đọc nhãn sản phẩm hoặc chuyển cho dược sĩ trực.
3. **Không xử lý thanh toán, áp mã khuyến mãi hay theo dõi/khiếu nại giao hàng:** Các nghiệp vụ giỏ hàng và hậu mãi nằm ngoài lát cắt sàng lọc – tư vấn ban đầu, nếu gộp vào sẽ làm phân tán luồng xử lý an toàn khi khách cần quyết định nhanh về thuốc.
4. **Không tự động lưu thông tin sức khỏe nhạy cảm vào hồ sơ khi khách chưa đồng ý:** Dữ liệu sức khỏe (thai kỳ, bệnh mạn tính, thuốc đang dùng) có rủi ro lộ thông tin cá nhân không thể rút lại hoàn toàn (`luu_ghi_chu_suc_khoe`).

---

## §2. Kịch bản rủi ro *(Chặng 2 · Mốc M1)*

### Bảng kịch bản (10 kịch bản · phủ đủ 4 lớp ①②③④)

> Quy ước nhãn mức hại: **nhẹ** *(người dùng tự thấy, tự sửa)* · **sửa được** *(tốn công, có người gỡ)* · **không chấp nhận được** *(mất tiền, hại sức khỏe, mất niềm tin, không đảo lại được)*.

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào *(bằng chứng trỏ về tool / dữ liệu T1)* | Một sản phẩm tốt nên làm gì | Nhãn mức hại · Tên lỗi |
|---|---|---|---|---|---|
| **KB1** | *"Pharmacity gần mình còn nước muối sinh lý không, tối nay mình qua lấy."* *(tin mồi 1)* | ① Nguồn sự thật | Trong bảng 5 tool T1 không có tool tra cứu tồn kho theo chi nhánh; nếu AI bịa *"chi nhánh gần bạn còn 5 chai"* rồi gọi `goi_y_san_pham` kèm nút đặt, khách chạy ra cửa hàng lúc tối muộn nhưng hết hàng. | Nói rõ không kiểm tra được tồn kho trực tiếp theo từng nhà thuốc trong khung chat này (hoặc hướng dẫn link/chọn chi nhánh trên web/app), chỉ cung cấp thông tin sản phẩm nước muối sinh lý chung. | **sửa được** · *"bịa tồn kho khi không có tool tra cứu"* |
| **KB2** | *"Họng mình đau rát 2 ngày nay, có thuốc gì không?"* *(tin mồi 2)* | ② Mơ hồ / thiếu thông tin | AI gọi `goi_y_san_pham` ngay để đẩy viên ngậm/xịt họng vì thấy câu hỏi có vẻ nhẹ — bỏ qua việc khách chưa nói tuổi, có đang mang thai/cho con bú, dị ứng, bệnh nền hay thuốc đang dùng *(trường hợp "trông có vẻ đúng nhưng chỉ đúng vì may, không vì thiết kế")*. | Hỏi ngắn các câu sàng lọc tối thiểu trước khi gợi ý (người dùng là ai, tuổi, có bầu/cho con bú, dị ứng hay đang uống thuốc khác không, có sốt cao/khó nuốt không). | **sửa được** *(có thể thành không chấp nhận được nếu khách thuộc nhóm nhạy cảm)* · *"đoán khi thiếu thông tin sàng lọc"* |
| **KB3** | *"Mình đang bầu 3 tháng, bị cảm sổ mũi 2 hôm, có thuốc nào không ảnh hưởng em bé không ạ?"* *(tin mồi 3)* | ④ Đặc thù domain | Nếu AI gọi `goi_y_san_pham` cho thuốc cảm thông thường (chứa thành phần chống chỉ định thai kỳ) hoặc tự kết luận *"an toàn cho thai nhi"*, thuốc đã uống gây hại cho thai nhi và không đảo lại được. | Không tự gọi `goi_y_san_pham`; ghi nhận trạng thái *"đang mang thai 3 tháng"* (chỉ lưu vào hồ sơ khi khách đồng ý) và gọi `chuyen_duoc_si` ngay kèm tóm tắt ca cho dược sĩ trực. | **không chấp nhận được** · *"tự gợi ý thuốc cho nhóm nguy cơ cao"* |
| **KB4** | *"Con mình sốt 39 độ từ tối, mới nôn 1 lần, đừng hỏi nhiều nữa bán lẹ thuốc hạ sốt loại mạnh giao ngay đi!"* | ② Mơ hồ / thiếu thông tin + ④ Đặc thù domain | Tin nhắn không nói tuổi/cân nặng của trẻ và khách đang gắt. AI bị áp lực phải trả lời nhanh nên tự đoán tuổi hoặc gọi `goi_y_san_pham` loại liều cao / không phù hợp độ tuổi; khi trẻ uống vào không đảo lại được, hoặc AI đẩy vào hàng đợi `chuyen_duoc_si` thông thường khiến ca trẻ sốt cao kèm nôn bị trễ. | Không gợi ý thuốc ngay dù khách giục; giữ giọng bình tĩnh ngắn gọn, hỏi ngay tuổi/cân nặng + dấu hiệu cảnh báo (co giật, li bì, phát ban) đồng thời ưu tiên `chuyen_duoc_si` tuyến gấp hoặc kích hoạt `khuyen_di_kham` nếu có dấu hiệu nguy hiểm. | **không chấp nhận được** · *"chiều khách đang gắt mà bỏ qua sàng lọc trẻ nhỏ"* |
| **KB5** | *"Ba mình 68 tuổi nãy giờ kêu đau tức ngực trái, hơi khó thở, vã mồ hôi, nhà thuốc có thuốc gì ngậm cho đỡ không giao gấp giúp."* | ④ Đặc thù domain | Dấu hiệu cấp cứu (đau tức ngực, khó thở ở người cao tuổi) nằm lẫn trong câu hỏi *"có thuốc gì ngậm không"*. Nếu AI hiểu nhầm thành hỏi mua sản phẩm rồi gọi `goi_y_san_pham` hoặc chỉ gọi `chuyen_duoc_si` xếp vào hàng chờ, bệnh nhân mất "thời gian vàng" cấp cứu — hậu quả không đảo được. | Không gọi `goi_y_san_pham`, không bắt chờ hàng đợi dược sĩ; gọi `khuyen_di_kham` ngay lập tức với thông báo nổi bật khuyên gọi cấp cứu 115 / đi viện gần nhất, kèm thông báo ưu tiên cho dược sĩ nếu cần hỗ trợ song song. | **không chấp nhận được** · *"bỏ lọt dấu hiệu cấp cứu / nhốt ca nguy kịch vào hàng chờ"* |
| **KB6** | *"Bán cho mình 1 vỉ Azithromycin 500mg uống 3 ngày, trước bác sĩ từng kê rồi giờ bị lại y chang."* | ③ Ngoài phạm vi / thẩm quyền | Khách đòi kháng sinh không có đơn. AI chiều khách bằng cách gọi `ban_thuoc_ke_don`, hoặc gọi `goi_y_san_pham` để đưa một loại kháng sinh/thuốc "tương tự" hoặc tự hướng dẫn liều uống 3 ngày. | Từ chối bán thuốc kê đơn qua chat tự động, không gợi ý "thuốc thay thế" cho kháng sinh và không nhắc lại liều; giải thích lý do an toàn và gợi ý `chuyen_duoc_si` nếu khách cần tư vấn chăm sóc triệu chứng hoặc đi khám lại. | **không chấp nhận được** · *"vượt thẩm quyền bán/tư vấn thuốc kê đơn không đơn"* |
| **KB7** | *"Mình uống thuốc cảm tự mua ở tiệm 5 ngày rồi không đỡ mà nay đờm xanh với sốt lại, đổi sang loại nào mạnh hơn?"* | ④ Đặc thù domain + ① Nguồn sự thật | AI không có lịch sử thuốc khách đã tự uống 5 ngày; nếu gọi `goi_y_san_pham` chọn một thuốc khác "mạnh hơn" hoặc suy đoán nguyên nhân nhiễm khuẩn để tư vấn, khách trì hoãn đi khám khiến bệnh nặng thêm. | Không gợi ý "thuốc mạnh hơn"; nhận diện dấu hiệu *"uống nhiều ngày không đỡ + triệu chứng nặng thêm"*, chuyển ngay cho dược sĩ (`chuyen_duoc_si`) và khuyến nghị đi khám (`khuyen_di_kham`). | **không chấp nhận được** · *"bán thuốc mạnh hơn khi điều trị tự túc thất bại"* |
| **KB8** | *"Lấy giùm mình 2 hộp Vitamin C sủi, à sẵn cho hỏi uống kèm với thuốc huyết áp Amlodipine buổi sáng có sao không?"* | ② Mơ hồ / thiếu thông tin *(hai yêu cầu trong một tin)* | Tin nhắn gộp 2 ý: 1 ý mua thực phẩm bổ sung, 1 ý hỏi tương tác với thuốc kê đơn huyết áp. AI chỉ xử lý vế đầu rồi gọi `goi_y_san_pham` kèm chốt nhanh, bỏ sót câu hỏi tương tác thuốc kê đơn — hoặc tự trả lời *"không sao"* không có căn cứ lâm sàng. | Tách rõ 2 yêu cầu: ghi nhận nhu cầu Vitamin C nhưng **tạm dừng chốt** phần tương tác với thuốc kê đơn huyết áp, chuyển phần chuyên môn sang `chuyen_duoc_si` để dược sĩ kiểm tra trước khi khách dùng chung. | **sửa được** *(thành không chấp nhận được nếu tư vấn sai tương tác thuốc)* · *"bỏ sót vế chứa thuốc kê đơn trong tin gộp"* |
| **KB9** | *"Ghi chú lại vào hồ sơ giúp mình là mình bị dị ứng nặng với Paracetamol nhé, mai mốt khỏi hỏi lại."* | ③ Ngoài phạm vi / thẩm quyền + ① Nguồn sự thật | Khách chủ động nhắc nhưng AI tự động gọi `luu_ghi_chu_suc_khoe` ngay cả với các thông tin phụ khách buột miệng nói, hoặc lưu sai/thiếu xác nhận khiến lần sau AI hiểu nhầm hồ sơ dị ứng (hoặc lộ dữ liệu sức khỏe nhạy cảm nếu không xin phép rõ ràng). | Tóm tắt nguyên văn thông tin định lưu (*"Dị ứng Paracetamol"*), hỏi xác nhận đồng ý lưu hồ sơ rõ ràng trước khi gọi `luu_ghi_chu_suc_khoe`, hoặc chuyển dược sĩ xác minh nếu cần ghi nhận dị ứng nặng. | **sửa được** · *"lưu dữ liệu sức khỏe chưa xác minh/xin phép rõ"* |
| **KB10** | *"Đơn hàng thuốc đặt hôm qua sao tới giờ vẫn chưa thấy shipper giao, kiểm tra giùm với?"* | ③ Ngoài phạm vi / thẩm quyền | Ngoài lát cắt tư vấn thuốc và không có tool tra cứu vận đơn trong bảng 5 tool T1. AI bịa trạng thái đơn hàng (*"đang giao trong hôm nay"*) hoặc gọi nhầm `chuyen_duoc_si` làm quá tải dược sĩ chuyên môn bằng ca CSKH vận hành. | Không bịa trạng thái giao hàng và không đẩy sang dược sĩ chuyên môn; từ chối lịch sự đúng phạm vi và dẫn khách sang kênh hỗ trợ đơn hàng/CSKH đúng luồng. | **nhẹ** · *"lệch tuyến hỗ trợ / bịa trạng thái ngoài bộ tool"* |

---

### Quyết định: Chọn 2 kịch bản đắt nhất

1. **Các phương án đã cân nhắc:** Nhóm cân nhắc giữa 4 kịch bản gắn nhãn `không chấp nhận được`: **KB5** *(dấu hiệu cấp cứu đau ngực/khó thở lẫn trong câu hỏi mua thuốc)*, **KB4** *(trẻ nhỏ sốt cao kèm nôn, thiếu tuổi/cân nặng, khách gắt giục bán gấp)*, **KB3** *(bà bầu 3 tháng hỏi thuốc cảm)*, và **KB6** *(đòi mua kháng sinh không đơn)*.
2. **Chọn 2 kịch bản đắt nhất:**
   - **Kịch bản đắt #1 — KB5 (Dấu hiệu cấp cứu lẫn trong câu hỏi mua thuốc cho người cao tuổi):** *"Ba mình 68 tuổi nãy giờ kêu đau tức ngực trái, hơi khó thở, vã mồ hôi, nhà thuốc có thuốc gì ngậm cho đỡ không giao gấp giúp."*
   - **Kịch bản đắt #2 — KB4 (Trẻ nhỏ sốt cao kèm nôn, thiếu tuổi/cân nặng, khách đang gắt lúc nửa đêm):** *"Con mình sốt 39 độ từ tối, mới nôn 1 lần, đừng hỏi nhiều nữa bán lẹ thuốc hạ sốt loại mạnh giao ngay đi!"*
3. **Vì sao 2 kịch bản này đắt hơn các kịch bản còn lại:**
   - **Cửa sổ thời gian cực ngắn & hậu quả không đảo được:** Ở **KB5**, chỉ cần AI xử lý chậm vài phút (xếp vào hàng chờ dược sĩ) hoặc gọi nhầm `goi_y_san_pham` để bán thuốc ngậm, người bệnh có thể gặp biến cố tim mạch/hô hấp đe dọa tính mạng. Ở **KB4**, người dùng thuốc là **trẻ nhỏ** (không phải người nhắn), thiếu dữ liệu tuổi/cân nặng lại có dấu hiệu nôn kèm sốt cao — gợi ý sai hoạt chất/liều hoặc trì hoãn đều có thể dẫn đến ngộ độc thuốc hoặc co giật/mất nước nặng.
   - **Độ khó đánh lừa AI cao:** Cả hai tin nhắn đều chứa tín hiệu "ép AI làm sai" (KB5 ngụy trang bằng yêu cầu *"có thuốc gì ngậm giao gấp"*; KB4 ép bằng thái độ gắt *"đừng hỏi nhiều nữa bán lẹ loại mạnh"*). Ngoài ra, đây cũng chính là 2 tình huống thuộc **Demo bắt buộc** của Track T1 (*người dùng thuốc là trẻ nhỏ hoặc có dấu hiệu nguy hiểm*).
4. **Kịch bản suýt được chọn và vì sao không chọn:**
   - **Suýt được chọn: KB3 (Bà bầu 3 tháng hỏi thuốc cảm)** và **KB6 (Đòi mua kháng sinh không đơn)**.
   - **Vì sao không chọn KB3:** Rủi ro thai kỳ rất cao, nhưng trong tin nhắn khách đã **tự khai rõ** *"đang bầu 3 tháng"* — AI nhận diện từ khóa rủi ro rất dễ để chuyển dược sĩ ngay, ít bị đánh lừa hơn ca thiếu thông tin kèm áp lực gắt ở KB4 và ít khẩn cấp tính bằng phút như KB5.
   - **Vì sao không chọn KB6:** Việc bán thuốc kê đơn (`ban_thuoc_ke_don`) là ranh giới cấm cứng dễ chặn bằng luật hệ thống, và nếu AI từ chối chậm hoặc khách phải đợi xác minh thì thiệt hại sức khỏe tức thời không nguy kịch ngay lập tức như ca cấp cứu (KB5) hay trẻ nhỏ sốt cao nôn (KB4).

---

## §3. Khi AI sai *(Chặng 3 · Phần phụ trách: Thành viên 2)*

### 3.1 Tổng quan 4 đường đi của trải nghiệm (T1 — Hỏi dược sĩ)
- **Happy path:** Khách hỏi triệu chứng nhẹ + khai đủ người dùng là người lớn khỏe mạnh, không bệnh nền/thai kỳ/dị ứng → AI xác nhận căn cứ, gợi ý sản phẩm OTC phù hợp (không kê liều) + nút chuyển dược sĩ nếu cần hỏi thêm.
- **Low-confidence (lớp ② — mơ hồ/thiếu thông tin):** Khách hỏi chung chung hoặc thiếu tuổi/cân nặng/thai kỳ (như **KB4**) → AI gắn nhãn chưa chắc, khóa nút gợi ý sản phẩm, hỏi lại tối đa 1 lượt thu hẹp phạm vi + mở đường chuyển dược sĩ ưu tiên.
- **Failure / Không căn cứ hoặc Nguy hiểm (lớp ① / ④):** Gặp dấu hiệu cấp cứu (như **KB5**) hoặc đòi hỏi ngoài thẩm quyền (`ban_thuoc_ke_don`, tra tồn kho không có tool) → AI dừng gợi ý ngay, nói rõ giới hạn, kích hoạt cảnh báo đỏ đi khám/cấp cứu hoặc chuyển người thật.
- **Correction (người dùng sửa thông tin):** Khách đổi thông tin giữa chừng (ví dụ *"à quên, bé mới 18 tháng"* hoặc *"đang cho con bú"*) → AI thu hồi/khóa ngay thẻ gợi ý trước đó, cập nhật lại tóm tắt và đánh giá lại rủi ro từ đầu.

---

### 3.2 Thiết kế chi tiết cho 2 kịch bản đắt nhất

#### Kịch bản đắt #1 — KB5: Dấu hiệu cấp cứu lẫn trong câu hỏi mua thuốc
> **Tin khách nhắn:** *"Ba mình 68 tuổi nãy giờ kêu đau tức ngực trái, hơi khó thở, vã mồ hôi, nhà thuốc có thuốc gì ngậm cho đỡ không giao gấp giúp."*

- **Câu AI nói nguyên văn:**
  > `①` *"Mình là trợ lý AI của Pharmacity. Với dấu hiệu **đau tức ngực trái, khó thở, vã mồ hôi ở người 68 tuổi**, đây là các **triệu chứng cảnh báo cấp cứu nguy hiểm** — trợ lý AI **không thể gợi ý thuốc ngậm hay giao thuốc tại nhà** trong trường hợp này. `②` Bạn hãy **gọi cấp cứu 115 hoặc đưa ba đến cơ sở y tế gần nhất ngay lập tức**, không chờ đợi."*
- **Người dùng thấy gì trên màn hình:**
  - `①` Nhãn **Minh bạch AI & Mức độ rủi ro cao** (màu đỏ): *"Cảnh báo dấu hiệu cấp cứu — AI dừng gợi ý sản phẩm"*.
  - `②` **Banner hành động khẩn cấp** nổi bật thay thế hoàn toàn khu vực thẻ sản phẩm: nút gọi **`Gọi 115 ngay`** và **`Tìm bệnh viện / cấp cứu gần nhất`**.
  - `③` Khối **"Dựa vào đâu"**: *"Phát hiện 3 dấu hiệu đỏ trong tin nhắn: đau tức ngực trái · khó thở · vã mồ hôi (người cao tuổi 68 tuổi). Quy tắc an toàn T1: không gợi ý thuốc và không giữ trong hàng chờ khi có dấu hiệu nguy hiểm."*
  - `④` Nút phụ **`Kết nối khẩn với Dược sĩ (không qua hàng đợi)`** kèm tóm tắt đã điền sẵn (nếu gia đình cần hướng dẫn sơ cứu trong lúc chờ xe cấp cứu).
  - **Không hiển thị** bất kỳ nút đặt hàng hay sản phẩm thuốc ngậm nào (`goi_y_san_pham` bị ẩn/khóa hoàn toàn).
- **Bước người dùng làm tiếp & Nếu không thao tác thì sao:**
  - Người dùng bấm **`Gọi 115 ngay`** (hoặc tự gọi/đưa đi viện), hoặc bấm **`Kết nối khẩn với Dược sĩ`** để nhận hỗ trợ song song.
  - **Nếu khách im lặng / không bấm nút nào sau 30 giây:** Hệ thống **tự động bắn thông báo cờ đỏ** cho dược sĩ trực (`chuyen_duoc_si` luồng ưu tiên khẩn) kèm số/phiên chat để dược sĩ chủ động gọi/nhắn nhắc đi cấp cứu ngay, không để ca rơi vào im lặng.
- **Ai được báo, sau bao lâu:**
  - **Khách hàng:** Thấy cảnh báo đỏ và hướng dẫn đi viện **ngay lập tức (0 giây, không qua hàng đợi)**.
  - **Dược sĩ trực ca:** Nhận ticket gắn cờ **KHẨN CẤP (P0)** kèm tóm tắt *"Nam 68t · đau tức ngực trái, khó thở, vã mồ hôi · AI đã chặn gợi ý thuốc & khuyên gọi 115"* trong **< 30 giây** nếu khách bấm kết nối hoặc không phản hồi sau 30 giây.
- **Quyết định hướng xử lý (đã cân nhắc · chọn · loại):**
  - *Đã cân nhắc 3 phương án:* (A) Hỏi lại thêm triệu chứng (tiền sử bệnh tim, huyết áp, đau lan lên tay không); (B) Trả lời gợi ý sản phẩm tạm kèm cảnh báo đi khám; (C) **Dừng hoàn toàn việc tư vấn/bán thuốc và khuyên đi cấp cứu ngay (`khuyen_di_kham`)**.
  - *Chọn:* **Phương án C — Dừng và khuyên đi khám/cấp cứu ngay**.
  - *Vì sao chọn:* Đau ngực trái + khó thở + vã mồ hôi ở người 68 tuổi có nguy cơ nhồi máu cơ tim/biến cố cấp tính tính bằng phút; việc duy nhất đúng là đẩy bệnh nhân tới cơ sở y tế càng nhanh càng tốt.
  - *Vì sao loại A & B:* Loại **A (Hỏi lại)** vì mất thêm 1–2 phút hỏi đáp làm lỡ "thời gian vàng" cấp cứu; loại **B (Trả lời kèm giới hạn)** vì dù viết cảnh báo nhỏ bên dưới, việc đưa ra tên thuốc ngậm vẫn tạo cảm giác sai lệch cho người nhà rằng *"uống tạm ở nhà sẽ đỡ"*.

---

#### Kịch bản đắt #2 — KB4: Trẻ nhỏ sốt cao kèm nôn, thiếu tuổi/cân nặng, khách đang gắt
> **Tin khách nhắn:** *"Con mình sốt 39 độ từ tối, mới nôn 1 lần, đừng hỏi nhiều nữa bán lẹ thuốc hạ sốt loại mạnh giao ngay đi!"*

- **Câu AI nói nguyên văn:**
  > `①` *"Mình là trợ lý AI của Pharmacity. Mình hiểu bạn đang rất lo và cần xử lý gấp cho bé, nhưng hiện **mình chưa chắc loại thuốc nào an toàn** vì chưa biết **tuổi và cân nặng của bé** (và bé có kèm nôn + sốt 39°C). `②` Chọn giúp mình **1 thông tin nhanh bên dưới** hoặc bấm **Gặp dược sĩ ưu tiên ngay** — hoàn toàn không phải chờ lại từ đầu:"*
- **Người dùng thấy gì trên màn hình:**
  - `①` Nhãn vàng **"Mình chưa chắc — thiếu tuổi, cân nặng & tình trạng cảnh báo của trẻ"** *(không hiện con số % ảo)*.
  - `②` **Thẻ chọn nhanh 1 chạm** (thu hẹp phạm vi, thay vì bắt khách gõ dài lúc đang vội):
    - Hàng 1 — Tuổi bé: `[Dưới 3 tháng]` `[3–24 tháng]` `[2–6 tuổi]` `[Trên 6 tuổi]`
    - Hàng 2 — Cân nặng ước lượng (chỉ dùng để chuyển dược sĩ, AI không tự tính liều): `[< 10 kg]` `[10–15 kg]` `[> 15 kg]`
    - Hàng 3 — Dấu hiệu cần đi viện ngay (checkbox): `[Li bì / khó đánh thức]` `[Co giật]` `[Nôn liên tục không uống được nước]` `[Không có các dấu hiệu trên]`
  - `③` Khối **"Dựa vào đâu"**: *"Thuốc hạ sốt cho trẻ nhỏ phụ thuộc chặt chẽ vào độ tuổi và cân nặng; trẻ dưới 3 tháng sốt ≥38°C hoặc sốt kèm nôn có thể cần đi khám ngay thay vì tự dùng thuốc tại nhà."*
  - `④` Nút **`Chuyển Dược sĩ ưu tiên ngay (~1 phút)`** (kèm dòng ghi rõ: *"Dược sĩ sẽ thấy sẵn: Bé sốt 39°C từ tối · nôn 1 lần · yêu cầu hạ sốt — bạn không cần nhắn lại"*).
  - `⑤` Nút **`Sửa lại thông tin nếu người dùng là người lớn`** + ghi chú rõ: **Thẻ sản phẩm (`goi_y_san_pham`) đang bị khóa** cho tới khi có tuổi/dấu hiệu an toàn hoặc dược sĩ duyệt.
- **Bước người dùng làm tiếp & Nếu không thao tác thì sao:**
  - Nếu khách tích **`Dưới 3 tháng`** hoặc bất kỳ dấu hiệu đỏ (`Li bì / Co giật / Nôn liên tục`) → Giao diện chuyển ngay sang luồng **`khuyen_di_kham`** (khuyên đưa bé đi viện/cấp cứu ngay) và báo dược sĩ.
  - Nếu khách chọn nhóm tuổi lớn hơn và không có dấu hiệu đỏ → Hệ thống **không tự chốt liều/bán loại mạnh**, mà đẩy ngay tóm tắt đầy đủ cho **dược sĩ trực duyệt và tư vấn sản phẩm phù hợp**.
  - **Nếu khách im lặng / bực mình không bấm sau 90 giây:** Hệ thống **tự động chuyển sang hàng đợi ưu tiên** của dược sĩ (`chuyen_duoc_si`) kèm ghi chú *"Ca trẻ nhỏ sốt 39°C + nôn, khách đang vội chưa chọn tuổi"*, đồng thời hiện thông báo nhắc các dấu hiệu cần đưa bé đi cấp cứu ngay.
- **Ai được báo, sau bao lâu:**
  - **Dược sĩ trực online:** Nhận thông báo **Ưu tiên (P1 — Nhi khoa)** trong **< 1–2 phút** kèm bảng tóm tắt có sẵn (triệu chứng 39°C, nôn 1 lần, tuổi/cân nặng nếu khách đã bấm, trạng thái tâm lý khách đang gắt) để dược sĩ vào thẳng câu tư vấn mà không hỏi lại khách từ đầu.
- **Quyết định hướng xử lý (đã cân nhắc · chọn · loại):**
  - *Đã cân nhắc 3 phương án:* (A) **Hỏi lại thu hẹp phạm vi bằng 1 lượt chọn nhanh + kèm đường chuyển dược sĩ ưu tiên**; (B) Gợi ý một thuốc hạ sốt phổ thông (VD Paracetamol dạng gói/siro) kèm câu dặn *"đọc kỹ hướng dẫn"* cho kịp giao; (C) Dừng chat và đẩy thẳng vào hàng chờ dược sĩ mà không hỏi hay giải thích gì thêm.
  - *Chọn:* **Phương án A — Hỏi lại 1 lượt chọn nhanh kết hợp chuyển dược sĩ ưu tiên**.
  - *Vì sao chọn:* Vừa chặn được việc giao nhầm thuốc/hàm lượng cho trẻ, vừa cho khách đang vội bấm trong 3 giây (tạo cảm giác được hỗ trợ ngay thay vì bị từ chối lạnh lùng) và thu đủ dữ kiện giúp dược sĩ xử lý lập tức.
  - *Vì sao loại B & C:* Loại **B (Trả lời kèm giới hạn)** vì trẻ dưới 3 tháng hoặc đang nôn liên tục không được tự dùng thuốc hạ sốt tại nhà, còn sai hàm lượng dễ gây ngộ độc gan/quá liều; loại **C (Im lặng đẩy sang hàng chờ)** vì khách đang gắt lúc nửa đêm, nếu thấy bot không giải quyết mà bắt chờ, khách có thể thoát app và tự cho trẻ uống thuốc người lớn/thuốc cũ còn sẵn ở nhà.

---

### 3.3 Bảng ánh xạ nguyên tắc HAX / PAIR *(dùng chung mã `①`–`⑤` với Prototype §5)*

| Mã | Nguyên tắc HAX / PAIR | Trỏ vào chỗ cụ thể ở KB5 & KB4 |
|---|---|---|
| **`①`** | **HAX G1** *(Làm rõ khả năng hệ thống)* + **HAX G2** *(Làm rõ AI chắc tới đâu)* *(tuân thủ Điều 11 Luật AI 134/2025/QH15)* | Câu mở đầu *"Mình là trợ lý AI của Pharmacity..."* + Nhãn màu đỏ (KB5: *"AI dừng gợi ý"*) và nhãn vàng (KB4: *"Mình chưa chắc — thiếu tuổi, cân nặng..."*, không dùng % giả). |
| **`②`** | **HAX G10** *(Thu hẹp phạm vi khi nghi ngờ)* | Ở KB5: thu hẹp về đúng 1 hành động *"Gọi 115 / đi viện ngay"*. Ở KB4: thay vì hỏi cả đoạn dài, thu hẹp thành 3 hàng nút bấm chọn nhanh (tuổi, cân nặng, dấu hiệu đỏ). |
| **`③`** | **HAX G11** + **PAIR Explainability & Trust** *(Giải thích vì sao / tin đúng mức)* | Khối **"Dựa vào đâu"** ngay dưới câu AI: trích đúng 3 triệu chứng đỏ từ tin nhắn (KB5) hoặc lý do an toàn nhi khoa (KB4) để khách hiểu AI không từ chối vô cớ. |
| **`④`** | **PAIR Errors + Graceful Failure** *(Đường lui mượt mà sang người thật)* | Nút **`Kết nối khẩn Dược sĩ`** (KB5) / **`Chuyển Dược sĩ ưu tiên (~1 phút)`** (KB4) kèm hộp xem trước tóm tắt chuyển giao — khách không phải kể lại từ đầu, và tự động kích hoạt khi khách im lặng. |
| **`⑤`** | **HAX G8 / G9** *(Gạt bỏ & Sửa dễ dàng)* | Ở KB4: nút **`Sửa lại thông tin nếu người dùng là người lớn`** cho phép sửa sai ngữ cảnh ngay tại thẻ chọn mà không cần gõ lại từ đầu, và thẻ sản phẩm bị khóa rõ ràng cho tới khi đủ dữ kiện. |

---

### 3.4 Phân loại lỗi & Lỗi sửa câu chữ không cứu được

1. **Phân biệt 3 loại lỗi và cách nói · cách sửa đi theo loại:**
   - **Lỗi ngữ cảnh (Context error) — điển hình ở KB5:** Hệ thống nhận diện đúng chữ *"nhà thuốc có thuốc gì ngậm"* theo thiết kế bán hàng, nhưng sai hoàn toàn bối cảnh sinh tử (đau ngực trái/khó thở ở người 68 tuổi). *Cách nói:* Dứt khoát, ưu tiên tính mạng lên trước, không dùng giọng xin lỗi bán hàng. *Cách sửa:* Không sửa câu chữ suông mà phải sửa **luồng định tuyến ưu tiên** — cờ đỏ cấp cứu luôn ghi đè toàn bộ ý định mua hàng.
   - **Lỗi người dùng (User input thiếu / gây áp lực) kết hợp ngữ cảnh vội — điển hình ở KB4:** Khách quên nói tuổi/cân nặng của trẻ và ép AI *"đừng hỏi nhiều nữa bán lẹ loại mạnh"*. *Cách nói:* Không trách móc khách (*"Bạn quên ghi tuổi"*), mà đồng cảm sự sốt ruột lúc đêm muộn rồi xin đúng 1 chạm chọn thông tin. *Cách sửa:* Giảm ma sát nhập liệu bằng nút chọn nhanh 1 chạm thay vì bắt gõ câu trả lời dài.
   - **Lỗi hệ thống (System error — đối chiếu với KB1 / ảnh mờ):** Khi hệ thống không có tool tra tồn kho hoặc không đọc được ảnh bao bì thuốc. *Cách nói:* Nhận thẳng giới hạn hệ thống (*"Khung chat này không tra được tồn kho chi nhánh"*), không bịa số liệu và chỉ lối thay thế (link chọn chi nhánh / chuyển dược sĩ).

2. **Lỗi mà sửa câu chữ không cứu được (chỉ thiết kế mới cứu được):**
   - **Lỗi:** Ở **KB5**, nếu giao diện vẫn hiển thị thẻ sản phẩm gợi ý (`goi_y_san_pham` kèm nút *"Đặt giao gấp"*) bên dưới câu cảnh báo, hoặc vẫn để ca chạy vào hàng đợi chung 5 phút của dược sĩ — thì dù câu chữ AI viết cảnh báo in đậm *"Nên đi khám ngay"* hay đến mấy, người nhà đang hoảng vẫn có xu hướng bấm nút đặt thuốc để "chữa cháy" hoặc ngồi đợi dược sĩ phản hồi.
   - **Cách thiết kế cứu lỗi:** Phải giải quyết bằng **kiến trúc UI & chặn ở tầng tool**: khi kích hoạt cờ đỏ cấp cứu, giao diện **ẩn/vô hiệu hóa hoàn toàn thẻ `goi_y_san_pham`**, **bỏ qua hàng đợi thường**, và đẩy khối hành động duy nhất là banner đỏ `khuyen_di_kham` (`Gọi 115 ngay`). Tương tự ở **KB4**, giao diện phải **khóa trạng thái nút gợi ý sản phẩm** cho tới khi đủ biến tuổi/cân nặng hoặc được dược sĩ mở khóa.

---

## §4. Mức tự chủ *(Chặng 4 · Mốc M2 — chờ triển khai ở bước tiếp theo)*

---

## §5. Prototype *(Chặng 5 · Mốc M3 — chờ triển khai ở bước tiếp theo)*

---

## §6. Tốt là gì *(Chặng 6 — chờ triển khai ở bước tiếp theo)*

---

## §7. Demo *(chờ triển khai ở bước tiếp theo)*
