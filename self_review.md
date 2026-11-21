# Tự đánh giá — Hoàng Sỹ Khiêm · G07

## 1. Tự rà soát 4 cảnh

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S02_rgb.png | `person` (ghế `rear_left`) | `seat_flip` | Để sót giá trị mặc định `seat = driver` của CVAT thay vì `rear_left` (bên phải ảnh) | Có |
| S04_ir.png | `child_seat`, `infant_seat` | `seat_flip` | `child_seat` bị sót mặc định `seat = driver`; `infant_seat` bị lệch `facing` (`rear`) so với `S04_rgb` (`front`) | Có |
| D05.png | `hand` (tài xế) | `lr_flip` | Gán trùng cả 2 bàn tay đều là `side = left` thay vì 1 trái (`left`), 1 phải (`right`) | Có |
| D06.png | `face` & `face13` | `flag` | 4 điểm `left_eye_*` đã bật `outside = 1` do khuất góc nghiêng nhưng `face.eye_left` vẫn để `open` | Có |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

* **Lỗi:** Quên sửa các thuộc tính mặc định mà CVAT tự điền sẵn (`seat = driver` ở OMS; `eye_left/right = unknown`, `glasses = unknown`, `gaze_zone = other` ở DMS) và nhầm chiều trái/phải của đối tượng (`hand.side`, `eye_left/right`) trên ảnh `D##` xoay 90°.
* **Vì sao mình mắc:** Khi vẽ nhanh hàng loạt box trước rồi mới quay lại sửa thuộc tính, dễ bỏ sót một số box giữa các cặp ảnh RGB/IR; ngoài ra trên ảnh xoay 90°, phản xạ tự nhiên vẫn nhìn theo trái/phải của màn hình thay vì trái/phải của người lái.
* **Lần sau tránh bằng cách:** Vẽ xong box nào mở ngay bảng **DETAILS** chỉnh đúng thuộc tính trước khi vẽ box tiếp theo; với ảnh xoay 90° luôn tự kiểm tra nguyên tắc tọa độ (`left_eye_*` có `y` lớn hơn `right_eye_*`) và chạy `cabin_check.py` ngay sau từng checkpoint (2–4 ảnh đầu) thay vì đợi làm hết mới kiểm.

## 3. Ghi chú về disagreement_log

* **Một dòng bạn của mình đúng và mình sai:** Ở `S02_rgb.png` / `S02_ir.png` (bỏ sót box `face` trên `person` ở ghế `rear_left`, theo §4.2) và ở `D04.png` (`person.seatbelt` gán nhầm `worn_incorrect` thay vì `worn_correct`, theo §4.7).
* **Mình học được gì từ dòng đó:** Kiểm tra tự động bằng script (`cabin_check`) đạt `0 lỗi, 0 cảnh báo` vẫn không phát hiện được lỗi bỏ sót nhãn (`missing` box `face` ở OMS) hay lỗi nhận định ngữ nghĩa (`worn_incorrect` vs `worn_correct`) — chỉ có lượt review chéo (`diff`) kết hợp đối chiếu lại ảnh gốc theo guideline mới bắt ra được và giúp sửa hoàn chỉnh ở `export_final_v2.zip`.
