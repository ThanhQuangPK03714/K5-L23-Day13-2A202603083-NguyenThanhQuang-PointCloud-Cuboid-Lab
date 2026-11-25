# Báo cáo Day 13 — Nguyễn Thanh Quang, 2A202603083

> Báo cáo dùng Bảng A và phiếu tự kiểm hiện có trong `submission/`. Những nhận xét đòi hỏi đọc trực tiếp ảnh CVAT được đánh dấu rõ khi chưa có thông tin xác nhận; không suy diễn từ ảnh chụp.

## 1. Kết luận trước

- **Học viên:** Nguyễn Thanh Quang — **MSSV:** `2A202603083`.
- **Task:** `D13-2A202603083`. **10 frame:** `s01f13`, `s01f40`, `s02f07`, `s02f30`, `s03f23`, `s03f56`, `s04f11`, `s04f50`, `s05f20`, `s05f44`.
- **Kết quả Bảng A trước lần sửa CVAT:** 80 cuboid trên 10 frame; ít nhất là `s03f23` với 5 cuboid. Phiếu tự kiểm có 23/80 cuboid mang cảnh báo. Đây là cảnh báo để xem lại, không phải kết luận hộp sai.
- Dòng hướng dẫn cấu hình cho biết username CVAT là `ThanhQuang`; môi trường làm việc hiện tại là Windows/PowerShell. Trình duyệt và phiên bản CVAT thực tế chưa được xác nhận.
- Sau khi nhìn các ảnh minh chứng hiện có, độ tin cậy cao hơn ở hộp gần có nhiều điểm; thấp nhất ở các hộp xa/thưa và hộp có kích thước/class bất thường. Đây là đánh giá định tính từ ảnh và phiếu tự kiểm, không phải điểm chính xác.
- Hai hộp ngoài phạm vi là `s01f13#8` (53,11 m) và `s03f23#3` (80,24 m). Cả hai cần được xử lý trong CVAT; với `s03f23`, cần có hộp khác hợp lệ trong phạm vi để vẫn đạt tối thiểu 5 cuboid ở frame.

**Thao tác CVAT:** theo hướng dẫn, xoay khung 3D để nhìn từ trên xuống, dùng kéo để xoay/dời và con lăn để phóng; tạo Shape rồi chỉnh hộp qua Top/Side/Front; bật hiển thị hướng cuboid và đối chiếu ảnh camera. Việc đã thực hiện từng thao tác và đã chạy `preview` chưa được ghi lại trong thông tin hiện có.

## 2. Đọc frame (Bước 2)

Theo hệ tọa độ VF, phía trước xe thu dữ liệu là hướng `−y`, `+x` hướng sang trái, `+y` hướng về sau, `+z` hướng lên. Mặt đường thường quanh `z ≈ −1,7 m`. Hai frame đọc kỹ theo thứ tự được giao là `s01f13` và `s01f40`.

| Frame | Phía trước trên ảnh top; hướng trục | Mặt đường | Vật xa nhất đọc được | Loại vật; phía sau/hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|
| `s01f13` | Ảnh CVAT `t2-c-ped-cyclist.png` cho thấy góc 3D nhìn dọc một đoạn đường; hướng tiến của cảnh ước về phía trên ảnh. Không có ảnh Top riêng để xác nhận cạnh trước hay mũi tên `+X/+Y`. | Mặt đường trong ảnh camera nhìn khá phẳng; độ dốc không đo được từ ảnh chụp. | Bảng A có hộp xa nhất ở 53,11 m (`#8`); đây là tâm hộp, không khẳng định giới hạn nhìn thấy của LiDAR. | Bảng A có Car, Bus, Pedestrian; camera thấy xe trên đường. Cụm điểm chưa đọc được không được ghi chú riêng. | Chưa có ghi chú phân biệt chắc chắn các cụm mơ hồ. |
| `s01f40` | Ảnh 3D cho thấy đường kéo sâu về phía trên màn hình; xem đây là hướng trước ước lượng. Trên ảnh Top không thấy nhãn trục đủ rõ để chốt hướng `+X/+Y`. | Mặt đường có vẻ tương đối phẳng, chưa đo độ dốc; nền quanh xe gần LiDAR xấp xỉ mức đường của sequence. | Hộp xa nhất trong Bảng A cách 43,25 m (`#7`); đây là hộp đã gán, không phải kết luận vật xa nhất toàn cảnh. | Camera/point cloud cho thấy nhiều xe con trước, hai bên và phía sau. Cụm xa vẫn thưa và khó xác định ranh giới. | Các vùng điểm rất thưa ngoài rìa chưa đủ cơ sở để gọi là vật thể cụ thể. |

**Chỉ camera cho biết:** màu sắc/ngoại hình, dấu hiệu giúp phân biệt đầu và đuôi xe, và hình ảnh các phần bị LiDAR che hoặc thiếu điểm. **Chỉ LiDAR cho biết:** khoảng cách theo mét, hình học 3D quanh xe theo 360°, và các điểm nằm ngoài góc nhìn camera. Đây là khác biệt theo nguyên lý cảm biến; ví dụ cụ thể ở hai frame chưa được ghi lại.

Tám frame lướt còn lại là `s02f07`, `s02f30`, `s03f23`, `s03f56`, `s04f11`, `s04f50`, `s05f20`, `s05f44`. Ảnh `t1-s02f07-top.png` cho thấy một giao lộ rộng với nhiều luồng xe; mặt đường nhìn tương đối phẳng. Các frame khác chưa có ảnh T1 kèm hướng top riêng, nên hướng trước từng ảnh chưa thể xác nhận. Điểm bất thường ngoài hai hộp xa đã nêu chưa được ghi trong bằng chứng hiện có.

**Ảnh T1 đang có:** `t1-s01f40-3d.png`, `t1-s01f40-top.png`, `t1-s02f07-3d.png`, `t1-s02f07-top.png`. Hai ảnh đúng frame cần thiết là `s01f40`; bộ còn lại đang là `s02f07`, cần thay bằng `s01f13` để khớp yêu cầu T1.

## 3. Ba cuboid có hướng dẫn (Bước 3)

Đối chiếu ảnh T2 với frame và số đo trong Bảng A cho thấy ảnh xe con là `s01f40#5`, ảnh Bus là `s01f13#7`, ảnh người/xe hai bánh ở frame `s01f13` tương ứng hộp Pedestrian `#3`. Kiểm tra Top/Side/Front dưới đây dựa trên phần hiển thị trong ảnh chụp; ảnh không thay thế việc mở lại task nếu cần chỉnh.

| Vật tham chiếu | Frame, idx | Class | Kích thước (d × r × c, m) | Yaw (°) | Ground gap (m) | Kết quả kiểm tra ảnh/Top/Side/Front |
|---|---|---|---|---:|---:|---|
| Xe con | `s01f40#5` | Car | 4,480 × 1,814 × 1,463 | 155,5 | −0,02 | Ảnh T2 cho thấy hộp bao xe trong các view; kích thước phù hợp xe con. Hướng cần so với đầu xe trên cam05; không đủ cơ sở khẳng định yaw đúng tuyệt đối. |
| Xe buýt | `s01f13#7` | Bus | 4,101 × 1,884 × 1,778 | 281,8 | −0,02 | Ảnh cho thấy hộp Bus trên đoạn đường; kích thước ngắn/thấp hơn nhiều so với bảng 4.3, cần sửa hoặc xác nhận lại class/hộp. |
| Người đi bộ/xe hai bánh | `s01f13#3` | Pedestrian | 0,412 × 0,642 × 1,423 | 0,0 | +0,51 | Ảnh CVAT hiển thị hộp quanh cụm điểm; cần kiểm tra lại vì ground gap và chiều cao bị cờ. Không thể khẳng định đáy/hướng chính xác từ ảnh này. |

Các điểm chưa đủ cơ sở để gán nhãn hoặc các hộp đã bỏ qua chưa được ghi trong dữ liệu nộp; cần bổ sung frame/vị trí và loại nghi vấn nếu có. Vòng hiệu chuẩn và điểm khác biệt với học viên khác cũng chưa được ghi lại.

## 4. Bốn hiện tượng chất lượng (Bước 4)

Tên bốn tệp ảnh thể hiện chủ đề ảnh chụp. Nguyên nhân dưới đây là giả thuyết chung phù hợp với hướng dẫn lab; nội dung point cloud cụ thể trong ảnh chưa được xác nhận nên không khẳng định là quan sát trực tiếp.

| Hiện tượng | Ảnh / nguồn | Nguyên nhân (giả thuyết) | Dấu hiệu cần kiểm trên point cloud | Hệ quả khi gán |
|---|---|---|---|---|
| Bóng che | `t4-1-bong-che.png`; frame `s04f11` (suy từ 12 hộp và IDs hiển thị) | Dải chắn đường/xe lớn có thể chặn một phần tia LiDAR. | Ảnh có dải chắn ở bên đường; sau một số bề mặt thấy vùng điểm thưa hơn vùng lân cận. | Vùng sau vật che là không quan sát được; không kết luận đường trống, bật `occluded` cho vật bị che nếu được gán. |
| Thưa ở xa | `t4-2-thua-o-xa.png`; frame `s04f11` (suy từ danh sách hộp trong ảnh) | Khoảng cách làm mật độ giảm; nguyên nhân cụ thể của ít điểm chưa được đo riêng. | Xe xa trên đường có ít điểm nằm trong hộp hơn xe gần, một số hộp lớn nhưng cloud bên trong thưa. | Chỉ giữ hộp khi đủ tỷ lệ điểm nhìn thấy hoặc camera nhận dạng được; nếu không ghi `LOW_VISIBILITY`/bỏ qua. |
| Phản xạ thấp/ít điểm bất thường | `t4-3-phan-xa-thap.png`; frame chưa xác định chắc chắn | Bề mặt tối/kính có thể phản hồi yếu; có thể chỉ là khoảng cách hoặc góc chiếu. | Ảnh camera thấy xe nhưng phần cloud gắn với hộp được chọn khá thưa so với xe gần hơn. | Đây là giả thuyết, không kết luận chỉ từ màu/intensity; đối chiếu ảnh và hình học trước khi đặt hộp. |
| Điểm lạ | `t4-4-diem-la.png`; frame `s05f44` (tên cloud hiển thị) | Nhiễu đo hoặc phản xạ đa đường có thể sinh điểm không thuộc vật; chưa xác định nguyên nhân. | Toàn cảnh có vài điểm/cụm rời ngoài các hộp xe; ảnh hiện tại không đủ phân giải để xác định một cụm cụ thể. | Không biến cụm rời thành cuboid nếu không có bằng chứng đó là vật thật. |

Các ảnh T4 đều là ảnh trong task đang mở trên màn hình CVAT; riêng T4-3 không đọc được tên frame từ ảnh, cần giữ nguồn là task cá nhân nhưng frame chưa xác định.

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

Các số trong mục này là **trước lần sửa CVAT tiếp theo**: phiếu có 23/80 dòng gắn cờ; hai hộp ngoài phạm vi là `s01f13#8` (53,11 m) và `s03f23#3` (80,24 m). Không được loại hộp bằng cách sửa CSV; cần cập nhật annotation trên CVAT rồi chạy `sync`. Nếu bỏ `s03f23#3`, frame còn 4 hộp trong phạm vi, vì vậy cần thêm một vật thật trong phạm vi trước khi đồng bộ để đạt tối thiểu 5. Lịch sử chỉnh sửa CVAT không có trong repo nên chưa thể xác nhận hai hộp nào đã sửa sau preview/selfcheck, hoặc cờ nào đã được xem và chủ động giữ nguyên. MSSV bạn đổi kiểm và hai số đã đối chiếu cũng chưa được cung cấp.

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

Nhãn 3D dùng mét vì mô hình cần vị trí, kích thước và khoảng cách vật thể trong không gian thực; pixel chỉ biểu diễn vị trí trên một ảnh camera và thay đổi theo góc nhìn/khoảng cách. Vật thưa điểm cần quy tắc riêng vì thiếu bằng chứng hình học có thể khiến người gán nhãn vẽ sai vị trí, kích thước hoặc class; chỉ nên vẽ khi phần nhìn thấy đủ hoặc ảnh camera cho cơ sở nhận dạng.

- **AI:** ChatGPT/Codex; yêu cầu hỗ trợ hoàn tất báo cáo và rà soát các số liệu/ảnh minh chứng; dùng để tính lại thống kê, đối chiếu nội dung ảnh chụp CVAT và soạn bản nháp. Các kết luận từ ảnh cần học viên mở lại CVAT xác nhận trước khi nộp.
- **Điều còn chưa chắc:** các ghi chú quan sát trực tiếp ở §2–§4, mapping ba ảnh T2 với frame/idx, nguồn ảnh T4, các cuboid đã sửa/giữ cảnh báo, đổi kiểm, trình duyệt và phiên bản CVAT chưa được cung cấp. Các mục này cần bổ sung từ ghi chép/CVAT trước khi xem báo cáo là bản cuối.
- Hai hộp `s01f13#8` và `s03f23#3` đều vượt 50 m. Việc chỉnh chúng trên CVAT và đồng bộ Bảng A/phiếu tự kiểm chưa hoàn thành; không sửa tay CSV.
