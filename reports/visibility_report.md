# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 13.62 khớp có v > 0 mỗi người
- Tổng: v=2 336 | v=1 59 | v=0 98

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 0 | 8 | 0% |
| 1 | left_eye | 20 | 1 | 8 | 3% |
| 2 | right_eye | 20 | 1 | 8 | 3% |
| 3 | left_ear | 16 | 8 | 5 | 28% |
| 4 | right_ear | 16 | 6 | 7 | 21% |
| 5 | left_shoulder | 26 | 2 | 1 | 7% |
| 6 | right_shoulder | 27 | 2 | 0 | 7% |
| 7 | left_elbow | 23 | 3 | 3 | 10% |
| 8 | right_elbow | 25 | 1 | 3 | 3% |
| 9 | left_wrist | 17 | 7 | 5 | 24% |
| 10 | right_wrist | 20 | 6 | 3 | 21% |
| 11 | left_hip | 23 | 6 | 0 | 21% |
| 12 | right_hip | 25 | 4 | 0 | 14% |
| 13 | left_knee | 13 | 4 | 12 | 14% |
| 14 | right_knee | 16 | 4 | 9 | 14% |
| 15 | left_ankle | 13 | 2 | 14 | 7% |
| 16 | right_ankle | 15 | 2 | 12 | 7% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
