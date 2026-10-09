# Website Quảng bá Sản phẩm - Chè Thái Nguyên

**Họ tên:** Đinh Bảo Khanh  
**MSSV:** DTC245200018  
**Đề tài:** Website Quảng bá Sản phẩm (WordPress)  
**Repository:** https://github.com/DTC245200018/Website_Quang_Ba_San_Pham

## 1. Mô tả hệ thống

Hệ thống triển khai website WordPress quảng bá sản phẩm Chè Thái Nguyên, bao gồm:

- Ứng dụng: WordPress + MySQL + phpMyAdmin
- Reverse Proxy: Nginx (HTTP → HTTPS, Security Headers, HSTS)
- Giám sát: Prometheus + Grafana + node-exporter + cAdvisor
- Log tập trung: Loki + Promtail (LogQL)
- Hardening: non-root, network isolation, no-new-privileges, read-only, mật khẩu mạnh, hạn chế quyền DB

## 2. Yêu cầu

- Ubuntu (đã cài Docker + Docker Compose)
- Tối thiểu 2GB RAM

## 3. Cách chạy

```bash
git clone https://github.com/DTC245200018/Website_Quang_Ba_San_Pham.git
cd Website_Quang_Ba_San_Pham

# Tạo file môi trường từ mẫu (tự đặt mật khẩu mạnh)
cp .env.example .env
nano .env

docker compose up -d
docker compose ps
```

## 4. Truy cập dịch vụ

| Dịch vụ    | Địa chỉ                      | Ghi chú                        |
|------------|------------------------------|--------------------------------|
| Website    | https://localhost            | Chứng chỉ tự ký – Accept Risk  |
| phpMyAdmin | https://localhost/phpmyadmin | Đăng nhập bằng user trong .env |
| Grafana    | http://localhost:3000        | admin / mật khẩu trong .env    |
| Prometheus | http://localhost:9090        | Chỉ lắng nghe 127.0.0.1        |
| Loki       | http://localhost:3100        | Chỉ lắng nghe 127.0.0.1        |

## 5. Cấu trúc thư mục

```text
Website_Quang_Ba_San_Pham/
├── docker-compose.yml
├── .env.example          # Mẫu biến môi trường (không chứa mật khẩu thật)
├── .gitignore
├── README.md
├── nginx/
│   ├── default.conf      # Reverse proxy + HTTPS + headers
│   └── ssl/              # Chứng chỉ self-signed (lab)
└── monitoring/
    ├── prometheus.yml
    ├── loki-config.yml
    ├── promtail-config.yml
    └── grafana/datasources/datasources.yml
```

## 6. LogQL mẫu

```text
{container="wordpress"}
{container=~"wordpress|nginx"}
{container="wordpress"} |= "error"
count_over_time({container="wordpress"}[5m])
```

## 7. Hardening đã áp dụng

- WordPress chạy non-root (user 33:33)
- Network isolation: db_net (internal), web_net, monitor_net
- Security headers + HSTS trên Nginx
- HTTPS + redirect HTTP → HTTPS
- no-new-privileges (WordPress, Nginx)
- read-only filesystem (Nginx)
- Grafana / Prometheus / Loki chỉ bind 127.0.0.1
- User MySQL chỉ có quyền trên database WordPress
- Mật khẩu mạnh, file .env không commit

## 8. Lịch sử commit

| Commit   | Nội dung                                                      |
|----------|---------------------------------------------------------------|
| Commit 1 | WordPress + MySQL + phpMyAdmin + Nginx reverse proxy          |
| Commit 2 | Prometheus + Grafana + node-exporter + cAdvisor                 |
| Commit 3 | Loki + Promtail + Hardening cơ bản                            |
| Commit 4+| HTTPS, HSTS, no-new-privileges, read-only, cập nhật README    |

## 9. Lưu ý

- File `.env` chứa mật khẩu thật — không commit lên GitHub (đã có trong `.gitignore`)
- Dùng `.env.example` làm mẫu, tự đặt mật khẩu mạnh trước khi chạy
- Chứng chỉ SSL là self-signed, phù hợp môi trường lab localhost
