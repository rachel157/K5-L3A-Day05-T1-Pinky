# Kế hoạch thực hiện LAB5 · Nhóm Pinky (4 thành viên)

- **Lớp:** L3A · **Track:** T1 · Y tế (Kênh chat *"Hỏi dược sĩ"* — Pharmacity)
- **Hạn nộp repo:** Trước **23:59 ngày 12/11/2026**
- **Mục tiêu điểm:** 100 điểm (`R1–R7`, trong đó **75đ** tập trung ở `§3`, `§4`, `§5`) + tối đa **+8đ bonus** (+5đ xung phong demo trước lớp · +3đ có ≥1 lời gọi AI thật không lộ key).

---

## 1. Quy tắc chống conflict (Git & Nội dung)

Để 4 người làm **song song cùng lúc** mà **không đụng Git merge conflict** và **không lệch logic** giữa các phần:

### A. Tách ranh giới file & vùng sở hữu (Git Ownership)

| Thành viên | File / Thư mục sở hữu | Quy tắc Git |
|---|---|---|
| **TV1** *(Lead phạm vi & Demo)* | `spec.md` (`§1`, `§2`, `§7`) + Bảng thành viên | Chỉ sửa phần đầu `spec.md` (`§1`, `§2`, `§7`) và làm **người gộp cuối (Integrator)** |
| **TV2** *(UX khi AI sai)* | Soạn vào file nháp riêng `draft-s3.md` *(hoặc nhánh `feat/tv2-ux`)* | Không sửa trực tiếp `spec.md` lúc làm song song; xong chuyển cho TV1 dán vào `§3` |
| **TV3** *(Mức tự chủ & Tiêu chí)* | Soạn vào file nháp riêng `draft-s4-s6.md` *(hoặc nhánh `feat/tv3-autonomy`)* | Không sửa trực tiếp `spec.md` lúc làm song song; xong chuyển cho TV1 dán vào `§4`, `§6` |
| **TV4** *(Prototype)* | Toàn bộ thư mục `prototype/` (`index.html`, ảnh đánh số...) + `draft-s5.md` | **Độc lập hoàn toàn** với `spec.md`, có thể code song song ngay từ đầu trên `prototype/` |

> **Luật Git nhanh cho cả nhóm:**
> 1. Luôn chạy `git pull --rebase` trước khi bắt đầu và trước khi push.
> 2. Không ai tự ý sửa file thuộc quyền sở hữu của người khác.
> 3. Trước khi nộp bài cuối giờ, **xóa toàn bộ file nháp (`draft-*.md`), `PLAN.md` và các file đề bài gốc**, chỉ giữ đúng `spec.md` + `prototype/` theo quy định nộp bài.

### B. Hợp đồng dữ liệu chung (Chống conflict logic giữa `§2 → §3 → §4 → Prototype`)

Cả 4 người bắt buộc dùng chung **cùng một bộ khóa (Single Source of Truth)** đã chốt ở `spec.md §2`:

1. **2 kịch bản đắt nhất cố định:**
   - **KB5 (Cấp cứu):** *"Ba mình 68 tuổi nãy giờ kêu đau tức ngực trái, hơi khó thở, vã mồ hôi, nhà thuốc có thuốc gì ngậm cho đỡ không giao gấp giúp."*
   - **KB4 (Trẻ nhỏ + khách gắt + thiếu thông tin):** *"Con mình sốt 39 độ từ tối, mới nôn 1 lần, đừng hỏi nhiều nữa bán lẹ thuốc hạ sốt loại mạnh giao ngay đi!"*
   - *(Ca bình thường để TV3 test ở §4: **KB2** — *"Họng mình đau rát 2 ngày nay, có thuốc gì không?"*).*
2. **Đúng 5 tên tool của Track T1:** `goi_y_san_pham` · `chuyen_duoc_si` · `khuyen_di_kham` · `luu_ghi_chu_suc_khoe` · `ban_thuoc_ke_don`.
3. **Bảng mã đánh số HAX/PAIR dùng chung** (TV2 chốt, TV4 gắn đúng số đó lên giao diện `prototype/`):
   - **Số `①` — HAX G1 / G2:** Nói rõ là trợ lý AI + hiển thị mức độ chưa chắc / thiếu thông tin gì (không dùng % ảo).
   - **Số `②` — HAX G10:** Thu hẹp phạm vi khi nghi ngờ (hỏi 1 câu sàng lọc hoặc cảnh báo đỏ ngay, không đoán).
   - **Số `③` — HAX G11 + PAIR Explainability:** Khối *"Dựa vào đâu"* trích nguồn/quy tắc rõ ràng.
   - **Số `④` — PAIR Errors + Graceful Failure:** Đường chuyển sang dược sĩ trực (`chuyen_duoc_si`) kèm tóm tắt ca + thời gian chờ.
   - **Số `⑤` — HAX G8 / G9:** Nút sửa thông tin / hủy / xác nhận đồng ý trước khi hành động (ví dụ xác nhận lưu hồ sơ hoặc dừng gợi ý).

---

## 2. Lộ trình thực hiện từng bước

### Bước 0 · Khởi tạo & Xin xác nhận nhóm 4 người *(5 phút · Cả nhóm)*
- [ ] **TV1** báo nhanh với TA trực bàn để xác nhận nhóm 4 người (lớp L3A) được nộp chung 1 repo với 4 mã học viên.
- [ ] Điền đủ **Họ tên, Mã học viên, Việc đã làm** của 4 thành viên vào đầu `spec.md` (kiểm tra kỹ mã học viên).

### Bước 1 · Khóa Mốc M1: Lát cắt & 10 Kịch bản rủi ro *(10 phút · TV1 chủ trì, cả nhóm rà)*
*(Phần khung `§1` và `§2` đã được dựng sẵn trong `spec.md`)*
- [ ] Cả nhóm đọc nhanh lại `§1` (lát cắt + 4 việc KHÔNG làm) và 10 kịch bản `KB1–KB10` ở `§2`.
- [ ] Chốt cứng **KB5** và **KB4** làm 2 ca đắt nhất để mở khóa cho TV2, TV3, TV4 làm song song.
- [ ] **Check Mốc M1 với TA:** Show bảng ≥8 kịch bản phủ 4 lớp + trỏ rõ tool/dữ liệu + lý do chọn 2 ca đắt nhất.

### Bước 2 · Chạy song song đợt 1 *(35 phút · Không conflict)*
Trong giai đoạn này **4 người làm độc lập hoàn toàn**:

- **TV1 (`§7` + Kịch bản Demo & Ca lạ):**
  - Chuẩn bị bộ trả lời mẫu cho các **ca lạ** TA hay hỏi lúc Demo: ca xưng hô mập mờ (*"em mình"*), ca gửi ảnh thuốc mờ, ca đổi thông tin giữa chừng (*"à quên đang cho con bú"*), ca hỏi mua cho thú cưng, ca khách im lặng timeout.
  - Viết trước kịch bản dẫn Demo 30 giây đầu.

- **TV2 (`draft-s3.md` → `§3 Khi AI sai` · 25đ):**
  - Làm đủ cho **KB5** và **KB4**:
    1. Câu AI nói **nguyên văn** (không hứa vượt thẩm quyền).
    2. Người dùng **thấy gì** trên màn hình (độ chắc, bằng chứng, nút bấm, nhãn cảnh báo).
    3. Bước người dùng **làm tiếp** + nếu im lặng không bấm thì sao.
    4. **Ai được báo** (dược sĩ trực / cấp cứu), sau bao lâu, người nhận thấy tóm tắt gì.
    5. Lập luận chọn hướng (*hỏi lại / trả lời kèm giới hạn / dừng & chuyển người*) và **vì sao loại 2 hướng kia**.
    6. Phân loại lỗi (*hệ thống / người dùng / ngữ cảnh*) + cách xin lỗi/sửa theo loại lỗi đó.
    7. Gắn bảng **HAX/PAIR (`①`–`⑤`)** vào đúng câu chữ hoặc vị trí màn hình.
    8. Chỉ ra **1 lỗi sửa câu chữ không cứu được** (ví dụ: ca cấp cứu KB5 hoặc khách gắt KB4 nếu AI vẫn hiện thẻ gợi ý sản phẩm/đẩy vào hàng đợi thường) và cách **thiết kế UI/luồng** cứu nó.

- **TV3 (`draft-s4-s6.md` → `§4 Mức tự chủ` · 25đ + `§6` · 5đ):**
  - Tổ chức **5 phút cả 4 người tự chấm độc lập** mức tự chủ cho 5 tool (ghi lại ai chọn gì để làm dẫn chứng cho mục *"tool khó xếp nhất"*).
  - Hoàn thiện bảng 5 tool (`Tự làm` / `Cần người duyệt` / `Cấm`):
    - Lý do bắt buộc theo **cost-of-error** (*sai ai chịu gì · đảo được không · chờ người mất gì*).
    - Phương án đã cân nhắc & vì sao loại.
    - Với tool *Cần người duyệt*: ai duyệt · thấy gì trước khi bấm · quá hạn thì sao.
  - Phân tích **tool khó xếp nhất** (gợi ý: `goi_y_san_pham` hoặc `khuyen_di_kham` / `luu_ghi_chu_suc_khoe` — nêu rõ ý kiến khác nhau của 4 người, cái giá trỏ về KB2/KB4/KB5, cách chốt hoặc tách bước).
  - Chạy thử bảng mức trên **KB5 + KB4 + KB2 (ca bình thường)** theo 3 câu kiểm tra của `02-guide.md`.
  - Chỉ ra một mức tự chủ đã đổi **lời hứa với người dùng** như thế nào.
  - Viết **3 tiêu chí Pass/Fail (`§6`)** và chấm thử trên KB5, KB4.

- **TV4 (`prototype/` — Khung UI & Luồng tương tác · 25đ):**
  - Dựng khung **1–2 màn hình HTML/JS thuần** (hoặc công cụ nhóm chọn — khuyến nghị 1 file `prototype/index.html` để mở thẳng không cần server, **không chép giao diện `prototype-sample`**).
  - Thiết kế sẵn 2 tab/case bấm chuyển qua lại ngay trên 1 màn hình (kèm tùy chọn xem màn hình phía Dược sĩ nhận ca nếu kịp):
    - **Case 1 (KB5):** Khách nhắn ca đau ngực/khó thở → Giao diện chặn hoàn toàn nút mua hàng, bật thẻ đỏ `khuyen_di_kham` (115 / đi viện ngay), không đẩy vào hàng đợi chờ, có nút xác nhận đã gọi/chuyển tiếp tóm tắt khẩn cho dược sĩ.
    - **Case 2 (KB4):** Khách gắt đòi hạ sốt loại mạnh cho con sốt 39°C kèm nôn → Hiện nhãn chưa chắc `①`, khóa nút `goi_y_san_pham` (`Cần sàng lọc / Dược sĩ duyệt`), bắt chọn nhanh tuổi/cân nặng + dấu hiệu cảnh báo hoặc chuyển thẳng `chuyen_duoc_si` kèm tóm tắt; đồng thời cho thấy nút **cấm** (`ban_thuoc_ke_don`) không tồn tại hoặc bị khóa kèm giải thích.

### Bước 3 · Điểm đồng bộ giữa giờ *(10 phút · Check Mốc M2)*
- [ ] **TV2 + TV3 + TV4** ngồi khớp nhanh 3 thứ:
  1. Câu chữ nguyên văn từ `draft-s3.md` (TV2) đổ vào `prototype/index.html` (TV4).
  2. Nhãn mức tự chủ từ `draft-s4-s6.md` (TV3) khớp 100% với trạng thái nút trên `prototype/index.html` (TV4).
  3. **5 số chú thích HAX/PAIR (`①`–`⑤`)** khớp chính xác giữa `§3`, `§5` và giao diện.
- [ ] **TV1** gộp `draft-s3.md` và `draft-s4-s6.md` vào `spec.md`, push lên Git.
- [ ] **Check Mốc M2 với TA:** Show bảng mức tự chủ 5 tool + lý do cost-of-error + tool khó nhất + test 3 kịch bản.

### Bước 4 · Hoàn thiện Prototype & Check Mốc M3 *(30 phút · TV4 chủ trì, TV1/2/3 review)*
- [ ] **TV4** hoàn tất luồng bấm xuyên suốt (end-to-end không kẹt) cho cả **KB4** và **KB5**, có nút bật/tắt số chú thích `①`–`⑤` (hoặc hiện sẵn).
- [ ] Kiểm tra đủ 4 tiêu chí chấm của **R5**:
  - Bấm hết được ca khó không can thiệp tay.
  - Thấy rõ độ chắc + bằng chứng AI dựa vào.
  - Thấy ít nhất 2 mức tự chủ khác nhau (có xác nhận / chuyển người / hoàn tác hoặc hủy / việc cấm không có nút làm).
  - Mỗi nguyên tắc HAX/PAIR trỏ được vào vị trí đánh số cụ thể.
- [ ] *(Nếu còn thời gian, +3đ bonus)*: Tích hợp 1 lời gọi AI thật cho bước sinh câu hỏi sàng lọc (nhập API key qua ô input trên trình duyệt lúc chạy, **tuyệt đối không hardcode key trong code**).
- [ ] Chụp **1–2 ảnh màn hình có đánh số `①`–`⑤`** lưu vào `prototype/` (ví dụ `prototype/screen-kb4.png`, `prototype/screen-kb5.png`).
- [ ] **TV4** viết phần `§5` (link/file, bảng giải thích từng số `①`–`⑤`, lập luận chọn cách thể hiện và cách khác đã loại), **TV1** gộp vào `spec.md`.
- [ ] **Check Mốc M3 với TA:** Cho TA bấm qua ca khó trên prototype.

### Bước 5 · Tập dượt Demo 4 người & Chốt ca lạ *(15 phút · Cả nhóm)*
- [ ] Chạy thử kịch bản Demo 4 phút theo đúng thứ tự chia bài:
  1. **TV1 (30s):** Đọc lát cắt T1 + việc KHÔNG làm + giới thiệu 2 ca đắt nhất.
  2. **TV4 (90s):** Bấm trực tiếp prototype qua ca khó nhất (KB4/KB5), chỉ từng số `①`–`⑤`.
  3. **TV2 (45s):** Giải thích vì sao câu chữ & UX được thiết kế như vậy (loại lỗi & lỗi sửa câu chữ không cứu được).
  4. **TV3 (45s):** Giải thích bảng mức tự chủ theo cost-of-error & công cụ khó xếp nhất.
  5. **Cả nhóm (xử lý ca lạ tại chỗ):** TV1 nhận đề, TV3 đối chiếu bảng mức tự chủ, TV2 đọc hướng xử lý UX, TV4 chỉ chỗ trên prototype.
- [ ] Cả 4 người tự kiểm tra chéo **2 câu TA chắc chắn hỏi**:
  - *"Chỉ cho tôi chỗ nào trên màn hình là nguyên tắc G__?"*
  - *"Hành động này sai thì ai mất gì, vì vậy nó ở mức nào?"*
- [ ] Cân nhắc **xung phong demo trước lớp** để lấy **+5đ bonus**.
- [ ] Sau khi demo xong, **TV1** ghi lại ca lạ đã nhận và phản hồi vào `spec.md §7`.

### Bước 6 · Dọn repo sạch & Nộp bài *(Trước 23:59 ngày 12/11/2026 · TV1)*
Theo quy định trong [README.md](README.md):
- [ ] Xóa các file nháp (`draft-*.md`), `PLAN.md`, và **các file đề bài gốc** (`01-challenge-brief.md`, `02-guide.md`, `03-spec-template.md`, `04-rubric.md`, `tracks/`, `prototype-sample/`, `README.md` đề gốc) khỏi repo nộp bài — chỉ giữ đúng:
  ```text
  K5-L3A-Day05-T1-Pinky/
  ├── spec.md
  └── prototype/
      ├── index.html
      └── (1–2 ảnh chụp màn hình có đánh số)
  ```
- [ ] Kiểm tra không có API key hay dữ liệu thật nào trong commit/code.
- [ ] Đảm bảo GitHub repo `K5-L3A-Day05-T1-Pinky` đang ở chế độ **Public** (mở thử bằng **Cửa sổ ẩn danh**).
- [ ] Push lần cuối và nộp link repo lên kênh của lớp trước **23:59 ngày 12/11/2026**.
