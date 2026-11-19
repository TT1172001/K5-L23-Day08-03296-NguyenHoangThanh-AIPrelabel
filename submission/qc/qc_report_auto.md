# Báo cáo QC tự động (nháp)

> **Gợi ý** do script tính; bạn xác nhận. Script chỉ đếm theo phiếu kiểm bằng mắt người.

## Mẫu
- 8 ảnh (3 ca khó + 5 ngẫu nhiên), 192 đối tượng.
- taxonomy v1.0

## Số đo
| Chỉ số | Giá trị | Mẫu số |
|---|---|---|
| Lỗi **theo đối tượng** | 3/192 = 1.6% | 192 đối tượng trong mẫu |
| **Ảnh có lỗi** (dùng cho ngưỡng) | 3/8 = 37.5% | 8 ảnh trong mẫu |
| Máy: accepted / edited / deleted | 434/522 = 83.1% / 19/522 = 3.6% / 60/522 = 11.5% | 522 box máy |
| Máy: added | 42/495 = 8.5% | 495 đối tượng sau duyệt |
| Bắt lỗi bẫy | 10/12 = 83.3% | 12 bẫy |

## Top 3 loại lỗi
| Loại lỗi | Số lỗi | Ảnh ví dụ |
|---|---|---|
| sai_lop | 3 | img_004.jpg, img_008.jpg, img_013.jpg |

**Lỗi hệ thống**: 'sai_lop' lặp lại -> sửa quy tắc / recipe, không chỉ sửa từng ảnh.

## Giới hạn thống kê
- 3 ảnh lỗi trong 8 ảnh mẫu: cận trên 95% (Wilson) của tỉ lệ ảnh lỗi thật là **69%**. Ngưỡng 2% không thể được **chứng minh** bằng mẫu nhỏ như vậy: hãy nói rõ giới hạn này trong báo cáo.

## Gợi ý quyết định
**REWORK**

- ảnh có lỗi 37.5% > 2%
- lỗi hệ thống: 'sai_lop' chiếm 3/3 lỗi

Cây quyết định: Hold (guideline chưa rõ / dữ liệu hỏng) -> Rework (> ngưỡng, lỗi lặp, bắt bẫy / nhất quán thấp) -> Release.
