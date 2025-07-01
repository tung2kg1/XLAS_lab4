bài 1:
đọc ảnh "fruit.jpg" và chuyển sang dạng ảnh xám (mode='L')
kết quả là một mảng NumPy 2 chiều với giá trị độ sáng từ 0–255.
sử dụng scipy.ndimage.shift để dịch ảnh theo trục X và Y:
shift=(0, 30) nghĩa là:
0 pixel theo trục Y (chiều dọc)
30 pixel theo trục X (chiều ngang)
các pixel bị đẩy ra ngoài sẽ được làm trống bằng giá trị mặc định 0

bài 2:
cắt chính xác từng vùng ảnh bằng slicing img[y1:y2, x1:x2]
[:, :, ::-1] đảo thứ tự màu biến RGB thành BGR
kết quả là màu sắc của vùng ảnh bị thay đổi
đu đủ được gán lại vào vị trí cũ
dưa hấu được gán vào vị trí mới hơn

bài 3:
mountain và boat là vùng ảnh lấy theo tọa độ (y1:y2, x1:x2)
mỗi vùng có kích thước 100x150 pixel
hàm nd.rotate thực hiện xoay vùng ảnh 45 độ
reshape=True giúp mở rộng khung ảnh để không bị cắt mất góc

bài 4:
cắt vùng có chiều cao 100 pixels và chiều rộng 100 pixels từ vị trí (y=100:200, x=150:250) đây là vùng cần phóng to
hàm nd.zoom dùng để nội suy đa chiều
(5, 5, 1) là hệ số phóng đại theo từng chiều:
5 lần theo chiều cao
5 lần theo chiều rộng
1 giữ nguyên số kênh màu RGB

bài 5:
tinhtien(list)
  dịch chuyển ảnh theo trục hoành 30 pixels
  shift=(0, 30) nghĩa là không theo chiều dọc, chỉ dịch 30 pixel sang phải
  ảnh kết quả bị lệch phải, phần bên trái được điền mặc định màu đen

xoay(list)
  xoay ảnh một góc 20° theo chiều ngược kim đồng hồ
  reshape=False giúp giữ nguyên kích thước ảnh gốc
  các pixel ở góc có thể bị cắt nếu nằm ngoài khung gốc

phongTo(list)
  phóng to ảnh gấp 2 lần

thuNho(list)
  thu nhỏ ảnh xuống 10% kích thước ban đầu

cooridnate_map(list)
  chèn lưới tọa độ lên ảnh RGB
  các dòng và cột xen kẽ được tô màu đen [0,0,0], chia ảnh thành ô vuông 100x100 pixel

random_transform()
  tự động chọn ngẫu nhiên 1 phép biến đổi trong danh sách:
  "T": tinhtien
  "X": xoay
  "P": phongTo
  "H": thuNho
  "C": cooridnate_map
  biến list_of_images chứa các đường dẫn đến ảnh

bài tập thêm 1:
ảnh gốc được dịch sang phải 50 pixel, xuống dưới 30 pixel
một canvas trống (đen) được tạo để chứa ảnh sau khi tịnh tiến
áp dụng biến dạng hình sin cho trục x để ảnh gợn sóng theo chiều ngang
hàm sin() giúp tạo hiệu ứng sóng mượt, tự nhiên
dùng np.mgrid để tạo ma trận chỉ số cho từng pixel
các tọa độ được biến đổi theo sóng để áp dụng cho từng pixel
map_coordinates nhận lưới tọa độ mới và nội suy lại ảnh tương ứng, lặp cho từng kênh màu (R, G, B).

bài tập thêm 2:
ảnh được đưa về chế độ RGBA, cho phép kiểm tra và giữ vùng trong suốt
tạo một dải giá trị từ 0 đến 1 theo chiều ngang ảnh x
dùng để tính trọng số tuyến tính cho nội suy giữa 2 màu
chỉ áp dụng gradient cho những pixel có độ trong suốt alpha > 0
mỗi kênh màu (R, G, B) được tính bằng cách nội suy tuyến tính từ color1 đến color2 cho hiệu ứng chuyển màu mượt mà từ trái sang phải
canvas là một tấm nền có kênh alpha (RGBA), nên vẫn giữ được vùng trong suốt
ảnh được ghép kèm mặt nạ chính nó (giữ alpha), nên hiệu ứng trong suốt không bị mất

bài tập thêm 3:
ảnh gốc được chuyển sang NumPy array để xử lý pixel-level
reshape=False: không mở rộng ảnh sau xoay
mode='constant', cval=255: nền trắng
flipud lật ảnh theo trục dọc
tạo ảnh trắng mới đủ lớn để chứa cả hai ảnh đã xử lý
dán ảnh vào vị trí, canh chỉnh khoảng cách 25px, 50px

bài tập thêm 4:
(5, 5, 1): tăng chiều cao & rộng gấp 5 lần, giữ nguyên số kênh màu
order=1: dùng nội suy tuyến tính, tránh răng cưa
tạo hiệu ứng lượn sóng nhẹ bằng sin & cos
warp_factor * dx * r: tạo hiệu ứng uốn cong từ tâm ảnh
duyệt từng kênh màu (R, G, B) và ánh xạ lại giá trị pixel theo tọa độ mới
mode='reflect': xử lý vùng ảnh ngoài biên bằng phản chiếu

bài tập thêm 5:
gaussian_blur(): 
  gaussian_filter(data, sigma=sigma): 
  sigma càng lớn, ảnh càng mờ
  bộ lọc Gaussian sử dụng phép tích chấp, tạo ra một ma trận filter kernel mới và áp dụng vào tâm của ảnh, các pixel càng ở xa tâm thì càng ít bị ảnh hưởng bởi ma trận kernel này

wave_transform(): 
  hàm sin có giá trị biến thiên liên tục từ -1 đến 1 nên hợp để tạo hiệu ứng sóng
  x, y = coords để lấy giá trị kích thước của hình, x là chiều ngang trong khi y là chiều dọc
  sin(y*0.1) để làm cho hình có hiệu ứng gợn sóng theo chiều dọc, lý do nhân y với 0.1 là để quyết định tần số của hiệu ứng gợn sóng, tham số này tỉ lệ thuận với tần số
  tham số amplitude là biên độ giao động của sóng
