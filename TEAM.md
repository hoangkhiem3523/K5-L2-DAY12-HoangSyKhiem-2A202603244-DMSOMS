# TEAM — Ngày 12

Mã bộ ảnh: **G07** · Hình thức: cá nhân

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| Hoàng Sỹ Khiêm | 2A202603244 | có | lucky (`lucky.zip`, cùng mã G07) | 37 / 37 dòng |

Ca hai bạn vẫn không thống nhất và đã hỏi coach (ghi tên ảnh + đối tượng):

- Đã hỏi và áp dụng cập nhật từ BTC cho ca `child_seat.occupied = yes` ở `S01` (`rear_right`, `rear_middle`) và `S08` (`rear_right`); toàn bộ 37 dòng bất đồng trong `disagreement_log.csv` đã được phân xử theo guideline và cập nhật lại trong `export_final_v2.zip`.

## Cá nhân

### HoangSyKhiem-2A202603244
- Phần mình làm: Tự cài đặt/setup CVAT local (`Day12-G07`), tự gán đủ 22 ảnh cốt lõi (16 ảnh OMS SVIRO + 6 ảnh DMS Drive&Act), chạy kiểm tra checkpoint A/B/Final (`0 lỗi, 0 cảnh báo`), ghi nhận `ir_log.csv` và soạn `class_list_v0.md`.
- Điều mình học được hôm nay: Cách ánh xạ ghế theo xe khi camera nhìn ngược (OMS), gán 13 điểm mặt trên ảnh NIR xoay 90° theo đúng trái/phải của người lái, tách biệt hiện tượng IR mô phỏng (`IR_sim`) với IR thật (`real_IR`), và giữ kỷ luật ghi `unknown` thay vì đoán.
