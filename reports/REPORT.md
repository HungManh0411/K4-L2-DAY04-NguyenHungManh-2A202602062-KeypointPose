# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Hùng Mạnh   Nhóm: Solo   Ngày: 16/09/2026

> Báo cáo được điền từ số liệu do các công cụ `check_pose_labels.py`, `visibility_report.py`, `evaluate_pose_annotations.py` và Colab notebook sinh ra.

---

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md và outputs/visibility_report.json sau Chặng 4 -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 330 / 135 / 28 |
| Thời gian trung bình mỗi ảnh | ~4.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. **`left_ear`**: 66% (19/29 khớp bị che)
2. **`right_ear`**: 52% (15/29 khớp bị che)
3. **`left_hip`**: 38% (11/29 khớp bị che)

**Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích:**

*Không hoàn toàn trùng khớp giữa "hay bị che" và "khó xác định vị trí giải phẫu".* 
Tai là khớp có tỷ lệ che khuất cao nhất do thường xuyên bị tóc, nón bảo hiểm hoặc góc nghiêng 3/4 của khuôn mặt che mất, nhưng vị trí giải phẫu của tai tương đối dễ ước lượng nhờ tính chất đối xứng của hộp sọ qua trục mắt - sống mũi. 
Ngược lại, khớp **hông (`hip`)** mới là khớp khó gán nhất trên thực tế vì bị che phủ hoàn toàn bởi trang phục (áo dài, quần, váy) và không có bề mặt nhìn thấy được, đòi hỏi người gán phải suy luận dựa trên nếp gấp thắt lưng và hướng đùi.

---

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một lần sau rework. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.946 | 0.949 |
| OKS@0.50 | 0.966 | 1.000 |
| OKS@0.75 | 0.966 | 1.000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 1 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy:**

- `train_13.jpg`: bổ sung 1 skeleton cho người thứ 3 ở góc ngoài cùng bên trái bị sót ở lần gán đầu tiên.
- `train_04.jpg` + người thứ 2 + `left_wrist`: chỉnh lại toạ độ cổ tay trái bị kéo quá xa sang cơ thể người bên cạnh (đưa về x = 0.505, y = 0.790).
- `train_19.jpg` + người thứ 1 + `right_wrist`: chỉnh lại toạ độ cổ tay phải bị trượt quá cao (đưa về x = 0.586, y = 0.574).

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?**

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh. Các cảnh báo của script ở `train_02` và `train_16` đã được đối chiếu kỹ lưỡng qua `outputs/vis_train/` và xác nhận là do tư thế ngoái đầu và vặn mình tự nhiên của cơ thể người.

---

## 3. Kiểm chéo

Bạn cùng nhóm: Solo (Tự đánh giá đối chiếu qua công cụ và guideline)

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Tỷ lệ v=1 | Nhận xét | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | --- | --- |
| `left_ear` | 19 | 66% | Cao nhất vùng đầu | Guideline: tai sau gáy khi quay nghiêng vẫn tính v=1 |
| `right_ear` | 15 | 52% | Cao nhì vùng đầu | Guideline: tai bị tóc/mũ che vẫn tính v=1 ước lượng |
| `left_hip` | 11 | 38% | Cao nhất vùng thân | Guideline: ước lượng qua nếp gấp thắt lưng |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- *Với tai bị khuất hoàn toàn phía sau đầu khi người quay mặt đi hướng khác, luôn dùng trục đối xứng qua sống mũi để ước lượng vị trí tai và gắn **v = 1**, chỉ dùng **v = 0** khi phần đầu bị cắt khỏi mép ảnh.*

---

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6 -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | *(từ notebook)* | *(từ notebook)* | *(từ notebook)* |
| pose_mAP50-95 | *(từ notebook)* | *(từ notebook)* | *(từ notebook)* |
| pose_precision | *(từ notebook)* | *(từ notebook)* | *(từ notebook)* |
| pose_recall | *(từ notebook)* | *(từ notebook)* | *(từ notebook)* |
| box_mAP50-95 | *(từ notebook)* | *(từ notebook)* | *(từ notebook)* |

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?**
   *(Điền sau khi chạy Chặng 6 trên Colab)*

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?**
   *(Điền sau khi chạy Chặng 6 trên Colab)*

3. **Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):**
   *(Điền sau khi chạy Chặng 6 trên Colab)*

4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**
   *(Điền sau khi chạy Chặng 6 trên Colab)*

5. **Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?**
   *(Điền sau khi chạy Chặng 6 trên Colab)*

---

## 5. Một rule evidence bạn đã dùng

**Ảnh `train_01.jpg`, người thứ 1, khớp `right_knee` (đầu gối phải):**

* **Căn cứ thị giác:** Phần thân trên và đùi của người này nằm trong khung hình, nhưng phần cẳng chân dưới đã chạm sát viền đáy ảnh. Điểm khớp đầu gối phải khi ước lượng giải phẫu nằm ngay trên mép cắt viền dưới và toạ độ ban đầu đo được là $y = 1.006$ (đã vượt qua giới hạn $1.0$ của chiều cao bức ảnh).
* **Quyết định trạng thái:** Do khớp đã lọt ra ngoài mép ảnh, theo đúng quy tắc của bài lab, khớp này không được phép gắn $v=2$ hay $v=1$ với toạ độ ngoài biên, mà bắt buộc phải gắn cờ **$v=0$** (Outside) với toạ độ `(0.000000, 0.000000)`. Điều này giúp tránh làm sai lệch hàm mất mát (loss function) khi model học bounding box và dự đoán keypoint.
