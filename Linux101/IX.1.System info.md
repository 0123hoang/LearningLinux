## 1.Thông số cơ bản về hệ thống
### 1./proc/bus
Chứa thông tin về các loại bus hiện có trên hệ thống, bao gồm thông tin về các thiết bị. ( ./input/devices)
### 2./usr/sbin/lsusb
Chứ thông tin của các thiết bị usb đang có trên hệ thống, gồm tên bus, mã số thiết bị , ID, tên thiết bị.
### 3./proc/interrupts
Chứa thông tin của các thiết bị, tiến trình đã thực hiện ngắt - hành động gọi cpu dừng thực hiện tác vụ hiện thời để thực hiện tác vụ cho nó. Gồm có IRQ (được định nghĩa sẵn), có tên tiến trình đã thực hiện việc ngắt.

### 4.lscpu, lsblk, lsusb
#### lscpu
Xem cấu hình cpu  
 - $lscpu	lệnh cơ bản
  - -e xem bổ sung mỗi cpu
  - -C xem cache của mỗi cpu
#### lsblk
Xem cấu hình ổ cứng
 - $lsblk	Lệnh cơ bản
  - -a	Xem tất cả
#### lsusb
Xem cấu hình cổng thiết bị USB hiện thời.
 - $lsusb	Lệnh cơ bản
### 5.Check bản phân phối
 - Check version Ubuntu: /etc/lsb-release
 - Check version Linux: /proc/version
 - Ngoài ra có thể sử dung câu lệnh $uname
  - -a	Hiện tất cả thông tin cơ bản của máy
  - -s-n-r-v-m-p-i-o	Hiện từng phần thông tin một
