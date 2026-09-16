# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 16.0 khớp có v > 0 mỗi người
- Tổng: v=2 320 | v=1 128 | v=0 28

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 23 | 5 | 0 | 18% |
| 1 | left_eye | 20 | 8 | 0 | 29% |
| 2 | right_eye | 21 | 7 | 0 | 25% |
| 3 | left_ear | 10 | 18 | 0 | 64% |
| 4 | right_ear | 13 | 15 | 0 | 54% |
| 5 | left_shoulder | 25 | 3 | 0 | 11% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 23 | 5 | 0 | 18% |
| 8 | right_elbow | 25 | 3 | 0 | 11% |
| 9 | left_wrist | 19 | 9 | 0 | 32% |
| 10 | right_wrist | 17 | 10 | 1 | 36% |
| 11 | left_hip | 18 | 10 | 0 | 36% |
| 12 | right_hip | 19 | 8 | 1 | 29% |
| 13 | left_knee | 15 | 8 | 5 | 29% |
| 14 | right_knee | 17 | 6 | 5 | 21% |
| 15 | left_ankle | 14 | 6 | 8 | 21% |
| 16 | right_ankle | 14 | 6 | 8 | 21% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
