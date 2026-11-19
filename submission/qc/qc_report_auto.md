# Báo cáo QC tự động (nháp)

> **Gợi ý** do script tính; bạn xác nhận. Script chỉ đếm theo phiếu kiểm bằng mắt người.

## Mẫu
- 5 ảnh (0 ca khó + 5 ngẫu nhiên), 100 đối tượng.
- taxonomy v1.0

## Số đo
| Chỉ số | Giá trị | Mẫu số |
|---|---|---|
| Lỗi **theo đối tượng** | 9/100 = 9.0% | 100 đối tượng trong mẫu |
| **Ảnh có lỗi** (dùng cho ngưỡng) | 5/5 = 100.0% | 5 ảnh trong mẫu |
| Máy: accepted / edited / deleted | 472/505 = 93.5% / 5/505 = 1.0% / 28/505 = 5.5% | 505 box máy |
| Máy: added | 7/484 = 1.4% | 484 đối tượng sau duyệt |
| Bắt lỗi bẫy | 12/12 = 100.0% | 12 bẫy |

## Top 3 loại lỗi
| Loại lỗi | Số lỗi | Ảnh ví dụ |
|---|---|---|
| sai_vi_tri_kich_thuoc | 5 | img_006.jpg, img_014.jpg, img_031.jpg |
| bo_sot | 2 | img_003.jpg, img_006.jpg |
| thua | 2 | img_031.jpg, img_035.jpg |

**Lỗi hệ thống**: 'sai_vi_tri_kich_thuoc' lặp lại -> sửa quy tắc / recipe, không chỉ sửa từng ảnh.

## Giới hạn thống kê
- 5 ảnh lỗi trong 5 ảnh mẫu: cận trên 95% (Wilson) của tỉ lệ ảnh lỗi thật là **100%**. Ngưỡng 2% không thể được **chứng minh** bằng mẫu nhỏ như vậy: hãy nói rõ giới hạn này trong báo cáo.

## Gợi ý quyết định
**REWORK**

- ảnh có lỗi 100.0% > 2%
- lỗi hệ thống: 'sai_vi_tri_kich_thuoc' chiếm 5/9 lỗi

Cây quyết định: Hold (guideline chưa rõ / dữ liệu hỏng) -> Rework (> ngưỡng, lỗi lặp, bắt bẫy / nhất quán thấp) -> Release.
