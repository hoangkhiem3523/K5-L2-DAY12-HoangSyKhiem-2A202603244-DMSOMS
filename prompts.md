# Khai báo dùng AI — Hoàng Sỹ Khiêm

Nhắc lại: **Không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.**

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Antigravity AI Assistant | Hỏi tóm tắt quy tắc Guideline Cabin v1.0 (phân biệt RGB vs IR-sim vs NIR thật, quy tắc `seat`, `gaze_zone`, `hand`, `child_seat` vs `infant_seat`, `light = unknown`) | Dùng làm bảng tra cứu nhanh khi tự gán nhãn 22 ảnh trên CVAT local | Có, đối chiếu trực tiếp với `GUIDE.md` và `COMMON_ERRORS.md` |
| 2 | Antigravity AI Assistant | Nhờ chạy lệnh tải `G07.zip` trực tiếp về máy local, tạo Project `Day12` (từ `labels_cabin_v1.json`) và Task `Day12-G07` (22 ảnh cốt lõi) trên CVAT local, thêm khung tag `frame_meta` mặc định theo tên file | Tiết kiệm thao tác cấu hình môi trường; toàn bộ box/skeleton/thuộc tính trên 22 ảnh do tự tay gán trực tiếp trên CVAT | Có, tự kiểm lại toàn bộ 22 tag `frame_meta` trên giao diện CVAT và xác minh `git status` không dính ảnh |
| 3 | Antigravity AI Assistant | Nhờ cập nhật thêm giá trị `"yes"` vào `child_seat.occupied` trong `scripts/cabin_schema.py` theo thông báo mới của BTC, export `export_final.zip` (không ảnh) và chạy `cabin_check.py` cho Checkpoint A, B và Final | Phát hiện và tự sửa các lỗi thực tế (`seat=driver`, trùng 2 tay `left` ở `D05`, lệch `eye_left`/`outside` ở `D06`) để đạt 0 lỗi, 0 cảnh báo | Có, kiểm tra file `check_cpA.txt`, `check_cpB.txt`, `check_final.txt` và `python -m zipfile -l export_final.zip` |
| 4 | Antigravity AI Assistant | Nhờ góp ý cấu trúc câu chữ cho bảng `class_list_v0.md` (E5) và định dạng mẫu báo cáo | Rà soát, chỉnh sửa theo đúng các ca thực tế đã gặp ở G07 | Có, đọc lại toàn bộ file trước khi nộp |
