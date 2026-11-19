# Ghi nhận kiểm tra mẫu QC có hỗ trợ AI

- Người kiểm tra ghi trong phiếu: **Codex (AI)**, theo yêu cầu của người học. Đây không phải kết quả kiểm tra độc lập của một người thứ hai.
- Phạm vi: 5 ảnh trong `qc_sample.csv` (0 ca khó được đánh dấu, 5 ảnh ngẫu nhiên theo seed 42), tổng 100 đối tượng đã gán. Đã đối chiếu ảnh gốc, ảnh overlay và tọa độ nhãn trong `submission/release/final_det.coco.json`.
- ID trong `qc_review.csv` là ID của **bản release được kiểm tra**, có thể khác ID trong `batch_reviewed.json`. Không dùng ID release để sửa file batch mà chưa khớp lại theo tên ảnh và vị trí box.
- Ghi nhận **ít nhất 9 lỗi rõ**: 2 bỏ sót, 2 nhãn thừa và 5 lỗi vị trí/kích thước. Cả 5 ảnh có ít nhất một lỗi. Tọa độ trong ghi chú mô tả vùng để tìm lỗi, không phải tọa độ box mới đã được gán chính xác.
- Áp dụng quy tắc v1.0: box chỉ bao phần nhìn thấy; mỗi người/xe là đối tượng riêng; mui gắn liền thuộc xe; đồ vật trưng bày không phải person. Không cần tạo quy tắc mới để xác định các lỗi đã ghi.
- Lỗi mui và thân xích lô ở img_035 được ghi thành 2 vấn đề trên 2 box: box xe thiếu phần mui (#477) và box mui rời thừa (#478). Đây là cùng một xe thật; cần đọc tỉ lệ lỗi theo quy ước đếm của script, không coi 2 box là 2 xe thật.
- Đây là kiểm tra thị giác, không phải bộ gold mới hay phép chứng minh không còn lỗi khác. Chưa đo dung sai biên khoảng 2 px trên tất cả 100 box; vùng đông, bị che và đối tượng xa có thể còn lỗi chưa phát hiện.
- Không sửa `batch_reviewed.json`, `qc_fixed.json` hoặc nhãn release trong lượt điền phiếu này. Số đo QC phản ánh bản release trước khi khắc phục các lỗi vừa ghi.
- `qc_report_auto.md` do script sinh từ phiếu này. Cần đọc kết quả cùng nguồn người kiểm tra là AI và giới hạn mẫu nhỏ; kết quả bẫy 12/12 trước đó là sau khi đã xem đáp án, không phải phép đo bắt bẫy độc lập.
