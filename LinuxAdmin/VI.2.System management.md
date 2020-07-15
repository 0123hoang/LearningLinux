1. Đặt lịch  
2. System logging  
3. Resourse management  

## 1. Đặt lịch  
### 1.1 command: at, crontab
#### $at
Dùng để đặt lịch, sắp xếp lịch các hoạt động, các câu lệnh.  
 - $at [-V] [-q queue] [-f file] [-mMlv] [timespec| -t time]
  -V	In ra phiên bản tiêu chuẩn lỗi hoặc thành công
  -m	Gửi mail cho user khi job hoàn thành kể cả không có output
  -M	Không gửi mail
  -f file	Đọc job từ file thay vì standard input
  -t time	Chạy job taij thời điểm nhất định [CCYYMMDDhhmm.ss]
  -v	Hiển thị thời gian job sẽ được chạy
 - $atq hoặc $at -l	Hiển thị tất cả các job đang chờ
 - $atrm [job] hoặc $at -d [job]	Hủy job theo number
*Khi truyền input qua stardard input xong, ấn ctdl+d để kết thúc*
#### $crontab
Dùng để tạo một file lập lịch riêng cho từng người dùng
 - $crontab [-u user] file
  - -l	Hiển thị crontab
  - -r	Xóa crontab
  - -e	Sửa crontab
 - Cấu file crontab
 - Có 5 trường giá trị đầu, tiếp theo đó là câu lệnh thực hiện
  - [minutes 0-59] [hours 0-23] [day 0-31] [month 1-12 JAN-DEC] [day of week 0-7 SUN-SAT] [Command]
*Kiểm tra systemctl cron hoặc restart lại*
## 2. System logging  
### 2.1 Các user đăng nhập trên hệ thống
Sử dụng $last để kiểm tra user đang đăng nhập
 - $last [user]
  - reboot
  - -x shutdown
 - $lastlog
*Có thể kết hợp sử dụng với grep*
### 2.2 Log đăng nhập
 - Xem log user đăng nhập, đổi mật khẩu trên /var/log/auth.log hoặc /var/log/secure.
### 2.3 Unity: syslogd
 - syslogd (hay những phiên bản mới hơn là rsyslog và syslog-ng) là một service deamon dùng để ghi lại log của hệ thống, giúp cho việc quản lý và gỡ lỗi dễ dàng hơn.
 - Các file log được lưu trữ ở /var/log/
 - Một số file log thông dụng
  - messages: chứa dữ liệu log chung
  - cron chứa dữ liệu cron deamon
  - maillog thông tin các máy chủ mail
### 2.4 xem log với tailf
 - tail -f dùng để hiện thời gian thực những thay đổi ở cuối file
### 2.5 Cách rotating logs
 - logrotating là một phương pháp dùng để xoay vòng log cũ và thay log mới vào, tránh việc lưu trữ dữ liệu thừa gây lãng phí tài nguyên
 - $logrotate	Kiểm tra phiên bản
 - /etc/logrotate.conf	file cấu hình chung
 - /etc/logrotate.d/	file cấu hình cụ thể từng file log riêng
 - Một số thông số trong file cấu hình
  - daily/monthly/weekly	rotate theo thời gian
  - size [number][k,M,G]	rotate theo dung lượng
  - create/nocreate	tạo file mới sau khi rotate file cũ/ghi đè file cũ
  - rotate [number]	Số lượng file tối đa lưu trữ
  - compress/nocompress	nén dữ liệu log cũ/không nén
## 3. Resourse management 
### 3.1 Xem ram và ram cache: /proc/meminfo
 - /proc/meminfo lưu trữ thông tin hiện thời về ram của máy tính
  - MemTotal	Tổng lượng RAM
  - MemFree	RAM còn trống
  .....
### 3.2 Các thông số trên command: free
 - $free kiểm tra các thông số của ram hiện thời
 - $free [OPTION]
  - -h	Hiện chỉ số làm tròn cho dễ đọc
  - -t	Hiện tổng số RAM
  - -w	Hiện nhiều thông tin hơn
  - -s N	Lặp lại mỗi N giây
  - -c N	Hiện N lần, rồi thoát
  - -l	Hiện thông tin low-high (ngưỡng trên-dưới)
### 3.3 Khởi tạo swap partion
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
### 3.4 Command: vmstat -S m, top, free -m, watch, iostat, mpstat, nmon, htop
#### $vmstat
DÙng để xem thông số ram ảo, đươc dùng để xác định nghẽn cổ chai
 - $vmstat -S m
 - Proc:

|---|---|
|r|Số lượng tiến trình đang chờ để chạy|
|b|Số lượng tiến trình ngủ không thể tác động (uninterruptible sleep) |

 - Memory

|---|---|
|swpd| Lượng memory ảo được sử dụng|
|free| Lượng memory rảnh rỗi|
|buff| Lượng memory dùng làm buffer|
|cache| Lượng memory dùng làm cache|
|inact|.... Không hoạt động|
|active|.... Có hoạt động|

 - Swap

|---|---|
|si| Lượng memory được swap từ disk (mỗi giây)|
|so| ...................... tới disk (Mỗi giây)|

 - IO

|---|---|
|bi|Số block/s nhận từ thiết bị block|
|bo|.......... gửi tới .............|

 - System

|---|---|
|in|Số lượng bị ngắt/s, bao gồm cả đồng hồ (clock)|
|cs|Số lượng context switchs/s (chuyển trạng thái từ process này->process khác)|

 - CPU

|---|---|
|us| Thời gian dùng để chạy mã non-kernel (user time, bao gồm cả nice time)|
|sy| .......................... kernel (system time)|
|id| Thời gian rảnh, bao gồm cả IO-wait time|
|wa| Bao gôm trong id|
|st| Thời gian bị mất từ máy ảo|

#### $nmon
Đây là lệnh xem thông số hệ thống, mạng theo hình ảnh trực quan.  
#### $watch
Đây là lệnh dùng để theo dõi 1 lệnh nào đó, xem xét output và lỗi một cách chi tiết (mặc định mỗi 2s)
 - $watch [command]
  - d	highlight phần khác nhau
#### $iostat
Hiển thị thông tin chung về CPU và các thiết bị input/output
#### $mpstat
Hiển thị chỉ thông tin chung về CPU (1 phần của $iostat)
#### $htop
Hiển thị cpu, process thời gian thực, theo giao diện trực quan
