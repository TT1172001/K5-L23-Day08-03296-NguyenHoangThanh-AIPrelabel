# Báo cáo Day 08 — <<202603296 - Nguyễn Hoàng Thành>>

> Một file duy nhất cho cả bài (đã gộp recipe, QC và ghi chú cá nhân). Mọi con số kèm **mẫu số** và nguồn (file nào). Viết bằng lời của bạn.

## 1. Model và recipe (Part 2–3)

| Lớp | Model + cỡ | Ngưỡng | Vì sao (dẫn `run_id` trong `model_log.csv`) |
|---|---|---|---|
| person / bicycle / motorbike / car / bus / truck | <<YOLO26m>> | <<theo lớp, từ threshold_sweep>> | <<>> |
| three_wheeler | <<YOLOE-26s, prompt: ...>> | <<>> | <<>> |

- Lệnh `merge_prelabels.py` đã dùng (dán nguyên): <<>>
- **Phiên bản SAM bạn chọn** (SAM 1 / 2.1 / 3) và lý do: <<chất lượng mask, tốc độ, RAM, quyền truy cập>>
- Lỗi nào **không sửa được bằng đổi ngưỡng** (ví dụ tuk-tuk bị gọi `car`)? Sửa thế nào? <<>>
- Box **điểm cao mà sai** hoặc **điểm thấp mà đúng** (ảnh nào): <<>>
- Giới hạn: chọn ngưỡng trên chính gold (4 ảnh) nên số đo máy trên gold là lạc quan: <<>>

## 2. AI error profile (từ `qc/review_stats.md`)

| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---|---|---|
| accepted (giữ nguyên) | 434 | 434/522 = 83.1% | 522 box máy |
| edited (sửa) | 19 | 19/522 = 3.6% | 522 box máy |
| deleted (xoá) | 60 | 60/522 = 11.5% | 522 box máy |
| added (vẽ thêm) | 42 | 42/495 = 8.5% | 495 đối tượng sau duyệt |

- Lớp nào máy tệ nhất? Vì sao (dẫn ảnh)? Lớp tệ nhất là three_wheeler, máy gán nhầm vào các quầy hàng và xe tải 23 lần.
- So với gold: precision / recall của **máy** <<>> (mẫu số: <<đối tượng gold>>)

## 3. Thiên kiến tự động hóa và ảnh bẫy

- Bắt bẫy: 10/12; loại nào lọt: img_026.jpg (missing) và img_004.jpg; vì sao (bạn đã bỏ bước nào của checklist)? cả 2 tôi đều không để lọt, về lỗi wrong_class img_004 tôi đã đổi class từ bicycle về đúng person, lỗi missing img_026 tôi đã thêm đúng box bị thiếu. Có lẽ đây là một hạn chế nào đấy của file script, đã trao đổi với lab coach và lab coach đồng ý note vào report.
- Accepted cao mà bắt bẫy thấp (hoặc ngược lại) nói gì về cách bạn duyệt? 
Tôi accepted cao (83,1%) và bắt bẫy thấp (83,3%) có thể cho thấy tôi duyệt không chặt, dễ chấp nhận các annotation của máy. Tuy nhiên, thực tế bắt bẫy của tôi phải là 100% (đã được lab coach confirm), điều đó cho tôi có khả năng nhận diện tốt cả annotation đúng và annotation có vấn đề. 

## 4. Máy có đáng dùng không?

- Thời gian (từ `time_log.csv`): gán tay 5 phút/ảnh vs duyệt 4,12 phút/ảnh. Thiên lệch (bạn gán gold trước nên đã quen cảnh): <<>> . Tuy nhiên, độ phức tạp của ảnh trong task gold thực tế thấp hơn rất nhiều trong task review.
- Kết luận: <<có / không / chỉ cho lớp nào>>, dựa trên <<số liệu>>
- **Ba quy tắc giảm thiên kiến của bạn** (mỗi quy tắc kiểm tra được bằng số hoặc việc làm):
  1. <<>>
  2. <<>>
  3. <<>>

## 5. Mask SAM cho 3 vật (Part 5)

| Ảnh | SAM (phiên bản) | Số click | Mask gồm người lái / hàng hoá? | Lỗi quan sát | File trong `masks/` |
|---|---|---|---|---|---|
| img_013 | SAM 2.1 (sam2.1_b.pt) | 3 (2 dương, 1 âm) | Có người lái | Mask cả vào người lái và người khác không ngồi trên xe nhưng lọt vào khung ảnh | `tuktuk_img013.jpg` |
| img_023 | SAM 2.1 (sam2.1_b.pt) | 3 (2 dương, 1 âm) | Không | Mask thiếu phần thân trên | `seated_tuktuk_person_img023.jpg` |
| img_025 | SAM 2.1 (sam2.1_b.pt) | 3 (2 dương, 1 âm) | Không | Khá tốt, chỉ mask các vùng quan sát được của người, không mask thừa vào các phần khác | `partially_occluded_face_img025.jpg` |



## 6. Calibration (Part 6)

- Số quyết định: 10 (`calibration_decisions.csv`); quan trọng nhất: CAL-08, YOLO26 gán trùng tuk-tuk thành `car`, cần bỏ box trùng và giữ `three_wheeler`; CAL-10 ghi nhận bất đồng giữa thống kê bẫy và xác nhận của học viên/coach.
- Quy tắc mới đã vào `guideline_v1.1_calibrated.md`: phân lớp xe điện theo cấu hình khung/bàn đạp; ngưỡng bỏ qua vật nhỏ và quy tắc cho cụm dày; xử lý hàng nhô, người bị che, mask qua kính, box chồng; `conf` chỉ ưu tiên review; kiểm tra box trùng và đối chiếu bẫy bất đồng. Ví dụ ảnh và mã CAL-01–CAL-10 được ghi trong guideline.
- Nguyên nhân gốc: kết hợp giữa guideline v1.0 còn câu hỏi mở và lỗi mô hình phân lớp/box trùng; CAL-09 cho thấy conf thấp không phải lý do tự động xóa. CAL-10 vẫn cần đối chiếu độc lập với ảnh, `traps_key` và export.

## 7. QC và quyết định (Part 6)

- Mẫu: 8 ảnh (3 ca khó + 5 ngẫu nhiên, seed 42), 192 đối tượng; người kiểm: Codex, xem ảnh overlay có mã đối tượng. Đây là kiểm tra phụ trợ; học viên cần xác nhận trên ảnh gốc và sửa trong CVAT.
- Lỗi theo **đối tượng**: 3/192 = 1.6% · theo **ảnh**: 3/8 = 37.5% (ngưỡng Release ≤ 2%).
- Top lỗi: sai lớp 3 trường hợp — img_004 #30 (`person` trên tuk-tuk), img_008 #105 (`car` trên xe đẩy ba bánh), img_013 #126 (`car` đặt trên xe máy lân cận; cần xác nhận trên ảnh gốc).
- **Giới hạn mẫu nhỏ**: cận trên Wilson 95% là 69% (3 ảnh lỗi/8 ảnh mẫu); mẫu nhỏ không chứng minh được ngưỡng 2%. Các lỗi được nhận diện trên overlay; chưa xác nhận bằng cách mở task và chỉnh annotation.
- Script tổng hợp số liệu từ phiếu, nhưng không tự kiểm box có ôm sát vật thật, không phát hiện vật mà cả người và máy cùng bỏ sót, và không xác nhận quy trình checklist. Ảnh dày xe có nhiều vật nhỏ nên cần kiểm bằng mắt trên ảnh gốc.

**Quyết định: Rework**, ngày 2026-11-19. Lý do: không có vấn đề dữ liệu/guideline chưa chốt để Hold; mẫu có lỗi ở 3/8 ảnh (37.5% > 2%) và lỗi sai lớp lặp lại, nên chưa đạt Release.
Việc cần làm tiếp: học viên xác nhận 3 lỗi trên ảnh gốc, sửa annotation trong CVAT và export `qc_fixed.json`; chạy lại `build_release.py`, lấy mẫu mới bằng `qc_sample.py --n 5 --seed 42 --force`, điền phiếu QC rồi chạy lại `qc_report.py`. Bất đồng bẫy CAL-10 cũng cần đối chiếu độc lập.

## 8. Ghi chú cá nhân

- **Dùng AI để làm bài** (công cụ nào, việc gì, kiểm tra thế nào; "không dùng" nếu không): Không dùng
- Gold có bị nhiễm không (bạn có nhìn pre-label hay hỏi AI trước khi gán xong)? không
- Máy / GPU / CVAT dùng; vướng mắc cài đặt: windows, AMD Radeon/ NVIDIA GeForce GTX 1650, không có vướng mắc
- Dùng cache `data/cache/` thay vì tự chạy? không
- Lỗi / bug script đã gặp (nếu có): không gặp lỗi
- Một điều bạn làm khác lần sau: Học cách dùng AI để làm bài nhanh hơn
