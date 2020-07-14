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
 - Vim (Vi IMproved), một trình soạn thảo văn bản, giúp kế thừa và cải tiến VI đã có. Đây cũng là một trình soạn thảo rất phổ biến.
#### Khởi động vim
|$vim [FILE_NAME]| Mở file với vim chủ yếu|
|---|---|
|+[num]|Mở vim ra và di chuyển con chuột đến đầu dòng đó|
|-d [file1] [file2]| Mở nhiều file cùng lúc rồi so sánh giữa chúng|
|-D|chuyển tới debug mode khi thực thi dòng lệnh đầu tiên từ script|
|-M|Không cho chỉnh sửa|
|-r| Hiện các swap file (đuôi .swp, xuất hiện khi hoạt động bị gián đoạn)|
|-r [file]| Sử dụng swap file để phục hồi|
#### Di chuyển con trỏ trong file
|h|qua trái|
|---|---|
|j|xuống dòng|
|k|lên dòng|
|l|qua phải|
|gg|lên đầu file|
|G| Cuối file|
|$ hoặc End| Cuối dòng|
|0 hoặc Home| Đầu dòng|
|ctrl+g|Hiển thị dòng|
#### Highlight
|v| Highlight vùng text(sau đó di chuyển con trỏ)|
|---|---|
|V| Chọn dòng hiện tại|
|ggVG| Chọn tất cả file|
#### Tìm kiếm
|/[STRING] hoặc ?[STRING]| TÌm nội dung trong file|
|---|---|
|shift+*|Tìm kiếm từ đó|
|n và N| tìm tiếp theo (lùi lại) các vị trí tìm được|
*Tìm kiếm có thể tìm theo biểu thức chính quy*
#### Xóa
|x|Xóa bên phải|
|---|---|
|X|Xóa bên trái|
|dw|Xóa từ đó|
|dd|Xóa dòng đó|
*Có thể thêm [NUMBER] vào đầu tương tự như sử dụng nó nhiều lần*
#### Chỉnh sửa
|yw|Copy từ đó|
|---|---|
|yy|Copy  dòng đó|
|v...y| Có thể bôi đen rồi ấn y cũng được|
|p|paste|
|i|Bật chế độ chèn text|
|R|Bật chế độ thay thế|
|a|Chèn ra sau con trỏ|
*Sử dụng [NUMBER] có thể gây nên các hành động không mong muốn*
#### Lưu và thoát
|:q|Thoát(Khi ko thực hiện thay đổi gì)|
|---|---|
|:q!|Thoát và không lưu|
|:w|Lưu|
|w!|Bắt buộc lưu (ghi đè)|
|:wq| Lưu và thoát|
![image](https://user-images.githubusercontent.com/43545058/87380501-d23da400-c5bc-11ea-8ddf-ca3ac6620f97.png)

### 1.5 Reset password root
 - Khi người quản trị bị mất mật khẩu quyền root, người quản trị có thể reset lại mk root khi có quyền truy cập vật lý vào thiết bị.
 - Khi khởi động máy, vào phần cấu hình khởi động boot sau đó nhấn e để chỉnh sửa
  ![image](https://user-images.githubusercontent.com/43545058/87381078-141b1a00-c5be-11ea-82a1-faf98c689791.png)
![image](https://user-images.githubusercontent.com/43545058/87381106-25fcbd00-c5be-11ea-930d-34cfe0e3b0e0.png)
 - Với Ubuntu, thay ro -> rw init=/bin/bash; với centOS, thay ro -> rw init=sysroot/bin/sh, rồi ấn ctrl X để thực hiện.
 ![image](https://user-images.githubusercontent.com/43545058/87381173-46c51280-c5be-11ea-9fa1-6e5ec0cb8fa4.png)
![image](https://user-images.githubusercontent.com/43545058/87381202-59d7e280-c5be-11ea-9228-bb0134034eb7.png)
 - CentOS 7:
  - chroot /sysroot
  - passwd root
  - [Nhập password mới]
  - touch /.autorelabel	update SElinux
  - exit
  - reboot	Khởi động lại máy
 - Ubuntu 20.04
  - mount -n -o remount, rw /
  - passwd root
  - [Nhập password mới]
  - exec /sbin/init	Khởi động lại hệ thống
### 1.6 User login banner
Đây là những dòng thông báo/cảnh báo người dùng khi đăng nhập vào hệ thống. Mỗi server cần có một banner login để tránh những người dùng ngây thơ cố tình đăng nhập trái phép vào hệ thống.
 - Thay đổi banner login : $ sudo vi /etc/issue.net
 - Thay đổi đường dẫn banner login với trên ssh: 
  - $sudo vi /etc/ssh/sshd_config
    - Chỉnh sửa đường dẫn banner tại: Banner /etc/issue.net (hoặc link khác)
 - Khởi động lại ssh service: systemctl restart sshd (restart/stop/start)
### 1.7 Lock, Unlock user
 - $passwd [OPTIONS] [USER] Đổi mật khẩu
    - -d	Xóa mật khẩu (mật khẩu sẽ thành để trống)
    - -e	Cho mật khẩu hiện thời hết hạn luôn, người dùng sẽ phải đổi mật khẩu với lần đăng nhập sau
    - -l	Khóa mật khẩu, người dùng sẽ không thể thay đổi mật khẩu
    - -u	Mở khóa mật khẩu
    - -S	Hiện thông tin về mật khẩu
*Muốn cho tài khoản không đăng nhập được (vẫn giữ nguyên mật khẩu), phải dùng $usermod -L [USER], mở khóa với $usermod -U [USER]*
### 1.8 Check Active Session  
 - Dùng $who hoặc $
 - Check log đăng nhập ở /var/log/secure hoặc /var/log/auth.log
 - Để buộc đăng xuất một tài khoản nào đó : $sudo pkill -KILL -u [USERNAME]
 Sử dụng $last để kiểm tra user đang đăng nhập
 - $last [user]
  - reboot
  - -x shutdown
 - $lastlog
*Có thể kết hợp sử dụng với grep*
### 1.9 System activity log
 - syslogd (hay những phiên bản mới hơn là rsyslog và syslog-ng) là một service deamon dùng để ghi lại log của hệ thống, giúp cho việc quản lý và gỡ lỗi dễ dàng hơn.
 - Các file log được lưu trữ ở /var/log/
 - Một số file log thông dụng
  - messages: chứa dữ liệu log chung
  - cron chứa dữ liệu cron deamon
  - maillog thông tin các máy chủ mail
### 1.10 du , df, lsblk, egrep, grep, sed , awk command
#### du
 - Kiểm tra bộ nhớ đã bị chiếm dụng (theo từng khối)
 - $du -ha	Kiểm tra bộ nhớ của từng file/folder hiện tại
 - $du -a --apparent-size Hiện bộ nhớ chiếm dụng theo byte chính xác
  - -d [NUM] Hiển thị số lớp folder tối đa
#### df
 - Kiểm tra dung lượng bộ nhớ còn trống hiện tại
 - $df -Th	Hiện tât cả và hiện dung lượng cho dễ nhìn
#### lsblk
 - Kiểm tra dung lượng ổ cứng
 - $lsblk
  - -a	Hiển thị tất cả
  - -p Đường dẫn tuyệt đối
  - -t [column]	Hiện theo tree (thường column lấy SIZE)
  - -f	Hiện file s
#### grep
 - grep là môt câu lệnh dùng để tìm kiến văn bản trong một file. Đây là một lệnh rất hữu ích và mạnh mẽ, được dùng phổ biến.
  - $grep [OPTIONS] [PATTERNS] [FILES]	In ra cấc dòng có chứa [PATTERNS]
    -  -n	Hiển thị số dòng tương ứng
    -  -c	Chỉ hiện thị số lượng khớp
    -  -l	Chỉ hiển thị file có chứa [PATTERNS]
    -  -L	Chỉ hiển thị file không chứa [PATTERNS]
    -  -i	Tìm kiếm cả chữ cái thường và in hoa
    -	-e	Tim kiếm nhiều kí tự cùng lúc với mỗi [PATTERNS] theo sau -e
    -	-E	Sử dụng biểu thức chính quy với [PATTERNS]
#### egrep
 - $egrep tương đương với $grep -E
#### sed
 - $sed s/[STRING_1]/[STRING_2]/ [FILENAME]	Dùng để thay thế [STRING_1] thành [STRING_2] 1 lần trên mỗi dòng
    - s///g	Dùng để thay thế toàn bộ trong STRING
    - y///g	Dùng để thay thế cho từng chữ cái trong STRING ( Giống tr)
    - -i	Dùng để lưu luôn vào file hiện tại
    - '[STRING]d' [FILE]	Dùng để xóa dòng, cụ thể với [STRING]
      - [NUM]d	Xóa dòng cụ thể, hoặc xóa theo khoảng ngăn bởi ,
      - $d	Xóa dòng cuối cùng
*[STRING] cũng tuân theo biểu thức chính quy*
    - '[NUM]i [STRING]' [FILE] dùng để thêm dòng vào vị trí cụ thể
      
*Phần ngăn cách / có thể được thay thế bằng : | _ lựa chọn là của mỗi người*
#### awk
 - $awk :Đây là một câu lệnh dùng để xử lí text trên file. Không chỉ có những cú pháp đơn giản như thay thế, đếm số lượng, sắp xếp. awk còn dùng để xử lí dữ liệu. Các câu lệnh của awk rất đa dạng và mạnh mẽ, có thể được đóng gói trong 1 file để dễ dàng tái sử dụng sau này. Tuy vậy, để thực hiện một số yêu cầu phức tạp hơn, ta có thể sử dụng với perl và python.
  - $awk [OPTION] [FILE]	Cấu trúc cơ bản
  - $awk '{print $0 "[STRING]" }'	[FILE]	In ra toàn bộ file. Trong đó $0 là toàn bộ file, $1 là tham số t1, $2 là tham số thứ 2, $n là tham số thứ n...
  - Một số biến của awk:
    - NR	Số thứ tự của dòng
    - NF	Số từ của dòng
    - FS	Khoảng cách giữa các từ của file được đọc, tương tự với file xuất ra ta có OFS
    - RS	Phân biệt giữa các dòng từ file được đọc, tương tự với file xuất ra ta có ORS
  - $awk có vòng lặp for, while, có khởi tạo và gán giá trị cho biến tương tự như ngôn ngữ C.
### 1.11 find, curl, wget command
#### find
- $find dùng để tìm kiếm file.
  -$find [PATH] [OPTIONS]	Tìm kiếm tất cả file và folder có trong folder đấy
    - -type d	Tìm kiếm tất cả folders con
    - -type f	Tìm kiếm tất cả files có trong thư mục đó
    - -name [STRING]	Tìm kiếm một file cụ thể
    - -iname [STRING]	TÌm kiếm bỏ qua chữ hoa thường
    - -user [NAME]	Tìm với tên người sở hữu
    - -cmin n	Tìm kiếm file đã thay đổi n phút trước
    - -ctime n Tìm kiếm file đã thay đổi n ngày trước
    - Tương tự với amin, atime, mmin, mtime với a là accessed time, m là modified time
    - *Số n có thể thay khoảng tìm kiếm -n hoặc +n*
    - -size n	Tìm kiếm theo dung lượng(100, -250M, +3G)
    - -perm n	Tìm kiếm theo quyền(777,762...)
    - -exec chmod n {} +	Thay đổi quyền của file hoăc folder đó
    - -maxdepth n	Tìm kiếm đệ quy tối đa n lớp
    - -delete	Xóa
    - -print/printf
    - -exec [COMMAND]
#### curl
 - Là một lệnh dùng để gửi và nhận dữ liệu từ server, qua các giao thức được hỗ trợ sẵn
 - $curl [SERVER]
 (SERVER có thể là http hoặc ftp,...)  
  - -0	DÙng http1.0 (bản lỗi thời)
  - -2/3	Dùng ssl version 2/3
  - -4/6	Chỉ dùng ipv4 hoặc v6 
  - -a	Khi upload, thì ghi append chứ không ghi đè
  - -d/--data [DATA]	Gửi data cụ thể, nếu là file thì là @[file_name]
  - -#	Thêm thanh tiến trình
  - -O Tải về file
  - -o [name] Tải về file và lưu file ở tên mới
  - -C -	Tiếp tục tải file đang bị dừng dở
  - -u [USERNAME]:[PASSWORD] Sử dụng tài khoản để tải file
  - -T [FILENAME] [LOCATION] Upload file lên 
#### wget
 - $wget dùng để tải dữ liệu từ trên mạng về
 - $wget [OPTION] [URL]
  - -b	Tải ở background
  - -o [FILE]	Đưa output ra file đó
  - -a	[FILE]	Append file đó
  - -c	Resume
  - -S	Print server responde
  - -4/-6	ipv4/6 only
  - --user=[USER]
  - --password=[PASS]
  - --http-user=[USER]/http-password=[PASS]
  - --proxy-user=[USER]/proxy-password=[PASS]
  - --secure-protocol=[SSLv2,SSLv3,TSL1_1,...]	Lựa chọn giao thức bảo mật
  - --certificate=[FILE]
  - --private-key=[FILE]	Lựa chọn khóa, chứng chỉ hợp lệ	
### 1.12 check system info
#### 1.Check bản phân phối
 - Check version Ubuntu: /etc/lsb-release
 - Check version Linux: /proc/version
 - Ngoài ra có thể sử dụng câu lệnh $uname
  - -a	Hiện tất cả thông tin cơ bản của máy
  - -s-n-r-v-m-p-i-o	Hiện từng phần thông tin một
#### 2./proc/bus
Chứa thông tin về các loại bus hiện có trên hệ thống, bao gồm thông tin về các thiết bị. ( ./input/devices)
#### 3./usr/sbin/lsusb
Chứ thông tin của các thiết bị usb đang có trên hệ thống, gồm tên bus, mã số thiết bị , ID, tên thiết bị.
#### 4./proc/interrupts
Chứa thông tin của các thiết bị, tiến trình đã thực hiện ngắt - hành động gọi cpu dừng thực hiện tác vụ hiện thời để thực hiện tác vụ cho nó. Gồm có IRQ (được định nghĩa sẵn), có tên tiến trình đã thực hiện việc ngắt.
### 1.13 setup time,date and hostname
#### date
 - $date	Hiện ngày giờ hiện tại
  - +%Y%m%d.....	Hiện ngày giờ tháng năm theo mẫu riêng
    - -	Nếu thêm - ở sau % thì không có pad 0 ở bên trái
    - _	Pad bằng khoảng trắng
    - 0	Pad bằng số 0
  - -s [STRING]	Đặt giờ 
#### Thay đổi timezone
 - Dùng $tzselect
 - $timedatectl [OPTION] [COMMAND]	Dùng để thay đổi hoặc xem thời gian
  - $timedatectl status	Câu lệnh cơ bản
  - $timedatectl set-timezone [ZONE], set-ntp [BOOL], set-time [TIME]	Đổi thuộc tính tương ứng
#### Thay đổi hostname
  - $hostnamectl [OPTION] [COMMAND]	Dùng đề thay đổi hoặc xem tên người dùng
  - $hostnamectl status	Xem cài đặt người dùng cơ bản
  - $hostnamectl set-hostname [NAME] đổi tên người dùng
    - set-icon-name, set-location	Đổi thuộc tính tương ứng
 
### 1.14 Clear RAM Memory Cache, Buffer and Swap Space
#### Clear RAM Memory Cache and buffer
 - sync; echo 1> /proc/sys/vm/drop_caches	chỉ xóa Pagecache
 -	echo 2	Xóa dentries và inodes
 -	echo 3	Xóa cả Pagecache, dentries và inodes
 - Sau đó kiểm tra lại với $free
#### Clear swap
 - $swapoff -a && swapon -a
### 1.15 Set system locales
 - locale là một tập các biến môi trường của máy tính về ngôn ngữ, quốc gia, character encoding.
 - locale tác động tới định dạng thời gian, số, tiền tệ trên hệ thống Linux.
 - $locale hoặc localectl	Xem các locale hệ thống
  - -a	Xem các locale có thể sử dụng
  - -k [LOCALE]	Xem thông tin chi tiết hơn từng locale
  - update-locale [VARIABLE]=[LOCALE] hoặc
  - localectl set-locale [VARIABLE]=[LOCALE]	Thay đổi locale
*locale được lưu trữ ở /etc/default/locale hoặc /etc/locale.conf*
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
