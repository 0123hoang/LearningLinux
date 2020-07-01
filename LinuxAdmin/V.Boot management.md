1.Kernel and grub.  
2.Linux boot steps.  

## 1.Kernel and grub
### 1.1. Kernel and grub
 - Kernel là bộ mã giúp giao tiếp giữa tầng vật lí với tầng logic.
 - GNU GRUB là một chương trình giúp khởi động hệ thống máy tính. Nó được sử dụng bởi phần lớn các bản phân phối của Linux, Solaris...
  - GRUB hỗ trợ các hệ thống tập tin của Unix, NTFS và FAT của Windows.
  - GRUB có thể tải OS từ môi trường mạng về.
  - GRUB cung cấp một giao diện dongf lệnh đơn giản (tương tự bash).
### 1.2. Initrd
 - Trong quá trình khởi động, initrd là một quá trình tải file system root tạm thời vào memory, từ đó ta có thể mount được file system.
### 1.3 $halt, $reboot and $poweroff
#### $halt
 - Câu lệnh này dùng để dừng toàn bộ tiến trình CPU, sau đó có thể reboot hoặc tắt máy.
#### $reboot
 - Câu lệnh này dùng để khởi động lại máy.
#### $poweroff
 - Câu lệnh này dùng để tắt máy.
## 2.Linux boot steps
### 2.1 Linux boot steps
#### Power on
 - Khi bật máy, BIOS là chương trình được chạy đầu tiên, tiến hành POST dùng để kiểm tra thông số các phần cứng máy tính. Sau đó BIOS sẽ tìm kiếm và khởi chạy hệ điều hành đầu tiên (thứ tự có thể thay đổi bởi người dùng).
#### MBR
 - Khi BIOS tìm kiếm trên đĩa cứng, nó sẽ chạy sector đầu tiên gọi là Master Boot Record (MBR). Sector này có kích thước 512 bytes, nó lưu trữ địa chỉ của các hệ điều hành có thể khởi động được.
#### Boot loader GRUB
 - GRUB cho phép lựa chọn 1 trong các hệ điều hành và tải kernel hệ điều hành lên bộ nhớ máy tính và giải nén.
#### initrd
 - Chương trình này dò quét các thiết bị có trên hệ thống, đồng thời có thể nạp thêm kernel các module bổ trợ.
#### init
 - Kernel khi được tải xong sẽ khởi tạo tiến trình đầu tiên (PID 1), đây là chương trình cha của tất cả các tiến trình khác.
#### Đăng nhập
 - Khi đăng nhập vào máy tính, một chương trình shell được bắt đầu và mọi thao tác và chương trình khác đều được thực hiện bởi shell này.
### 2.2 What is runlevels, runlevel configuration
#### Runlevels
 - Khi tiến trình init (PID 1) khởi tạo các tiến trình khác, nó sẽ dựa vào runlevel để quyết định thực thi các script tương ứng tại /etc/rc.d, với kí tự K ở đầu tương ứng cho khởi động, còn S tương ứng cho việc kết thúc.
So sánh runlevel của SysV và systemd targets
|Sysvinit Runlevel|Systemd Target| Function|
|---|---|---|
|0|runlevel0.target, poweroff.target|System halt/shutdown|
|1,S,SINGLE|runlevel1.target, rescue.target|Single-user mode|
|2,4|runlevel2.target, runlevel4.target, multi-user.target|User-defined/Site-specific runlevels. By default, identical to 3.|
|3|runlevel3.target, multi-user.target|Multi-user, non-graphical mode, text console only|
|5|runlevel5.target, graphical.target|Multi-user, graphical mode|
|6|runlevel6.target, reboot.target|Reboot|
|emergency|emergency.target|Emergency mode|
#### RUnlevels configuration
Hệ thống init cổ điển SysV đã được thay thế bởi systemd.
 - Xem các targets có thể có
  - $systemctl list-units --type=target
 - Thay đổi target (ví dụ từ graphic sang multi-user)
  - $systemctl isolate mutli-user.target
 - Đặt mặc định
  - $systemctl set-default multi-user.target
 - Chọn mặc định
  - $systemctl get-default
### 2.3 deamon or demon?
 - Deamon là một loại chương trình trên các hệ điều hành Linux hoạt động ẩn trong background mà không cần sự kiểm soát của user. Deamon thường được kết thúc với kí tự d ở cuối.
 - Một số đặc điểm của deamon:
  - Không thể bị gián đoạn và chị hoạt động khi nhận dược đầu vào.
  - Tách khỏi tiến trình cha và thiết bị đầu cuối.
  - umask(0) để bỏ qua bất cứ quyền nào mà tiến trình có thể đã thừa hưởng.
  - Đóng file descriptors và mở lại theo ý của bạn.
  - Có thể hoạt động ngay cả khi đăng xuất, có thể sử dụng để giám sát hệ thống.
### 2.4 What is service ?
 - service (dịch vụ) là một chương trình chạy ở background. Service cũng được hiểu như là deamon. Ví dụ như sshd,httpd...
### 2.5 unit file and systemd
#### Định nghĩa
 - Unit file là một phần tử cho phép systemd quản lý tài nguyên hệ thống.
#### Vị trí của unit file
 - Unit file được giữ phiên bản copy ở /lib/systemd/system/, đây là địa chỉ mặc định khi một phần mềm cài đặt unit file trên hệ thống.
 - Unit file đang chạy có ở /run/systemd/system, có độ ưu tiên ở mức trung bình. Sửa đổi ở thư mục này sẽ bị mất đi khi reboot.
 - Unit file nên được sửa ở /etc/systemd/system/, đây là thư mục được ưu tiên trước tất cả các vị trí khác.
 - Muốn ghi đè cài đặt của unit file, ta nên tạo một thư mục với phần mở rộng .d đi kèm, trong đó là một unit file với phần mở rộng .conf dùng để ghi đè hoặc mở rộng cấu hình unit file. 
#### Các loại unit file
Các loại unit file được phân biệt bởi phần mở rộng ở đuôi file.  
|Phần mở rộng|Ý nghĩa|
|---|---|
|.service|Mô tả cách quản lý một dịch vụ hoặc một ứng dụng trên server. Nó bao gôm cả start và stop dịch vụ, trường hợp cụ thể cho tự động start và sự phụ thuộc, yêu cầu phần mềm liên quan|
|.socket|Mô tả một socket mạng hoặc IPC, hoặc bộ đêm FIFO dùng để kích hoạt socket|
|.device|Mô tả thiết bị được đăng kí và quản lý bởi udev hoặc file system. .device không phải là bắt buộc đối với mọi thiết bị.|
|.mount|Định nghĩa 1 mountpoint trên hệ thống quản lí bởi systemd. Tên file là đường dẫn mount với \ thay bằng -.|
|.automount|Định nghĩa 1 mountpoint sẽ được tự động mount. Tên file phải trùng với file .mount|
|.swap| Mô tả swap space trên hệ thống. Tên của unit phải liên kết với thiết bị hoặc file path|
|.target| Dùng để hệ thống chuyển sang trạng thái khác, hoặc cung cấp các điểm đồng bộ (synchronization points) cho các unit khác khi boot hoặc chuyện trạng thái|
|.path| Định nghĩa một đường dẫn để dùng cho path-based activation.|
|.timer| Định nghĩa  một bộ thời gian được quản lí bởi systemd, tương tự với cron job dùng cho kích hoạt trễ hoặc được lập lịch. Unit được kích hoạt khi đúng giờ|
|.snapshot|Được tạo tự động bởi lệnh $systemctl snapshots, giúp lưu lại trang thái hiện thời của systemd để có thể roll back với systemctl isolate [NAME]|
|.slice|Quản lí tài nguyên của một nhóm các tiến trình|
|.scope| Được tạo tự đọng bởi systemd từ dữ liệu nhận được từ bus interfaces của nó. Dùng để quản lí tiến tình hệ thống được tạo bên ngoài|

#### Cấu trúc unit file
Để biết thêm thông tin về các thuộc tính các trường trong unit file, truy cập [tại đây](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files#where-are-systemd-unit-files-found) hoặc chi tiết hơn tại [man page của systemd](https://man7.org/linux/man-pages/man1/systemd.1.html)
### 2.6 Work hard with systemd
#### Unit commands
 - $systemctl status [UNIT]	Kiểm tra trạng thái service
  - list-units/list-sockets/list-timers	Hiện các unit hiện thời trong bộ nhớ
    - -a/ --all	để hiện tất cả các service inactive
    - -t/ --type	để hiện phần mở rộng tương ứng
  - enable/disable	Bật/Tạm ngưng dịch vụ
  - start/stop	Mở/Tắt dịch vụ
  - reload/restart/try-restart	Tải lại hoặc khởi động lại dịch vụ
  - isolate	Khởi động 1 unit và dừng các unit còn lại
  - kill	Gửi tín hiệu tới unit
#### Lệnh thực hiện với unit file
 - $systemctl list-unit-files [PARTTERNS]	Hiện các unit files
  - enable/disable/reenable
  - mask/unmask
  - halt/poweroff/reboot/suspend/hibernate
  
