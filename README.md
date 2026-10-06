# Website Quảng bá Sản phẩm - Chè Thái Nguyên

**Họ tên:** Đinh Bảo Khanh  
**MSSV:** DTC245200018  
**Đề tài:** Website Quảng bá Sản phẩm (WordPress)

## 1. Mô tả hệ thống

Hệ thống triển khai website WordPress quảng bá sản phẩm Chè Thái Nguyên, bao gồm đầy đủ các thành phần:

- Ứng dụng: WordPress + MySQL + phpMyAdmin
- Reverse Proxy: Nginx (HTTP → HTTPS, Security Headers)
- Giám sát: Prometheus + Grafana + node-exporter + cAdvisor
- Log tập trung: Loki + Promtail (LogQL)
- Hardening: non-root, network isolation, mật khẩu mạnh, HSTS

## 2. Yêu cầu hệ thống

- Ubuntu (đã cài Docker + Docker Compose)
- Tối thiểu 2GB RAM
- Cổng 80, 443, 3000, 9090, 3100 chưa bị chiếm

## 3. Cách chạy

1. Clone repository
2. Tạo file .env (mật khẩu mạnh theo MSSV)
3. Chạy: docker compose up -d
4. Kiểm tra: docker compose ps

## 4. Truy cập các dịch vụ

| Dịch vụ       | Địa chỉ                     | Tài khoản / Ghi chú                          |
|---------------|-----------------------------|----------------------------------------------|
| Website       | https://localhost           | WordPress (chứng chỉ tự ký)                  |
| phpMyAdmin    | https://localhost/phpmyadmin| root / (xem trong .env)                      |
| Grafana       | http://localhost:3000       | admin / Grafana_DTC245200018_2026!           |
| Prometheus    | http://localhost:9090       | -                                            |
| Loki          | http://localhost:3100       | -                                            |

## 5. Cấu trúc thư mục

- docker-compose.yml
- .env (không commit)
- .gitignore
- nginx/default.conf + ssl/
- monitoring/ (prometheus, loki, promtail, grafana)

## 6. LogQL mẫu

{container="wordpress"}
{container=~"wordpress|nginx"}
{container="wordpress"} |= "error"

## 7. Hardening đã áp dụng

- WordPress chạy non-root (user 33:33)
- Network isolation: db_net (internal), web_net, monitor_net
- Nginx: Security Headers + HSTS + chuyển hướng HTTP → HTTPS
- Mật khẩu mạnh theo MSSV
- Grafana / Prometheus / Loki chỉ bind 127.0.0.1

## 8. Commit lịch sử

1. Commit 1: WordPress + MySQL + phpMyAdmin + Nginx reverse proxy
2. Commit 2: Prometheus + Grafana + node-exporter + cAdvisor
3. Commit 3: Loki + Promtail + Hardening

## 9. Dashboard Grafana đã import

- Node Exporter Full (ID: 1860)
- Docker / cAdvisor monitoring (ID: 14282)
