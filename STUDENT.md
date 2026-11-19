# Thông tin học viên

- Họ và tên: Phạm Minh Quân
- MSHV: 2A202603307
- Lớp (L2 / L3 / khác): L2B
- Tài khoản GitHub: phamminhquan1101
- Repo bài nộp (URL): [github.com/phamminhquan1101/K5-L2B-DAY08-2A202603307-PhamMinhQuan-AIPrelabel.git](https://github.com/phamminhquan1101/K5-L2B-DAY08-2A202603307-PhamMinhQuan-AIPrelabel.git)
- Commit chốt bài (hash): d1aaff6859dc31b2bdbd52bda425952c7173a959
- Máy: Windows, CPU, RAM 8 GB, không GPU; model chạy local với `--device cpu`, không dùng Colab.
- CVAT (Docker): chạy local

## Kết quả tóm tắt (điền cuối bài; số lấy từ `submission/qc/*`, kèm mẫu số)

- Máy: accepted **472/505 = 93.5%**; edited **5/505 = 1.0%**; deleted **28/505 = 5.5%** (mẫu số: 505 box máy, đã loại vùng bẫy theo script); added **7/484 = 1.4%** (mẫu số: 484 đối tượng sau duyệt trong phạm vi tính). Nguồn: `submission/qc/review_stats.json` và `.md`.
- So với gold: precision của **máy** **55/73 = 75.3%**; recall **55/101 = 54.5%** trên 4 ảnh gold (55 TP, 18 FP, 46 FN). Đây là đánh giá trong mẫu vì chọn ngưỡng trên chính gold.
- Bắt bẫy: vòng đầu **3/12 = 25.0%**; sau sửa  **12/12 = 100.0%**. 
- Thời gian: gán tay **19/4 = 4.75 phút/ảnh**; duyệt có pre-label vòng đầu **40/24 = 1.67 phút/ảnh**, theo `submission/time_log.csv`. Chưa tính đủ thời gian sửa bổ sung và QC.
- Lỗi QC: theo ảnh **5/5 = 100.0%**; theo đối tượng theo quy ước script **9/100 = 9.0%**. Có 5 lỗi vị trí/kích thước, 2 bỏ sót và 2 nhãn thừa. Mẫu gồm 5 ảnh ngẫu nhiên, 0 ảnh bổ sung theo `can_review`, seed 42. QC do **Codex (AI)** thực hiện theo yêu cầu; chưa có lượt xác nhận độc lập của người kiểm khác. Nguồn: `submission/qc/qc_review.csv`, `qc_report_auto.md`, `qc_visual_review.md`.
- **Quyết định hiện tại: Rework** — bản release được kiểm có lỗi ở 5/5 ảnh mẫu, vượt ngưỡng 2%; 5/9 lỗi là lỗi hình học lặp. Các lỗi QC này chưa được sửa trong release, cần khắc phục và kiểm tra lại trước khi cân nhắc Release. Ngày ghi nhận: 2026-11-19.
