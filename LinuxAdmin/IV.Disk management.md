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
    - Ví dụ :$dd if=/dev/sr0 of=/home/hope/exampleCD.iso bs=2048 conv=noerror,sync.  
    		$ssh username@54.98.132.10 "dd if=/dev/sda | gzip -1 -" | dd of=backup.gz
    
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
 - $parted thường được sử dụng song song với fdisk - dùng để xử lý với những ổ đĩa có dung lượng lớn hơn 2TB và phải sử dụng GPT.
 - Trình tự thêm 1 phân vùng mới:
  - $parted /dev/[DEV_NAME]
  - Viết lệnh mkpart primary
  - File system type ? Viết etx4
  - Start và end có thể chọn giá trị theo số thứ tự byte hoặc số MB, GB, TB (VD: 200MB)
  - Nếu có cảnh báo thì ấn I (Ignore).
  
### 3.4.Format partition với dd
 - Thay thế toàn bộ bit trong partition với 0
  - $dd if=/dev/zero of=/dev/[DEV_NAME]
 - Thay thế toàn bộ bit trong partion với số ngẫu nhiên
  - $dd if=/dev/urandom of=/dev/sda1
## 4.File system
### 4.1. Filesystem là gì? man fs
 - Filesystem hay được gọi là hệ thống tập tin được dùng để quản lý cách đọc và lưu trữ dữ liệu. Hiện nay có nhiều phiên bản filesystem được ra đời và sử dụng, các phiên bản sau thì được cải tiến hơn phiên bản trước (Sử dụng $man fs để xem)
### 4.2. So sánh các filesystem: etx2,etx3,etx4,XFS,vfat.
|Tên filesystem| Năm giới thiệu |Kích thước file tối đa|Kích thước ổ đĩa tối đa|Hỗ trợ journaling|Sử dụng|
|--- |--- |--- |--- |--- |---|
|etx2|1993|2TB|32TB|Không|Đã lạc hậu|
|etx3|1999|2TB|32TB|Có|Đã lạc hậu|
|etx4|2006|16TB|1EB|Có|Nên sử dụng nếu chạy hệ thống Linux|
|XFS|1994|8EB|8EB|Có|Nên sử dụng cho các hệ thống server lớn|
|vfat| | | | | Phần mở rộng giúp cho tên file có thể dài hơn, có thể giúp cho hệ điều hành Linux và Window trao đổi file với nhau}
|NFTS|2001|16EB|16EB|Có|Nên sử dụng nếu đang chạy hệ thống, dễ dàng trao đổi với các hệ thống tập tin khác| 
### 4.3. Xem các filesystem hỗ trợ: cat /proc/filesystems
 -  Các filesystem được hỗ trợ trên Linux có thể được xem ở /proc/filesystems
### 4.4 Command: mkfs, lsblk -f, df -Th
#### mkfs
 - $mkfs -t [TYPE] /dev/[DEV_NAME]	Dùng để tạo filesystem cho phân vùng đó.
#### lsblk -f
 - Để xem phân vùng đó sử dụng filesystem nào, có thể sử dụng câu lệnh $lsblk -f
#### df -Th
 - $df dùng để xem trạng thái sử dụng phân vùng
  - $df -Th	Hiện ra thông tin dễ đọc hơn.
## 5.Mounting
### 5.1. Mount là gì?
 - Mount là hoạt động giúp cho filesystem đọc và quản lý dữ liệu trên ổ cứng/phân vùng, nghĩa là gắn việc truy cập đến phân vùng đó thông qua các điểm kết nối trong cây thư mục, ví dụ như 1 cây thư mục trống. Nếu phân vùng đó chưa được mount, thì các dữ  liệu trên đó không thể đọc. Thông thường khi máy tính khởi động nó sẽ bắt đầu mount và khi tát nguồn nó sẽ unmont để xác nhận rằng dữ liệu đã được xử lí hết ở bộ nhớ tạm thời.
### 5.2. Mount partition vào thư mục
 - Các bước thực hiện như sau:
  - $sudo mkdir [NEW_FOLDER]	Tạo thư mục mới
  - $sudo mount /dev/[DEV_NAME] [NEW_FOLDER]	Mount phân vùng đó vào thư mục vừa tạo
  - $df -Th	Để kiểm tra 
### 5.3. Command: du, df, mount, umount
#### duaa
 - Kiểm tra bộ nhớ đã bị chiếm dụng (theo từng khối)
 - $du -ha	Kiểm tra bộ nhớ của từng file/folder hiện tại
 - $du -a --apparent-size Hiện bộ nhớ chiếm dụng theo byte chính x
#### df
 - Kiểm tra dung lượng bộ nhớ còn trống hiện tại
 - $df -ah	Hiện tât cả và hiện dung lượng dễ nh
#### mount
(Đã được đề cập ở trên) 
#### umount
 - Dùng để un mount phân vùng
 - $umount [FOLDER]
  - -f hoặc -l	Sử dụng nếu thiết bị đó đang bận
### 5.4. File /etc/fstab
 - Nếu muốn máy tính tự động mount mỗi khi khởi động, ta sẽ sửa trên file /etc/fstab.
### 5.5. Cách mount vĩnh viễn với /etc/fstab
 - Copy UUID của phân vùng đó 
  - Có thể xem ở lsbls -f
 - Vào /etc/fstab
 - Thêm dòng UUID= [UUID]	Mountpoint([FOLDER]	Type[ext4]	dump default	pass 0
## 6. LVM
### 6.1.RAID là gì
#### Định nghĩa
 - RAID là hệ thống dự phòng và phục hồi dữ liệu, tránh được khả năng mất mát ổ cứng không dáng có.
#### Phân loại
Có 6 loại RAID, nhưng thường các loại được sử dụng như sau:
 - Loại 0: RAID loại 0 coi hai ổ đĩa như một ổ đĩa logic và tiến hành ghi song song cả hai ổ cùng 1 lúc.
  - Ưu điểm: Tận dụng hết tài nguyên phần cứng, truy xuất kết quả nhanh.
  - Nhược điểm: Không có dự phòng, chết 1 ổ là chết hết.
 ![alt Raid loại 0](https://en.wikipedia.org/wiki/Standard_RAID_levels#/media/File:RAID_0.svg)
 - Loại 2: RAID loại 1 tiến hành ghi 1 dữ liệu trên đồng thời 2 hay nhiều ổ cứng.
  - Ưu điểm: Đơn giản, dễ cài đặt, nếu chết 1 ổ thì vẫn còn 1 ổ khác dự phòng.
  - Nhược điểm: Vì là ghi đồng thời  trên 2 ổ nên tốc độ ghi chậm hơn, và không tận dụng hết tài nguyên phần cưng (có 2 mất 1)
 ![alt Raid loại 1](https://en.wikipedia.org/wiki/Standard_RAID_levels#/media/File:RAID_1.svg)
 - Loại 5: RAID loại 5 kiểm tra tính đúng đắn bằng bit chẵn lẻ, và các phân vùng dự phòng được đặt lần lượt qua ccs ổ cứng.
  - Ưu điểm: Khi bị mất 1 ổ cứng thì có thể phục hồi lại được, hiệu năng sử dụng cao hơn với RAID 1.
  - Nhược điểm: Không tận dụng hết tài nguyên phần cứng, cài đặt khó khăn hơn.
  ![alt Raid loại 5](https://en.wikipedia.org/wiki/Standard_RAID_levels#/media/File:RAID_5.svg)
 - Người ta có thể sử dụng nhiều phương pháp RAID cùng lúc để tăng hiệu quả làm việc
  - RAID 1+0
 ![alt Raid 1+0](https://www.enterprisestorageforum.com/imagesvr_ce/5994/DATA_RAID_10.png)
  - RAID 5+0
  ![alt Raid 5+0](https://www.microsemi.com/images/storage/fig9.jpg)
   
### 6.2. LVM là gì
 - Logical Volume Management (VLM) là một phương pháp quản lý các storage device, các logical volume dễ dàng, linh hoạt hơn. Với VLM bạn có thể thay đổi kích thước mà không cần phải sửa lại partition table của hệ điều hành.
![alt Sơ đồ VLM](https://news.cloud365.vn/wp-content/uploads/2019/08/LVM.png)
### 6.3. Các thuật ngữ: physical volume (pv), volume group (vg), logical volume (lv)
 - Physical volume (pv): Là những thành phần cơ bản được sử dụng bởi LVM dể xây dựng lên các tầng cao hơn . Một Physical Volume không thể mở rộng ra ngoài phạm vi một ổ đĩa. Chúng ta có thể kết hợp nhiều Physical Volume thành Volume Groups.
  - Tạo pv: $pvcreate /dev/[DEV_NAME]
  - Kiểm tra pv: $pvs
 - Volume group (vg): Nhiều Physical Volume trên những ổ đĩa khác nhau được kết hợp lại thành một Volume Group. Volume Group được sử dụng để tạo ra các Logical Volume, trong đó người dùng có thể tạo, thay đổi kích thước, lưu trữ, gỡ bỏ và sử dụng.
  - Tạo vg: vgcreate /dev/[DEV_NAME]
  - Kiểm tra pv: $vgs
 - Logical volume (lv): Một Volume Group được chia nhỏ thành nhiều Logical Volume. Nó được dùng cho các để mount tới hệ thống tập tin (File System) và được format với những chuẩn định dạng khác nhau như ext2, ext3, ext4…
  - Tạo lv: lvcreate /dev/[DEV_NAME]
  - Kiểm tra pv: $lvs
 - Sau đó để sử dụng phân vùng đó ta cần mount trước, rồi tạo filesystem trên đó (mkfs)
### 6.4. Example: work with logical volume
#### Extend logical volume 
VD  
 - $lvextend -L [+]2G /dev/[DEV_NAME]	Thực hiện lệnh thêm kích thước
 - $resize2fs /dev/[DEV_NAME]	Nhớ cập nhật lại filesystem
Đôi khi vg không có đủ dung lượng, cần extend vg trước:  
 - $vgextend [VG_NAME] /dev/[DEV_NAME]
#### Resize logical volume 
 - $lvs /dev/[LV_NAME]	Kiểm tra dung lượng còn trống của lv
 - $df -Th /dev/[LV_NAME]	Kiểm tra dung lượng tổng của lv
-> Từ đó ta có thể biết được tối ra có thể rút gọn lại bao nhiêu dung lượng
Trình tự các bước để resize như nhau: umount->reduce filesystem->reduce lv-> remount.  
 - umount /dev/[DEV_NAME]	unmount lv
 - $e2fsck -f /dev/[DEV_NAME]	Kiểm tra filesystem sau khi umount (Pass cả 5 thì ổn)
 - $resize2fs /dev/[DEV_NAME] [NUMBER][G|T]	Resize lại filesystem
 - $lvreduce -L [-][NUMBER][K,M,G,T] /dev/[DEV_NAME]	Resize lại lv
 - $e2fsck -f /dev/[DEV_NAME]	Check lại cho chắc
 - $mount /dev/[DEV_NAME] /[MOUNT_PATH]	remount lại lv

#### Mirror a logical volume
#### Xóa logical volume
 - $umount /dev/[DEV_NAME]
 - $lvremove /dev/ơDEV_NAME]
### 6.5. Example: resize a physical volume
### 6.6. Example: mirror a logical volume
### 6.7. Example: snapshot a logical volume
