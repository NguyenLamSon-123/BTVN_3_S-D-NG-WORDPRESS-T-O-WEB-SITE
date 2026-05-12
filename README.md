# BTVN_3_S-D-NG-WORDPRESS-T-O-WEB-SITE
# nguyễn lam sơn_K225480106076
# MSSV_K225480106076
## Bài Làm 
# bước 1: SỬ DỤNG DOCKER TRÊN UBUNTU ĐỂ TẠO docker ccompose chứa:
Mariadb: sử dụng image: mariadb:latest để làm hệ quản trị csdl cho wordpress <br>
Phpmyadmin: sư dụng image: phpmyadmin:latest để đăng nhập vào mariadb rồi tạo csdl trống (chỉ để xem, ko cần tạo bảng từ đây, wordpress sẽ làm hết) <br>
WordPress: Sử dụng image: wordpress:latest, truyền các tham số môi trường cho wordpress là các thông tin truy cập csdl mariadb, tạo bởi Phpmyadmin <br>
## file yml:
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1ed690db-62c0-4560-a8d0-7f3fd4c72078" />
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d3ef05cf-ea4a-4e64-a5db-5c3771a2bb07" />
## Chạy lệnh sau để tải images và chạy các containers ngầm
docker compose up -d
# <img width="1104" height="317" alt="image" src="https://github.com/user-attachments/assets/3d0641d1-6f12-49c3-937e-bb2cb2726f08" />
# Bước 2: Public Website qua Cloudflare Tunnel lên Sub-domain
thêm router 
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/32c1b563-d5d7-4ce8-a633-dd8442bc0901" />
## trong bài em tạo folder tnnel_gateway để tạo file nginx kết nối các mạng với nhau 
## docker‑compose.yml:
# Khởi động hai container: nginx (reverse‑proxy) và cloudflared (client của Cloudflare Tunnel) và tạo mạng Docker tunnel_shared mà các dự án khác có thể tham gia.
## nginx/default.conf:
# Chứa các cấu hình Virtual‑Host của Nginx, ánh xạ sub‑domain (ví dụ btvn3wordpress.nlamson1301.id.vn) tới các container nội bộ thông qua proxy_pass.
tunnel_gateway = cổng dùng để phơi bày (publish) tất cả các service Docker nội bộ lên Internet một cách an toàn thông qua Cloudflare Tunnel. <br>
Nó gồm Nginx (reverse‑proxy) + cloudflared (client tunnel) + một mạng Docker chung (tunnel_shared). <br>
Các dự án khác chỉ cần đính kèm vào mạng tunnel_shared và thêm một Virtual‑Host trong nginx/default.conf. <br>
Kết quả: không cần mở cổng inbound, bảo mật cao, dễ mở rộng và quản lý DNS đơn giản. <br>
# <img width="364" height="118" alt="image" src="https://github.com/user-attachments/assets/b0c9342c-f50f-4252-a260-daabfda7870f" />
# <img width="1427" height="576" alt="image" src="https://github.com/user-attachments/assets/664d6319-56ce-411a-b662-a84bf40b7ca9" />
# <img width="1361" height="869" alt="image" src="https://github.com/user-attachments/assets/51bded4a-a399-4b06-b5cd-3fc74c0344f7" />
# Bước 3: Hoàn thành các bài viết trên WordPress 
Tiêu đề gợi ý: Giới thiệu bản thân - [Họ và Tên của bạn] - [Mã Sinh Viên] <br> 
Nội dung: Viết một đoạn văn ngắn giới thiệu về tên tuổi, quê quán, lớp học. <br>
Media kèm theo: * Sử dụng block Image để tải lên ảnh chân dung hoặc ảnh sinh hoạt. <br>
Sử dụng block Audio để tải lên 1 file ghi âm ngắn lời chào (hoặc chèn link nhạc sở thích). <br>
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6b9a9b1d-991c-467a-8ab0-3564ab2baf2b" />
# https://btvn3wordpress.nlamson1301.id.vn/2026/05/12/bai-poster-gioi-thieu-ban-than/
# Bước 4: Nhận xét việc sử dụng mã nguồn mở WordPress
## Công sức triển khai:
Rất tiết kiệm thời gian. Thay vì phải code từ đầu (HTML/CSS/JS/PHP) và tự thiết kế database, WordPress cung cấp sẵn toàn bộ khung hệ thống (CMS). Kết hợp với Docker, việc dựng trọn vẹn cụm Web + Database chỉ tốn vài phút qua 1 file cấu hình duy nhất.
## Độ Dễ/Khó sử dụng:
Cực kỳ thân thiện và dễ dùng. Giao diện quản trị (Dashboard) trực quan, hỗ trợ kéo thả (Gutenberg block editor) giúp người không chuyên sâu về code vẫn đăng bài, chèn ảnh, video dễ dàng như dùng Word. Kho giao diện (Themes) và Trình cắm (Plugins) đồ sộ giúp mở rộng tính năng nhanh chóng.
## ốn kém tài nguyên máy chủ:
• RAM: Ở mức cơ bản (chạy qua Docker gồm MariaDB + WP), hệ thống tiêu tốn khoảng 300MB - 500MB RAM khi rảnh rỗi. Cần máy chủ tối thiểu 1GB RAM để chạy mượt mà. <br>
• CPU: Nhẹ nhàng khi ít truy cập, nhưng PHP/MySQL có thể ngốn CPU nếu cài quá nhiều Plugin nặng hoặc bị phình to cơ sở dữ liệu. <br>
• Ổ cứng (Storage): Source code ban đầu khá nhẹ (khoảng vài chục MB), nhưng sẽ tăng dần theo thời gian khi upload nhiều hình ảnh, video vào thư mục wp_data <br>
## Khả năng bảo trì & Bảo mật:
Dễ dàng sao lưu (backup) thông qua việc ánh xạ volume của Docker (db_data và wp_data). Tuy nhiên, vì là mã nguồn mở phổ biến nhất thế giới, WordPress thường xuyên là mục tiêu bị scan lỗ hổng, đòi hỏi quản trị viên phải update core và plugin thường xuyên.
