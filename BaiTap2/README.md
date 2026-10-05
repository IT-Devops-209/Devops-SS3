# Bài 2: Cấu hình trang lỗi tùy chỉnh (Custom Error Page 404)

## Mục tiêu
* Cấu hình dịch vụ Web Server Nginx để phục vụ một trang lỗi 404 (Not Found) tự thiết kế thay cho trang báo lỗi mặc định đơn điệu của hệ thống.
* Bảo mật tệp tin HTML báo lỗi bằng chỉ thị `internal` của Nginx để ngăn người dùng truy cập trực tiếp vào tệp nguồn này.

## File nộp bài
* `404.html`: File giao diện trang báo lỗi tĩnh.
* `my-web.conf`: File cấu hình Server Block của Nginx đã bổ sung điều hướng lỗi.

## Các lệnh thực hiện trên VPS (Tham khảo):

```bash
# 1. Tạo thư mục web root cho my-web (nếu chưa có)
sudo mkdir -p /var/www/my-web/html

# 2. Tạo file 404.html
sudo nano /var/www/my-web/html/404.html
# (Copy nội dung file 404.html trong bài tập vào đây)

# 3. Tạo/Cập nhật file cấu hình Server Block
sudo nano /etc/nginx/sites-available/my-web.conf
# (Copy nội dung file my-web.conf trong bài tập vào đây)

# Kích hoạt (nếu tạo mới)
sudo ln -sf /etc/nginx/sites-available/my-web.conf /etc/nginx/sites-enabled/

# 4. Kiểm tra cấu hình và Reload Nginx
sudo nginx -t
sudo systemctl reload nginx
```

## Kiểm tra (Log kết quả thực tế)
```bash
$ curl -I http://103.72.57.95/invalid-path-demo
HTTP/1.1 404 Not Found
Server: nginx/1.18.0 (Ubuntu)
Date: Mon, 05 Oct 2026 07:18:12 GMT
Content-Type: text/html
Content-Length: 354
Connection: keep-alive
ETag: "614c3a-162"

$ curl -I http://103.72.57.95/404.html
HTTP/1.1 404 Not Found
Server: nginx/1.18.0 (Ubuntu)
Date: Mon, 05 Oct 2026 07:18:15 GMT
Content-Type: text/html
Content-Length: 162
Connection: keep-alive
```
