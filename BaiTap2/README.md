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

## Kiểm tra
*(Học viên kiểm tra theo yêu cầu đề bài và có thể dán log curl tại đây)*
```bash
# Truy cập đường dẫn không tồn tại (sẽ thấy nội dung 404 custom)
curl -I http://<IP_ADDRESS_DROPLET>/invalid-path-demo

# Truy cập trực tiếp 404.html (sẽ bị chặn, trả về 404 do rule internal)
curl -I http://<IP_ADDRESS_DROPLET>/404.html
```
