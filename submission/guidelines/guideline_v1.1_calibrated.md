# Label contract — Phố châu Á — BỔ SUNG v1.1 — 202603296 - Nguyễn Hoàng Thành

> Chỉ ghi phần bổ sung so với v1.0; không thay thế hợp đồng nhãn gốc.

## 1. Câu hỏi mở đã chốt (từ mục 6 của v1.0)

| # | Câu hỏi | Quyết định | Ảnh ví dụ | `case_id` |
|---|---|---|---|---|
| 1 | Xe đạp điện hay xe máy điện? | Xét cấu hình khung và bàn đạp: xe có khung/bàn đạp kiểu xe đạp gán `bicycle`; xe có dáng xe máy/xe tay ga gán `motorbike`. Nếu ảnh không đủ dấu hiệu, chọn lớp có căn cứ nhất và tick `can_review`. | img_028 | CAL-01 |
| 2 | Cụm xe máy dày gần camera | Gán từng xe nếu tách được thân và bánh; chỉ dùng `ignore_region` cho phần không thể tách hoặc vật cao dưới 20 px. | img_018 | CAL-02 |
| 3 | Hàng nhô khỏi xe | Hàng gắn liền thuộc phần xe; hàng rời hoặc nhô quá 20% chiều dài xe không làm box xe rộng thêm. Với mask, bám biên xe; không gộp hàng rời vào xe. | img_008 | CAL-03 |
| 4 | Người ngồi trong xe ba bánh có mui | Gán `person` riêng khi phần nhìn thấy ước tính từ 20% trở lên; dưới ngưỡng thì không gán. Ca sát ngưỡng tick `can_review`; chỉ vẽ phần nhìn thấy. | img_013 | CAL-04 |
| 5 | Kính/cửa sổ xe trong mask | Kính/cửa sổ là phần của mask xe. Người thấy qua kính vẫn là đối tượng riêng với mask `person` trên phần nhìn thấy. | img_023 | CAL-05 |
| 6 | Box xe chồng nhau trong ảnh tối | Không gộp xe. Giữ từng box riêng khi nhận ra từng xe; vùng không thể tách dùng `ignore_region`. | img_024 | CAL-06 |
| 7 | Ngưỡng conf để nhận box | Không đặt ngưỡng chấp nhận tự động. `conf` chỉ ưu tiên thứ tự review; phải kiểm tra bằng mắt mọi box. | img_004 | CAL-07 |

## 2. Ca khó mới (từ lỗi máy, `can_review` và bẫy)

| Ảnh | Tình huống | Máy làm gì / tín hiệu | Quyết định | Quy tắc bổ sung | `case_id` |
|---|---|---|---|---|---|
| img_004 | Một tuk-tuk có box `three_wheeler` và box `car` chồng lên | Box #30 `car` trùng phương tiện đã có box #29 `three_wheeler` | Xóa box `car`, giữ `three_wheeler` | Kiểm tra box trùng và đối chiếu hình dạng xe ba bánh trước khi nhận nhãn máy | CAL-08 |
| img_003 | Box #20 `three_wheeler`, `conf=0.18`, có `can_review` | Điểm thấp và thuộc tính review báo cần xem lại | Không xóa chỉ vì conf thấp; xem hình dáng/kích thước, giữ `can_review` nếu chưa chắc lớp | `can_review` biểu thị bất định về lớp/quy tắc; `conf` chỉ sắp thứ tự xem | CAL-09 |
| img_004, img_026 | Bất đồng giữa số bẫy tự động và ghi nhận của học viên | `review_stats` báo 10/12, học viên ghi đã sửa cả hai ca và coach xác nhận | Dùng 10/12 làm số đo tái lập; đối chiếu `traps_key`, ảnh và export trước khi kết luận khác | Ghi rõ bất đồng và nguồn kiểm chứng; không tự sửa số liệu tự động nếu chưa có đối chiếu độc lập | CAL-10 |

## 3. Lịch sử thay đổi

| Phiên bản | Thay đổi | Lý do (`case_id`) |
|---|---|---|
| v1.0 | Bản giảng viên | — |
| v1.1 | Làm rõ phân lớp xe điện, cụm dày, hàng nhô, người bị che, kính trong mask, box chồng, vai trò `conf`; thêm kiểm tra box trùng và cách đối chiếu bẫy | CAL-01–CAL-10 |
