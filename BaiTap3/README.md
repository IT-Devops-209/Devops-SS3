# Bài 3: Bảo mật tài nguyên bằng HTTP Basic Authentication

## Mục tiêu
* Cài đặt và sử dụng công cụ băm mật khẩu để bảo vệ một đường dẫn hoặc thư mục nhạy cảm trên Web Server.
* Cấu hình Nginx yêu cầu xác thực tài khoản/mật khẩu trực tiếp trên trình duyệt bằng cơ chế HTTP Basic Authentication.

## File nộp bài
* `.htpasswd`: File mẫu chứa tên đăng nhập và mật khẩu đã được băm.
* `my-web.conf`: File cấu hình Nginx Server Block đã thêm block location `/admin` hỗ trợ Basic Auth.

## Các lệnh thực hiện trên VPS (Tham khảo):

```bash
# 1. Cài đặt công cụ băm mật khẩu apache2-utils
sudo apt update
sudo apt install apache2-utils -y

# 2. Tạo file mật khẩu (hệ thống sẽ yêu cầu nhập mật khẩu bảo mật 2 lần)
sudo htpasswd -c /etc/nginx/.htpasswd admin_user

# 3. Tạo thư mục quản trị (tùy chọn) để test
sudo mkdir -p /var/www/my-web/html/admin
sudo nano /var/www/my-web/html/admin/index.html
# (Có thể viết "Đây là trang quản trị" vào file index.html này)

# 4. Cập nhật cấu hình Server Block (Thêm block location /admin)
sudo nano /etc/nginx/sites-available/my-web.conf
# (Copy cấu hình từ file my-web.conf mẫu vào đây)

# 5. Kiểm tra cấu hình và reload Nginx
sudo nginx -t
sudo systemctl reload nginx
```

## Kiểm tra
*(Học viên kiểm tra theo yêu cầu đề bài và dán log terminal curl tại đây)*

```bash
# 1. Không kèm thông tin đăng nhập (sẽ trả về 401 Unauthorized)
curl -I http://<IP_ADDRESS_DROPLET>/admin

# 2. Kèm thông tin đăng nhập đúng (sẽ ra 200 OK nếu có file hoặc 404 nếu thư mục trống, nhưng không phải 401)
curl -u admin_user:<PASSWORD> -I http://<IP_ADDRESS_DROPLET>/admin
```
