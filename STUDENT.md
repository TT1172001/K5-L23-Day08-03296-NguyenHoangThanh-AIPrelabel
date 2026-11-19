# Thông tin học viên

- Họ và tên: Nguyễn Hoàng Thành
- MSHV: 202603296
- Lớp (L2 / L3 / khác): L2
- Tài khoản GitHub: TT1172001
- Repo bài nộp (URL): https://github.com/TT1172001/K5-L23-Day08-03296-NguyenHoangThanh-AIPrelabel
- Commit chốt bài (hash): <<>>
- Máy: <<hệ điều hành, GPU (CUDA / MPS / không có)>> windows, AMD Radeon/ NVIDIA GeForce GTX 1650
- CVAT (Docker): <<phiên bản, ví dụ v2.81.0>> 2.81.0

## Kết quả tóm tắt (điền cuối bài; số lấy từ `submission/qc/*`, kèm mẫu số)

- Máy: accepted / edited / deleted: 434 / 19 / 60 (mẫu số: 11.5% | 522 box máy) / added: 42 (mẫu số: 8.5% | 495 đối tượng)
| accepted (giữ nguyên) | 434 | 434/522 = 83.1% | 522 box máy |
| edited (sửa) | 19 | 19/522 = 3.6% | 522 box máy |
| deleted (xoá) | 60 | 60/522 = 11.5% | 522 box máy |
| added (vẽ thêm) | 42 | 42/495 = 8.5% | 495 đối tượng sau duyệt |
- So với gold: precision / recall của **máy** 32.8% / 63.3%
- Bắt bẫy: 10/12 = 83.3% 
- Thời gian: gán tay 5 phút/ảnh; duyệt có pre-label 4.12 phút/ảnh
- Lỗi QC (kiểm tra overlay phụ trợ): 37.5% theo ảnh (3/8), 1.6% theo đối tượng (3/192); cận trên Wilson 95%: 69%.
- **Quyết định:** Rework — 3 lỗi sai lớp trong 3/8 ảnh mẫu, cần xác nhận và sửa annotation rồi QC lại.
