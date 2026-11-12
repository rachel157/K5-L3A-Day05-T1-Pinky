## §5. Prototype *(Chặng 5 · Mốc M3)*

### 5.1 Sản phẩm

- **File:** `prototype/index.html` — mở trực tiếp bằng trình duyệt, không cần server.
- **Công cụ:** HTML/CSS/JavaScript thuần; giao diện responsive, chủ đề xanh lá nhạt.
- **Phần mock:** cuộc gọi 115, chuyển ticket cho dược sĩ và trạng thái hàng đợi đều là mô phỏng giao diện; không có backend, không gửi dữ liệu thật.
- **Lời gọi AI thật:** Không. Prototype dùng các nhánh quyết định cố định từ KB4 và KB5, phù hợp yêu cầu bài thiết kế và tránh đưa dữ liệu sức khỏe vào dịch vụ bên ngoài.

### 5.2 Luồng bấm được

1. **KB4 — Trẻ sốt 39°C kèm nôn:** khách chọn nhanh tuổi, cân nặng và dấu hiệu cảnh báo. Nếu chọn `Dưới 3 tháng` hoặc dấu hiệu đỏ, prototype chuyển sang `khuyen_di_kham`; nếu chưa có dấu hiệu đỏ, ca được chuyển dược sĩ ưu tiên và AI không tự đưa liều hay bán thuốc. Người dùng có thể sửa/nhập lại toàn bộ thông tin. Có nút mô phỏng 90 giây im lặng, trạng thái đang chờ nhận ca, trạng thái dược sĩ đã nhận và trạng thái quá hạn “Chưa có dược sĩ nhận ca” kèm đường đi khám; giao diện không bịa thời gian chờ khi hệ thống chưa có dữ liệu.
2. **KB5 — Đau ngực, khó thở, vã mồ hôi:** prototype chặn hoàn toàn gợi ý sản phẩm, thay bằng nút `Gọi 115 ngay` và `Kết nối khẩn với dược sĩ`. Ca không đi qua hàng đợi thường. Có nút mô phỏng 30 giây im lặng để tự gửi tóm tắt P0, và nhánh mô phỏng gửi tóm tắt thất bại; trong nhánh lỗi, nút gọi 115 vẫn khả dụng và tách biệt với trạng thái chuyển ca.

### 5.3 Ánh xạ vị trí đánh số với HAX/PAIR và mức tự chủ

| Mã trên prototype | Thiết kế cụ thể | Nguyên tắc | Mức tự chủ thể hiện |
|---|---|---|---|
| `①` | Nhãn vàng “Chưa đủ thông tin” ở KB4 và nhãn đỏ “Cảnh báo dấu hiệu cấp cứu” ở KB5; câu mở đầu nói rõ đây là trợ lý AI | HAX G1/G2 | AI tự công bố giới hạn và trạng thái chưa chắc |
| `②` | KB4 dùng ba nhóm chọn nhanh để thu hẹp phạm vi; KB5 chỉ còn hành động gọi 115/đi viện | HAX G10 | `khuyen_di_kham` được tự kích hoạt khi có dấu hiệu đỏ; không đoán sản phẩm |
| `③` | Khối “Dựa vào đâu?” liệt kê dữ kiện lấy trực tiếp từ tin nhắn và quy tắc an toàn | HAX G11 + PAIR Explainability | Người dùng kiểm tra được căn cứ trước khi hành động |
| `④` | Thẻ chuyển dược sĩ kèm tóm tắt; KB5 ghi rõ luồng P0 không qua hàng đợi thường | PAIR Errors + Graceful Failure | `chuyen_duoc_si` là đường lui an toàn; `goi_y_san_pham` cho trẻ cần dược sĩ duyệt |
| `⑤` | KB4 có nút sửa/nhập lại; KB5 có xác nhận đã gọi cấp cứu; nút mua thuốc bị khóa | HAX G8/G9 | Người dùng sửa được đầu vào; `ban_thuoc_ke_don` bị cấm và không có hành động khả dụng |

### 5.4 Quyết định thiết kế

- **Các phương án đã cân nhắc:** (A) giao diện chat thuần chỉ có câu trả lời; (B) form y tế dài bắt nhập đầy đủ trước khi thấy phản hồi; (C) giao diện chat kết hợp thẻ trạng thái, nút chọn nhanh, căn cứ và đường chuyển người.
- **Chọn phương án C:** khách vẫn nhận ra ngữ cảnh chat quen thuộc, nhưng các quyết định an toàn trở thành thành phần có thể nhìn và bấm. Nút chọn nhanh giảm thời gian nhập ở KB4; banner đỏ thay hoàn toàn vùng sản phẩm ở KB5.
- **Loại phương án A:** câu chữ cảnh báo không đủ ngăn khách bấm mua nếu thẻ sản phẩm vẫn tồn tại; cũng không chứng minh được mức tự chủ trên màn hình.
- **Loại phương án B:** form dài làm tăng ma sát khi phụ huynh đang gấp và có thể khiến họ thoát luồng trước khi được chuyển dược sĩ.
- **Cách hiện độ chắc:** dùng câu chữ cụ thể “thiếu tuổi, cân nặng và dấu hiệu cảnh báo”, không dùng phần trăm tin cậy giả. Màu vàng biểu thị cần sàng lọc; màu đỏ chỉ dùng cho ca cần cấp cứu.
- **Cách hiện bằng chứng:** khối “Dựa vào đâu?” trích lại đúng dữ kiện trong tin nhắn và nêu quy tắc an toàn dẫn tới quyết định. Khối này tách khỏi câu trả lời chính để người dùng có thể đọc nhanh nhưng vẫn kiểm chứng được.
- **Cách hiện quyền tự chủ:** bảng bên phải ghi đủ 5 tool với `Tự làm / Cần dược sĩ duyệt / Cần khách đồng ý / Cấm` và cái giá khi sai hoặc chờ; trong luồng chính, trạng thái này được thể hiện bằng hành động khả dụng, nút bị khóa, đường chuyển dược sĩ và các trạng thái thành công/quá hạn/lỗi gửi.

### 5.5 Ảnh minh chứng

- `prototype/screen-kb4.png`: luồng trẻ sốt cao với các vị trí `①–⑤`.
- `prototype/screen-kb5.png`: luồng cấp cứu với các vị trí `①–⑤`.

> Ảnh được chụp sau khi nhóm review và chốt câu chữ cuối cùng để bảo đảm khớp 100% với §3 và §4.
