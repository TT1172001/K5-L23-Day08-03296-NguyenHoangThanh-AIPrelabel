# Số đo review (det)

- Ảnh tính: 24 (batch,gold); box máy: 522; đối tượng sau duyệt: 495; ngưỡng accepted: IoU >= 0.9
- Đã **loại 18 đối tượng nằm quanh các bẫy** khỏi bảng này: lỗi nhân tạo không phản ánh chất lượng máy.

## 1. Bốn trạng thái (AI error profile)
| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---|---|---|
| accepted (giữ nguyên) | 434 | 434/522 = 83.1% | 522 box máy |
| edited (sửa) | 19 | 19/522 = 3.6% | 522 box máy |
| deleted (xoá) | 60 | 60/522 = 11.5% | 522 box máy |
| added (vẽ thêm) | 42 | 42/495 = 8.5% | 495 đối tượng sau duyệt |

Trong số deleted + added, **11 trường hợp là đổi lớp** (máy gán sai lớp, box đúng chỗ): đó không phải máy bỏ sót. Số vật máy **sót thật** ≈ added − đổi lớp = 31.

### Theo lớp
| Lớp | accepted | edited | deleted | added |
|---|---|---|---|---|
| person | 244 | 7 | 16 | 14 |
| bicycle | 4 | 0 | 2 | 3 |
| motorbike | 121 | 7 | 8 | 13 |
| car | 45 | 2 | 6 | 3 |
| bus | 2 | 0 | 1 | 3 |
| truck | 2 | 0 | 4 | 3 |
| three_wheeler | 16 | 3 | 23 | 3 |

## 2. So với gold (nhãn gán tay từ đầu)
Ảnh gold: 4; đối tượng gold: 30 (mẫu số của recall)
| | TP | FP | FN | Precision | Recall |
|---|---|---|---|---|---|
| Máy (pre-label) | 19 | 39 | 11 | 32.8% (KTC 95%: 22–46%) | 63.3% (KTC 95%: 46–78%) |

*Lạc quan: ngưỡng được chọn trên chính các ảnh gold này (in-sample), và chỉ có 4 ảnh. Đọc khoảng tin cậy, đừng đọc con số đơn.*


(Ảnh gold không nằm trong nhãn sau duyệt: không tính residual / new error. Đó là chủ ý: duyệt lại ảnh bạn vừa tự gán tay sẽ đo trí nhớ, không đo thiên kiến.)

## 3. Ảnh bẫy
- Bắt được 10/12 = 83.3% (KTC 95%: 55–95%) (ngưỡng đạt: 80%). Chỉ 12 bẫy nên khoảng tin cậy rất rộng: đọc cùng danh sách bẫy lọt, đừng kết luận chỉ từ %.
  - extra: 3/3
  - loose: 3/3
  - missing: 2/3
  - wrong_class: 2/3
  - lọt: img_026.jpg (missing), img_004.jpg (wrong_class)

## Gợi ý đọc số
- `deleted` cao → ngưỡng thấp hoặc ánh xạ nhãn sai; `added` cao → máy bỏ sót (xem lớp nào);
- `accepted` thấp (< 50%) → sửa mỗi box lâu hơn vẽ mới: đổi model/ngưỡng hoặc vẽ tay;
- bắt bẫy < 80% → duyệt lại lô, siết checklist (quét ảnh TRƯỚC khi nhìn nhãn máy).
