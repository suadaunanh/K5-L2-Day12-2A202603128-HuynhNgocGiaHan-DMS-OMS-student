# Danh sách lớp v0 — Huỳnh Ngọc Gia Hân · G01

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face` · `eye_left`, `eye_right` | open / closed / unknown, gán riêng từng mắt | Phát hiện buồn ngủ: đếm số khung mắt nhắm liên tiếp (kiểu PERCLOS) để cảnh báo | Có khi thấy mí. Không khi kính phản xạ đèn NIR (D02, D03) hoặc mắt khuất do quay đầu (D04) |
| 2 | `face` · `glasses` | none / clear / sun / unknown | Biết khi nào trạng thái mắt kém tin cậy để hạ độ tin cậy cảnh báo, thay vì báo nhầm "mắt nhắm" | Một phần. NIR xuyên qua kính râm nên kính râm có thể trông như kính trong; vùng mắt tối thì ghi unknown (D01) |
| 3 | `face` · `gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát hiện mất tập trung: nhìn khỏi đường quá lâu, nhìn xuống đùi; kiểm tra tài xế có nhìn gương trước khi đổi làn | Có theo hướng đầu (D04 nhìn gương giữa). Kém khi không thấy mắt, phải ghi other |
| 4 | `face13` (13 điểm mặt, cờ occluded / outside) | 13 điểm + cờ cho từng điểm | Ước lượng hướng đầu, độ mở mắt và miệng (ngáp); cờ che khuất dạy model không tin điểm bị che | Có khi mặt rộng ít nhất 60 px. Điểm dưới kính phản xạ thường phải bật cờ (D03) |
| 5 | `hand` · `side`, `on_wheel` | left / right theo người · yes / no / unknown | Kiểm tra tay trên vô lăng (hands-on) khi xe cần tài xế lấy lại lái | Có với tay gần đèn. Tay ở vùng tối xa đèn thì unknown (D04) |
| 6 | `phone` · `state` | held / mounted / lap | Phát hiện dùng điện thoại khi lái, phân biệt cầm tay (nguy hiểm) với đặt trên giá | Có theo hình dạng (D05). Màn hình sáng không đáng tin trên NIR, chỉ vẽ khi thấy cạnh hoặc góc máy |
| 7 | `person` (driver) · `seatbelt` | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây, phát hiện thắt sai (dưới nách, sau lưng) | Một phần. Dây có thể trùng độ sáng với áo (D06) |
| 8 | tag `frame_meta` · `camera_blocked` | yes / no | Phát hiện camera bị che để hệ thống báo "DMS không hoạt động" thay vì đưa kết quả sai | Có |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` · `seat` | driver / front_passenger / rear_left / rear_middle / rear_right, gọi theo xe | Đếm người theo ghế; bật nhắc dây an toàn và cấu hình túi khí theo ghế có người | Có theo hình dạng (S02, S04) |
| 2 | `person` · `seatbelt` (hàng sau) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây cho từng ghế sau, nơi cảm biến khóa dây thường không có | Một phần. Mất màu dây nhưng vẫn thấy không có dây (S02_ir) |
| 3 | `child_seat` · `occupied` | no / unknown (không có yes) | Biết ghế có lắp ghế trẻ em để tắt túi khí ghế đó và nhắc kiểm tra trẻ khi khóa xe | Có theo hình dạng; viền dễ lẫn vào ghế xe (S06_ir) |
| 4 | `infant_seat` · `facing` | rear / front | Ghế em bé quay về sau ở ghế trước bắt buộc tắt túi khí; nhắc quên trẻ trong xe | Có theo hình dạng tay cầm và đáy ghế (S03, S05) |
| 5 | `object` · `type`, `seat` | bag / box / cup / other | Nhắc quên đồ khi rời xe; phân biệt vật với người để không báo nhầm có người hoặc trẻ | Có với vật lớn. Vật nhỏ như cốc mất viền, box nhỏ hơn RGB (S01_ir) |
| 6 | `face` (box, không thuộc tính) | có / không | Xác nhận ghế có người thật, không phải túi hay ghế trẻ em; làm điểm bắt đầu cho các tính năng theo từng hành khách | Có khi người quay về camera |
| 7 | Ghế trống (không vẽ box, đếm trong ghi chú) | số ghế trống | Suy ra trạng thái đầy đủ của cabin; ghế không có box nào phải chắc chắn là trống | Có |
| 8 | tag `frame_meta` · `modality`, `light` | rgb / ir · day / night / unknown | Cho model biết loại ảnh và điều kiện sáng để chọn ngưỡng và đánh giá lỗi theo từng điều kiện | Không áp dụng (thông tin của khung hình) |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: `gaze_zone` không có giá trị `unknown`, còn `other` vừa nghĩa là "nhìn vùng khác ngoài danh sách", vừa nghĩa là "không chắc". Khi cả hai mắt đều `unknown` vì kính phản xạ, có nên thêm `gaze_zone = unknown` để model không học nhầm rằng nhìn vùng khác giống với không thấy mắt không?
   Khung ví dụ: G01 D02 và G01 D03 (cả hai mắt `unknown`, kính `clear`).
   Bạn tạm gán thế nào và vì sao: `gaze_zone = other` và ghi lý do vào ghi chú, đúng theo mục 4.5. Mình không đoán vùng nhìn từ hướng đầu vì quy tắc 4.4-1 nói không thấy thì không đoán.

2. Câu hỏi: `infant_seat · facing` chỉ có `rear` và `front`, không có `unknown`. Nếu trên ảnh IR-sim không nhìn rõ tay cầm hay đáy ghế để biết hướng thì phải ghi gì? Quy tắc 4.4-1 bảo "không thấy thì unknown", nhưng thuộc tính này lại bắt buộc chọn một hướng.
   Khung ví dụ: G01 S05_ir (ghế em bé ở rear_right), so với G01 S03_ir (ghế em bé ở rear_left).
   Bạn tạm gán thế nào và vì sao: `facing = front` theo hình dạng thấy trên chính ảnh IR (quy tắc 4.4-4), không chép từ ảnh RGB. Mình ghi ca này vào ghi chú ca khó để coach xem. Hướng ghế quyết định có tắt túi khí hay không, nên đoán sai ở đây nguy hiểm hơn ghi `unknown`.
