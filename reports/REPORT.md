# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Xuân Quang   Nhóm: T045   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 27 |
| v=2 / v=1 / v=0 | 315 / 106 / 38 |
| Thời gian trung bình mỗi ảnh | Chưa ghi nhận |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` — 56%
2. `right_ear` — 52%
3. `left_eye` — 33%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Có. Tai và mắt thường bị tóc, mũ bảo hiểm hoặc góc quay của đầu che khuất; vì vậy chúng
có tỷ lệ `v=1` cao. Tuy nhiên, “bị che” không đồng nghĩa không xác định được: với mắt/tai,
đường viền đầu, mũ và trục mũi vẫn là căn cứ để ước lượng vị trí giải phẫu.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.890 | 0.915 |
| OKS@0.50 | 0.900 | 0.931 |
| OKS@0.75 | 0.900 | 0.931 |
| Lỗi `dao_trai_phai` | 1 | 0 |
| Lỗi `nham_nguoi` | 2 | 1 |
| Lỗi `xoa_khop_bi_che` | 4 | 4 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- Chưa rework ở thời điểm viết báo cáo. Lượt đánh giá ban đầu đạt OKS trung bình 0.915 và mức “Xuất sắc”.
- Danh sách rework đã xác định: `train_13.jpg` thiếu hai người; `train_03.jpg` người #1 nhầm `right_hip`;
  `train_04.jpg`, `train_06.jpg`, `train_10.jpg` có khớp bị che đặt `Outside`; `train_19.jpg` người #2 trượt `right_wrist`.
- Sau khi sửa trong CVAT và export lại, cần chạy lại evaluator để điền cột “Sau rework”.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh ở lượt đánh giá hiện tại.

## 3. Kiểm chéo

Bạn cùng nhóm: chưa thực hiện

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| Chưa có nhãn bạn cùng nhóm để so sánh | — | — | — | Chưa thực hiện |
| Chưa có nhãn bạn cùng nhóm để so sánh | — | — | — | Chưa thực hiện |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Chưa có kết quả kiểm chéo. Quy ước hiện tại: nếu khớp còn trong ảnh và có căn cứ từ phần cơ thể liền kề,
  dùng `v=1`; chỉ dùng `v=0` khi khớp thực sự nằm ngoài khung.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` tăng từ 0.6853 lên 0.6908, tức `+0.0055`. Đây là cải thiện rất nhỏ,
   hợp lý vì chỉ fine-tune trên 20 ảnh trong khi model gốc đã được train trên COCO.

2. Sau fine-tune, `box_mAP50-95` là 0.8041 còn `pose_mAP50-95` là 0.6908, chênh `0.1133`.
   Model tìm người (box) dễ hơn định vị chính xác 17 khớp (pose), đặc biệt khi khớp bị che.

3. `test_07` có tư thế bị bàn che phần thân dưới. Một số keypoint chân bị kéo xuống vùng bàn/thân dưới
   thay vì vị trí khớp có thể kiểm chứng, thuộc loại **trượt hẳn** do che khuất nặng.

4. `train_14` có OKS model-vs-nhãn thấp nhất trong các skeleton ghép được (`0.678`). Kết quả gold
   trước rework không nêu lỗi nghiêm trọng cho ảnh này, nên nhãn thủ công đáng tin hơn dự đoán model;
   cần ưu tiên bằng chứng gold/ảnh gốc hơn chỉ số model-vs-nhãn.

5. Không hoàn toàn trùng. Annotation tệ nhất là `train_13.jpg` vì thiếu hai người so với gold; ở lượt so
   model-vs-nhãn, ảnh này còn báo lệch số người (model 3 / nhãn 1). Trong khi đó OKS thấp nhất của một
   skeleton ghép được là `train_14` (0.678). Điều này cho thấy lỗi đếm người và lỗi định vị khớp là hai dạng lỗi khác nhau.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->

Ở `train_09.jpg`, người #1 đội mũ bảo hiểm nên `left_ear` và `right_ear` không nhìn thấy trực tiếp.
Hai tai vẫn nằm trong khung; vị trí được ước lượng từ mắt, đường viền mũ bảo hiểm và trục đầu.
Vì vậy hai điểm được gán `Occluded` (`v=1`) thay vì `Outside` (`v=0`). Dùng `v=0` trong trường
hợp này sẽ khiến model không học được vị trí tai khi người đội mũ bảo hiểm.
