# Bài 4: Chạy song song nhiều cổng dịch vụ (Nginx Virtual Hosts)

## Mục tiêu
* Cấu hình Nginx chạy song song hai trang web tĩnh độc lập trên cùng một địa chỉ IP máy chủ ảo thông qua việc phân chia cổng dịch vụ (8080 và 8090).
* Thiết lập tường lửa UFW và Cloud Firewall mở cổng tương ứng để người dùng ngoài internet truy cập được.

## File nộp bài
* `beta-app-index.html`: File giao diện cho Beta App.
* `internal-app-index.html`: File giao diện cho Internal App.
* `multi-port.conf`: File cấu hình chung Nginx cấu hình cả 2 block server lắng nghe cổng 8080 và 8090.

## Các lệnh thực hiện trên VPS (Tham khảo):

```bash
# 1. Tạo các thư mục lưu trữ mã nguồn riêng biệt
sudo mkdir -p /var/www/beta-app/html/
sudo mkdir -p /var/www/internal-app/html/

# 2. Tạo nội dung file index.html cho từng trang
sudo nano /var/www/beta-app/html/index.html
# (Copy nội dung từ file beta-app-index.html)

sudo nano /var/www/internal-app/html/index.html
# (Copy nội dung từ file internal-app-index.html)

# 3. Tạo file cấu hình multi-port.conf (có 2 server block)
sudo nano /etc/nginx/sites-available/multi-port.conf
# (Copy nội dung từ file multi-port.conf)

# 4. Kích hoạt Server Block bằng cách tạo symlink
sudo ln -sf /etc/nginx/sites-available/multi-port.conf /etc/nginx/sites-enabled/

# 5. Mở cổng trên tường lửa UFW
sudo ufw allow 8080/tcp
sudo ufw allow 8090/tcp

# Lưu ý: Cần truy cập DigitalOcean cấu hình Cloud Firewall mở cổng 8080, 8090 Inbound Rules nữa nhé.

# 6. Kiểm tra cấu hình và reload Nginx
sudo nginx -t
sudo systemctl reload nginx
```

## Kiểm tra
*(Học viên dán kết quả curl hoặc ảnh chụp màn hình trình duyệt tại đây)*
```bash
# Trả về giao diện Beta App
curl http://<IP_ADDRESS_DROPLET>:8080

# Trả về giao diện Internal App
curl http://<IP_ADDRESS_DROPLET>:8090
```
