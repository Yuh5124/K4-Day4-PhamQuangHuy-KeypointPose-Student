# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Phạm QUang Huy  Nhóm: ______   Ngày: 16/9/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán |20 |
| Số skeleton |27 |
| v=2 / v=1 / v=0 |337 / 103 / 19 |
| Thời gian trung bình mỗi ảnh |3p |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1.right_ear — 15/27 = 55.6%
2.left_ear — 14/27 = 51.9%
3.left_eye — 9/27 = 33.3%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Về mặt dữ liệu, kết quả này hợp lý: hai tai và mắt trái là những khớp nhỏ, thường bị tóc, mũ bảo hiểm, hoặc góc quay nghiêng của đầu che một phần — đúng loại tình huống mà GUIDELINE_MINI.md mục 2 đã liệt kê sẵn ("Tai bị tóc hoặc mũ bảo hiểm che một phần"), khác với các khớp thân/chân vốn hiếm khi bị che hoàn toàn. [CẦN ĐIỀN — xác nhận lại bằng trải nghiệm thật của bạn: đây có đúng là những khớp bạn phải dừng lại lâu nhất khi gán không, hay có khớp khác (VD: cổ tay, mắt cá) bạn thấy khó hơn dù %v=1 không cao nhất? Nếu khác, giải thích tại sao — có thể vì "khó xác định vị trí giải phẫu" khác với "hay bị che"]
<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình |0.9184 | Không rework |
| OKS@0.50 |0.8966 | |
| OKS@0.75 |0.8966 | |
| Lỗi `dao_trai_phai` |1 | |
| Lỗi `nham_nguoi` |3 | |
| Lỗi `xoa_khop_bi_che` |0 | |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

-train_13 — thiếu hẳn 2 người (gold có 3 người, bạn chỉ gán 1) → cần thêm 2 skeleton còn thiếu.
-train_16, người thứ 2 — đảo trái/phải toàn bộ skeleton (đổi lại cặp trái/phải sẽ tăng OKS mạnh); cùng người này còn có nhầm người ở right_elbow, right_wrist, và trượt hẳn ở left_elbow (lệch 136px), left_wrist (lệch 219px).
-train_04, người khớp với gold_person 1 — nhầm người ở left_wrist (điểm gần cổ tay của người khác hơn).
-Nhiều ảnh (train_02, train_03, train_06, train_09, train_12, train_14, train_15, train_16, train_17, train_18, train_20...) có cờ v khác gold (bạn ghi v=1 gold ghi v=2 hoặc ngược lại) ở tai, vai, hông, gối, cổ chân — không trừ OKS nhưng là dấu hiệu guideline về ranh giới v=1/v=2 chưa thống nhất với gold, nên rà lại theo mục 2 của GUIDELINE_MINI.


**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->
Xảy ra ở train_16, người thứ 2 (OKS chỉ 0.4755 — thấp nhất trong toàn bộ 27 người khớp được với gold, so với mức trung bình 0.9184). Đây rõ ràng là một ảnh khó: cùng một người này còn dính thêm lỗi nhầm người và trượt hẳn ở nhiều khớp khác, cho thấy khả năng cao đây là cảnh có nhiều người chồng lấn nhau hoặc tư thế xoay người phức tạp khiến bạn khó xác định đâu là trái/phải theo cơ thể. [CẦN ĐIỀN — mở ảnh train_16 để xác nhận cụ thể tình huống, VD: người quay lưng, tư thế cúi/xoay, hay đứng cạnh người khác gây rối mắt]
## 3. Kiểm chéo

Bạn cùng nhóm: Không làm nhóm

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->
Không làm nhóm
-

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 |0.8450 | 0.8450|+0.0000 |
| pose_mAP50-95 |0.6853 |0.6908 | +0.0055|
| pose_precision |0.9734 |0.9792 | +0.0058|
| pose_recall |0.8462 |0.8462 | +0.0000|
| box_mAP50-95 |0.8119 | 0.8041| -0.0078|

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
pose_mAP50-95 tăng rất nhẹ: +0.0055 (0.6853 → 0.6908) — gần như trong biên độ nhiễu vì tập test chỉ có 10 ảnh / 13 người. Đáng chú ý hơn: log huấn luyện (Cell 10) cho thấy checkpoint tốt nhất rơi vào epoch 9/80; từ epoch ~11 đến ~30, box_mAP50 sụp từ 0.98 xuống còn 0.16-0.48 trước khi hồi phục một phần. Nói cách khác, 20 ảnh của tôi không dạy được điều gì bền vững — chúng chỉ đủ để đẩy model lệch tạm thời khỏi phân bố COCO rộng mà nó đã học, rồi model phải "quên" bớt phần đó để hồi phục lại gần baseline.
2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
box_mAP50-95 (0.8119 → 0.8041) và pose_mAP50-95 (0.6853 → 0.6908) chênh nhau khoảng 0.11-0.13 ở cả hai lần đo. Model tìm người (box) dễ hơn nhiều so với định vị chính xác 17 khớp — vì phát hiện một hộp bao quanh người là bài toán thô, còn định vị khớp đòi hỏi độ chính xác không gian cao hơn nhiều, đặc biệt với khớp nhỏ (cổ tay, mắt cá).
3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

test_02 model nhầm người
4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
OKS thấp nhất giữa nhãn của tôi và model là ở train_16, OKS = 0.284 (thấp hẳn so với phần còn lại, dải từ 0.63 đến 0.945). Điều này được xác nhận chéo bởi eval_vs_gold.json: cũng chính train_16 (người thứ 2) có OKS thấp nhất so với gold (chỉ 0.4755, so với trung bình 0.9184 toàn bộ 27 người) — với lỗi thật là đảo trái/phải, kèm nhầm người và trượt hẳn ở khuỷu tay/cổ tay. Vậy model đúng hơn bạn ở ảnh này: bằng chứng là gold đồng ý với model chứ không đồng ý với nhãn gốc của bạn tại train_16.
5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
Có. train_16 vừa là ảnh tôi gán tệ nhất so với model (OKS=0.284), vừa là ảnh có OKS thấp nhất so với gold (0.4755 — thấp hơn hẳn ảnh thấp thứ nhì là train_04 ở 0.861). Việc cả hai phép so sánh độc lập (với model và với gold) đều chỉ ra cùng một ảnh, cùng một người, là bằng chứng khá chắc rằng đây là một ảnh khó về bản chất — nhiều khả năng do nhiều người đứng gần/chồng nhau hoặc tư thế xoay người gây nhầm lẫn trái/phải — chứ không phải bạn ngẫu nhiên sai một chỗ. Cần xem ảnh gốc train_16 để xác nhận trực quan lý do cụ thể (chồng người, góc chụp, hay tư thế bất thường).
## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
(1)Train_11 + người thứ 1 + right_ankle và left_ankle. Khi người ngồi bị che bởi bàn và con mèo nhưng mà theo dự đoán và tưởng tượng đặt mình vào thì người đang ngồi nên phần angkle sẽ vẫn còn trong site 
