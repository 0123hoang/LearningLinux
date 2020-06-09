1. Disk device  
2. Disk device system  
3. Disk partitions  
4. File system  
5. Mounting  
6. LVM  

## 1.Disk device
### 1.1 ata controler và scsi controler ?
Cả hai đều là các giao diện phần cứng giúp kết nối ổ đĩa với máy tính.
#### ata controller
 - Viết tắt của Advanced Technology Attachment. Đây là một loại thiết bị  tích hợp bộ điều khiển trực tiếp trên drive. Máy tính có thể sử dụng thiết bị ATA trực tiếp mà ko cần bộ điều khiển riêng biệt nào.  
 - ATA còn được gọi là IDE (Intergrated Drive Electronics).  
 - Serial ATA (SATA) là một giao diện sử dụng để kết nối ổ đĩa cứng ATA với máy tính. SATA là phiên bản mới hơn của phiên bản Parallel ATA (PATA) trước đó. Hiện nay SATA có nhiều phiên bản như SATA, SATA2, SATA3 với tốc độ tối đa 500MB/s.
#### scsi controller
 - Viết tắt của Small Computer System Interface (SCSI), là một giao diện chuẩn cho kết nối các thiết bị ngoại vi tới máy tính. Nó có thể kết nối cùng lúc 16 thiết bị ngoại vi cùng sử dụng chung 1 bus bao gồm 1 host adapter.
 - SAS (Serial Attached SCSI) là một giao diện chuẩn thay thế cho SCSI. Hiện nay có rất nhiều phiên bản với phiên bản 5 hiện tại hỗ trợ băng thông tới 24GB/s.
#### So sánh SATA và SAS
 - Về hiệu năng và băng thông, độ tin cậy, hiện nay SAS đều bỏ xa SATA.
 - SAS còn có cơ chế hỗ trợ hot-plug: Cắm trực tiếp rồi sử dụng ngay chứ ko cần phải khởi động lại hệ thống.
 - SAS có thể tương thích ngược với SATA, điều ngược lại thì không.
 - Giá thành SAS cao hơn SATA nên không phổ biến với người dùng hệ thống mà được sử dụng chủ yếu ở trong server.
### 1.2 Block device là gì
#### Block device
 - Là thiết bị lưu trữ của máy tính hỗ trợ đọc (viết) dữ liệu theo từng khối, từng sector, hoặc cluster. Thuật ngữ này cũng được sử dụng với các thiết bị đọc theo từng từ ( 1-8 byte).
 - Ví dụ: Đĩa mềm, đĩa cứng, CD, DVD, RAM.
#### Character device
 - Đối lập với block device là character device, là các thiết bị không có phương tiện lưu trữ địa chỉ vật lý, như là các tape, cổng serial, chuột, bàn phím, sound card, USB camera,... Các thiết bị này được đọc ghi dữ liệu theo các dòng byte
#### Major and minor
 - Các thiết bị trên Linux được phân biệt bởi 2 phần: major và minor
  - Major: Biểu thị loại thiết bị nào (đĩa IDE, đĩa SCSI, cổng serial,...)
  - Minor: Biểu thị thiết bị (đĩa đầu tiên, cổng serial thứ 2,...)
## 2. Disk devide system
### 2.1.Command: fdisk, lshw, lsblk, lsscsi -c, dd, lsscsi
#### fdisk 
 - Lệnh này dùng để xem và quản lý các ổ đĩa
 - $fdisk -l	Xem tất cả phân vùng hiện thời
 - $fdisk -l /dev/[DISK_NAME]	Xem ổ đĩa cụ thể (sda,sdb,sdc)
  - -s	Kiểm tra dung lượng
Khi xem từng ổ đĩa cụ thể, ta có thêm các lưạ chọn dùng để quản lý:
  - m	Help
  - p	Hiện tất cả các phân vùng
  - d	Xóa phân vùng
  - l	Hiện danh sách các phân vùng có thể được nhận biết
  - n	Thêm phân vùng
  - t	Thay đổi kiểu phân vùng
  - w	Ghi vào đĩa và thoát
  - q	Thoát ra và không lưu gì
  - x->f	Thêm các câu lệnh nâng cao khác -> Sửa lỗi thứ tự phân vùng
#### lsblk
Hiện danh sách các block device.  
 - $lsblk [OPTION] [devices...]
  - -p	Hiện ra cả đường dẫn device
  - -i	In theo kí hiệu ASCII
#### lsscsi
Hiện danh sách các thiết bị SCSI, NVM
 - $lsscsi [OPTION] [devices...]
  - -s	Hiển thị size
#### dd
Dùng để copy byte-to-byte ,convert hoặc copy một file
 - $dd [OPERANT] Cấu hình dd
  - bs=BYTES	Đọc và ghi BYTES byte cùng một lúc (mặc định là 512), ghi đè ibs và obs
  - if=FILE	Đọc từ FILE
  - of=FILE	Ghi ra FILE
  - conv=CONVS,	Một số tùy chọn khi Convert
    - noerror	Tiếp tục sau khi đọc bị lỗi
    -....
  - iflag=FLAGS,	Một số flag
    - nonblock	use non-blocking I/O
    - noatime	Không cập nhật access time
    - nofollow	Không cập nhật symlinks
    - Ví dụ :dd if=/dev/sr0 of=/home/hope/exampleCD.iso bs=2048 conv=noerror,sync
    
## 3.Partion
### 3.1.Partion là gì? So sánh primary, extended và logical partion
 - Partion (phân vùng) là một phần không gian đĩa cứng. Có hai bảng phân vùng phổ biến nhất là MBR và GPT. MBR có thể xử lý được tối đa 2TB, còn GPT có thể có 128 phân vùng, kích thước tối đa 8ZiB.
*fdisk không hoạt động được với GPT và với phân vùng lớn hơn 2TB, và cho phép tối đa 4 phân vùng trên Linux*
Có 3 loại phân vùng chính: primary, extended và logical.  
 - Primary partiton: Là những phân vùng có thể được dùng để boot hệ điều hành
 - Extended partition: Là vùng dữ liệu còn lại khi ta đã phân chia ra các primary partition, extended partition chứa các logical partition trong đó. Mỗi một ổ đĩa chỉ có thể chứa 1 extended edition
 - Logical partition: Các phân vùng nhỏ nằm trong extended partition, thường dùng để chứa dữ liệu.
### 3.2.Xem danh sách partion: fdisk -l, cat /proc/partitions
 - Xem danh sách các phân vùng có thể dùng $fdisk -l hoặc ở trog file /proc/partitions.  
### 3.3.Khởi tạo partition với fdisk và parted
#### Khởi tạo partition với fdisk
 - Trình tự thêm một phân vùng mới:
  - $fdisk /dev/[DEV_NAME]
  - Chọn n
  - Chọn kiểu phân vùng: p (primary) hoặc e (extended)
  - Chọn số thứ tự phân vùng
  - Chọn điểm bắt đầu phân vùng (bỏ trống mặc định là byte đầu tiên khả dĩ)
  - Chọn điểm kết thúc phân vùng (bỏ trống mặc định là byte cuối cùng khả dĩ). Có thể chọn số cụ thể hoặc theo giá trị tương đối theo KB, GB, TB, PB (K,M,G,T,P). Ví dụ tạo phân vùng có 5GB dung lượng: +5G
  - Sau đó chọn w để xác nhận thay đổi, hoặc q để hủy.
![102330859_596056017715476_2380803091888511723_n](https://user-images.githubusercontent.com/43545058/84133384-4bebea80-aa71-11ea-817a-fe69aaf43f36.png)
#### Khởi tạo partition với parted
### 3.4.Format partition với dd
## 4.File system
### 4.1. Filesystem là gì? man fs
### 4.2. So sánh các filesystem: etx2,etx3,etx4,XFS,vfat.
### 4.3. Xem các filesystem hỗ trợ: cat /proc/filesystems
### 4.4 Command: mkfs, lsblk -f, df -Th
## 5.Mounting
### 5.1. Mount là gì?
### 5.2. Mount partition vào thư mục
### 5.3. Command: du, df, mount, unmount
### 5.4. File /etc/fstab
### 5.5. Cách mount vĩnh viễn với /etc/fstab
## 6. LVM
### 6.1.RAID là gì
### 6.2. LVM là gì
### 6.3. Các thuật ngữ: physical volume (pv), volume group (vg), logical volume (lv)
### 6.4. Example: exxtend a logical volume
### 6.5. Example: resize a physical volume
### 6.6. Example: mirror a logical volume
### 6.7. Example: snapshot a logical volume
