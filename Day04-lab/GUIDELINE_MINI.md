# Mini guideline - Tên: Phạm Quang Huy  |  người gán: Phạm Quang Huy  |  ngày: 16/09/2026

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

| Tình huống | Luật  bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài |Vẫn ước lượng vị trí khớp hông dựa vào tư thế/đường viền cơ thể qua lớp vải, gán v=1 (bị che nhưng đoán được), không bỏ trống. Chỉ dùng v=0 nếu hông thực sự ở ngoài khung ảnh. | Luật bắt buộc mục 1 nói rõ: bị che nhưng còn trong khung → v=1, vẫn phải đặt chấm; không được xoá vì "không chắc chắn 100%".|
| Tai bị tóc hoặc mũ bảo hiểm che một phần |Nếu còn thấy được một phần bề mặt tai (dái tai, viền vành tai) → v=2. Nếu bị che gần hết, chỉ đoán được vị trí dựa vào hình dạng đầu → v=1. |Đây là khớp mơ hồ nhất trong dữ liệu thật của bạn — visibility_report.json cho thấy tai là khớp có %v=1 cao nhất (55.6% / 51.9%), và eval_vs_gold.json cho thấy right_ear là khớp bị lệch cờ với gold nhiều nhất cả lớp (12/27 lần) → ranh giới "còn thấy bề mặt hay không" là điểm cả lớp hay tranh cãi, nên cần quy tắc rõ như trên. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) |Các khớp từ hông trở lên gán bình thường theo mức nhìn thấy. Các khớp phía dưới (gối, cổ chân) không xuất hiện trong khung → v=0, không đặt chấm. | Đúng luật bắt buộc: ra ngoài mép ảnh → v=0, không đặt chấm — không nội suy vị trí khớp nằm hẳn ngoài khung hình.|
| Cổ tay nằm sau tay lái / sau thân mình |Ước lượng vị trí hợp lý dựa vào hướng cẳng tay và tư thế, gán v=1 (bị che, còn trong khung). |Cổ tay vẫn nằm trong khung ảnh (chỉ bị vật thể/thân mình che), không phải trường hợp "ra ngoài mép ảnh" nên không dùng v=0. |
| Hai người chồng lên nhau |Gán riêng biệt đủ 17 điểm cho từng người. Khớp bị người kia che → v=1 theo ước lượng giải phẫu (dựa vào chi cùng phía: vai-khuỷu-cổ tay). Nếu không thể xác định khớp thuộc về ai do chồng lấn quá nhiều, ưu tiên gán theo thân người có phần liền kề (vai, hông) rõ ràng hơn. |Tránh lỗi "nhầm người" — đúng loại lỗi mà eval_vs_gold.json phát hiện thật ở train_04 (cổ tay bị gán nhầm gần người khác); đây là bằng chứng cụ thể cho thấy quy tắc này quan trọng với chính dữ liệu của bạn. |
| Người nhỏ đến mức nào thì không gán nữa |Nếu chiều cao khung người trong ảnh nhỏ đến mức không còn phân biệt được các khớp riêng biệt bằng mắt thường (ước chừng dưới ~20-25px chiều cao, hoặc các khớp dồn lại thành một chấm mờ) thì bỏ qua, không gán người đó. |Gán khớp cho người quá nhỏ tạo nhãn nhiễu (toạ độ gần như đoán mò), gây hại nhiều hơn lợi khi huấn luyện model — ngưỡng cụ thể nên điều chỉnh theo độ phân giải ảnh thực tế trong bộ 20 ảnh của bạn. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh train_16, người thứ 2, khớp toàn bộ cặp trái/phải (vai, khuỷu tay, cổ tay, hông, gối, cổ chân...)

- Mơ hồ ở chỗ nào:theo eval_vs_gold.json, đổi lại toàn bộ cặp trái/phải của skeleton này thì OKS tăng hẳn (từ 0.4755 lên rất cao) → tôi đã xác định trái/phải theo hướng nhìn của ảnh thay vì theo hướng cơ thể người (rule bắt buộc ở mục 1). Đây là dấu hiệu người trong ảnh xoay lưng, nghiêng người, hoặc đứng cạnh người khác gây rối mắt.
- Bạn quyết thế nào:xác định trái/phải theo hướng người xem nhìn vào ảnh, thay vì tưởng tượng đứng ở vị trí của người trong ảnh rồi xác định trái/phải theo cơ thể họ.
- Vì sao:người này đang quay lưng lại camera hoặc nghiêng người, nên việc "tưởng tượng đứng vào vị trí của họ" khó và dễ nhầm hơn bình thường, dẫn đến áp dụng nhầm quy tắc trái/phải theo khung ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì:model sẽ học sai lệch hoàn toàn khái niệm "tay trái/tay phải" cho các tư thế quay lưng/nghiêng người — hậu quả nặng hơn nhiều so với lệch tọa độ, vì nó làm hỏng cả một nhóm hành vi tương tự (VD: nhận diện "giơ tay phải" sẽ luôn bị đảo ngược ở các pose tương tự).

### Ca 2 - ảnh `train_04`, người thứ `1`, khớp `left_wrist`

- Mơ hồ ở chỗ nào:eval_vs_gold.json ghi nhận đây là lỗi nhầm người — chấm cổ tay trái bạn đặt gần vị trí cổ tay của người khác trong ảnh hơn là của người tôi đang gán. Dấu hiệu ảnh có nhiều người đứng gần/chồng nhau, dễ nhầm tay của ai với ai.
- Bạn quyết thế nào:đặt điểm cổ tay trái ở vị trí trông hợp lý nhất trong vùng hai người chồng lấn, dựa theo cẳng tay nhìn thấy gần đó, mà không lần theo chuỗi vai → khuỷu tay → cổ tay của đúng người đang gán
- Vì sao:hai người đứng sát/chồng nhau khiến các chi tay đan xen trên ảnh; đã ưu tiên "điểm nhìn có vẻ đúng gần nhất" thay vì xác nhận lại bằng cách dò theo từng đoạn xương từ vai xuống.
- Nếu người khác quyết ngược lại thì model học sai cái gì:Nếu người khác quyết ngược lại thì model học sai cái gì: model sẽ học sai liên kết giữa một cổ tay và đúng thân người sở hữu nó — trong ảnh đông người, model dễ "gán nhầm chi" giữa hai người đứng cạnh nhau dù phát hiện đúng vị trí khớp.

### Ca 3 - ảnh `train_01`, người thứ `2`, khớp `right_ear`

- Mơ hồ ở chỗ nào: ghi v=1 (bị che, đoán vị trí), gold ghi v=2 (thấy rõ). Đây là ranh giới mơ hồ nhất trong toàn bộ dữ liệu: right_ear là khớp bị lệch cờ visibility nhiều nhất so với gold (12/27 lần, tức gần một nửa số người) — cho thấy cả lớp, không chỉ riêng bạn, chưa thống nhất ngưỡng "che một phần vẫn tính v=2" hay "che một phần thì hạ xuống v=1".
- Bạn quyết thế nào:thấy tai bị che một phần bởi tóc/góc nghiêng đầu nên chọn hạ xuống v=1, dù phần tai còn lại vẫn đủ rõ để xác định đúng vị trí giải phẫu.
- Vì sao:có xu hướng thận trọng — hễ thấy bất kỳ phần che nào là hạ xuống v=1, thay vì hỏi "còn đủ bề mặt để tự tin xác định vị trí không" (tiêu chí gold có vẻ đang dùng)
- Nếu người khác quyết ngược lại thì model học sai cái gì:
model sẽ học một ngưỡng "occlusion" không nhất quán cho khớp tai, làm giảm khả năng model tự tin báo "khớp này tôi không chắc" khi suy luận trên ảnh mới.

## 4. Sau khi so visibility report với bạn cùng nhóm
Không làm nhóm nên ko so được
- Khớp lệch `%v=1` nhiều nhất: `______` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất:
