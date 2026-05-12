# BTVN_3_S-D-NG-WORDPRESS-T-O-WEB-SITE
# nguyễn lam sơn_K225480106076
# MSSV_K225480106076
## Bài Làm 
## bước 1: SỬ DỤNG DOCKER TRÊN UBUNTU ĐỂ TẠO docker ccompose chứa:
Mariadb: sử dụng image: mariadb:latest để làm hệ quản trị csdl cho wordpress <br>
Phpmyadmin: sư dụng image: phpmyadmin:latest để đăng nhập vào mariadb rồi tạo csdl trống (chỉ để xem, ko cần tạo bảng từ đây, wordpress sẽ làm hết) <br>
WordPress: Sử dụng image: wordpress:latest, truyền các tham số môi trường cho wordpress là các thông tin truy cập csdl mariadb, tạo bởi Phpmyadmin <br>
## file yml :
version: '3.8'

services: <br>
  db: <br> 
    image: mariadb:latest
    container_name: btvn3_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_DATABASE: wordpress_db
      MYSQL_USER: wp_user
      MYSQL_PASSWORD: wp_password
    volumes:
      - db_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: btvn3_phpmyadmin
    restart: always
    ports:
      - "8080:80"
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: root_password
    depends_on:
      - db

  wordpress:
    image: wordpress:latest
    container_name: btvn3_wordpress
    restart: always
    ports:
      - "8000:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: wp_password
      WORDPRESS_DB_NAME: wordpress_db
    depends_on:
      - db
    volumes:
      - wp_data:/var/www/html

volumes:
  db_data:
  wp_data:
## Chạy lệnh sau để tải images và chạy các containers ngầm
docker compose up -d
# <img width="1104" height="317" alt="image" src="https://github.com/user-attachments/assets/3d0641d1-6f12-49c3-937e-bb2cb2726f08" />
## Bước 2: Public Website qua Cloudflare Tunnel lên Sub-domain
thêm router 
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/32c1b563-d5d7-4ce8-a633-dd8442bc0901" />
## trong bài em tạo folder tnnel_gateway để tạo file nginx kết nối các mạng với nhau 
## docker‑compose.yml: Khởi động hai container: nginx (reverse‑proxy) và cloudflared (client của Cloudflare Tunnel) và tạo mạng Docker tunnel_shared mà các dự án khác có thể tham gia.
## nginx/default.conf: Chứa các cấu hình Virtual‑Host của Nginx, ánh xạ sub‑domain (ví dụ btvn3wordpress.nlamson1301.id.vn) tới các container nội bộ thông qua proxy_pass.
tunnel_gateway = cổng dùng để phơi bày (publish) tất cả các service Docker nội bộ lên Internet một cách an toàn thông qua Cloudflare Tunnel. <br>
Nó gồm Nginx (reverse‑proxy) + cloudflared (client tunnel) + một mạng Docker chung (tunnel_shared). <br>
Các dự án khác chỉ cần đính kèm vào mạng tunnel_shared và thêm một Virtual‑Host trong nginx/default.conf. <br>
Kết quả: không cần mở cổng inbound, bảo mật cao, dễ mở rộng và quản lý DNS đơn giản. <br>
# 
