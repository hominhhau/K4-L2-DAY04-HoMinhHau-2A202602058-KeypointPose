# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Hồ Minh Hậu-2A202602058   Ngày: 17/9/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 331 / 134 / 28 |
| Thời gian trung bình mỗi ảnh | 4 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` — 62%
2. `right_ear` — 52%
3. `left_eye` — 38% (cùng mức `right_eye` 38%, `left_wrist` 31%, `right_wrist` 31%, `left_hip` 31%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng một phần. Tai (`left_ear`, `right_ear`) và mắt (`left_eye`, `right_eye`) có tỉ lệ %v=1 cao nhất vì trong các bức ảnh người đứng nghiêng hoặc quay lưng, góc nghiêng và tóc hay che lấp vùng tai và mắt nhưng phần đầu vẫn thuộc phạm vi ảnh. Khó khăn ở các khớp này là việc phân biệt giữa việc chọn `v=1` (đặt chấm ước lượng khi bị che) hay `v=2` khi tóc che nhẹ. Trong khi đó, các khớp hông (`left_hip`, `right_hip`) khó hơn ở khâu ước lượng tọa độ giải phẫu bên dưới lớp trang phục.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.842 | 0.949 |
| OKS@0.50 | 0.931 | 1.000 |
| OKS@0.75 | 0.897 | 1.000 |
| Lỗi `dao_trai_phai` | 1 | 0 |
| Lỗi `nham_nguoi` | 1 | 0 |
| Lỗi `xoa_khop_bi_che` | 11 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_13.jpg`, người #1 (người mặc vest thắt cà vạt ngậm điếu thuốc phía trước): Cả skeleton bị đảo trái/phải — đổi lại toàn bộ các cặp khớp trái/phải (`right_ear`, `left_shoulder`, `right_elbow`, `right_wrist`) và bổ sung lại các khớp bị thiếu.
- `train_01.jpg`, người #2: Khớp `right_wrist` bị kéo nhầm sang cơ thể người bên cạnh — kéo chấm gán về đúng vị trí cổ tay người #2.
- `train_13.jpg`, người #2 (người mặc áo phông vàng đứng phía sau): Khớp `left_knee` trước đó bị xoá nhầm (để `v=0`) — đặt lại chấm ước lượng và chọn cờ `v=1` (Occluded) do bị người mặc vest che khuất; bổ sung các khớp bị thiếu `nose`, `left_eye`, `right_eye`, `right_ear`.
- `train_12.jpg`, người #1 (người đi xe máy): Khớp `left_knee` và `left_ankle` bị xoá nhầm (để `v=0`) do bị chồng thùng carton chở trên xe máy che khuất — chuyển từ `v=0` sang `v=1` và đặt chấm giải phẫu ước lượng.
- `train_14.jpg`, người #2: Khớp `right_ankle` bị xoá nhầm (`v=0`) — chuyển sang `v=1` và bổ sung khớp `left_ankle`.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Lỗi đảo trái/phải xảy ra ở ảnh `train_13.jpg`, người #1 (người đàn ông mặc vest thắt cà vạt ngậm điếu thuốc ở phía trước). Đây là bức ảnh đông người đứng đè lên nhau và nhân vật chính đứng hơi nghiêng người cầm bao thuốc/túi xách. Khi thao tác nhanh, tôi đã nhìn theo hướng màn hình (bên trái/phải của ảnh) thay vì xác định theo trục cơ thể thực tế của nhân vật (bên trái/phải giải phẫu của người đó). Sau khi dùng công cụ `visualize_pose.py` kiểm tra đường nối cờ xanh (trái) và cam (phải), tôi đã sửa lại chính xác cặp trái/phải và đưa số lỗi đảo trái/phải về 0 ở lượt chạy sau rework.

## 3. Kiểm chéo

Bạn cùng nhóm: Làm solo (không có bạn cùng nhóm)

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| - | - | - | - | Làm solo |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Không áp dụng (thực hiện bài lab độc lập).

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.845 | 0.845 | 0.0 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   `pose_mAP50-95` tăng 0.0055 (từ 0.6853 lên 0.6908). Việc fine-tune trên 20 ảnh đã giúp model học thêm quy chuẩn đánh cờ visibility `v=1` nhất quán đối với các khớp bị che lấp theo chuẩn bài lab. Mức tăng nhẹ chứng minh tập dữ liệu 20 ảnh được gán chính xác, giúp mô hình cải thiện độ chính xác tọa độ khớp mà không làm hỏng tri thức gốc của mô hình COCO baseline.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?

   Sau fine-tune, `box_mAP50-95` là 0.8041 trong khi `pose_mAP50-95` là 0.6908, chênh lệch 0.1133. Mô hình tìm *người* (box) dễ hơn tìm *khớp* (pose). Nguyên nhân là vì ô chữ nhật bounding box chỉ cần bao quanh toàn bộ cơ thể dựa trên các đặc trưng diện rộng nổi bật, còn 17 keypoint đòi hỏi độ chính xác tuyệt đối ở cấp độ pixel cho từng điểm giải phẫu và dễ bị ảnh hưởng bởi góc che, tự che khuất hoặc đảo trái/phải.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   Ở các ảnh test có mật độ người đông và đứng sát nhau (ví dụ ca 2 người đè lên nhau), mô hình mắc lỗi **Nhầm người**: keypoint khuỷu tay và cổ tay của người phía trước bị kéo sang cơ thể của người đứng ngay sau do khoảng cách giữa hai nhân vật quá nhỏ.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Ảnh `train_12.jpg` (người đàn ông đi xe máy chở chồng thùng carton) có OKS giữa nhãn và mô hình thấp nhất (OKS 0.723 trước rework). Trong ảnh này, nhãn của tôi đúng. Người #1 lái xe máy có phần gối và mắt cá chân bị chồng thùng carton xếp trước xe che lấp. Tôi đã tuân thủ quy tắc đặt `v=1` (đường đứt nét) và ước lượng tọa độ khớp theo trục đùi–cẳng chân, trong khi mô hình bị trượt keypoint do vật che lấp ở phần chân.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?

   Có, ở các ảnh như `train_13.jpg` (người mặc vest đứng trước người mặc áo vàng) và `train_12.jpg` (người đi xe máy chở thùng carton), cả điểm gán ban đầu của tôi (trước rework) và kết quả đoán của mô hình đều đạt OKS thấp nhất (OKS ở `train_13.jpg` lần 1 chỉ 0.451 và 0.494). Điều này cho thấy đây là những bức ảnh chứa trường hợp khó (edge cases): nhiều người đứng chồng lấp đè lên nhau ở các khoảng cách khác nhau (người mặc áo phông vàng bị người mặc vest che khuất một phần ngực/vai), tư thế quay nghiêng khuất chi và nhiều khớp bị che lấp nặng, khiến cả con người lẫn thuật toán máy học đều dễ gặp sai sót nếu không phân tích kỹ lưỡng.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người, khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

Trong ảnh `train_12.jpg`, người #1 (người đàn ông đi xe máy), tôi phải quyết định trạng thái cho cặp khớp chân gồm gối và mắt cá chân (`left_knee`, `left_ankle`). Bằng chứng thị giác trên ảnh cho thấy phần đùi, gối và cẳng chân của người lái xe bị chồng thùng carton/bìa giấy chất phía trước xe che khuất, tuy nhiên toàn bộ phần chân và xe vẫn nằm gọn 100% bên trong khung ảnh (không bị ranh giới mép ảnh cắt đứt). Theo quy tắc gán nhãn của bài lab, khớp bị vật thể (thùng carton) che khuất nhưng còn trong khung ảnh bắt buộc phải chọn trạng thái **v=1 (Occluded)** (thể hiện bằng đường nối đứt nét) và đặt chấm ước lượng theo trục giải phẫu đùi–cẳng chân. Tôi không chọn `v=0 (Outside)` vì khớp không hề bị trượt ra ngoài mép ảnh; việc chọn `v=1` giúp giữ lại điểm OKS và giúp mô hình học đúng tư thế ngồi lái xe.
