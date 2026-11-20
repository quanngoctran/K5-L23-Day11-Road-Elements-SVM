# Báo cáo cá nhân — Ngày 11

- Họ tên Trần Ngọc Quân · Mã học viên `2A202603250`
- CVAT Community `2.74.1` · Guideline Road v1.0
- Export từ CVAT localhost lúc 19:59 ngày 20/11/2026 (giờ Việt Nam)
- Hồ sơ `revision: 2` sau tự kiểm. Bản `revision: 1` (export 17:18) lưu trong `r1/`, manifest cũ đổi tên thành `hoangminhhai-manifest-r1.json`.
- Trạng thái hồ sơ: đã thêm bốn vạch còn thiếu ở A06, A07, A08, A14. Sau lần tự kiểm thứ hai trên 6 ảnh mẫu, đã thêm R2 ở A04 và A08, một đèn ở A06, ba biển ở A08, rồi export lại. Bốn job ở trạng thái completed. Còn xin coach miễn QC1-02 ở A05 và nhận lô của Nguyễn Thị Hồng Hạnh để QC chéo.

## 1. Kiểm project

[Project d11-road-hoangminhhai](http://localhost:8080/projects/3) có bốn task `front`, `front-extra`, `fisheye` và `fisheye-extra`, mỗi task một job.

| Mục | Kết quả | Ghi chú |
| --- | --- | --- |
| 6 label đúng kiểu | Đã kiểm | `lane`, `road_curb` dùng Polyline. `crosswalk`, `drivable` dùng Polygon. `traffic_light`, `traffic_sign` dùng Rectangle. |
| Thuộc tính và giá trị | Đã kiểm schema | Cấu hình theo `configs/road_v1_labels.json`. Trong export, lint không báo giá trị ngoài danh mục. |
| 4 tag ảnh | Đã kiểm 28/28 ảnh | `camera`, `timeofday`, `weather`, `source` đều có `value`. Tag camera khớp tên file, kể cả hậu tố `FV`, `RV`, `MVL`, `MVR`. |

Kết thúc polyline bằng `N`. Dùng công cụ Tag ở thanh trái, thêm `camera` trước và kiểm giá trị từng tag trong **Objects → Details**. Checker chạy `SELFTEST PASS` và bộ test trong repo đạt 31/31.

## 2. Số lượng đã gán

| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 28 | 2 | 3 | 4 | 27 | 16 |
| front-extra (4/4 có tag và shape) | 4 | 10 | 0 | 1 | 0 | 9 | 9 |
| fisheye | 8 | 4 | 2 | 5 | 3 | 6 | 0 |
| fisheye-extra (4/4 có tag, 3/4 có shape) | 4 | 0 | 0 | 3 | 1 | 3 | 0 |

Tổng 28 ảnh, 112 tag và 136 shape. B11 có bốn tag. Trong ảnh thấy các vạch ô đỗ và vùng kẻ chéo, nên chưa thêm `lane`.

## 3. Bảng ca khó

| Ảnh | Nhãn | Phân vân giữa | Đã chọn | Lý do |
| --- | --- | --- | --- | --- |
| A05 | `lane` | L1/R1 với lề và hàng xe đỗ | Không gán L1, R1. Xin coach miễn QC1-02. | Phóng to cả hai bên đều không có vệt sơn. Mép phải là lề không sơn nên gán `road_curb`. |
| A06 | `lane` | R1 là vạch trắng sát lề hay không gán | Thêm R1 solid white từ `(1279,590)` tới `(1123,547)` | Vạch trắng liền sát lề phải là vạch mép (QCVN 3.1). Guideline yêu cầu gán vạch này là `lane`, không phải `road_curb`. |
| A07 | `lane` | L1 với mép dải bê tông | Thêm L1 solid white từ `(255,428)` tới `(360,390)` | Phóng 4 lần thấy một vạch trắng mảnh tách khỏi chân dải bê tông. Ảnh mưa nên vạch mờ, sẽ hỏi coach có giữ `road_curb` không. |
| A08 | `lane` | L1 với vạch dừng và vạch mép xa | Thêm L1 dashed white từ `(260,420)` tới `(470,322)` | Vạch đứt mảnh hội tụ cùng điểm với R1. Vạch liền dài phía trên là vạch của làn khác, vạch ngang là vạch dừng. |
| A14 | `lane` | R1 với đoạn chìm trong vùng tối | Thêm R1 solid white từ `(1279,552)` tới `(987,452)` | Ảnh đêm nhưng phóng 3 lần vẫn thấy vạch liền bên phải. Không còn lý do miễn. |
| A04 | `drivable` | `alternative` có lấn làn xe đạp không | Giữ nguyên, thêm R2 solid white | Mép polygon nằm đúng vạch trắng trong. Làn xe đạp sơn xanh nằm ngoài vạch trắng thứ hai, không thuộc vùng lái. |
| A08 | `lane` | Vạch liền bên trái có phải L2 | Không gán L2, chỉ thêm R2 là vạch mép phải | Vạch bên trái nối vào vạch dừng của làn khác. Gán nhầm `position` là lỗi nặng, nặng hơn bỏ sót. |
| A08 | `traffic_sign` | Cụm biển mờ trên cột phải là một hay nhiều biển | Ba hộp: hai `prohibitory`, một `supplementary` | Phóng 8 lần thấy hai biển đỏ trắng và một tấm trắng nhỏ bên dưới. Biển cao 15–26 px nên nhóm biển là phán đoán theo màu và vị trí. |
| A11 | `traffic_sign` | Biển ven đường với biển hiệu xa | Chờ đối chiếu kế hoạch ảnh | Trọng tâm trong script chọn ảnh có biển. Chưa xác định biển cần gán đủ rõ trên ảnh tối. |
| B02 | `traffic_light` | Vỏ đèn ở cạnh cột với vùng nền trời | Dời hộp vào vỏ, thêm cụm đèn đỏ xa nhìn thấy | Đối chiếu hình phóng to và giữ hộp theo vỏ. |
| B06 | `drivable` | Mặt đường trước và sau dải chắn | Thu `direct` về phía gần dải chắn | Vùng sau dải chắn nằm ngoài phần xe đi được theo guideline. |
| B11 | `lane` | Vạch đường với vạch đỗ xe | Giữ tag, chờ xác nhận có vạch đường dọc | Vạch dễ thấy thuộc ô đỗ xe hoặc vạch gạch chéo. |

Trọng tâm front được đối chiếu với các quy tắc chọn ảnh trong `src/road_lab/select_bdd_road.py`. `manifest_front.csv` của repo hiện chỉ có tiêu đề cột. Trọng tâm fisheye dùng `manifests/manifest_fisheye.csv`, trong đó các dòng vẫn ghi trạng thái ứng viên. Bảng Task 1/2 do lớp chốt còn cần để xác nhận đủ nhóm nhãn trên từng ảnh.

## 4. Phiếu QC 1 nhận được

Chưa nhận phiếu QC 1 của Nguyễn Thị Hồng Hạnh, chưa có quyết định QC chéo. Các thay đổi dưới đây là kết quả tự kiểm. Bốn dòng QC1-02 đã sửa trong `revision: 2`.

| Mã lỗi | Ảnh | Đã sửa |
| --- | --- | --- |
| QC1-02 | A06 | Thêm L1 vàng ở đoạn nhìn thấy. Thêm R1 solid white ở vạch mép sát lề phải. |
| QC1-02 | A07, A08, A14 | Soi lại ở độ phóng 3–4 lần thì cả ba ảnh đều thấy vạch. Đã thêm L1 ở A07 (solid), A08 (dashed) và R1 ở A14. Tọa độ ghi ở bảng ca khó. |
| QC1-04 | A02, A03, A11 | Chỉnh tâm vạch. Tại `y=511` ở A02, R1 chuyển từ `x=871` về `x=836`. |
| QC1-07 | A12, B06, B07 | Thêm `direct` cho A12, thu `direct` ở B06, thêm vùng qua đường thứ hai ở B07. |
| QC1-08 | B02 | Dời box trên nền trời về cụm đèn và thêm cụm đèn đỏ ở xa. |
| QC1-11 | B03 | Chỉnh polyline R1 giữa các đoạn sơn nhìn thấy để bám cung. |
| QC1-04 | A04, A08 | Tự kiểm lần hai trên 6 ảnh mẫu: thêm R2 ở A04 (vạch trong cạnh làn xe đạp) và A08 (vạch mép phải). |
| QC1-08 | A06 | Thêm cụm đèn quay ngang ở góc trên trái, `(33,28)–(92,71)`, `color = unknown`. |
| QC1-10 | A08 | Thêm ba biển trên cột bên phải, tại `(990–1009, 184–247)`. |

## 5. Kiểm file export

| File | Mở lại được bằng lint | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera trong XML |
| --- | --- | --- | --- | --- |
| `hoangminhhai-front.zip` | Có | A01 · 4 / 4 | A02 · 3 / 3 | Có, `front` |
| `hoangminhhai-fisheye.zip` | Có | B01 · 1 / 1 | B02 · 6 / 6 | Có, `fisheye_front` ở B01/B02 |

Hai ZIP là CVAT for images 1.1, chỉ có `annotations.xml`, giữ nguyên tên ảnh. Số ảnh 16 và 12, `contains_images: false` ở cả hai mục manifest. Lint ở `revision: 1` báo sáu cảnh báo QC1-02 trên A05, A06, A07, A08, A14. Sau khi thêm bốn vạch và các nhãn của lần tự kiểm thứ hai, lint ở `revision: 2` chỉ còn hai cảnh báo QC1-02 ở A05, là ảnh không có vạch nên xin coach miễn. Nội dung nhãn fisheye không đổi giữa hai lần, chỉ khác thời điểm export. CVAT dùng localhost, cần coach xác nhận chấp nhận nguồn export khi nộp.

## 6. Ảnh chụp màn hình

1. Tab Labels của project, đủ 6 label vẽ và 4 tag.

   ![Labels của project](minh-chung/labels.jpg)

2. A12 trong CVAT, có lane, vùng lái, crosswalk, đèn và biển.

   ![Nhãn front trên A12](minh-chung/front.jpg)

3. B03 trong CVAT, đoạn polyline R1 và các điểm điều khiển.

   ![Polyline fisheye B03](minh-chung/fisheye.jpg)

4. Trích đoạn `<polyline label="lane">` từ XML thực tế của ZIP front.

   ![Trích đoạn annotations.xml](minh-chung/xml-a02.jpg)

## 7. Tự đánh giá

Cảnh báo gặp nhiều nhất khi kiểm file là QC1-02. Khi tự kiểm ảnh, lỗi cần sửa rõ nhất là QC1-04, đường polyline lệch tâm vệt sơn. Ở Ngày 20, phóng ảnh đến độ phân giải gốc, đánh dấu các điểm giữa trên từng vệt sơn và xem lại hai đầu polyline trước khi save.

Ví dụ, R1 ở A02 lệch khoảng 35 px theo phương ngang tại hàng `y=511`. Nếu dùng tọa độ đó làm đích row-anchor, mô hình nhận mục tiêu nằm lệch về phía lề dù vạch thật không ở đó. Việc chỉnh về tâm vạch làm mất sai lệch có hệ thống ở hàng này.

Lần tự kiểm thứ hai còn tìm thấy nhóm lỗi bỏ sót đối tượng nhỏ ở xa: một đèn quay ngang ở A06 và ba biển cao 15–26 px ở A08. Detector học từ lô thiếu nhãn sẽ coi các vùng đó là nền, nên recall với đèn, biển nhỏ giảm đúng ở cỡ vật mà xe cần nhận sớm. Lần sau, sau khi gán xong, quét riêng một lượt quanh mép ảnh và các cột đèn để tìm vật từ 8 px.

## 8. Row-anchor

Ảnh A01, vạch R1. Kết quả kiểm bằng `road_lab.rowanchor` trên ZIP front `revision: 2`. R1 ở A01 không đổi so với `revision: 1`.

| y | 720 | 680 | 640 | 600 | 560 |
| --- | ---: | ---: | ---: | ---: | ---: |
| x của bài này | không có làn | không có làn | 594,3 | 539,6 | 483,6 |
| x của Hạnh | chờ ZIP | chờ ZIP | chờ ZIP | chờ ZIP | chờ ZIP |

1. Điểm xa có `y=517`. Nếu dừng sớm 40 px theo chiều dọc tại `y=557`, không hàng nào trong năm hàng đổi trạng thái. Ba hàng `560`, `600`, `640` vẫn cắt đường. Hai hàng `680` và `720` đã ngoài phạm vi ngay từ đầu.
2. Tại `y=720`, bài này không có giao điểm nên không tính chênh lệch pixel. Tại `y=560`, bài này có `x=483,6`, chờ tọa độ của bạn để tính. Sai lệch gần xe ảnh hưởng trực tiếp hơn đến phép ước lượng vị trí làn cho LKA nếu cả hai bài đều có giao điểm tại hàng đó.
3. Khi mỗi hàng chỉ cắt vạch một lần, đảo thứ tự các điểm vẫn cho cùng giao điểm nội suy. Checker chiều vẽ và cảnh báo của `rowanchor` có thể phát hiện thứ tự bị đảo.
