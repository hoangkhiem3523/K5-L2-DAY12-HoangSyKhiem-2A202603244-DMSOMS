# Danh sách lớp v0 — Hoàng Sỹ Khiêm · G07

## DMS (7 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `eye_left` / `eye_right` | `open` / `closed` / `unknown` | Phát hiện buồn ngủ (PERCLOS, nhắm mắt kéo dài) và kiểm tra mức độ tỉnh táo của tài xế | Có, trừ khi bị chói phản xạ LED trên tròng kính hoặc mắt bị che khuất |
| 2 | `face13` (13 điểm mặt) | 13 tọa độ keypoint + cờ `occluded`/`outside` | Tính tỷ lệ mở mắt (EAR), mở miệng (ngáp - MAR) và ước lượng tư thế đầu (head pose) chính xác | Có, nhưng điểm ở vùng kính lóa hoặc nửa mặt quay nghiêng phải đánh cờ `occluded`/`outside` |
| 3 | `gaze_zone` | `road` / `mirror_l` / `mirror_r` / `mirror_c` / `cluster` / `center` / `lap` / `other` | Phát hiện tài xế mất tập trung, rời mắt khỏi mặt đường quá ngưỡng thời gian an toàn | Có (dựa vào hướng đầu và đồng tử), nhưng giảm độ chính xác khi mắt bị lóa kính trên NIR |
| 4 | `glasses` | `none` / `clear` / `sun` / `unknown` | Nhận biết điều kiện gây cản trở/nhiễu vùng mắt để hiệu chỉnh ngưỡng độ tin cậy của model nhận diện mắt | Khó phân biệt `clear` và `sun` vì kính râm trên ảnh NIR có thể trở nên bán trong suốt |
| 5 | `hand` (`side`, `on_wheel`) | `left` / `right`; `on_wheel`: `yes` / `no` / `unknown` | Giám sát chế độ cầm lái (hands-on/hands-off detection), cảnh báo khi cần tài xế lấy lại quyền điều khiển | Có, nhận diện tốt qua hình dạng bàn tay và vị trí tương đối so với vô lăng |
| 6 | `phone` (`state`) | `held` / `mounted` / `lap` | Phát hiện hành vi sử dụng điện thoại gây sao nhãng khi lái xe | Có qua hình dạng khung chữ nhật/màn hình sáng, nhưng dễ nhầm với vật tối cầm tay nếu thiếu cạnh rõ |
| 7 | `person.seatbelt` (driver) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Cảnh báo tài xế chưa thắt dây an toàn hoặc thắt sai quy cách (dưới nách/lệch vai) trước và trong khi chạy | Có, nhưng dải dây cùng tông sáng với áo tối hoặc bị tay che sẽ phải ghi `unknown` |

## OMS (6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` (`seat`) | `rear_left` / `rear_middle` / `rear_right` | Đếm số hành khách và định vị ghế có người ngồi để điều khiển túi khí, cảnh báo nhắc dây an toàn | Có, nhận diện rõ hình dáng cơ thể trên từng vị trí ghế |
| 2 | `person.seatbelt` (OMS) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Kiểm tra tình trạng thắt dây an toàn của từng hành khách ngồi hàng ghế sau | Hạn chế ở vùng tối thiếu tương phản; trên ảnh IR-sim không có màu sắc nên khó tách dải dây với áo tối |
| 3 | `face` (OMS) | Bounding box vùng mặt | Xác định có người thật (phân biệt với đồ vật cồng kềnh) và làm đầu vào cho các chức năng tiện ích/an toàn cabin | Có nếu hành khách hướng mặt về camera; không vẽ khi quay mặt đi hoặc bị che |
| 4 | `child_seat` (`seat`, `occupied`) | `seat`; `occupied`: `yes` / `no` / `unknown` | Phát hiện ghế trẻ em và trạng thái có trẻ ngồi để kích hoạt cảnh báo quên trẻ trên xe (CPD) và điều chỉnh túi khí | Có hình dạng khung ghế rõ; trạng thái `occupied` dễ khó phân biệt khi vùng lòng ghế bị tối/che |
| 5 | `infant_seat` (`seat`, `facing`) | `seat`; `facing`: `rear` / `front` | Nhận diện nôi sơ sinh và hướng đặt (quay ngược/xuôi) để vô hiệu hóa túi khí tương ứng, tránh chấn thương cho trẻ | Có qua hình dáng nôi và quai xách, nhận biết được hướng đặt |
| 6 | `object` (`seat`, `type`) | `seat`; `type`: `bag` / `box` / `cup` / `other` | Phân biệt đồ vật để trên ghế (túi, hộp...) với hành khách nhằm tránh báo động giả dây an toàn và cảnh báo bỏ quên đồ | Có qua hình khối, nhưng mất thông tin màu sắc và họa tiết nên khó phân loại chi tiết khi tối |

## 2 câu hỏi leo thang

1. **Câu hỏi:** Guideline v1.0 quy định `child_seat.occupied` chỉ có 2 giá trị `no` / `unknown` và ghi bộ ảnh SVIRO không có trẻ em ngồi, nhưng thực tế trong bộ ảnh vẫn có trẻ ngồi trong ghế trẻ em lẫn nôi sơ sinh (BTC mới bổ sung `occupied = yes` cho `child_seat`). Vậy với `infant_seat` (nôi sơ sinh) khi có trẻ nằm bên trong, Guideline v1.1 có bổ sung thuộc tính `occupied` (`yes`/`no`/`unknown`) tương tự `child_seat` hay tách thêm nhãn `person`?
   * **Khung ví dụ:** `S03_rgb` / `S03_ir` (`infant_seat` ghế `rear_middle`), `S05_rgb` / `S05_ir` (`infant_seat` ghế `rear_right`), và `S01` / `S08` (`child_seat`).
   * **Bạn tạm gán thế nào và vì sao:** Với `child_seat` (`S01`, `S08`), dùng `occupied = yes` theo thông báo BTC. Với `infant_seat` (`S03`, `S05`), không tự ý thêm thuộc tính mới ngoài schema để tránh lỗi `cabin_check`, mà tạm bật cờ `occluded = 1` có sẵn của CVAT để đánh dấu nôi đang có trẻ (ghi nhận đây chỉ là giải pháp tạm vì `occluded` vốn mang nghĩa "bị che khuất", cần thuộc tính `occupied` riêng cho `infant_seat`).

2. **Câu hỏi:** Trên ảnh NIR tài xế bị xoay 90° có đeo kính cận, khi tròng kính hoặc góc nghiêng mặt làm không phân biệt chắc chắn được trạng thái khe mắt (`open` hay `closed`) nhưng vẫn ước lượng được vị trí tương đối của các điểm mắt trên `face13`, thì có bắt buộc phải bật cờ `occluded` cho cả 4 điểm mắt đó hay chỉ cần để `eye_left/right = unknown`?
   * **Khung ví dụ:** `D04.png` (đeo kính cận, `eye_left/right = unknown`), `D06.png` (mắt trái bị khuất góc nghiêng, 4 điểm `left_eye_*` bật `outside=1`).
   * **Bạn tạm gán thế nào và vì sao:** Áp dụng quy tắc §4.4-1 (không thấy rõ thì chọn `unknown`, không đoán `closed`) và đồng bộ cờ điểm mặt trên `face13` (`outside=1` cho `left_eye_*` khi khuất hẳn ở `D06.png`, bật `occluded` khi bị che khuất) để `cabin_check` không báo mâu thuẫn giữa thuộc tính `face` và `face13`.
