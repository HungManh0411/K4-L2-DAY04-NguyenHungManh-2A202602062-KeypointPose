# Mini guideline - nhóm: Solo  |  người gán: Nguyễn Hùng Mạnh  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| **Hông của người mặc quần áo dài** | Căn cứ vào vị trí nếp gấp thắt lưng / đáy xương chậu, ước lượng điểm nối giữa xương đùi và thân mình, gán **v = 1**. *(Tham khảo ảnh mẫu: `outputs/vis_train/train_05.jpg`)* | Quần áo dài che khuất bề mặt da nhưng khung xương chậu là mốc cơ thể cố định; model cần điểm này để suy ra tư thế đứng/ngồi. |
| **Tai bị tóc hoặc mũ bảo hiểm che một phần** | Nếu thấy chân tóc/vành mũ ngay vùng tai $\rightarrow$ ước lượng tâm tai đối xứng với mắt/mũi và gán **v = 1**. Nếu góc nghiêng khuất tai phía sau $\rightarrow$ vẫn nội suy vị trí hộp sọ gán **v = 1**. *(Tham khảo: `outputs/vis_train/train_08.jpg`)* | Tai là mốc xoay của hộp sọ, giữ liên kết với mắt và mũi. |
| **Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên)** | Các khớp lọt ra ngoài viền ảnh (đầu gối, mắt cá chân) $\rightarrow$ gán **v = 0**, toạ độ (0, 0). Các khớp còn trong ảnh gán **v = 2** hoặc **v = 1**. *(Tham khảo: `outputs/vis_train/train_01.jpg`)* | Khớp đã ra khỏi khung hình thì không đoán mò toạ độ ngoài ảnh. |
| **Cổ tay nằm sau tay lái / sau thân mình** | Dựa vào hướng cẳng tay và khuỷu tay để chiếu vị trí bàn tay/cổ tay, gán **v = 1**. *(Tham khảo: `outputs/vis_train/train_03.jpg`)* | Cẳng tay định hướng đường đi của xương tay, dễ dàng nội suy điểm cuối. |
| **Hai người chồng lên nhau** | Gán dứt điểm từng người một. Khớp của người phía sau bị người phía trước che $\rightarrow$ gán **v = 1** theo cấu trúc giải phẫu người sau. *(Tham khảo: `outputs/vis_train/train_07.jpg`)* | Tránh lỗi nghiêm trọng nhất là nối xương của người này sang người kia (`nham_nguoi`). |
| **Người nhỏ đến mức nào thì không gán nữa** | Nếu chiều cao người $< 30$ pixel hoặc không nhận diện được đầu/chân thì không gán skeleton. | Người quá nhỏ không đủ độ phân giải để xác định 17 khớp, gán bừa sẽ làm nhiễu dữ liệu huấn luyện. |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_02.jpg`, người thứ `1`, khớp `left_shoulder, right_shoulder (vai)`

- **Mơ hồ ở chỗ nào:** Người quay đầu nhìn sang hướng khác khiến trục hai mắt ngược chiều với trục hai vai trên ảnh 2D, công cụ tự động cảnh báo nghi ngờ đảo trái/phải (`dao_trai_phai`).
- **Bạn quyết thế nào:** Giữ nguyên quy ước trái/phải theo cơ thể người: vai trái vẫn là vai bên trái của họ, mắt trái là mắt bên trái của họ.
- **Vì sao:** Đây là tư thế vặn người / ngoái đầu tự nhiên của giải phẫu thực tế, không phải lỗi gán nhãn nhầm bên.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Model sẽ học sai định nghĩa vai và cánh tay khi người quay đầu, dẫn đến dự đoán đảo ngược tay trái thành tay phải trong các tư thế xoay người.

### Ca 2 - ảnh `train_16.jpg`, người thứ `1`, khớp `left_hip, right_hip (hông)`

- **Mơ hồ ở chỗ nào:** Thân trên và thân dưới xoay ngược chiều nhau khi vận động, trục hông và vai bị vặn chéo nhau trên mặt phẳng chiếu 2D.
- **Bạn quyết thế nào:** Xác định đúng hông trái/phải dựa vào hướng của chân trụ và khung chậu cơ thể, đánh dấu **v = 1** cho khớp hông bị khuất ở góc nghiêng.
- **Vì sao:** Khớp hông liên kết với đùi và chân, phải tuân theo hướng chân và xương chậu thay vì hướng mặt.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Model sẽ đảo lộn hai chân (chân trái thành chân phải) khi người vặn mình, làm hỏng dự đoán pose dáng khi vận động.

### Ca 3 - ảnh `train_01.jpg`, người thứ `1`, khớp `right_knee (đầu gối phải)`

- **Mơ hồ ở chỗ nào:** Khớp nằm sát mép dưới bức ảnh, toạ độ ban đầu đo được là 1.006 (vượt nhẹ ra ngoài khung ảnh) khi đang để v=2.
- **Bạn quyết thế nào:** Xác định khớp đã chạm mép dưới và một phần nằm ngoài khung hình, sửa lại và gắn cờ **v = 0** (Outside) với toạ độ (0, 0).
- **Vì sao:** Đúng quy tắc: khớp đã ra khỏi khung ảnh thì phải là v=0, không được giữ toạ độ ngoài ảnh với v=2.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Model sẽ học toạ độ ngoài biên bất hợp lý (out-of-bounds coordinate), gây nhiễu loss khi tối ưu bounding box và keypoint.

## 4. Sau khi so visibility report với bạn cùng nhóm (Tự phân tích Solo)

- Khớp có `%v=1` cao nhất: `left_ear` (64%) và `right_ear` (54%), kế tiếp là `left_hip` (36%) và `right_wrist` (36%).
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Là do **đặc thù giải phẫu và trang phục**: Tai thường xuyên bị tóc, nón mũ hoặc góc nghiêng 3/4 của khuôn mặt che khuất nhiều nhất. Hông bị áo dài/quần phủ kín.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Với tai bị khuất hoàn toàn phía sau đầu khi người quay mặt đi hướng khác, luôn dùng trục đối xứng qua sống mũi để ước lượng vị trí tai và gắn **v = 1**, chỉ dùng **v = 0** khi phần đầu bị cắt khỏi mép ảnh.
