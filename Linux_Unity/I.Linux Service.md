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
 - Là một lệnh dùng để gửi dữ liệu tới server, qua các giao thức được hỗ trợ sẵn
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
 - $wget dùng để tải dữ liệu từ trên mạng về theo giao thức http,https hoặc ftp
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
 - File host là một tập tin lưu trữ thông tin IP của các máy chủ và tên miền (domain) được trỏ tới. Nó có thể được gọi là một DNS nhỏ trên máy tính của bạn. File host giúp cho các hệ điều hành biết được IP của máy chủ nơi một tên miền cụ thể nào đó được quản lý. Máy tính luôn kiểm tra file này đầu tiên trước khi thực hiện việc tìm kiếm trên server DNS ngoài.
 - Cấu trúc file hosts
  - [IP_ADDRESS] [NAME] [CNAME]
  - Trong đó [NAME] và [CNAME] là URL hoặc là tên phụ của địa chỉ IP đó.
### 1.17 system hardware infomation
#### Kiểm tra dung lượng ổ cứng
 - $lsblk
  - -a	Hiển thị tất cả
  - -p Đường dẫn tuyệt đối
  - -t [column]	Hiện theo tree (thường column lấy SIZE)
  - -f	Hiện file s
 - $lvs
 - $fdisk
 - $df

#### Kiểm tra thông tin phần cứng
 - $lshw	Kiểm tra phần cứng chung
 - $lsusb	Kiểm tra các kết nối usb (chuột, bàn phím...)
 - $lscpu	Kiểm tra thông tin cpu
#### Xem RAM,cpu,...
 - $vmstat
 - $nmon
 - $htop
### 1.18 tip and trick keyboard
|Key| Công dụng|
|---|---|
|Alt+B hoặc ctrl+ <-|Lùi bên trái 1 từ|
|Alt+F hoặc ctrl+ ->|Lùi bên phải 1 từ|
|ctrl+E hoặc End| Cuối dòng|
|ctrl+A hoặc Home| Đầu dòng|
|ctrl+U|Xóa từ đầu dòng đến con trỏ|
|ctrl+K|Xóa từ con trỏ đến cuối dòng|
|Super+ Up/Down|Phóng to/Thu nhỏ sửa sổ|
|Super+ 1-9| Khởi động nhanh ứng dụng từ thanh công cụ|
### 1.19 scp command
- $scp gửi dữ liệu giữa hai host với nhau qua ssh
 - $csp [OPTIONS] [SOURCE] [TARGET]
  - [SOURCE]  là tập tin có thể trên máy local hoặc máy trên mạng
  - [TARGET] có thể là máy local hoặc máy trên mạng
  - -4/-6 Sử dụng ipv4/v6
  - -P [PORT] Sử dụng port nào
  - -v	Hiện nhiều thông tin hơn
  
## 2. Tìm hiểu Bash Script  
### 2.1 Tìm hiểu các cú pháp cơ bản thường dùng trong Bash Script
#### Cấu trúc file bash
 - Đuôi là .sh
 - Nội dung file bash
 #!/bin/bash	//Chỉ ra shell thực thi, dòng đầu tiên của file bắt buộc ntn  
// tiếp theo là những câu lệnh thực thi  
#### Sử dụng file bash .sh
 - $bash filename.sh hoặc
 - Đưa file bash vào trong những folder trong $PATH, rồi $[FILENAME]
 - Nếu không thì phải chỉ ra địa chỉ của tệp này: $ ./[FILENAME]
#### Truyền tham số cho bash script
 - $ bash [FILE] [ARG1] [ARGn]....
 - Sử dụng tham số
  - $0: Tên file
  - $1: Tham số thứ 1 [ARG1]
  - $2: Tham số thứ 2 [ARG2]...
*$ trước kí tự biểu thị đó là biến*

#### Input/Output
##### Input truyền từ tham số (ở trên)
##### Input là các biến mỗi trường
  - Đã có sẵn có chỉ việc lấy ra dùng
  - $set để hiện tất cả các biến môi trường
##### Input truyền từ terminal
  - $read [VAR1] [VAR2]
    - -p '[STRING]' để hiện dấu nhắc
    - -s	Ẩn đi giá trị vừa nhập
##### Input truyền từ file
 - $while read [VAR1] [VARn]
    do  
    //Cac hanh dong  
    done < [INPUT_FILE];  
 - Với câu lệnh read,
  - Mặc định là các ký tự space, tab, /r/n là những ksi tự phân cách. Nếu số biến đặt nhỏ hơn số từ, thì biến cuối cùng sẽ chiếm toàn bộ từ còn lại => đặt 1 biến để đọc cho cả dòng
  - -a	Để lưu giá trị theo mảng ${a[n]}), bắt đầu từ 0
  - -d [DELIMITER]	Để định nghĩa dấu ngăn cách giữa các từ
    - Có thể sử dụng biến môi trường thay thế: IFS=[DELIMITER]
##### Input cho các tiến trình con của nó
 - $export [VAR] hoặc $export [VAR]=[VAL] (dùng cho external command)
##### Output 
 - Terminal : $echo [VAR]
 - File : > [VAR]
#### Câu lệnh rẽ nhánh
##### If-else
 if [[ [ĐIỀU_KIỆN] ]]  
   then  
   // COMMAND  
   fi  
 -------------------------------------------
  if [[ [ĐIỀU_KIỆN] ]]  
   then  
   //COMMAND_1  
   else  
   //COMMAND_2  
   fi  
 --------------------------------------------
   if [[ [ĐIỀU_KIỆN] ]]  
   then  
   //COMMAND_1  
   elif [[ [ĐIỀU_KIỆN_1] ]] && [[ [ĐIỀU_KIỆN_2] ]]  
   then  
   //COMMAND_2  
   else  
   //COMMAND_3  
   fi  
 -----------------------------------------------
 - Toán tử so sánh số học: -eq -ne -lt -gt -le -ge -o(or) -a (and)
 - Toán tử so sánh chuỗi: = hoặc == != \> \< -n(if not empty)
 - Kiểm tra tập tin: -f [FILE](là file?) -x(executatible?) -d(irectory) -e(xist) -w(ritable) -r(eadable) -s(size >0 ?) [F1] -ef [F2] (file f1 f2 là một)
*Sử dụng [[ [ĐIỀU_KIỆN] ]] thay vì [ [ĐIỀU_KIỆN] ] hoặc ( [ĐIỀU_KIỆN] ) để thực hiện được các kết quả mong muốn.*
##### Case
case [VAR] in  
 [VAL1] )  
 //COMAMND_1  
 ;;  
 [VAL2] | [VAL3] | [VAL4] )  
 //COMMAND_2  
 ;;  
 * ) //default value  
 //COMMAND_3  
 ;;  
esac //Viet nguoc cua case :)
#### VÒng lặp
##### For loop
 for i in 1 2 3 4 5  
 do  
 //[COMMAND]  
 done  
 - Trong đó, câu lệnh for có thể thay thế như sau
  - for i in {1..5}
  - for i in {0..10..2}	Bước nhảy 2 đơn vị
  - for (( c=1; c<=5; c++ ))	Giống cấu trúc phổ biến, nhưng mà có hai ngoặc đơn
  - for i in $( ls )	Chạy giá trị trên mảng $ls
##### While do
 - While sẽ thực hiện vòng lặp chừng nào điều kiện đó còn đúng
 while [[ [ĐIỀU_KIỆN] ]]  
 do  
 //[COMMAND]  
 done  
##### Until
 - Until sẽ thực hiện vòng lặp chừng nào điều kiện đó còn sai
 until [[ [ĐIỀU_KIỆN] ]]  
 do  
 //[COMMAND]  
 done  
##### String
 - Toán tử so sánh chuỗi: = hoặc == != \> \< -n(if not empty)
 - Độ dài xâu: expr length $[STRING]
 - Tìm độ dài khớp với S2 trong S1 (đếm từ 1) : expr match "$S2" 'S1' hoặc expr "$S2" : '$s1' ($s1 có thể là partern)
 - Tìm vị trí khớp đầu tiên của S2 trong S1 (đếm từ 1): expr index "$S2" S1
 - Hiện xâu bắt đầu từ vị trí(đếm từ 0) ${S2:[INDEX]:[LENGTH]} (đây là hiển thị xâu mới, không phải cắt trực tiếp)
 - Tạo ra string con : expr substr $S2 [INDEX] [NUMBER]
*https://tldp.org/LDP/abs/html/string-manipulation.html*
##### Array
###### Tạo array mới
  - declare -a [ARRAY]
  - [ARRAY] = (VAL1 VAL2 VALn)
  - [ARRAY[0]] = VAL1 ; ARRAY[1]=VAL2
###### In ra giá trị array
  - for i in "${my_array[@]}"; do echo "$i"; done	Với @, mối giá trị $i là 1 phần từ của mảng
  - for i in "${my_array[*]}"; do echo "$i"; done	Với *, $i đầu tiên của vòng lặp là toàn bộ các phần tử cảu mảng
###### In ra giá trị index 
 - (đếm từ 0): for key in "${!my_array[@]}"; do echo "$key"; done	Sử dụng ! trước my_array
###### Lấy ra kích thước mảng
 - echo "the array contains ${#my_array[@]}	Sử dụng # và @
###### Thêm phần tử vào mảng
 - Thêm vào cuối	my_array+=(VAR1 VAR2)
 - Thêm vào vị trí cụ thể	my_array+=( [index]=VAR )
###### Xóa phần tử khỏi mảng
 - unset my_array[INDEX] (mất luôn cả index đó)
 
### 2.2 Cho danh sách các package : wget, curl, mtr , httpd. Viết bash script liệt kê các package nằm trong danh sách đã sẵn trên hệ thống , sau đó cài đặt các package chưa được cài đặt
##### Ý tưởng
 - Kiểm tra package đã được cài đặt hay chưa: Có nhiều cách để kiểm tra, nhưng cách này đầu ra khá ngắn gọn: $dpkg -V [package]
 - Nếu chưa được cài đặt thì cài đặt: sudo apt install [package]
##### Xây dựng
 - Dòng đầu tiên khai báo bash: #!bin/bash
 - Khai báo mảng cho các package cần được kiểm tra: package_array = (wget curl mtr httpd)
 - Tạo một vòng lặp chạy trên mảng đó với biến $i: for i in ${package_array[@]} do //[COMMAND] done
 - Mỗi vòng lặp ta kiểm tra xem package đó đã được cài đặt hay chưa với câu lệnh dpkg -V $i
  - Trả về là 1 string: nếu đã cài đặt thì là xâu rỗng, nếu chưa thì hiện thông báo chưa được cài đặt
  - check_install = echo "dpkg -V $i"
 - Nếu chưa được cài đặt thì cài đặt, không thì không làm gì: if [[ -n $check_install ]] then yum install $i -y fi
###### Hoàn thiện
#!bin/bash  
package_array = (wget curl mtr httpd)  
for i in ${package_array[@]} ;do   
 - check_install =$(dpkg -V $i)  
 - if [ -n $check_install ] ;then   
  - yum install $i -y   
 - fi  
done  
###### Thực thi
 - Lưu lại với tên check_package.sh, thay đổi quyền file là 755
 - Chạy script với lệnh sudo bash check_package.sh >log 2>errorlog
###### Đánh giá kết quả
 - File log ghi lại log cài đặt và file error log giúp gỡ lỗi file bash và các lỗi khác.
 - Trên Debian/Ubuntu nếu sử dụng câu lệnh cài đặt apt-get install [PACKAGE], thì httpd là gói con  trong gói phần mềm khác.
## 3. Cài đặt các dịch vụ trên Centos  
### 3.1 Cài đặt Nginx WebServer, Apache Webserver
#### Cài đặt Nginx WebServer
 - Cập nhật: yum update -y
 - Tạo repo cho ngĩn vi /etc/yum.repos.d/nginx.repo
 - Dán nội dung dưới đây vào
[nginx-stable]  
name=nginx stable repo  
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/  
gpgcheck=  
enabled=1
 - Cài đặt nginx: yum install nginx -y
 - Khởi động nginx: systemctl enable nginx ; systemctl start nginx
 - Truy cập qua website là địa chỉ máy tính đó
*https://blog.hostvn.net/chia-se/huong-dan-cai-dat-nginx-tren-centos-7.html#25_Buoc_5_Cau_hinh_Firewalld_Neu_co*
#### Cài đặt Apache WebServer
 - Cập nhật: yum update -y
 - Cài đặt apache: yum install httpd -y
 - Khởi động apache: systemctl enable httpd ; systemctl start httpd
### 3.2 Tìm hiểu Database, các loại database, hệ quản trị sơ sở dữ liệu
#### Khái niệm
 - Database (cơ sở dữ liệu) là một tập hợp những data (dữ liệu) có liên quan với nhau . Database được duy trì dưới dạng một tập hợp các tập tin trong hệ điều hành hay được lưu trữ trong các hệ quản trị cơ sở dữ liệu. 
#### Phân loại
 - Phân loại theo mục đích
  - Database dạng file:Đây là dạng dữ liệu được lưu trữ dưới dạng các file. Database dạng file thường được sử dụng nhất là *.mdb Foxpro,một số định dạng file khác làtext, ascii, *.dbf.
  - Database quan hệ:Đây là dạng dữ liệu (thực thể) khác nhau được lưu trữ trong các bảng dữ liệu. Giữa các thực thể này có mối liên hệ với nhau gọi là các quan hệ với nhau. Các hệ quản trị hỗ trợ database quan hệ nổi tiếng có thể kể đến: MS SQL server, Oracle, MySQL…
  - Database hướng đối tượng: Đây là dạng dữ liệu cũng được lưu trữ trong các bảng dữ liệu. Điều khác biệt là các bảng có bổ sung thêm các tính năng hướng đối tượng như lưu trữ thêm các hành vi, nhằm thể hiện hành vi của đối tượng. Mỗi bảng xem như một lớp dữ liệu. Một dòng dữ liệu trong bảng là một đối tượng. Các hệ quản trị có hỗ trợ database hướng đối tượng như: MS SQL server, Oracle, Postgres SQL
  - Database bán cấu trúc:Đây là dạng dữ liệu được lưu dưới định dạng XML, các thông tin mô tả dữ liệu, đối tượng được trình bày trong các thẻ tag. Với ưu điểm lưu trữ được hầu hết các loại dữ liệu khác nhau, database bán cấu trúc là hướng mới trong nghiên cứu và ứng dụng về cơ sở dữ liệu.
 - Phân loại theo hệ diều hành
  - Database sử dụng hệ điều hành Linux:MySQL, Mariadb
  - Database sử dụng hệ điều hành Windows: SQL Server – MSSQL
#### hệ quản trị sơ sở dữ liệu
 - Hệ quản lý cơ sở dữ liệu (Database Management System - DBMS) là phần mềm tương tác với người dùng cuối, ứng dụng và chính cơ sở dữ liệu để thu thập và phân tích dữ liệu. Phần mềm DBMS bao gồm các tiện ích cốt lõi được cung cấp để quản trị cơ sở dữ liệu. Tổng cộng của cơ sở dữ liệu, DBMS và các ứng dụng liên quan có thể được gọi là "hệ thống cơ sở dữ liệu". Thông thường thuật ngữ "cơ sở dữ liệu" cũng được sử dụng để nói đến bất kỳ DBMS, hệ thống cơ sở dữ liệu hoặc ứng dụng nào được liên kết với cơ sở dữ liệu.
### 3.3 Tìm hiểu cấu hình enable log mysql
 - Với mysql < 5.1.29, để bật query log, sửa dòng sau ở /etc/my.cnf ở phần [mysqld]:
  - log   = /path/to/query.log  #works for mysql < 5.1.29
  - Enable từ MySQL console: SET general_log = 1;
 - Với mysqld > 5.1.29, để bật query log:
  - general_log_file = /path/to/query.log
  - general_log =1
  - SET global general_log=1
  - SET global log_output = 'file';
*Nhớ phân quyền ghi cho file và thư mục cha của nó.
### 3.4 Cài đặt MYSQL Server, MariaDB Server, PHPmyadmin. Các command phổ biến làm việc với MYSQL Database, người dùng và phân quyền. So sánh Mysql và MariaDB
#### MySQL
 - Tải về repo mysql: sudo wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
 - Cài đặt repo đó: sudo rpm -Uvh mysql80-community-release-el7-3.noarch.rpm
 - Tải về mysql: yum install mysql-server
#### MariaDB Server
 - wget https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
 - chmod +x mariadb_repo_setup
 - sudo ./mariadb_repo_setup
 - sudo yum install MariaDB-server
#### PHPmyadmin
 - Với PHPmyadmin, ta có thể sử dụng mySQL và mariaDB trên giao diện.
 - sudo yum install epel-release
 - yum install http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el7.rf.x86_64.rpm
 - yum install phpmyadmin
 - vi  /etc/httpd/conf.d/phpmyadmin.conf để thay đổi tại block <Directory /usr/share/phpMyAdmin/> </Directory>
  - # Require ip 127.0.0.1
  - # Require ip ::1
  - Require all granted
 - Copy file /etc/phpMyAdmin/config.sample.inc.php thành config.inc.php
 - Sửa file config.inc.php : thay tất cả các giá trị cookie thành http
 - Truy cập bằng web http://IP/phpmyadmin
 - Nếu bị lỗi truy cập Forbidden thì yum install php
#### Một số MySQL command
##### Tài khoản
 - [mysql dir]/bin/mysql -h hostname -u root -p	Đăng nhập
 - SET PASSWORD FOR 'user'@'hostname' = PASSWORD('passwordhere');	Đổi mk
 - SELECT User,Host FROM mysql.user;	Xem tất cả tài khoản
 - CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';	Tạo tài khoản.
  - GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';	Trao quyền truy cập mọi dữ liệu của db
 - https://stackoverflow.com/questions/33510184/how-to-change-the-mysql-root-account-password-on-centos7/34207996#34207996
 dùng để thay đổi mật khẩu root lần đầu tiên.
##### Database, table
 - create database [db name];
 - show databases;
 - use [db name];
 - show tables;
 - describe [table name];
 - drop database [db name];
 - drop table [table name]
##### Query
 - CREATE TABLE [table name] ( [name] [type]([byte]) [OPTION], );
*[OPTION] có thể là primary key, unique, not null...*
 - INSERT INTO [table name] (columns,) VALUES (values,);
 - UPDATE [table name] set [VAL_1]=[VAL 2],
 - ALTER table [table name] [OPTION] ;	Thay đổi một thuộc tính của cột.
 -....
##### Phân quyền
 - SHOW GRANTS [FOR 'bloguser'@'localhost'];	Xem phân quyền người dùng;
 - GRANT [permission] ON [database].[table] TO 'user'@'localhost';

|permisstion| Loại|
|---|---|
|ALL| cho hết|
|CREATE||
|DELETE||
|DROP||
|EXECUTE|Cho phép thực hiện function hoặc procedure trên global hoặc database|
|GRANT OPTION| Cho phép người dùng thêm hoặc xóa quyền người dùng khác|
|INSERT||
|SELECT||
|SHOW DATABASE||
|UPDATE||
*Sau đó reset lại bằng FLUSH PRIVILEGES;*
*https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html*
 - Để thu hồi quyền hạn, ta sử dụng REVOKE
  - REVOKE [perrmission] on [db].[table] from '[user]'@'[host]'
#### So sánh MySQL và MariaDB
 - MariaDB được tách ra thành 1 nhánh của MySQL
 - Cả hai có cấu trúc tương tự nhau, có thể chuyển đổi qua lại mà ko gây hỏng hóc dữ liệu.
 - MySQL được phát triển bởi Oracle, còn MariaDB là mã nguồn mở
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
#### Yêu cầu 
 - Máy đã cài đặt sẵn MySQL 5.7 và PHP 7.1
#### Tải và cài Wordpress thủ công
 - $wget https://wordpress.org/latest.tar.gz	Tải về wordpress
 - $tar xzvf latest.tar.gz	Giải nén
 - $mv wordpress/* /var/www/html/	Di chuyển sang thư mục html
 - $rm latest.tar.gz	Xóa file
 - $ cd /var/www/html/wordpress
 - $ mv wp-config-sample.php wp-config.php
 - $ vi wp-config.php	Sửa file với nội dung sau
 ![image](https://user-images.githubusercontent.com/43545058/88134553-207f2280-cc0f-11ea-8666-f21f5d0062ff.png)
 - Sau đó truy cập sử dụng đường dẫn IP/wordpress (trước đó dùng php là IP/phpmyadmin), sau đó cài thêm một số tùy chọn cho trang wordpress của mình.
#### Tải và cài Wordpress tự động
 - Tải về wordpress: wget https://wordpress.org/latest.tar.gz
 - Giải nén: tar xzvf latest.tar.gz
 - Đổi tên: mv wordpress/wp-config-sample.php wordpress/wp-config.php
##### Thay đổi nội dung file wp-config.php:
###### 1. Đọc dữ liệu từ terminal
  - $read -p "database: " database
  - $read -p "username: " username
  - $read -ps "password: " password
  - $read -p "hostname: " hostname
##### 2. Sửa đổi trên file wp-config.php
  - file-config="/var/www/html/wordpress/wp-config.php"
  - sed -i s/database_name_here/$database/1 $file-config
  - sed  -i s/username_here/$username/1 $file-config
  - sed -i s/password_here/$password/1 $file-config
  - sed -i s/localhost\'/$hostname\'/1 $file-config
  
 - Di chuyển sang thư mục mới mv wordpress/* /var/www/html/
 - Xóa file: rm latest.tar.gz
 - Xong rồi truy cập qua địa chỉ trình duyệt IP/wordpress
##### Script hoàn chỉnh
#!bin/bash
## download wordpress tar file
wget https://wordpress.org/latest.tar.gz  
tar xzvf latest.tar.gz  
mv wordpress/wp-config-sample.php wordpress/wp-config.php  
######################################### read variable
read -p "database: " database  
read -p "username: " username  
read -ps "password: " password  
echo "\n"  
read -p "hostname: " hostname  
############################################ edit config file
file-config="/var/www/html/wordpress/wp-config.php"  
sed -i s/database_name_here/$database/1 $file_config  
sed  -i s/username_here/$username/1 $file_config  
sed -i s/password_here/$password/1 $file_config  
sed -i s/localhost\'/$hostname\'/1 $file_config  
############################################# clean things
rm latest.tar.gz
### 5.2 Node 2 : Cài đặt MariaDB 10.1. Chuyển database từ node 1 sang node 2. Đảm bảo dữ liệu nguyên vẹn
### 5.3 Node 1 : Cập nhật cấu hình database kết nối đến node 2, đảm bảo Website hoạt động bình thường ( không mất mát dữ liệu )
