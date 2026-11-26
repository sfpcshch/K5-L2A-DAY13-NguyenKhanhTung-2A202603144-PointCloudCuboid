# Báo cáo Day 13 — Nguyễn Khánh Tùng, 2A202603144

## 1. Kết luận trước

- **Mã số học viên (MSSV):** `2A202603144`
- **Task CVAT:** `D13-2A202603144`
- **10 frame được giao:** `s01f15`, `s01f34`, `s02f17`, `s02f45`, `s03f01`, `s03f41`, `s04f19`, `s04f46`, `s05f07`, `s05f39`
- **Môi trường:** Hệ điều hành Linux (CachyOS), trình duyệt Thorium / Chromium, phiên bản CVAT `2.82.0` (Server & UI)
- **MSSV bạn ngồi cạnh (đổi kiểm):** Không áp dụng (bỏ qua bước kiểm chéo theo hướng dẫn lớp)

Sau khi hoàn thành gán nhãn 10 frame trên CVAT và chạy `python scripts/lab.py sync`, bộ dữ liệu cá nhân có tổng cộng **81 cuboid** (gồm **78 cuboid** trong phạm vi chấm `≤ 50 m`, trong đó **67 cuboid ≤ 30 m**, và **3 cuboid > 50 m**). Hai frame ít cuboid nhất là `s01f34` và `s03f41` (`4` cuboid/frame) do mật độ phương tiện ở hai frame này ít hơn và học viên không suy đoán trên cụm quá thưa điểm `< 15–20%` thể tích ở xa. Phiếu tự kiểm `selfcheck` gắn cờ **4/81 cuboid** (`4,9%`). Phần tin nhất là góc hướng `yaw_deg` (0 cuboid bị `length < width`, hướng nhất quán theo chiều lưu thông các làn: khoảng `270°` cho xe cùng chiều và khoảng `90°` cho xe ngược chiều) và đa số đáy cuboid áp sát mặt đường; phần còn yếu là 4 hộp bị cờ kích thước (`s03f01 #7–#9` rộng `2,431 m`, `s05f39 #2` cao `2,432 m`), một số hộp có `ground_gap_m` hơi dương (`0,25–0,42 m`), và 3 hộp vẽ ngoài ngưỡng `50 m`.

Cách dùng CVAT:
- Đổi góc nhìn: kéo chuột trái để xoay khung 3D, kéo chuột phải để dời tâm nhìn, lăn con lăn để phóng to/thu nhỏ; nháy đúp vào khoảng trống để về góc nhìn mặc định; xoay khung 3D nhìn thẳng từ trên xuống để quan sát toàn cảnh 360°.
- Vẽ và chỉnh một cuboid: chọn nhãn và kiểu Shape (không dùng Track), đưa hộp tới cụm điểm rồi nháy đúp để đặt; chỉnh tâm, dài, rộng và góc xoay yaw ở khung Top; chỉnh chiều cao và kéo cạnh đáy chạm mặt đường ở khung Side và Front (không xoay ở Side/Front).
- Bật mũi tên hướng và xem trước: bật Appearance → Cuboid orientation để thấy vạch đỏ (`+X`) dọc thân và chỉ về đầu xe; chạy `python scripts/lab.py preview` để chiếu cuboid lên 7 ảnh camera và góc nhìn từ trên xuống (BEV).

## 2. Đọc frame (Bước 2)

Hai frame đọc kỹ: `s01f15` và `s01f34`.
Ảnh chụp lưu trong `report/individual/images/`: `t1-s01f15-3d.png`, `t1-s01f15-top.png`, `t1-s01f34-3d.png`, `t1-s01f34-top.png` (hai ảnh `-top.png` có đánh dấu mũi tên phía trước `−y`, `+x`, `+y`).

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| `s01f15` | Phía trước là `−y` (cạnh trên theo hướng chuẩn BEV — khớp với mũi tên đã vẽ trên `t1-s01f15-top.png`); `+x` chỉ sang trái, `+y` chỉ ra sau | Tương đối phẳng, thấp hơn LiDAR khoảng `1,7 m` (`z_bottom` các hộp gần `#1–#5` từ `−1,505 m` đến `−1,804 m`) | Trong phạm vi `50 m` là `#7 Car` ở `49,10 m` (ngoài `50 m` còn thấy cụm thưa `#6 Car` ở `81,25 m`, `15` điểm) | `Car`, `Bus` | Xe con `#3` ở **phía sau bên trái** sát hông xe (`x = +3,170 m, y = +1,575 m, dist_m = 3,54 m`, `cam05`), xe `#1` ở **phía sau** hơi lệch phải (`x = −0,989 m, y = +19,387 m, dist_m = 19,41 m`, `cam09`) và xe `#2` ở **phía sau bên phải** (`x = −3,958 m, y = +21,640 m, dist_m = 22,00 m`, `cam08`/`cam09`) | Các cụm điểm thưa rải rác ở rìa lề đường và cự ly xa không đủ hình dạng nhận diện xe hay người (không phỏng đoán) |
| `s01f34` | Phía trước là `−y` (khớp mũi tên đã vẽ trên `t1-s01f34-top.png`); `+x` chỉ sang trái, `+y` chỉ ra sau | Phẳng, thấp hơn LiDAR khoảng `1,7 m` (`z_bottom` từ `−1,696 m` đến `−1,815 m`) | `#4 Car` phía sau cách `47,11 m` (`50` điểm) | `Car` | Xe `#1` ở **trước-trái** (`x = +5,297 m, y = −7,065 m, dist_m = 8,83 m`, `cam06`), xe `#2` ở **phía sau** (`x = +0,634 m, y = +9,966 m, dist_m = 9,99 m`, `cam09`) và `#3` ở **phía sau bên trái** (`x = +2,528 m, y = +18,185 m, dist_m = 18,36 m`, `cam09`/`cam05`) | Cụm điểm thưa/tán xạ ở xa dọc lề đường không đủ cơ sở xác định loại vật |

Ba điều chỉ camera cho biết:
1. Màu sắc và kiểu dáng thân xe chi tiết (phân biệt xe con, taxi, bán tải, van hay xe buýt).
2. Tín hiệu đèn giao thông, đèn phanh/xi-nhan và nội dung trên biển báo.
3. Tư thế, hướng mặt của người đi bộ và việc người đó đang dắt xe hay đang điều khiển xe hai bánh.

Ba điều chỉ LiDAR cho biết:
1. Khoảng cách vật lý chính xác theo mét từ tâm LiDAR đến bề mặt vật thể trong không gian 3D.
2. Kích thước hình học thực tế (dài, rộng, cao bằng mét) và cao độ mặt đường không bị méo phối cảnh.
3. Hình học toàn cảnh 360° liên tục quanh xe thu dữ liệu, bao gồm cả các vùng góc mù nằm giữa 7 camera hoặc khu vực tối.

Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| `s02f17` | Cạnh trên ảnh (hướng `−y`) | Nhiều xe chạy ngược chiều ở làn **bên trái** (`#3–#8`, `x = +3,6 m` đến `+9,9 m`, `yaw ≈ 90°`) |
| `s02f45` | Cạnh trên ảnh (hướng `−y`) | Có xe chạy ngược chiều ở làn **bên trái** (`#1, #4, #5`, `x ≈ +5 m`, `yaw ≈ 87°–91°`) và xe cùng chiều phía trước/sau (`#2, #3, #6`) |
| `s03f01` | Cạnh trên ảnh (hướng `−y`) | Nhiều xe con nối đuôi nhau ở làn **bên trái** (`#2–#4, #6–#9`, `x = +2,5 m` đến `+3,35 m`) và làn giữa (`#1, #5`) |
| `s03f41` | Cạnh trên ảnh (hướng `−y`) | Có 1 `Cyclist` (`#4`) ở **phía sau bên phải** (`x = −7,843 m, y = +14,891 m, dist_m = 16,83 m`, `cam08`) |
| `s04f19` | Cạnh trên ảnh (hướng `−y`) | Mật độ phương tiện rất cao (`19` cuboid), gồm 7 xe ngược chiều ở làn **bên trái** (`#13–#19`, `x = +9,7 m` đến `+16,2 m`, `yaw ≈ 90°`) |
| `s04f46` | Cạnh trên ảnh (hướng `−y`) | Có xe gần ở **bên phải** (`#1` ở `x = −6,23 m`, `#2` ở `x = −3,41 m`), 2 xe xa phía sau (`#3, #4`, `34–39 m`) và 1 xe ngược chiều phía trước bên trái (`#7`, `yaw = 86,1°`) |
| `s05f07` | Cạnh trên ảnh (hướng `−y`) | Mật độ dày (`11` cuboid), có xe tải (`#8 Truck`) phía sau (`y = +17,21 m`, `dist_m = 17,22 m`) và 3 xe ngược chiều ở làn **bên trái** (`#9–#11`, `x = +8,8 m` đến `+13,0 m`, `yaw = 89,4°`) |
| `s05f39` | Cạnh trên ảnh (hướng `−y`) | Có xe buýt (`#3 Bus`) ở phía trước bên trái (`x = +2,94 m, y = −24,79 m`, `dist_m = 24,97 m`), 1 xe ngược chiều bên trái (`#4`, `yaw = 89,4°`) và 1 xe xa phía sau (`#5`, `56,53 m`, `29` điểm) |

## 3. Ba cuboid có hướng dẫn (Bước 3)

Ảnh chụp lưu trong `report/individual/images/`: `t2-a-car.png`, `t2-b-bus-truck.png`, `t2-c-ped-cyclist.png`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | `s01f15, #3` | `Car` | Đúng đầu xe (`272,2°`) | Chạm đường (`−1,724 m`) | Đã kiểm Top/Side/Front + ảnh camera | `l = 4,700 m`, `w = 1,900 m`, `h = 1,500 m` (nằm trong khoảng chuẩn Sedan Bảng 4.3) |
| (b) xe buýt hoặc xe tải | `s01f15, #5` | `Bus` | Đúng đầu xe (`281,0°`) | Chạm đường (`−1,804 m`) | Đã kiểm Top/Side/Front + ảnh camera | `l = 11,000 m`, `w = 2,630 m`, `h = 3,168 m` (`l, h` trong khoảng chuẩn Xe buýt thường; `w` nhỉnh hơn `2,60 m` chỉ `0,03 m`, trong dung sai `±5%`) |
| (c) người đi bộ hoặc xe hai bánh | `s03f41, #4` | `Cyclist` | Đúng hướng di chuyển (`270,2°`) | Chạm đường (`−1,707 m`) | Đã kiểm Top/Side/Front + ảnh camera | `l = 1,750 m`, `w = 0,700 m`, `h = 1,600 m` (nằm trong khoảng chuẩn xe đạp có người Bảng 4.3) |

Vòng hiệu chuẩn: Theo hướng dẫn của Lab coach tại buổi học, không cần chạy lệnh tạo task chung `python scripts/lab.py task --code <MÃ CHUNG>`, tập trung hoàn thiện task cá nhân `D13-2A202603144` theo đúng guideline (bán tải là `Truck`, người dắt xe là `Pedestrian`, xe hai bánh có người lái là `Cyclist`, vạch đỏ hướng về đầu xe).
Các vật bỏ qua vì không đủ cơ sở: trong quá trình rà các frame, các cụm quá thưa điểm (`< 15–20%` thể tích) ở cự ly xa `40–50 m` hoặc bị che khuất mạnh khiến không xác định chắc chắn được loại vật hay hướng đầu xe đều được bỏ qua theo guideline (`LOW_VISIBILITY` / `UNCERTAIN_SCOPE`) thay vì đoán.

## 4. Bốn hiện tượng chất lượng (Bước 4)

Ảnh chụp lưu trong `report/individual/images/`: `t4-1-bong-che.png`, `t4-2-thua-o-xa.png`, `t4-3-phan-xa-thap.png`, `t4-4-diem-la.png`. Nguồn: các frame trong bộ 10 frame cá nhân.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | `t4-1-bong-che.png` (frame cá nhân, ví dụ vùng sau xe tải `s05f07 #8` dài `9,000 m`, cao `2,732 m`) | *Giả thuyết:* Vật chắn kích thước lớn ở gần hấp thụ/chắn đường truyền thẳng của tia laser LiDAR nên không chiếu tới vùng phía sau nó. | Phía sau vật che xuất hiện khoảng trống hình nón gần như không có điểm phản xạ, trong khi hai bên góc che vẫn dày điểm. | Vùng trống là khu vực chưa quan sát được (không phải đường trống); nếu có vật bị che một phần phía sau phải bật thuộc tính `occluded` và vẽ hộp bao cả phần suy luận được theo Bảng 4.3. |
| 2. Thưa ở xa | `t4-2-thua-o-xa.png` (frame cá nhân, ví dụ xe xa như `s04f46 #4` ở `38,64 m` còn `75` điểm hoặc `s01f15 #7` ở `49,10 m` còn `47` điểm) | *Giả thuyết:* LiDAR 40 tia có góc phân kỳ tia cố định nên càng ra xa khoảng cách giữa các tia càng lớn, làm mật độ điểm trên vật giảm nhanh. | Vật ở cự ly `30–50 m` chỉ còn vài lát tia thưa hoặc vài chục điểm rời rạc, không thấy đầy đủ các mặt thân xe. | Áp quy tắc `15–20%` thể tích (kết hợp đối chiếu camera) mới vẽ; nếu quá ít điểm và không đủ cơ sở suy luận thì bỏ qua, ghi `LOW_VISIBILITY`. |
| 3. Phản xạ thấp / ít điểm bất thường | `t4-3-phan-xa-thap.png` (frame cá nhân; ví dụ trong cùng tầm `~19 m`, `s04f19 #9` có `233` điểm ít hơn nhiều so với `s04f19 #14` có `783` điểm) | *Giả thuyết:* Bề mặt tối màu hoặc kính hấp thụ/phản xạ gương tia laser khiến năng lượng hồi tiếp (intensity) về cảm biến thấp hơn vật sáng màu cùng tầm. | Vật tối màu hoặc vùng kính xe có ít điểm và cường độ phản xạ thấp hơn hẳn các vật sáng màu ở khoảng cách tương đương. | Ghi nguyên nhân là giả thuyết (không khẳng định chắc chắn); đối chiếu ảnh camera để bao trọn thân xe theo kích thước thực, không nới hộp theo quầng sáng (intensity chỉ tham khảo). |
| 4. Điểm lạ | `t4-4-diem-la.png` (frame cá nhân) | *Giả thuyết:* Phản xạ đa đường (multipath) qua bề mặt bóng/ẩm hoặc nhiễu tán xạ của cảm biến tạo ra điểm sai lệch hình học. | Xuất hiện điểm hoặc cụm điểm dị thường nằm dưới mặt đường hoặc lơ lửng giữa không trung, không tương ứng vật thể nào trên ảnh camera. | Tuyệt đối không gán các điểm lạ này thành vật thể (bỏ qua hoàn toàn). |

## 5. Số từ cuboid của tôi (Bước 5)

Số liệu lấy trực tiếp từ `submission/D13-2A202603144-bangA.csv` và `submission/D13-2A202603144-tukiem.csv`. Mặt đường danh định khoảng `−1,70 m`.

| Cuboid (frame, idx, class) | z (m) | h (m) | z đáy = z − h/2 (m) | dist_m (m) | r = √(x²+y²+z²) (m) | yaw_deg (°) | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| `s01f15, #3, Car` | `−0,974 m` | `1,500 m` | `−1,724 m` | `3,54 m` | `3,67 m` | `272,2°` | Chạm đường (`z đáy` lệch `−1,70 m` chỉ `0,024 m`, `ground_gap_m = 0,05 m`) |
| `s05f07, #8, Truck` | `−0,121 m` | `2,732 m` | `−1,487 m` | `17,22 m` | `17,22 m` | `272,2°` | Chạm đường (`z đáy` trong khoảng `−1,70 ± 0,30 m`, `ground_gap_m = 0,25 m` hơi cao nhẹ so với mặt đường cục bộ) |
| `s04f46, #1, Car` | `−0,779 m` | `1,500 m` | `−1,529 m` | `7,34 m` | `7,39 m` | `269,6°` | Chạm đường (`z đáy` trong khoảng `−1,70 ± 0,30 m`, `ground_gap_m = 0,27 m`) |

Số liệu chi tiết:
- **(a) Cuboid gần nhất và xa nhất:**
  - Cuboid gần nhất: `s01f15 #3 (Car)` với `dist_m = 3,54 m`, `r = 3,67 m`.
  - Cuboid xa nhất toàn bảng A: `s01f15 #6 (Car)` với `dist_m = 81,25 m`, `r = 81,27 m` *(lưu ý: vượt ngưỡng `50 m` nên không được chấm; trong phạm vi chấm `≤ 50 m`, cuboid xa nhất là `s01f15 #7 (Car)` với `dist_m = 49,10 m`, `r = 49,11 m`)*.
- **(b) So kích thước với Bảng 4.3:**
  - Xe con `s01f15 #3 (Car)`: `l = 4,700 m` (chuẩn Sedan `3,8–5,0 m`), `w = 1,900 m` (chuẩn `1,70–1,90 m`), `h = 1,500 m` (chuẩn `1,40–1,60 m`) → nằm trong khoảng chuẩn Bảng 4.3.
  - Xe hai bánh `s03f41 #4 (Cyclist)`: `l = 1,750 m` (chuẩn xe đạp có người `1,6–1,9 m`), `w = 0,700 m` (chuẩn `0,50–0,70 m`), `h = 1,600 m` (chuẩn `1,60–1,90 m`) → nằm trong khoảng chuẩn Bảng 4.3.
  - Giải thích chiều nằm ngoài khoảng ở các hộp bị cờ: `s03f01 #7, #8, #9 (Car)` có `w = 2,431 m` vượt khoảng `1,70–2,10 m` (có khả năng hộp đang bị nới rộng quá thân xe hoặc bao cả phần chìa/gương), và `s05f39 #2 (Car)` có `l = 5,524 m`, `h = 2,432 m` vượt khoảng Car (có thể là xe cỡ lớn/van cao).
- **(c) Góc hướng `yaw_deg`:**
  - Xe cùng chiều **phía trước (`y < 0`)**: `s05f39 #1 (Car)` (`y = −12,950 m`, `dist_m = 14,10 m`, `yaw_deg = 270,2°`) và `s02f17 #2 (Car)` (`y = −12,440 m`, `dist_m = 12,45 m`, `yaw_deg = 271,6°`) → rất gần `270°`.
  - Xe ngược chiều làn đối diện: `s02f17 #6 (Car)` (`dist_m = 5,58 m`, `yaw_deg = 90,0°`) và `s04f19 #14 (Car)` (`dist_m = 19,26 m`, `yaw_deg = 89,4°`) → rất gần `90°`.

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m (m) | Số điểm (`points_in`) |
|---|---|---|
| `s04f46, #1, Car` | `7,34 m` (bin `0–10 m`) | `2824` điểm |
| `s05f07, #10, Car` | `14,23 m` (bin `10–20 m`) | `1288` điểm |
| `s03f01, #9, Car` | `24,01 m` (bin `20–30 m`) | `181` điểm |
| `s04f46, #3, Car` | `34,23 m` (bin `30–50 m`) | `120` điểm |

Quy luật mật độ: Khi khoảng cách tăng gấp `~4,7` lần (từ `7,34 m` lên `34,23 m`), số điểm LiDAR trong cuboid giảm khoảng `23,5` lần (từ `2824` điểm xuống còn `120` điểm), khớp với xu hướng giảm nhanh trên đường cong ở slide bài giảng (xe con, trung vị: khoảng `1968` điểm ở `0–10 m`, `718` ở `10–20 m`, `214` ở `20–30 m`, `72` ở `30–40 m`, `19` ở `60–80 m`).

Quá trình tự kiểm tra (`preview` / `selfcheck`):
- Phiếu tự kiểm ghi nhận `4/81` cuboid có cảnh báo kích thước (`s03f01 #7, #8, #9` rộng `2,43 m` ngoài khoảng `1,4–2,3 m` và `s05f39 #2` cao `2,43 m` ngoài khoảng `1,2–2,3 m`). Các cảnh báo này hiện được giữ nguyên sau khi đối chiếu (cần rà lại trên CVAT nếu tiếp tục chỉnh sửa để siết sát thân xe hơn và hạ nhẹ đáy các hộp có `ground_gap_m` từ `0,25–0,42 m`).

Đổi kiểm với bạn ngồi cạnh:
- Không áp dụng (bỏ qua bước kiểm chéo theo hướng dẫn lớp); học viên đã tự đối chiếu lại số liệu báo cáo (như `z_bottom` của `s01f15 #3` = `−1,724 m` và `dist_m` của `s04f46 #1` = `7,34 m`) khớp hoàn toàn với `submission/D13-2A202603144-bangA.csv` và `submission/D13-2A202603144-tukiem.csv`.

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

- **(a) Từ phía model học trên nhãn 3D (2 câu):**
  - *Vì sao nhãn 3D đo bằng mét chứ không phải pixel?* Mô hình nhận thức 3D cần dự đoán trực tiếp vị trí, khoảng cách và kích thước vật lý thực bằng mét trong hệ tọa độ quanh xe để phục vụ dự đoán quỹ đạo và lập kế hoạch chuyển động, trong khi tọa độ pixel 2D phụ thuộc phối cảnh xa-gần và tiêu cự camera nên không cho biết trực tiếp khoảng cách thật.
  - *Vì sao vật thưa điểm cần quy tắc riêng (`15–20%`, không đoán)?* Nếu người gán nhãn đoán bừa trên cụm quá ít điểm sẽ tạo ra nhãn nhiễu (label noise) sai lệch tâm, kích thước và góc yaw, làm mô hình học sai đặc trưng hình học và tăng lỗi dự đoán hoặc phát hiện ảo (false positive).
- **(b) Cách dùng AI (3 dòng):**
  - Công cụ: Antigravity (trợ lý AI).
  - Câu hỏi chính: Hỗ trợ setup môi trường Python 3.12 và CVAT cục bộ, giải thích hệ tọa độ VF (`−y` phía trước, `+x` trái, `+z` lên), tạo script hỗ trợ cục bộ (chụp ảnh đúng tên, xem thống kê chỉ đọc từ bảng A) và rà lỗi logic/đối chiếu số liệu báo cáo trước khi nộp.
  - Cách dùng kết quả: Dùng để cấu hình môi trường và tự kiểm tra tính nhất quán bài nộp; không gửi dữ liệu VF, ảnh camera hay ảnh chụp màn hình cho AI; toàn bộ cuboid trong task `D13-2A202603144` do học viên tự vẽ trên CVAT.
- **(c) Điều còn chưa chắc / hạn chế:**
  - Hiện bài còn `3` cuboid vẽ ở cự ly `> 50 m` (`s01f15 #6` ở `81,25 m`, `s02f45 #5` ở `61,47 m`, `s05f39 #5` ở `56,53 m`) nằm ngoài phạm vi chấm, `4` cuboid bị cờ kích thước ở `s03f01` & `s05f39`, `2` frame `s01f34` & `s03f41` mới có `4` cuboid (`< 5` theo mức khuyến nghị), và một số cuboid có `ground_gap_m` từ `0,25–0,42 m` còn có thể hạ đáy sát mặt đường hơn nếu mở CVAT chỉnh lại.
