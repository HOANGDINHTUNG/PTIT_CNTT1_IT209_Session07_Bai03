# Bài 3: Thiết lập Cơ sở dữ liệu và Tự cấu hình dịch vụ Systemd cho Spring Boot

## 1. Mục tiêu
Thực hành khởi tạo cơ sở dữ liệu MySQL và cấu hình phân quyền người dùng bảo mật. Tự thiết kế và viết tệp tin cấu hình dịch vụ Systemd hoàn chỉnh từ đầu để quản lý tiến trình ứng dụng Spring Boot và chạy dưới quyền một người dùng giới hạn (non-root).

## 2. Các bước cấu hình trên VPS

### Bước 2.1: Cấu hình cơ sở dữ liệu MySQL

Đã đăng nhập vào MySQL (`sudo mysql -u root -p`) và chạy các lệnh SQL sau:

```sql
CREATE DATABASE springboot_db;
CREATE USER 'spring-admin'@'localhost' IDENTIFIED BY 'SpringSecure@123';
GRANT ALL PRIVILEGES ON springboot_db.* TO 'spring-admin'@'localhost';
FLUSH PRIVILEGES;
```

### Bước 2.2: Tạo user chạy ứng dụng và cấu hình Systemd

Trên terminal của VPS, đã chạy các lệnh sau:

```bash
# 1. Tạo user hệ thống không có quyền đăng nhập shell
sudo useradd -r -s /bin/false spring-runner

# 2. Tạo tệp tin cấu hình dịch vụ
sudo nano /etc/systemd/system/spring-app.service
# (Copy nội dung từ file spring-app.service đã viết và dán vào, sau đó lưu lại)

# 3. Nạp lại systemd daemon và khởi động dịch vụ
sudo systemctl daemon-reload
sudo systemctl start spring-app.service
sudo systemctl enable spring-app.service
```

*Lưu ý: Ứng dụng app.jar cần phải tồn tại sẵn ở đường dẫn `/opt/spring-app/app.jar` và thư mục này nên được cấp quyền sở hữu cho user `spring-runner`.*

## 3. Kết quả kiểm tra

**Kiểm tra trạng thái dịch vụ:**
```bash
sudo systemctl status spring-app.service
```
*(Học viên hãy chụp ảnh màn hình hoặc dán kết quả text của lệnh systemctl vào đây)*

**Kiểm tra cổng lắng nghe (port 8082):**
```bash
ss -tlnp | grep 8082
```
*(Học viên hãy chụp ảnh màn hình hoặc dán kết quả text của lệnh ss -tlnp vào đây)*
