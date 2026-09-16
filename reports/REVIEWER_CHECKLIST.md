# Reviewer checklist - điền khi kiểm bài người khác

Người gán: Hồ Minh Hậu - 2A202602058   Người kiểm: Hồ Minh Hậu (Tự kiểm tra / Solo) - 2A202602058   Ngày: 17/9/2026

Chạy trước khi soi bằng mắt:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels dataset/labels/train
python3 tools/visualize_pose.py --images dataset/images/train --labels dataset/labels/train --out outputs/vis_train
python3 tools/visibility_report.py --labels dataset/labels/train
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | :---: | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | Đã bổ sung các khớp thiếu ở lượt rework (`train_13.jpg`, `train_14.jpg`) |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ | Đã sửa lỗi đảo trái/phải ở `train_13.jpg` (người #1) |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | Đã kéo khớp `right_wrist` ở `train_01.jpg` (người #2) về đúng người |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☑ | Đã gán `v = 1` kèm chấm ước lượng ở `train_12.jpg`, `train_13.jpg`, `train_14.jpg` |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ | Đã rà soát toàn bộ 20 ảnh core |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | Chỉ dùng `v = 2` cho khớp nhìn thấy rõ nét |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | Đã kiểm tra qua `check_pose_labels.py` |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | Đã xác nhận cấu trúc nhãn txt 56 cột (box + 17x3 kpts) |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ | Đã tạo `outputs/visibility_report.json` & `reports/visibility_report.md` |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ | Đã ghi rõ 3 ca mơ hồ cụ thể trong `GUIDELINE_MINI.md` |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | Chạy thành công 0 lỗi |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_13.jpg` | 1 | Các cặp khớp trái/phải | Đảo trái/phải toàn bộ skeleton người mặc vest | Đổi lại vị trí các cặp khớp theo trục giải phẫu cơ thể nhân vật (`right_ear`, `left_shoulder`, `right_elbow`, `right_wrist`) |
| `train_01.jpg` | 2 | `right_wrist` | Nhầm người (kéo cổ tay sang cơ thể người bên cạnh) | Kéo chấm gán keypoint `right_wrist` về đúng vị trí cổ tay người #2 |
| `train_13.jpg` | 2 | `left_knee`, `nose`, `left_eye` | Xóa nhầm khớp bị che (`v=0`) và thiếu điểm keypoint | Đặt lại chấm ước lượng, chọn cờ `v=1` (Occluded) cho gối bị người mặc vest che khuất và bổ sung các khớp thiếu |
| `train_12.jpg` | 1 | `left_knee`, `left_ankle` | Xóa nhầm khớp bị che (`v=0`) do bị thùng carton chở trên xe máy che | Đặt chấm ước lượng giải phẫu và chuyển cờ từ `v=0` sang `v=1` (Occluded) |
| `train_14.jpg` | 2 | `right_ankle`, `left_ankle` | Xóa nhầm khớp bị che (`v=0`) và thiếu điểm `left_ankle` | Chuyển sang `v=1` và bổ sung điểm `left_ankle` ước lượng |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: Xóa nhầm các khớp bị vật thể/người khác che khuất (để `v=0` thay vì `v=1` kèm chấm ước lượng) và nhìn theo hướng màn hình dẫn đến đảo trái/phải giải phẫu.
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**?: Đây là lỗi thao tác trong quá trình gán nhãn nhanh trên CVAT; sau khi đối chiếu với công cụ `visualize_pose.py` và quy chuẩn guideline, các lỗi đã được sửa triệt để ở lượt rework.
