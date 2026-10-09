# Website Quảng bá Sản phẩm - Chè Thái Nguyên

**Họ tên:** Đinh Bảo Khanh  
**MSSV:** DTC245200018  
**Đề tài:** Website Quảng bá Sản phẩm (WordPress)

## 1. Mô tả hệ thống

Hệ thống triển khai website WordPress quảng bá sản phẩm Chè Thái Nguyên với đầy đủ các thành phần:

- Ứng dụng: WordPress + MySQL + phpMyAdmin
- Reverse Proxy: Nginx (HTTP → HTTPS, Security Headers, HSTS)
- Giám sát: Prometheus + Grafana + node-exporter + cAdvisor
- Log tập trung: Loki + Promtail (LogQL)
- Hardening: non-root, network isolation, no-new-privileges, read-only, mật khẩu mạnh, hạn chế quyền DB

## 2. Yêu cầu hệ thống

- Ubuntu (đã cài Docker + Docker Compose)
- Tối thiểu 2GB RAM
- Cổng 80, 443, 3000, 9090, 3100 chưa bị chiếm

## 3. Cách chạy

1. Clone repository:
   git clone https://github.com/dinhbaokhanh01/Website_Quang_Ba_San_Pham.git
   cd Website_Quang_Ba_San_Pham

2. Tạo file .env (tham khảo mẫu bên dưới)

3. Khởi động:
   docker compose up -d
   docker compose ps

### File .env mẫu

MSSV=DTC245200018
WP_DB=wp_DTC245200018
WP_USER=wp_DTC245200018
WP_PASSWORD=WpMatKhau_DTC245200018_2026!
MYSQL_ROOT_PASSWORD=RootMySQL_DTC245200018_2026!
GRAFANA_USER=admin
GRAFANA_PASSWORD=Grafana_DTC245200018_2026!
BIND_ADDR=127.0.0.1

## 4. Truy cập các dịch vụ

- Website: https://localhost (chứng chỉ tự ký, Accept Risk)
- phpMyAdmin: https://localhost/phpmyadmin (root / xem .env)
- Grafana: http://localhost:3000 (admin / Grafana_DTC245200018_2026!)
- Prometheus: http://localhost:9090 (chỉ 127.0.0.1)
- Loki: http://localhost:3100 (chỉ 127.0.0.1)

## 5. Cấu trúc thư mục

- docker-compose.yml
- .env (không commit)
- .gitignore
- nginx/default.conf + nginx/ssl/
- monitoring/ (prometheus, loki, promtail, grafana)

## 6. LogQL mẫu

{container="wordpress"}
{container=~"wordpress|nginx"}
{container="wordpress"} |= "error"
count_over_time({container="wordpress"}[5m])

## 7. Hardening đã áp dụng

- WordPress chạy non-root (user 33:33)
- Network isolation: db_net (internal), web_net, monitor_net
- Nginx: Security Headers + HSTS + chuyển hướng HTTP → HTTPS
- Nginx: read_only filesystem + no-new-privileges
- WordPress: no-new-privileges
- Mật khẩu mạnh theo MSSV
- Grafana / Prometheus / Loki chỉ bind 127.0.0.1
- User MySQL chỉ có quyền trên database WordPress (không có quyền global)

## 8. Dashboard Grafana đã import

- Node Exporter Full (ID: 1860)
- Docker / cAdvisor monitoring (ID: 14282)

## 9. Lịch sử commit

1. Commit 1: WordPress + MySQL + phpMyAdmin + Nginx reverse proxy
2. Commit 2: Prometheus + Grafana + node-exporter + cAdvisor
3. Commit 3: Loki + Promtail + Hardening cơ bản
4. Commit 4: HTTPS + README + Hardening nâng cao
