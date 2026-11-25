# Báo cáo Day 13 — Nguyễn Khánh Tùng, 2A202603144

> **Nhắc trước khi nộp:** Xoá mọi dấu `TODO(học viên)` và xoá toàn bộ các dòng gợi ý bắt đầu bằng `>`. Mỗi con số lấy từ **bảng A** (`submission/D13-2A202603144-bangA.csv`) hoặc **phiếu tự kiểm** (`submission/D13-2A202603144-tukiem.csv`), **bắt buộc kèm đơn vị (`m`, `°`)**. Ảnh lưu trong `report/individual/images/` (nộp LMS bằng ZIP, không commit); trong báo cáo chỉ ghi **tên file ảnh**. Repo là **public**: không ghi biển số, chữ biển hiệu/biển chỉ đường, tên đường, địa danh, đặc điểm nhận dạng cá nhân; nhắc bạn khác chỉ ghi **MSSV**, không ghi họ tên.

## 1. Kết luận trước

- **Mã số học viên (MSSV):** `2A202603144`
- **Task CVAT:** `D13-2A202603144`
- **10 frame được giao:** `s01f15`, `s01f34`, `s02f17`, `s02f45`, `s03f01`, `s03f41`, `s04f19`, `s04f46`, `s05f07`, `s05f39`
- **Môi trường:** Hệ điều hành Linux (CachyOS), trình duyệt TODO(học viên), phiên bản CVAT `2.82.0` (Server & UI)
- **MSSV bạn ngồi cạnh (đổi kiểm):** TODO(học viên)

TODO(học viên): Viết 2–3 câu tổng kết sau khi hoàn thành vẽ và chạy `sync`: tổng số cuboid đã vẽ, frame nào ít cuboid nhất và vì sao; `selfcheck` cờ bao nhiêu cuboid trên tổng số; bạn tin phần nào nhất và phần nào còn yếu nhất (hướng, đáy, kích thước, class, vật xa).

TODO(học viên): Cách dùng CVAT (viết đúng 3 dòng):
- Đổi góc nhìn (nhìn từ trên xuống, xoay, dời): ...
- Vẽ và chỉnh một cuboid (Shape, nháy đúp, Top/Side/Front): ...
- Bật mũi tên hướng (Cuboid orientation) và xem cuboid trên ảnh camera (`preview`): ...

## 2. Đọc frame (Bước 2)

Hai frame đọc kỹ: `s01f15` và `s01f34`.
Ảnh chụp lưu trong `report/individual/images/`: `t1-s01f15-3d.png`, `t1-s01f15-top.png`, `t1-s01f34-3d.png`, `t1-s01f34-top.png` (hai ảnh `-top.png` có đánh dấu phía trước `−y`, `+x`, `+y`).

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| `s01f15` | TODO(học viên) | TODO(học viên) | TODO(học viên) | TODO(học viên) | TODO(học viên) | TODO(học viên) |
| `s01f34` | TODO(học viên) | TODO(học viên) | TODO(học viên) | TODO(học viên) | TODO(học viên) | TODO(học viên) |

TODO(học viên): Liệt kê **3 điều chỉ camera cho biết** và **3 điều chỉ LiDAR cho biết**, lấy từ chính hai frame `s01f15` và `s01f34` ở trên.

Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| `s02f17` | TODO(học viên) | |
| `s02f45` | TODO(học viên) | |
| `s03f01` | TODO(học viên) | |
| `s03f41` | TODO(học viên) | |
| `s04f19` | TODO(học viên) | |
| `s04f46` | TODO(học viên) | |
| `s05f07` | TODO(học viên) | |
| `s05f39` | TODO(học viên) | |

## 3. Ba cuboid có hướng dẫn (Bước 3)

Ảnh chụp: `t2-a-car.png`, `t2-b-bus-truck.png`, `t2-c-ped-cyclist.png`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | TODO(học viên) | | | | | |
| (b) xe buýt hoặc xe tải | TODO(học viên) | | | | | |
| (c) người đi bộ hoặc xe hai bánh | TODO(học viên) | | | | | |

TODO(học viên): Ghi ngắn gọn (nếu lớp có bỏ qua vòng hiệu chuẩn theo hướng dẫn lab coach thì ghi rõ, còn nếu có thì ghi điểm lệch/chốt guideline). Sau đó liệt kê các vật bạn **bỏ qua vì không đủ cơ sở** (frame, vị trí, mô tả, loại nghi vấn như `UNCERTAIN_CLASS` / `UNCERTAIN_SCOPE` / `LOW_VISIBILITY`), hoặc ghi "không có".

## 4. Bốn hiện tượng chất lượng (Bước 4)

Nguyên nhân ghi là **giả thuyết**. Frame mượn thì ghi rõ nguồn (ví dụ MSSV của bạn cho mượn).

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | TODO(học viên) (`t4-1-bong-che.png`) | | | |
| 2. Thưa ở xa | TODO(học viên) (`t4-2-thua-o-xa.png`) | | | |
| 3. Phản xạ thấp / ít điểm bất thường | TODO(học viên) (`t4-3-phan-xa-thap.png`) | | | |
| 4. Điểm lạ | TODO(học viên) (`t4-4-diem-la.png`) | | | |

## 5. Số từ cuboid của tôi (Bước 5)

Lấy từ `submission/D13-2A202603144-bangA.csv`; số điểm lấy từ cột `points_in` của `submission/D13-2A202603144-tukiem.csv`. Mặt đường khoảng −1,7 m: z đáy lệch hơn 0,3 m là lơ lửng hoặc chìm.

| Cuboid (frame, idx, class) | z (m) | h (m) | z đáy = z − h/2 (m) | dist_m (m) | r = √(x²+y²+z²) (m) | yaw_deg (°) | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| TODO(học viên) | | | | | | | |
| TODO(học viên) | | | | | | | |
| TODO(học viên) | | | | | | | |

TODO(học viên):
- **(a) Cuboid gần nhất và xa nhất:** ghi `frame`, `idx`, `class`, `dist_m` (m) và `r` (m) của từng cái.
- **(b) So kích thước với bảng 4.3:** ghi `length`, `width`, `height` (m) của một người đi bộ (hoặc xe hai bánh) và một xe con so với khoảng tham chiếu ở bảng 4.3; chiều nào nằm ngoài khoảng thì giải thích vì sao.
- **(c) Góc hướng `yaw_deg`:** ghi `yaw_deg` (°) của một xe cùng chiều phía trước và một xe ngược chiều (nếu có); nhận xét có gần `270°` và `90°` không.

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m (m) | Số điểm (`points_in`) |
|---|---|---|
| TODO(học viên) | | |
| TODO(học viên) | | |
| TODO(học viên) | | |
| TODO(học viên) | | |

TODO(học viên): Viết một câu quy luật ("xa gấp … lần thì số điểm còn khoảng …") và so với đường cong ở slide bài giảng (xe con, trung vị: khoảng 1968 điểm ở 0–10 m, 718 ở 10–20 m, 214 ở 20–30 m, 72 ở 30–40 m, 19 ở 60–80 m). Sau đó ghi phần tự kiểm: **ít nhất 2 cuboid bạn đã sửa** sau `preview` / `selfcheck` (`frame`, `idx`, sửa gì), và những cảnh báo bạn **giữ nguyên** kèm lý do.

TODO(học viên): **Đổi kiểm với bạn ngồi cạnh:** MSSV người đã kiểm chéo với bạn, hai con số nào được đối chiếu với bảng A, khớp hay lệch (lệch thì đã sửa gì).

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

TODO(học viên):
- **(a) Từ phía model học trên nhãn này (2 câu):**
  - Vì sao nhãn 3D đo bằng mét chứ không phải pixel? ...
  - Vì sao vật thưa điểm cần quy tắc riêng (15–20%, không đoán)? ...
- **(b) Cách dùng AI (3 dòng — công cụ, câu hỏi chính, bạn dùng kết quả thế nào):**
  - Công cụ: ...
  - Câu hỏi chính: ...
  - Cách dùng kết quả: ...
- **(c) Điều còn chưa chắc / lỗi phát hiện sau hạn** *(không chấm; viết bằng lời, không chép số từ phiếu hồ sơ lỗi)*: ...
