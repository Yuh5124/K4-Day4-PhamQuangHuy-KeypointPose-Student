# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 16.3 khớp có v > 0 mỗi người
- Tổng: v=2 337 | v=1 103 | v=0 19

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 5 | 0 | 19% |
| 1 | left_eye | 18 | 9 | 0 | 33% |
| 2 | right_eye | 20 | 7 | 0 | 26% |
| 3 | left_ear | 13 | 14 | 0 | 52% |
| 4 | right_ear | 12 | 15 | 0 | 56% |
| 5 | left_shoulder | 26 | 1 | 0 | 4% |
| 6 | right_shoulder | 26 | 1 | 0 | 4% |
| 7 | left_elbow | 24 | 3 | 0 | 11% |
| 8 | right_elbow | 23 | 4 | 0 | 15% |
| 9 | left_wrist | 20 | 7 | 0 | 26% |
| 10 | right_wrist | 21 | 5 | 1 | 19% |
| 11 | left_hip | 21 | 6 | 0 | 22% |
| 12 | right_hip | 20 | 6 | 1 | 22% |
| 13 | left_knee | 19 | 6 | 2 | 22% |
| 14 | right_knee | 21 | 4 | 2 | 15% |
| 15 | left_ankle | 16 | 5 | 6 | 19% |
| 16 | right_ankle | 15 | 5 | 7 | 19% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
