# Mini guideline - nhóm: Solo (Hồ Minh Hậu)  |  người gán: Hồ Minh Hậu - 2A202602058  |  ngày: 17/9/2026

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

| Tình huống | Luật nhóm bạn chọn | Vì sao | Ảnh mẫu |
| --- | --- | --- | --- |
| Hông của người mặc quần áo dài | Chọn `v = 1` (Occluded), đặt chấm ước lượng tại vị trí giải phẫu hông/đỉnh xương chậu | Hông bị lớp vải che khuất nhưng vẫn nằm hoàn toàn trong khung ảnh; ước lượng theo đường nối vai-gối để không làm mất điểm OKS. | ![Hông mặc quần áo dài](reports/screenshort/anh1.png) |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Chọn `v = 1` (Occluded), đặt chấm theo tọa độ giải phẫu ước lượng của tai | Tai nằm dưới tóc hoặc mũ bảo hiểm nhưng phần đầu vẫn ở trong khung hình; không chọn `v = 0` vì chi tiết không nằm ngoài ranh giới mép ảnh. | ![Tai bị mũ che](reports/screenshort/anh2.png) |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Chọn `v = 0` (Outside) cho các khớp nằm ngoài mép ảnh (gối, mắt cá), **không** đặt chấm | Các khớp chi dưới đã trượt hoàn toàn ra khỏi mép ảnh, không còn pixel nào hiển thị trong khung hình. | ![Người bị cắt mép ảnh](reports/screenshort/anh3.png) |
| Cổ tay nằm sau tay lái / sau thân mình | Chọn `v = 1` (Occluded), đặt chấm ước lượng tại vị trí cổ tay sau tay lái/thân mình | Cổ tay bị che bởi vật thể (tay lái xe máy/thùng hàng/thân người) nhưng vị trí tay vẫn thuộc phạm vi bên trong bức ảnh. | ![Cổ tay sau tay lái](reports/screenshort/anh4.png) |
| Hai người chồng lên nhau | Gán đủ skeleton 17 điểm cho cả 2 người; các khớp bị người trước che chọn `v = 1` | Đảm bảo không bỏ sót skeleton người đứng sau; các khớp bị che lấp được chấm ước lượng theo trục cơ thể thực tế của nhân vật đó. | ![Hai người chồng lên nhau](reports/screenshort/anh5.png) |
| Người nhỏ đến mức nào thì không gán nữa | Chỉ gán người có diện tích người rõ nét (đường kính cơ thể > 20px hoặc nhìn rõ hình thể); không gán người quá nhỏ mờ ở hậu cảnh xa | Tránh gây nhiễu dữ liệu gán nhãn cho mô hình khi các khớp không thể xác định nổi tọa độ giải phẫu. | ![Người nhỏ ở hậu cảnh](reports/screenshort/anh6.png) |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_12.jpg`, người thứ `1` (người đi xe máy), khớp `left_knee` / `left_ankle`

- Mơ hồ ở chỗ nào: Chân và gối của người lái xe bị chồng thùng carton xếp chở phía trước xe che khuất.
- Bạn quyết thế nào: Đặt chấm ước lượng theo trục đùi–cẳng chân và chọn trạng thái `v = 1` (Occluded).
- Vì sao: Người và xe nằm hoàn toàn bên trong khung ảnh, khớp chỉ bị vật thể (thùng carton) che khuất chứ không trượt ra ngoài mép ảnh.
- Nếu người khác quyết ngược lại: Model sẽ học sai rằng người ngồi lái xe chở thùng hàng thì không có khớp gối/mắt cá chân, xoá mất khớp khỏi bảng tính điểm OKS.

### Ca 2 - ảnh `train_13.jpg`, người thứ `2` (người mặc áo phông vàng), khớp `left_shoulder` / `left_knee`

- Mơ hồ ở chỗ nào: Người mặc áo phông vàng đứng phía sau người mặc vest, phần vai và chân bị người mặc vest phía trước che một phần.
- Bạn quyết thế nào: Chọn `v = 1` (Occluded) cho vai và gối bị che khuất, đặt chấm giải phẫu ước lượng dựa theo dáng đứng của áo vàng.
- Vì sao: Nhân vật nằm gọn trong khung hình, phần cơ thể bị che lấp bởi người đứng trước trong cảnh đông người.
- Nếu người khác quyết ngược lại: Model sẽ nhầm lẫn keypoint của người đứng sau kéo sang người đứng trước (xảy ra lỗi nhầm người).

### Ca 3 - ảnh `train_13.jpg`, người thứ `1` (người mặc vest ngậm điếu thuốc), khớp `right_ear` / `right_shoulder`

- Mơ hồ ở chỗ nào: Người đứng góc nghiêng châm thuốc, dễ nhầm lẫn giữa bên trái/bên phải của khung màn hình với bên trái/bên phải giải phẫu.
- Bạn quyết thế nào: Xác định trái/phải theo trục cơ thể giải phẫu của nhân vật (màu xanh = bên trái, màu cam = bên phải trong `vis_train`), chọn `v = 1` cho tai phải bị đầu nghiêng che.
- Vì sao: Đảo trái/phải là lỗi nguy hiểm nhất làm giảm điểm OKS mạnh.
- Nếu người khác quyết ngược lại: Model sẽ bị học ngược chiều xương vai/tay, làm sai lệch toàn bộ cấu trúc pose.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `62%` / họ `Solo - không đối chiếu`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Đã tự quy chuẩn theo guideline: Tai bị tóc/mũ/góc nghiêng che lấp nhưng đầu còn trong ảnh thì chọn `v = 1`.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Tai và các khớp vùng đầu bị tóc/góc nghiêng/mũ bảo hiểm che lấp một phần nhưng toàn bộ đầu vẫn nằm trong khung ảnh: Bắt buộc chọn `v = 1` (Occluded) và đặt chấm theo vị trí giải phẫu ước lượng, không chọn `v = 0` (Outside).
