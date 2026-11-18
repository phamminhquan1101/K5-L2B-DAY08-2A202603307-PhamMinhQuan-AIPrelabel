# Báo cáo Day 08 — 2A202603307- Phạm Minh Quân

> Một file duy nhất cho cả bài (đã gộp recipe, QC và ghi chú cá nhân). Mọi con số kèm **mẫu số** và nguồn (file nào). Viết bằng lời của bạn.

## 1. Model và recipe (Part 2–3)

| Lớp          | Model + cỡ                                | Ngưỡng           | Căn cứ từ threshold_sweep                                                                                                                        |
| ------------- | ------------------------------------------ | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| person        | YOLO26m                                    | 0.30 (mặc định) | Không ngưỡng nào trong khoảng quét 0.10–0.50 đạt recall 90%; recall cao nhất là 32/47 = 68.1% tại 0.10.                                 |
| bicycle       | YOLO26m                                    | 0.30 (mặc định) | Không đạt mục tiêu recall 90%; recall cao nhất là 6/10 = 60.0% trong khoảng 0.10–0.50.                                                     |
| motorbike     | YOLO26m                                    | 0.30 (mặc định) | Không đạt mục tiêu recall 90%; recall cao nhất là 17/26 = 65.4% tại 0.10 trong khoảng 0.10–0.50.                                          |
| car           | YOLO26m                                    | 0.50               | Ngưỡng cao nhất không vượt 0.50 vẫn đạt recall 9/10 = 90.0%; precision 9/13 = 69.2%.                                                       |
| bus           | YOLO26m                                    | 0.30 (mặc định) | Gold không có bus (0 vật), không đủ dữ liệu để chọn ngưỡng hoặc đánh giá recall của lớp.                                         |
| truck         | YOLO26m                                    | 0.30 (mặc định) | Gold chỉ có 1 vật (< 5), không đủ mẫu để chọn ngưỡng riêng.                                                                            |
| three_wheeler | YOLOE-26s, prompt mặc định từ taxonomy | 0.15               | Ngưỡng cao nhất không vượt 0.50 vẫn đạt recall ít nhất 90%: 7/7 = 100%; precision 7/9 = 77.8%. Tại 0.20, recall giảm còn 6/7 = 85.7%. |

### Kết quả quét ngưỡng trên 4 ảnh gold

Nguồn: `qc/threshold_sweep.csv`, `qc/threshold_sweep.md` và `qc/threshold_sweep.png`. Script đề xuất ngưỡng cao nhất không vượt 0.50 còn đạt recall ít nhất 90%; lớp có dưới 5 vật gold không được chọn ngưỡng riêng. Các lớp không đạt mục tiêu giữ mặc định 0.30 theo cơ chế dự phòng của script, không có nghĩa ngưỡng này đạt recall 90% hoặc là ngưỡng tối ưu.

| Lớp          | Ngưỡng áp dụng | TP | FP | FN | Precision = TP/(TP+FP) | Recall = TP/(TP+FN)             |
| ------------- | ------------------ | -- | -- | -- | ---------------------- | ------------------------------- |
| person        | 0.30               | 25 | 10 | 22 | 25/35 = 71.4%          | 25/47 = 53.2%                   |
| bicycle       | 0.30               | 4  | 1  | 6  | 4/5 = 80.0%            | 4/10 = 40.0%                    |
| motorbike     | 0.30               | 10 | 3  | 16 | 10/13 = 76.9%          | 10/26 = 38.5%                   |
| car           | 0.50               | 9  | 4  | 1  | 9/13 = 69.2%           | 9/10 = 90.0%                    |
| bus           | 0.30               | 0  | 1  | 0  | 0/1 = 0%               | Không xác định: 0 vật gold |
| truck         | 0.30               | 0  | 2  | 1  | 0/2 = 0%               | 0/1 = 0% (mẫu quá ít)        |
| three_wheeler | 0.15               | 7  | 2  | 0  | 7/9 = 77.8%            | 7/7 = 100%                      |

![Precision và recall theo ngưỡng trên gold](qc/threshold_sweep.png)

Biểu đồ thể hiện recall của person, bicycle và motorbike giảm khi tăng ngưỡng. Với car, tăng ngưỡng từ 0.30 lên 0.50 giữ nguyên 9 TP và 1 FN, đồng thời giảm FP từ 6 xuống 4. Với three_wheeler, ngưỡng 0.15 giữ đủ 7 vật gold nhưng vẫn có 2 FP. Đây là các số đo của phép quét trước bước gộp pre-label; số đo sau gộp sẽ được tính riêng ở Part 4.

Chuỗi tham số script đề xuất để dùng ở Part 4:

```text
python scripts/merge_prelabels.py work/prelabel/yoloe_all/pre.coco.json work/prelabel/y26_all/pre.coco.json --thr "car=0.5,three_wheeler=0.15" --conf 0.3 --iou 0.6 --split "batch,gold" --out work/prelabel/merged_review/pre.coco.json
```

- Lệnh `merge_prelabels.py` đã dùng (dán nguyên): <<>>
- **Phiên bản SAM bạn chọn** (SAM 1 / 2.1 / 3) và lý do: <<chất lượng mask, tốc độ, RAM, quyền truy cập>>
- Lỗi nào **không sửa được bằng đổi ngưỡng** (ví dụ tuk-tuk bị gọi `car`)? Sửa thế nào? <<>>
- Box **điểm cao mà sai** hoặc **điểm thấp mà đúng** (ảnh nào): <<>>
- Giới hạn: ngưỡng được chọn và đánh giá trên cùng 4 ảnh gold, nên precision/recall ở ngưỡng đã chọn có thể lạc quan và chưa chứng minh khả năng tổng quát hóa trên batch. Ngưỡng chỉ là ước lượng từ mẫu nhỏ; bus có 0 vật, truck có 1 vật nên chưa đủ cơ sở chọn ngưỡng riêng. Recall 7/7 = 100% của three_wheeler chỉ đúng trên mẫu gold này, không bảo đảm không bỏ sót trên ảnh khác.

## 2. AI error profile (từ `qc/review_stats.md`)

| Trạng thái | Số  | Tỉ lệ | Mẫu số                     |
| ------------ | ---- | ------- | ---------------------------- |
| accepted     | <<>> | <<>>    | <<box máy>>                 |
| edited       | <<>> | <<>>    | <<>>                         |
| deleted      | <<>> | <<>>    | <<>>                         |
| added        | <<>> | <<>>    | <<đối tượng sau duyệt>> |

- Lớp nào máy tệ nhất? Vì sao (dẫn ảnh)? <<>>
- So với gold: precision / recall của **máy** <<>> (mẫu số: <<đối tượng gold>>)

## 3. Thiên kiến tự động hóa và ảnh bẫy

- Bắt bẫy: <<.../12>>; loại nào lọt: <<>>; vì sao (bạn đã bỏ bước nào của checklist)? <<>>
- Accepted cao mà bắt bẫy thấp (hoặc ngược lại) nói gì về cách bạn duyệt? <<>>

## 4. Máy có đáng dùng không?

- Thời gian (từ `time_log.csv`): gán tay <<... phút/ảnh>> vs duyệt <<... phút/ảnh>>. Thiên lệch (bạn gán gold trước nên đã quen cảnh): <<>>
- Kết luận: <<có / không / chỉ cho lớp nào>>, dựa trên <<số liệu>>
- **Ba quy tắc giảm thiên kiến của bạn** (mỗi quy tắc kiểm tra được bằng số hoặc việc làm):
  1. <<>>
  2. <<>>
  3. <<>>

## 5. Mask SAM cho 3 vật (Part 5)

| Ảnh        | SAM (phiên bản) | Số click | Mask gồm người lái / hàng hoá? | Lỗi quan sát | File trong `masks/` |
| ----------- | ----------------- | --------- | ------------------------------------ | -------------- | --------------------- |
| <<img_0xx>> | <<>>              | <<>>      | <<>>                                 | <<>>           | <<>>                  |

- Đối chiếu guideline mục 4.2: quy tắc nào giúp, quy tắc nào thiếu? <<>>
- *(Tuỳ chọn)* Interactor SAM trong CVAT: làm được / không (lỗi gì, đã thử gì); khác gì offline: <<>>

## 6. Calibration (Part 6)

- Số quyết định: <<>> (`calibration_decisions.csv`); quan trọng nhất (dẫn `CAL-xx`): <<>>
- Quy tắc mới đã vào `guideline_v1.1_calibrated.md`: <<>>
- Nguyên nhân gốc: <<guideline mơ hồ / chưa nắm quy tắc / máy>>

## 7. QC và quyết định (Part 6)

- Mẫu: <<n></n>> ảnh (<<m></m>> ảnh `can_review` + <<k></k>> ngẫu nhiên, seed 42); người kiểm: bạn (**hạn chế**: bạn vừa là người duyệt).
- Lỗi theo **đối tượng**: <<.../... = ...%>> · theo **ảnh**: <<.../... = ...%>> (ngưỡng Release ≤ 2%)
- Top lỗi (trong: bỏ sót, thừa, sai lớp, sai vị trí/kích thước, sai thuộc tính, sai mask) + một ảnh ví dụ mỗi loại: <<>>
- **Giới hạn mẫu nhỏ** (cận trên 95% trong `qc_report_auto.md`): <<>>
- Script kiểm được / không kiểm được (ví dụ ôm sát, cùng bỏ sót với máy, theo checklist thật): <<>>

**Quyết định: <<Release / Hold / Rework>>**, ngày <<YYYY-MM-DD></yyyy>>. Lý do (đi theo Hold → Rework → Release): <<>>
Nếu Rework / Hold, việc cần làm tiếp: <<>>

## 8. Ghi chú cá nhân

- **Dùng AI để làm bài** (công cụ nào, việc gì, kiểm tra thế nào; "không dùng" nếu không): <<>>
- Gold có bị nhiễm không (bạn có nhìn pre-label hay hỏi AI trước khi gán xong)? <<không / có, như sau: ...>>
- Máy / GPU / CVAT dùng; vướng mắc cài đặt: <<>>
- Dùng cache `data/cache/` thay vì tự chạy? <<không / có, vì ...>>
- Lỗi / bug script đã gặp (nếu có): <<>>
- Một điều bạn làm khác lần sau: <<>>
