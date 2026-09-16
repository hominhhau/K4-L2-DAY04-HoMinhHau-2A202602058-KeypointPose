# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.03 khớp có v > 0 mỗi người
- Tổng: v=2 331 | v=1 134 | v=0 28

So sánh với `..\ban_cung_nhom\dataset\labels\train` (0 skeleton).
Cột **lệch** là hiệu số phần trăm v=1 - chỗ nào lệch nhiều nhất là chỗ guideline chưa nói rõ.

| # | Khớp | %v=1 (bạn) | %v=1 (đối chiếu) | lệch |
| ---: | --- | ---: | ---: | ---: |
| 3 | left_ear | 62% | 0% | 62 |
| 4 | right_ear | 52% | 0% | 52 |
| 1 | left_eye | 38% | 0% | 38 |
| 2 | right_eye | 38% | 0% | 38 |
| 0 | nose | 31% | 0% | 31 |
| 9 | left_wrist | 31% | 0% | 31 |
| 10 | right_wrist | 31% | 0% | 31 |
| 11 | left_hip | 31% | 0% | 31 |
| 13 | left_knee | 28% | 0% | 28 |
| 14 | right_knee | 24% | 0% | 24 |
| 7 | left_elbow | 21% | 0% | 21 |
| 12 | right_hip | 17% | 0% | 17 |
| 15 | left_ankle | 17% | 0% | 17 |
| 16 | right_ankle | 17% | 0% | 17 |
| 8 | right_elbow | 14% | 0% | 14 |
| 5 | left_shoulder | 7% | 0% | 7 |
| 6 | right_shoulder | 3% | 0% | 3 |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
