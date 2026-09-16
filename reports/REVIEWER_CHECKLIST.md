# Reviewer checklist - điền khi kiểm bài người khác (Solo Review)

Người gán: Nguyễn Hùng Mạnh   Người kiểm: Nguyễn Hùng Mạnh    Ngày: 16/09/2026

Chạy trước khi soi bằng mắt (thực hiện tự kiểm tra trên bài của chính mình):

```bash
python tools/check_pose_labels.py --images dataset/images/train --labels dataset/labels/train
python tools/visualize_pose.py --images dataset/images/train --labels dataset/labels/train --out outputs/vis_train
python tools/visibility_report.py --labels dataset/labels/train --out outputs/visibility_report.json --markdown reports/visibility_report.md
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | :---: | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | Đạt | Đạt: 28/28 skeleton đều giữ trọn vẹn cấu trúc 17 điểm (các khớp bị cắt ngoài mép ảnh được gắn cờ v=0 theo đúng quy định, không bị xoá điểm) |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | Đạt | Đã soi kỹ qua vis_train: train_02 và train_16 người quay đầu/vặn mình nên mắt và vai ngược chiều tự nhiên, không đảo trái/phải |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | Đạt | Không có lỗi nhầm người, mỗi người có khung xương riêng biệt |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | Đạt | 128 khớp v=1 (27%), đều được chấm ước lượng giải phẫu đúng vị trí |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | Đạt | 28 khớp v=0 (chủ yếu là mắt cá chân và đầu gối ở ảnh bị cắt mép dưới) |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | Đạt | 320 khớp v=2 đều nằm đúng khớp nhìn thấy rõ trên cơ thể |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | Đạt | Đạt chuẩn COCO Keypoints 1.0 từ CVAT |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | Đạt | Đã convert qua `coco_kp_to_yolo_pose.py`, đủ 56 số/dòng |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | Đạt | Đã xuất `reports/visibility_report.md` và `outputs/visibility_report.json` |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | Đạt | Đã ghi nhận các ca khó (quay đầu, người bị cắt mép, che khuất) |
| 11 | `check_pose_labels.py` chạy 0 lỗi | Đạt | Exit code 0, ĐẠT định dạng (0 lỗi, các warning đã kiểm chứng) |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_01.txt` | 1 | `right_knee` | Toạ độ ra ngoài mép ảnh (1.006) khi để v=2 | Đã sửa lại và gắn v=0 chuẩn xác |
| `train_02.txt` | 1 | `head/body` | Cảnh báo ngược chiều mắt - vai do quay đầu | Giữ nguyên vì đúng giải phẫu thực tế người đang ngoái đầu |
| `train_16.txt` | 1 | `head/body` | Cảnh báo ngược chiều mắt - hông do vặn mình | Giữ nguyên vì đúng tư thế vặn người thực tế |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: Nhầm lẫn giữa tư thế quay đầu/vặn mình với lỗi đảo trái-phải, và phân vân giữa việc ước lượng khớp khuất ($v=1$) với việc để ngoài khung ($v=0$).
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**? Chủ yếu là do **guideline cần làm rõ**: cần quy định cụ thể cách xử lý khi người quay đầu và tiêu chuẩn ước lượng vị trí giải phẫu cho khớp bị che khuất.
