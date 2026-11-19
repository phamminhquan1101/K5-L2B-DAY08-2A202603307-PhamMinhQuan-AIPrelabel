# Label contract — Phố châu Á — BỔ SUNG v1.1 — 2A202603307 - Phạm Minh Quân

> Bản bổ sung cho guideline v1.0, được soạn với hỗ trợ của Codex. Giữ nguyên các định nghĩa lớp, quy tắc hình học và mục `rules` trong taxonomy. Bảng quyết định tương ứng: [calibration_decisions.csv](../qc/calibration_decisions.csv).

## 1. Câu hỏi mở đã chốt (từ mục 6 của v1.0)

| # | Câu hỏi | Quyết định | Ảnh ví dụ | `case_id` |
|---|---|---|---|---|
| 7 | Có ngưỡng conf nào cho phép chấp nhận box máy mà không kiểm tra không? | Không. Conf chỉ dùng lọc ứng viên và ưu tiên review. Mọi box còn lại phải được đối chiếu với ảnh thật về lớp, vị trí/kích thước và thuộc tính; chỉ giữ nguyên sau khi kiểm tra. Đồng thời quét ảnh để tìm vật bị bỏ sót. | `img_002`, `img_007`, `img_011`, `img_032` | CAL-03 |

**Bằng chứng:** theo `model_log.csv`, tăng conf YOLO26m từ 0.25 lên 0.50 giảm một số nhãn sai nhưng làm mất vật đúng ở các ảnh trên. Ngưỡng cao hơn không thay thế kiểm tra nội dung ảnh. Log không ghi điểm của từng box lỗi, vì vậy không khẳng định một box cụ thể là lỗi điểm cao. Đây là quy tắc review, không thay đổi ngưỡng suy luận đã chọn ở Part 3 hoặc cách tính số đo của script. Không khẳng định quy tắc mới đã được thực hiện ở vòng review đầu tiên.

### Các câu hỏi còn mở

| Câu của v1.0 | Nội dung | Trạng thái và cách xử lý |
|---|---|---|
| 1 | Xe đạp điện/xe máy điện nhỏ | Chưa đủ bằng chứng để chốt ranh giới lớp; ghi `can_review` khi gặp ca không chắc. |
| 2 | Khối xe máy đỗ dày đặc gần camera | Chưa chốt ngưỡng riêng; tiếp tục áp dụng quy tắc v1.0 về vật rõ, kích thước và vùng không tách được. |
| 3 | Hàng nhô ra khỏi xe | Chưa có ca đối chiếu đủ rõ để bổ sung quy tắc mask; giữ v1.0 và ghi ca mơ hồ. |
| 4 | Ước lượng người còn nhìn thấy ít nhất 20% | Chưa có phép đánh giá độ nhất quán của ước lượng này; giữ v1.0, ghi ca không chắc. |
| 5 | Kính nhìn xuyên thấy người/nội thất | Chưa chốt chi tiết; giữ quy định kính thuộc xe của v1.0. Không đồng nhất kính với khoang hở trong CAL-02. |
| 6 | Box máy chồng nhau trong ảnh đêm | Chưa có ca đã phân xử để chốt quy tắc riêng; không tự gộp các đối tượng chỉ vì box chồng nhau. |

Các câu chưa chốt phải được xem xét khi QC để xác định câu nào thực sự ảnh hưởng nhãn release; không coi guideline đã hết điểm mơ hồ chỉ vì đã chốt câu 7.

## 2. Ca khó mới (từ lỗi của máy, bẫy lọt, box `can_review`)

| Ảnh | Tình huống | Máy làm gì (model nào) | Quyết định của bạn | Quy tắc mới (vào mục nào của v1.0) |
|---|---|---|---|---|
| `img_002`, `img_007`, `img_013` (CAL-01) | Tuk-tuk/xích lô bị gọi sai lớp | Theo log: YOLO26m gọi thành truck, motorbike hoặc car; SAM không tự sửa lớp của detector | Áp dụng three_wheeler cho các xe trong ca này theo định nghĩa v1.0; cần đối chiếu nhãn cuối | Không thêm quy tắc mới: lỗi máy đối với mục 2 và mục 5 v1.0. |
| `img_013` (CAL-02) | Mask xe dính người và thiếu phần xe | SAM 2.1 tiny còn tô người qua khoang hở và thiếu phần đầu xe/bậc chân | Phần người không thuộc mask xe; giữ mask bản đầu làm bằng chứng lỗi, không khẳng định đã sửa đạt chuẩn | Không thêm quy tắc mới: áp dụng mục 4.2.1 và 4.2.4 v1.0; chưa giải quyết câu hỏi về kính. |

Ảnh mask của CAL-02: [tuktuk_img013.jpg](../masks/tuktuk_img013.jpg). Các ca trên ghi lại cách áp dụng quy tắc hiện hành; không xác nhận mọi lỗi tương ứng trong batch hoặc mask đã được sửa. Bản bổ sung này không trực tiếp thay đổi file nhãn.

## 3. Lịch sử thay đổi

| Phiên bản | Thay đổi | Lý do (ca nào, `case_id`) |
|---|---|---|
| v1.0 | Bản giảng viên | — |
| v1.1 | Chốt câu hỏi 7: không có ngưỡng conf miễn kiểm tra; ghi ảnh ví dụ và các câu 1–6 còn mở. Ghi nhận hai lỗi máy theo quy tắc cũ, không đổi định nghĩa lớp hoặc ngưỡng chấm. | CAL-03; đối chiếu CAL-01 và CAL-02. |
