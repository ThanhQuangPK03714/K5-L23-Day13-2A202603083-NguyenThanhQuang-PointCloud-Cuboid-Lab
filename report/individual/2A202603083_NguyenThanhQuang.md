# Báo cáo Day 13 — Nguyễn Thanh Quang, 2A202603083

> Báo cáo dùng Bảng A và phiếu tự kiểm hiện có trong `submission/`. Những nhận xét đòi hỏi đọc trực tiếp ảnh CVAT được đánh dấu rõ khi chưa có thông tin xác nhận; không suy diễn từ ảnh chụp.

## 1. Kết luận trước

- **Học viên:** Nguyễn Thanh Quang — **MSSV:** `2A202603083`.
- **Task:** `D13-2A202603083`. **10 frame:** `s01f13`, `s01f40`, `s02f07`, `s02f30`, `s03f23`, `s03f56`, `s04f11`, `s04f50`, `s05f20`, `s05f44`.
- **Kết quả bảng hiện tại:** 80 cuboid trên 10 frame; ít nhất là `s03f23` với 5 cuboid. Phiếu tự kiểm có 23/80 cuboid mang cảnh báo. Đây là cảnh báo để xem lại, không phải kết luận hộp sai.
- Dòng hướng dẫn cấu hình cho biết username CVAT là `ThanhQuang`; môi trường làm việc hiện tại là Windows/PowerShell. Trình duyệt và phiên bản CVAT thực tế chưa được xác nhận.
- Độ tin cậy về vị trí, kích thước và class cần được đánh giá lại trên từng frame. Riêng `s03f23#3` có tâm cách LiDAR 80,24 m, vượt phạm vi 50 m của bài; cần kiểm tra lại hoặc bỏ hộp này theo quy tắc phạm vi.

**Thao tác CVAT:** theo hướng dẫn, xoay khung 3D để nhìn từ trên xuống, dùng kéo để xoay/dời và con lăn để phóng; tạo Shape rồi chỉnh hộp qua Top/Side/Front; bật hiển thị hướng cuboid và đối chiếu ảnh camera. Việc đã thực hiện từng thao tác và đã chạy `preview` chưa được ghi lại trong thông tin hiện có.

## 2. Đọc frame (Bước 2)

Theo hệ tọa độ VF, phía trước xe thu dữ liệu là hướng `−y`, `+x` hướng sang trái, `+y` hướng về sau, `+z` hướng lên. Mặt đường thường quanh `z ≈ −1,7 m`. Hai frame đọc kỹ theo thứ tự được giao là `s01f13` và `s01f40`.

| Frame | Phía trước trên ảnh top; hướng trục | Mặt đường | Vật xa nhất đọc được | Loại vật; phía sau/hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|
| `s01f13` | Chưa có ghi chú vị trí trên ảnh top; hướng thực tế của `+x/+y` trên ảnh cần xác nhận. | Chưa ghi nhận quan sát trực tiếp. | Chưa ghi nhận. | Bảng A có Car, Bus và Pedestrian; vị trí phía sau/hai bên chưa ghi nhận. | Chưa ghi nhận. |
| `s01f40` | Chưa có ghi chú vị trí trên ảnh top; hướng thực tế của `+x/+y` trên ảnh cần xác nhận. | Chưa ghi nhận quan sát trực tiếp. | Chưa ghi nhận. | Bảng A có Car; vị trí phía sau/hai bên chưa ghi nhận. | Chưa ghi nhận. |

**Chỉ camera cho biết:** màu sắc/ngoại hình, dấu hiệu giúp phân biệt đầu và đuôi xe, và hình ảnh các phần bị LiDAR che hoặc thiếu điểm. **Chỉ LiDAR cho biết:** khoảng cách theo mét, hình học 3D quanh xe theo 360°, và các điểm nằm ngoài góc nhìn camera. Đây là khác biệt theo nguyên lý cảm biến; ví dụ cụ thể ở hai frame chưa được ghi lại.

Tám frame lướt còn lại là `s02f07`, `s02f30`, `s03f23`, `s03f56`, `s04f11`, `s04f50`, `s05f20`, `s05f44`; hướng trước trên ảnh top và điểm bất thường từng frame chưa được ghi lại.

**Ảnh T1 đang có:** `t1-s01f40-3d.png`, `t1-s01f40-top.png`, `t1-s02f07-3d.png`, `t1-s02f07-top.png`. Tên ảnh cho biết ảnh thứ hai là `s02f07`, trong khi hai frame đọc kỹ đầu danh sách là `s01f13` và `s01f40`; cần xác nhận/đổi bộ ảnh để khớp yêu cầu T1.

## 3. Ba cuboid có hướng dẫn (Bước 3)

Các dòng dưới đây là ví dụ lấy từ Bảng A theo class; số liệu không xác nhận rằng đây chính là ba hộp trong ảnh T2. `yaw` và `ground_gap` là giá trị ghi trong CSV/phiếu, không thay cho việc kiểm tra hình học trực quan.

| Vật tham chiếu | Frame, idx | Class | Kích thước (d × r × c, m) | Yaw (°) | Ground gap (m) | Kết quả kiểm tra ảnh/Top/Side/Front |
|---|---|---|---|---:|---:|---|
| Xe con gần | `s01f40#1` | Car | 4,991 × 2,171 × 1,611 | 270,6 | −0,12 | Chưa xác nhận bằng ảnh. |
| Xe buýt | `s02f07#1` | Bus | 10,221 × 2,772 × 3,210 | 83,1 | +0,07 | Chưa xác nhận bằng ảnh. |
| Xe hai bánh có người | `s02f07#3` | Cyclist | 1,591 × 1,494 × 0,898 | 280,6 | +0,10 | Chưa xác nhận bằng ảnh. |

Các điểm chưa đủ cơ sở để gán nhãn hoặc các hộp đã bỏ qua chưa được ghi trong dữ liệu nộp; cần bổ sung frame/vị trí và loại nghi vấn nếu có. Vòng hiệu chuẩn và điểm khác biệt với học viên khác cũng chưa được ghi lại.

## 4. Bốn hiện tượng chất lượng (Bước 4)

Tên bốn tệp ảnh thể hiện chủ đề ảnh chụp. Nguyên nhân dưới đây là giả thuyết chung phù hợp với hướng dẫn lab; nội dung point cloud cụ thể trong ảnh chưa được xác nhận nên không khẳng định là quan sát trực tiếp.

| Hiện tượng | Ảnh / nguồn | Nguyên nhân (giả thuyết) | Dấu hiệu cần kiểm trên point cloud | Hệ quả khi gán |
|---|---|---|---|---|
| Bóng che | `t4-1-bong-che.png`; frame/nguồn chưa ghi | Vật gần như xe buýt, xe tải hoặc tường có thể chặn tia LiDAR. | Tìm vùng thưa/thiếu điểm phía sau vật che trong khi hai bên còn điểm. | Phần khuất là chưa quan sát được, không phải đường trống; bật `occluded` và chỉ suy luận phần vật có cơ sở. |
| Thưa ở xa | `t4-2-thua-o-xa.png`; frame/nguồn chưa ghi | Mật độ điểm thường giảm theo khoảng cách. | Tìm vật ở khoảng 30–50 m chỉ còn vài chục điểm. | Chỉ vẽ nếu có đủ khoảng 15–20% thể tích hoặc ảnh camera giúp nhận dạng; nếu không thì bỏ qua và ghi `LOW_VISIBILITY`. |
| Phản xạ thấp/ít điểm bất thường | `t4-3-phan-xa-thap.png`; frame/nguồn chưa ghi | Bề mặt tối hoặc kính có thể phản xạ ít hơn; đây chỉ là giả thuyết. | So mật độ/độ phản hồi với vật cùng tầm, đồng thời kiểm tra ảnh camera. | Không kết luận class hay nguyên nhân chỉ từ intensity; cần đủ bằng chứng hình học/ảnh. |
| Điểm lạ | `t4-4-diem-la.png`; frame/nguồn chưa ghi | Nhiễu đo, phản xạ đa đường hoặc lỗi cảm biến có thể tạo điểm không phù hợp hình học; chưa xác định nguyên nhân. | Kiểm tra cụm điểm lẻ loi, giữa không khí hoặc nằm dưới mặt đường. | Không tạo cuboid cho điểm lạ nếu không có bằng chứng đó là vật thật. |

Theo hướng dẫn nộp, nếu ảnh lấy từ task chung hoặc task người khác thì phải nêu nguồn; nguồn của bốn ảnh hiện chưa được cung cấp.

## 5. Số từ cuboid của tôi (Bước 5)

`z đáy = z − h/2`; ground gap âm nghĩa là đáy thấp hơn mặt đường ước lượng, dương nghĩa là cao hơn. Ngưỡng nhận xét theo hướng dẫn là khoảng 0,3 m.

| Cuboid (frame, idx, class) | z (m) | h (m) | z đáy (m) | dist_m (m) | r = √(x²+y²+z²) (m) | yaw (°) | Nhận xét từ phiếu |
|---|---:|---:|---:|---:|---:|---:|---|
| `s01f40#1`, Car | −1,132 | 1,611 | −1,937 | 3,41 | 3,59 | 270,6 | Gap −0,12 m: gần mặt đường ước lượng. |
| `s01f13#3`, Pedestrian | −0,496 | 1,423 | −1,208 | 9,55 | 9,57 | 0,0 | Gap +0,51 m: cờ lơ lửng, cần xem lại trên CVAT. |
| `s02f07#1`, Bus | −0,263 | 3,210 | −1,868 | 14,21 | 14,22 | 83,1 | Gap +0,07 m: gần mặt đường ước lượng. |

- **Gần nhất trong bảng:** `s01f40#1` Car, `dist_m = 3,41 m`, `r = 3,59 m`.
- **Xa nhất trong bảng:** `s03f23#3` Bus, `dist_m = 80,24 m`, `r = 80,24 m`. Vật này ngoài phạm vi bài (`≤50 m`) và kích thước `5,009 × 2,113 × 1,972 m` cũng bị cờ ngoài khoảng class Bus; đây là mục cần kiểm tra ưu tiên. Xa nhất nếu chỉ xét `≤50 m` là `s01f13#10` Car, `dist_m = 49,59 m`, `r = 49,61 m`.
- **So kích thước tham chiếu:** Pedestrian `s01f13#3` có `0,412 × 0,642 × 1,423 m`; chiều cao thấp hơn khoảng người lớn (1,50–1,95 m), có thể là trẻ em/tư thế khom hoặc hộp chưa sát—cần xác nhận ảnh. Car `s01f40#2` có `4,563 × 1,980 × 1,645 m`, gần khoảng SUV/minivan/van chở người (4,4–5,3 × 1,8–2,1 × 1,65–2,1 m); cao thấp hơn cận dưới khoảng 0,005 m, có thể do làm tròn/đặt hộp. Không thể kết luận kiểu xe chỉ bằng kích thước.
- **Yaw ghi trong Bảng A:** `s01f40#1` là 270,6° (gần hướng cùng chiều tham chiếu 270°); `s02f30#1` là 88,8° (gần hướng ngược chiều tham chiếu 90°). Cần đối chiếu hướng thực tế trên ảnh để xác nhận phân loại cùng/ngược chiều.

| Cuboid | dist_m (m) | points_in |
|---|---:|---:|
| `s01f40#1` Car | 3,41 | 2.073 |
| `s02f07#3` Cyclist | 10,43 | 430 |
| `s04f50#5` Car | 30,71 | 145 |
| `s05f44#8` Car | 48,43 | 78 |

Trong mẫu này, khoảng cách tăng từ 3,41 m lên 10,43 m (khoảng 3,1 lần) thì số điểm giảm từ 2.073 xuống 430 (còn khoảng 21%); ở 48,43 m còn 78 điểm. Xu hướng ít điểm hơn ở xa phù hợp với ví dụ slide, nhưng các vật khác class và kích thước nên không thể xem đây là phép so sánh cùng một vật hay một quy luật chính xác.

Phiếu tự kiểm có 23/80 dòng được gắn cờ. Hai cờ cần ưu tiên xem lại là `s01f13#3` (đáy cao hơn mặt đường 0,51 m) và `s03f23#3` (80,24 m, class Bus/kích thước bất thường). Lịch sử chỉnh sửa CVAT không có trong repo nên chưa thể xác nhận hai hộp nào đã sửa sau preview/selfcheck, hoặc cờ nào đã được xem và chủ động giữ nguyên. MSSV bạn đổi kiểm và hai số đã đối chiếu cũng chưa được cung cấp.

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

Nhãn 3D dùng mét vì mô hình cần vị trí, kích thước và khoảng cách vật thể trong không gian thực; pixel chỉ biểu diễn vị trí trên một ảnh camera và thay đổi theo góc nhìn/khoảng cách. Vật thưa điểm cần quy tắc riêng vì thiếu bằng chứng hình học có thể khiến người gán nhãn vẽ sai vị trí, kích thước hoặc class; chỉ nên vẽ khi phần nhìn thấy đủ hoặc ảnh camera cho cơ sở nhận dạng.

- **AI:** Không dùng AI.
- **Điều còn chưa chắc:** các ghi chú quan sát trực tiếp ở §2–§4, mapping ba ảnh T2 với frame/idx, nguồn ảnh T4, các cuboid đã sửa/giữ cảnh báo, đổi kiểm, trình duyệt và phiên bản CVAT chưa được cung cấp. Các mục này cần bổ sung từ ghi chép/CVAT trước khi xem báo cáo là bản cuối.
- `s03f23#3` có khoảng cách vượt 50 m; cần xác nhận đây là hộp cần loại khỏi bài và cập nhật CVAT/Bảng A/phiếu tự kiểm nếu có vẽ nhầm. Không sửa tay CSV.
