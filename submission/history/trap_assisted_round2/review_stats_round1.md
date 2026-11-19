# Số đo review (det)

- Ảnh tính: 24 (batch,gold); box máy: 505; đối tượng sau duyệt: 484; ngưỡng accepted: IoU >= 0.9
- Đã **loại 25 đối tượng nằm quanh các bẫy** khỏi bảng này: lỗi nhân tạo không phản ánh chất lượng máy.

## 1. Bốn trạng thái (AI error profile)
| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---|---|---|
| accepted (giữ nguyên) | 472 | 472/505 = 93.5% | 505 box máy |
| edited (sửa) | 5 | 5/505 = 1.0% | 505 box máy |
| deleted (xoá) | 28 | 28/505 = 5.5% | 505 box máy |
| added (vẽ thêm) | 7 | 7/484 = 1.4% | 484 đối tượng sau duyệt |

Trong số deleted + added, **3 trường hợp là đổi lớp** (máy gán sai lớp, box đúng chỗ): đó không phải máy bỏ sót. Số vật máy **sót thật** ≈ added − đổi lớp = 4.

### Theo lớp
| Lớp | accepted | edited | deleted | added |
|---|---|---|---|---|
| person | 267 | 0 | 1 | 1 |
| bicycle | 6 | 0 | 2 | 1 |
| motorbike | 139 | 0 | 1 | 1 |
| car | 39 | 0 | 0 | 0 |
| bus | 3 | 0 | 0 | 1 |
| truck | 4 | 0 | 2 | 0 |
| three_wheeler | 14 | 5 | 22 | 3 |

## 2. So với gold (nhãn gán tay từ đầu)
Ảnh gold: 4; đối tượng gold: 101 (mẫu số của recall)
| | TP | FP | FN | Precision | Recall |
|---|---|---|---|---|---|
| Máy (pre-label) | 55 | 18 | 46 | 75.3% (KTC 95%: 64–84%) | 54.5% (KTC 95%: 45–64%) |

*Lạc quan: ngưỡng được chọn trên chính các ảnh gold này (in-sample), và chỉ có 4 ảnh. Đọc khoảng tin cậy, đừng đọc con số đơn.*


(Ảnh gold không nằm trong nhãn sau duyệt: không tính residual / new error. Đó là chủ ý: duyệt lại ảnh bạn vừa tự gán tay sẽ đo trí nhớ, không đo thiên kiến.)

## 3. Ảnh bẫy
- Bắt được 3/12 = 25.0% (KTC 95%: 9–53%) (ngưỡng đạt: 80%). Chỉ 12 bẫy nên khoảng tin cậy rất rộng: đọc cùng danh sách bẫy lọt, đừng kết luận chỉ từ %.
  - loose: 1/3
  - missing: 1/3
  - extra: 0/3
  - wrong_class: 1/3
  - lọt: img_018.jpg (loose), img_027.jpg (missing), img_026.jpg (extra), img_004.jpg (missing), img_021.jpg (extra), img_005.jpg (loose), img_025.jpg (extra), img_009.jpg (wrong_class), img_034.jpg (wrong_class)

## Gợi ý đọc số
- `deleted` cao → ngưỡng thấp hoặc ánh xạ nhãn sai; `added` cao → máy bỏ sót (xem lớp nào);
- `accepted` thấp (< 50%) → sửa mỗi box lâu hơn vẽ mới: đổi model/ngưỡng hoặc vẽ tay;
- bắt bẫy < 80% → duyệt lại lô, siết checklist (quét ảnh TRƯỚC khi nhìn nhãn máy).
