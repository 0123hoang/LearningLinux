1. Linux basic   
2. Tìm hiểu Bash Script  
3. Kiểm tra hiệu năng Network  
4. Cài đặt các dịch vụ trên Centos  
5. Cài đặt Wordpress  

## 1. Linux basic   
### 1.1 How SSH work and SSH-keypair
![image](https://user-images.githubusercontent.com/43545058/87286853-90acea80-c523-11ea-8eba-9de7283aa405.png)

| Client gửi yêu cầu kết nối tới server, bao gồm khóa công khai của client|----> |
| ---|--- |
| <------|Server gửi khóa công khai, mã hóa bằng khóa công khai của client|
|Client sẽ xác thực khóa công khai đó để xác thực đúng là server cần gửi| |
|Client gửi tới server danh sách các thuật toán mã hóa cho phép, mã hóa bằng khóa công khai của server|---->|
|<------|Server gửi lại thuật toán mã hóa tốt nhất được chấp nhận, mã hóa bằng khóa công khai của client||
|Client gửi đi khóa phiên dùng cho phiên SSH này, mã hóa bằng khóa công khai của server)|---->|
|Client gửi đi tài khoản và mật khẩu, mã hóa bằng khóa công khai|------>|
|<----|Server chấp nhận|
|Kết nối thành công|--->|
### 1.2 Crontab
Dùng để tạo một file lập lịch riêng cho từng người dùng
 - $crontab [-u user] file
  - -l	Hiển thị crontab
  - -r	Xóa crontab
  - -e	Sửa crontab
 - Cấu file crontab
 - Có 5 trường giá trị đầu, tiếp theo đó là câu lệnh thực hiện
  - [minutes 0-59] [hours 0-23] [day 0-31] [month 1-12 JAN-DEC] [day of week 0-7 SUN-SAT] [Command]
   - [COMMAND] có thể là một lệnh hoặc là command file
*Kiểm tra systemctl cron hoặc restart lại*
### 1.3 Create swap partion
 - Kiểm tra dung lượng swap
  - $free -m hoặc $swapon -s
 - Kiểm tra dung lượng ổ cứng
  - $lsblk
 - Tạo một partion mới
  - Sử dụng $fdisk
 - Tạo phân vùng swap
  - $mkswap [PATH]
 - Tự động kích hoạt phân vùng khi máy tính khởi động
  - Thêm vào file /etc/fstab:	[PATH]	swap swap default 0 0
 - Kích hoạt swap
  - $swapon -a
 - Kiểm tra lại
  - $lsblk hoặc $swapon -s hoặc $free -m
### 1.4 Basic use vim editor
### 1.5 Reset password root
### 1.6 User login banner
### 1.7 Lock, Unlock user
### 1.8 Check Active Session  
### 1.9 System activity log
### 1.10 du , df, lsblk, egrep, grep, sed , awk command
### 1.11 find, curl, wget command
### 1.12 check system info
### 1.13 setup time,date and hostname
### 1.14 Clear RAM Memory Cache, Buffer and Swap Space
### 1.15 Set system locales
### 1.16 File /etc/hosts
### 1.17 system hardware infomation
### 1.18 tip and trick keyboard
### 1.19 wget, curl,scp command
## 2. Tìm hiểu Bash Script  
### 2.1 Tìm hiểu các cú pháp cơ bản thường dùng trong Bash Script
### 2.2 Cho danh sách các package : wget, curl, mtr , httpd. Viết bash script liệt kê các package nằm trong danh sách đã sẵn trên hệ thống , sau đó cài đặt các package chưa được cài đặt 
## 3. Cài đặt các dịch vụ trên Centos  
### 3.1 Cài đặt Nginx WebServer, Apache Webserver
### 3.2 Tìm hiểu Database, các loại database, hệ quản trị sơ sở dữ liệu
### 3.3 Tìm hiểu cấu hình enable log mysql
### 3.4 Cài đặt MYSQL Server, MariaDB Server, PHPmyadmin. Các command phổ biến làm việc với MYSQL Database, người dùng và phân quyền. So sánh Mysql và MariaDB
### 3.5 Cài đặt NTP ( Network Time Protocol )
### 3.6 Cài đặt, cấu hình Yum Local Repository 
## 4. Kiểm tra hiệu năng Network
### 4.1 Tìm hiểu các command phổ triển trong netstat và ss
### 4.2 Cài đặt, tìm hiểu các command trong nuttcp
### 4.3 Cài đặt, tìm hiểu các command iper3
### 4.4 Cài đặt, tìm hiểu các command trong bwctl
### 4.5 So sách 3 công cụ trên
### 4.6 mtr, tracroute
## 5. Cài đặt Wordpress  
### 5.1 Node 1 : Cài đặt Wordpress thủ công sử dụng MYSQL 5.7 và PHP 7.1, viết script cài đặt Wordpress sử dụng  MYSQL 5.7 và PHP 7.1
### 5.2 Node 2 : Cài đặt MariaDB 10.1. Chuyển database từ node 1 sang node 2. Đảm bảo dữ liệu nguyên vẹn
### 5.3 Node 1 : Cập nhật cấu hình database kết nối đến node 2, đảm bảo Website hoạt động bình thường ( không mất mát dữ liệu )
