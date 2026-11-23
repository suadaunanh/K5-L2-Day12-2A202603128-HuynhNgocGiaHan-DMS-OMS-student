# Tự đánh giá — Huỳnh Ngọc Gia Hân · G01

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S02_rgb | `person` rear_right | `missing` | Có box `person` nhưng không có box `face`. Mục 4.2 yêu cầu người lớn kèm box `face`, trừ khi không thấy mặt (4.5) | Chưa. Review chéo cho thấy Minh có box `face` ở đây; thêm box nếu thấy mặt. Review chéo còn phát hiện S02 bị đảo ghế (mục 3). Sửa cả hai rồi export lại thành export_final_v2.zip |
| S06_ir | `object` bag rear_middle | `box` | Box túi trên IR cao 236 px, trên RGB chỉ 189 px. Có thể box IR lỏng phía dưới, chỗ túi lẫn vào ghế (4.4-5) | Chưa, cần kiểm viền trên ảnh IR |
| D01 | `face` · `eye_right`, `face13` | `ok` | Mắt phải tối nên ghi `unknown`, 4 điểm mắt phải bật cờ `occluded`. Đúng 4.4-1 và 4.6 | Không cần |
| D03 | `face13` | `ok` | Kính phản xạ, 12/13 điểm bật cờ `outside` vì không đoán được vị trí, không đặt điểm cho đủ (4.6) | Không cần |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: ghi `occupied = yes` cho `child_seat`. Checkpoint A báo lỗi này ở S02 (RGB và IR). Đến lần chạy check cuối đầu tiên, lỗi vẫn lặp lại ở S03, S05 (2 box) và S07, tổng cộng 8 lỗi. Mình đã sửa hết trong export_final.zip, và check_final.txt hiện báo 0 lỗi.

Vì sao mình mắc: mình hiểu `occupied` theo nghĩa thường là "ghế đang có đồ hoặc có trẻ" và chọn theo cảm giác, không xem lại bảng 4.1. Bảng đó chỉ cho `no` và `unknown`, vì SVIRO không có trẻ ngồi. Sau Checkpoint A mình chỉ sửa S02 mà không rà lại các ảnh gán tiếp theo. Khi copy shape từ RGB sang IR, lỗi còn bị nhân đôi.

Lần sau tránh bằng cách:
- Trước khi gán một nhãn mới, đọc danh sách giá trị cho phép trong bảng 4.1.
- Sau mỗi lỗi ở checkpoint, tìm cùng nhãn đó trong mọi ảnh đã gán và sửa luôn.
- Chạy `cabin_check --only` sau mỗi 2 cảnh, không đợi đến lần kiểm cuối.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó:

Dòng `S02_rgb.png | person | seat | mine = rear_right | peer = rear_left` (cùng với `infant_seat` và ảnh S02_ir, 4 dòng). Box người của mình nằm bên phải ảnh nhưng mình ghi `rear_right`; ghế em bé bên trái ảnh mình ghi `rear_left`. Camera OMS nhìn về phía sau xe nên trái ảnh là `rear_right` (4.4-3). Nguyễn Nhật Minh gọi đúng theo xe, còn mình gọi theo ảnh.

Mình học được rằng `cabin_check` không bắt được lỗi đảo ghế, vì cả hai giá trị đều hợp lệ. Lỗi này chỉ lộ ra khi review chéo. Các ảnh S## khác mình gọi ghế đúng, nên đây là lỗi lơ đễnh chứ không phải hiểu sai. Lần sau, với mỗi box OMS mình nói to "trái ảnh là rear_right" trước khi chọn `seat`, đúng như bước 2 của 4.3.

Thêm một dòng nữa cần kiểm: ở S02 và S04, Minh có box `face` cho người lớn còn mình không có (4.2). Nếu thấy mặt thì mình cũng sai ở dòng này.
