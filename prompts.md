# Khai báo dùng AI — Huỳnh Ngọc Gia Hân

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ   | Bạn hỏi gì (tóm tắt)                                                                                          | Bạn dùng kết quả thế nào                                               | Bạn có kiểm lại không? Bằng gì?                      |
| - | ----------- | ------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------- | ----------------------------------------------------------- |
| 1 | Claude Code | Đọc repo, liệt kê các file còn thiếu so với SUBMISSION.md, chạy lại `cabin_check` cho export_final.zip | Chạy lại check_final.txt với manifest G01; lập danh sách file cần nộp | Có, đọc lại SUBMISSION.md và kết quả `cabin_check` |
| 10 | Claude Code | Điền disagreement_log.csv: dò vị trí box theo tọa độ trong XML để phân xử ghế S02 (verdict peer, 4.4-3); ghi mục guideline và cách quyết định cho 96 dòng cần xem ảnh, để trống verdict. Cập nhật mục 3 self_review. AI không xem ảnh | Mình và Minh xem ảnh trên CVAT rồi điền verdict theo cột action | Có một phần: ghế S02 kiểm bằng tọa độ box và quy tắc 4.4-3. 96 dòng còn lại chưa phân xử |
| 11 | Claude Code | Kiểm export_final_v2.zip: chạy cabin_check, so với export_final.zip và với bản của Minh; cập nhật disagreement_log, self_review; đóng gói lại zip | Mình tự sửa S02 trên CVAT và export v2; AI chỉ kiểm và cập nhật ghi chú | Có, diff v1 với v2 chỉ có đúng 4 chỗ seat ở S02, check_final_v2 0 lỗi |
