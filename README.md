# Website Quảng bá Sản phẩm (WordPress)

**Họ tên:** Đinh Bảo Khanh  
**MSSV:** DTC245200018  
**Đề tài:** Website Quảng bá Sản phẩm dùng WordPress

## Mô tả hệ thống

Hệ thống triển khai website WordPress quảng bá sản phẩm với đầy đủ các thành phần:

- WordPress + MySQL + phpMyAdmin
- Nginx reverse proxy (security headers)
- Prometheus + Grafana (giám sát)
- Loki + Promtail (log tập trung)
- Hardening: non-root, network isolation, mật khẩu mạnh

## Yêu cầu

- Docker + Docker Compose
- Ubuntu (khuyến nghị)

## Cách chạy

1. Clone repository
2. Tạo file .env (tham khảo nội dung mật khẩu theo MSSV)
3. Chạy lệnh: docker compose up -d
4. Kiểm tra: docker compose ps

## Truy cập dịch vụ

| Dịch vụ       | Địa chỉ                      | Tài khoản / Ghi chú                  |
|---------------|------------------------------|--------------------------------------|
| Website       | http://localhost             | WordPress                            |
| phpMyAdmin    | http://localhost/phpmyadmin  | root / (xem trong .env)              |
| Grafana       | http://localhost:3000        | admin / Grafana_DTC245200018_2026!   |
| Prometheus    | http://localhost:9090        | -                                    |
| Loki          | http://localhost:3100        | -                                    |

## Cấu trúc thư mục

- docker-compose.yml
- .env (không commit)
- .gitignore
- nginx/default.conf
- monitoring/ (prometheus, loki, promtail, grafana)
- README.md

## LogQL mẫu

{container="wordpress"}
{container=~"wordpress|nginx"}
{container="wordpress"} |= "error"

## Hardening đã áp dụng

- WordPress chạy non-root (user 33:33)
- Network isolation: db_net (internal), web_net, monitor_net
- Security headers trên Nginx
- Mật khẩu mạnh theo MSSV
- Grafana/Prometheus/Loki chỉ bind 127.0.0.1
