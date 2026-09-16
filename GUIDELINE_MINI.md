# Mini guideline - nhóm: T045  |  người gán: Nguyễn Xuân Quang  |  ngày: 16/09/2026

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
| Hông của người mặc quần áo dài | Đặt tại tâm khớp háng theo hướng nối từ vai xuống đùi; nếu không thấy trực tiếp thì vẫn đặt điểm và dùng `Occluded` (`v=1`). | Quần áo che bề mặt nhìn thấy, không làm khớp biến mất; model cần vị trí giải phẫu nhất quán. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Ước lượng vị trí tai từ mắt, sống mũi, đường viền đầu hoặc mũ bảo hiểm; dùng `Occluded` khi tai vẫn nằm trong ảnh. | Tai thường bị che nhưng vị trí tương đối vẫn suy ra được; dùng `Outside` sẽ bỏ mất khớp một cách sai luật. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp thực sự vượt ngoài mép ảnh dùng `Outside` (`v=0`); không đặt tọa độ cho các điểm này. | Không còn bằng chứng hình ảnh cho vị trí của khớp; đây là trường hợp đúng để dùng `v=0`. |
| Cổ tay nằm sau tay lái / sau thân mình | Đặt cổ tay theo hướng cẳng tay và vị trí nắm tay lái/đầu mút tay, rồi dùng `Occluded`. | Cổ tay vẫn ở trong khung và có thể suy ra từ chuỗi vai–khuỷu–cổ tay. |
| Hai người chồng lên nhau | Hoàn tất đủ 17 điểm của một người trước; mỗi điểm phải theo đúng cơ thể người đó, điểm bị người khác che dùng `Occluded`. | Tránh kéo khớp sang người kế bên, đồng thời giữ được đủ skeleton cho từng người. |
| Người nhỏ đến mức nào thì không gán nữa | Với bộ 20 ảnh này vẫn gán mọi người nhìn thấy; không tự đặt ngưỡng bỏ người nhỏ. Nếu không thể suy ra một khớp, dùng cờ phù hợp cho điểm đó. | Dataset đã được chọn để mọi người có thể gán; bỏ người làm giảm độ bao phủ và tạo lỗi thiếu người. |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01.jpg`, người thứ `1`, khớp `left_elbow` và `left_wrist`

- Mơ hồ ở chỗ nào: cánh tay trái bị thân người/vật phía trước che một phần, nên khó thấy tâm khuỷu và cổ tay.
- Bạn quyết thế nào: đặt hai điểm theo hướng nối từ vai qua khuỷu đến bàn tay và dùng `Occluded` (`v=1`).
- Vì sao: hai khớp vẫn ở trong ảnh và hướng của cánh tay cho đủ căn cứ để ước lượng vị trí.
- Nếu người khác quyết ngược lại thì model học sai cái gì: dùng `Outside` sẽ dạy model bỏ qua cổ tay/khuỷu trong các tư thế bị che tương tự.

### Ca 2 - ảnh `train_09.jpg`, người thứ `1`, khớp `left_ear` và `right_ear`

- Mơ hồ ở chỗ nào: mũ bảo hiểm che khuôn mặt và tai; không thấy trực tiếp và người đang ngồi nghiêng trên xe máy.
- Bạn quyết thế nào: ước lượng hai tai theo vị trí mắt, mũ và trục đầu; đánh dấu `Occluded`.
- Vì sao: tai bị che nhưng vẫn nằm trong khung hình; đường viền mũ cung cấp ngữ cảnh để đặt vị trí hợp lý.
- Nếu người khác quyết ngược lại thì model học sai cái gì: bỏ tai hoặc gán `Outside` làm model học thiếu keypoint đầu khi người đội mũ bảo hiểm.

### Ca 3 - ảnh `train_04.jpg`, người thứ `1`, khớp `right_hip`, hai gối và hai cổ chân

- Mơ hồ ở chỗ nào: người ở bên trái bị cắt bởi mép dưới ảnh; `right_hip`, hai gối và hai cổ chân không xuất hiện đầy đủ trong ảnh.
- Bạn quyết thế nào: chỉ giữ các khớp còn có căn cứ trong ảnh; các khớp thực sự ngoài khung được đánh dấu `Outside` (`v=0`).
- Vì sao: vị trí của các khớp nằm ngoài ảnh không thể suy ra đáng tin cậy từ dữ liệu nhìn thấy.
- Nếu người khác quyết ngược lại thì model học sai cái gì: đặt điểm phỏng đoán ngoài ảnh sẽ tạo tọa độ nhiễu và làm sai quy ước `v=0`.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `chưa có dữ liệu kiểm chéo` (bạn `chưa có` / họ `chưa có`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Chưa có nhãn bạn cùng nhóm để kết luận.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Chưa thực hiện kiểm chéo; giữ luật hiện tại cho tới khi có kết quả `visibility_report.py --compare`.
