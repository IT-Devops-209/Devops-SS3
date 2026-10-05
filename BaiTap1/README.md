# Bài 1: Thay đổi cổng kết nối SSH (SSH Port Hardening)

## Mục tiêu
* Thay đổi cổng dịch vụ SSH mặc định từ 22 sang 2222 trên máy chủ ảo nhằm hạn chế tối đa các cuộc quét cổng tự động của botnet trên mạng internet.
* Thực hiện cấu hình tường lửa UFW trước khi khởi động lại dịch vụ để tránh nguy cơ bị khóa quyền truy cập (Lockout).

## Yêu cầu
**Bối cảnh:** Ngay khi máy chủ ảo được khởi tạo, cổng 22 liên tục bị brute-force. Bạn quyết định thay đổi cổng truy cập SSH sang cổng 2222 để gia tăng bảo mật.

**Ràng buộc:**
* Thực hiện thay đổi chỉ thị cổng kết nối sang `Port 2222` trong tệp cấu hình hệ thống `/etc/ssh/sshd_config` (hoặc tệp tin cấu hình drop-in tương ứng).
* Sử dụng tường lửa UFW trong OS để mở cổng `2222/tcp` trước khi khởi động lại dịch vụ SSH daemon.
* Chỉ thực hiện thao tác bằng tài khoản người dùng thường `devops` có quyền `sudo`, không sử dụng tài khoản `root` trực tiếp.

## Kiểm tra
**Lệnh kiểm tra:**
1. Từ máy tính cá nhân, chạy lệnh kiểm tra kết nối qua cổng mới:
```bash
ssh -p 2222 devops@<IP_ADDRESS_DROPLET>
```
*(Yêu cầu: Kết nối thành công qua cổng 2222 bằng SSH Key)*

2. Chạy lệnh kiểm tra kết nối qua cổng mặc định cũ:
```bash
ssh -p 22 devops@<IP_ADDRESS_DROPLET>
```
*(Yêu cầu: Kết nối qua cổng 22 bị chặn hoàn toàn, báo lỗi Connection refused hoặc Timeout)*

## Hướng dẫn nộp bài

### Các lệnh thực hiện trên VPS (Tham khảo):
```bash
# 1. Đăng nhập bằng user devops (đã tạo ở bài trước)
ssh -i /path/to/private_key devops@<IP_ADDRESS_DROPLET>

# 2. Chỉnh sửa cấu hình cổng SSH
sudo nano /etc/ssh/sshd_config
# (Tìm dòng '#Port 22' hoặc 'Port 22', sửa lại thành 'Port 2222' rồi lưu lại)

# 3. Cho phép UFW mở cổng mới 2222/tcp
sudo ufw allow 2222/tcp

# 4. Khởi động lại dịch vụ SSH để áp dụng
sudo systemctl restart ssh
# (Lưu ý: Không tắt Terminal hiện tại, hãy mở 1 Terminal mới để test kết nối trước khi thoát)
```

### Log kiểm tra kết nối (Kết quả thực tế):
```bash
$ ssh -p 22 devops@103.72.57.95
ssh: connect to host 103.72.57.95 port 22: Connection refused

$ ssh -p 2222 devops@103.72.57.95
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-84-generic x86_64)

devops@ubuntu-s-1vcpu-1gb-sgp1-01:~$
```
