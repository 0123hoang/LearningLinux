1. Quyền cơ bản  
2. Quyền nâng cao  
3. File links  
## 1.Quyền cơ bản
### 1.Quyền hạn là gì
Quyền hạn là quyền của một cơ quan, tổ chức hoặc cá nhân được xác định theo phạm vi nội dung, lĩnh vực hoạt động, cấp và chức vụ, vị trí công tác và trong phạm vi không gian, thời gian nhất định theo quy định của pháp luật.  
Quyền hạn trong Linux được thực hiện tương tự so với môi trường xã hội, nhưng được áp dụng vào môi trường ảo, trên hệ điều hành Linux, theo quy định của người quản trị hệ thống. 
### 2.Các loại file trong linux
 - -	File thông thường: Là những file thông thường
 - d	Thư mục: Là những thư mục thông thường
 - c	Thiết bị: Là những file kết nối với thiết bị cho việc input/output theo stream (bytes, octets) như serial ports, parallel ports, sounds cards. File có đuôi là cdev
 - d	Block: Là những file kết nối với thiết bị cho việc input/output theo khối như hard disk, memory USB cameras, Disk-On-Key.
 - s	Socket: Dùng cho kết nối giữa các processes. Chúng cũng được dùng cho các dịch vụ như X windows, syslog và etc. Socket có thể xử lí đồng thời hai luồng vào và ra cùng lúc và cho phép nhiều người dùng cùng truy cập kết nối đồng thời. VD: kết nối ssh qua port socket 22
 - p	Named pipe: Cũng dùng cho các kết nỗi giữa các processes nhưng bị giới hạn truy cập: chỉ 1 kết nối vào, và 1 đầu ra (FIFO). VD: [COMMAND] | [COMMAND] nghĩa là dùng pipe() thông qua | để truyền input từ process trước sang process sau.
 - l	Liên kết tượng trưng (symbolic link): Có thể hiểu là 1 hoặc nhiều con trỏ trỏ đến file gốc. Có hai loại symbolic link:
 !(Hardlink){https://kythuatmaytinh.files.wordpress.com/2012/02/hard.png}
  - Hard links: Tạo một file mới có giá trị inode trùng với file gốc
   - VD: [a]->[data]; [b]->[data]; Nếu xóa [b] thì [a]->[data], [null]->[data]; Do vậy dữ liệu gốc vẫn có thể truy cập, sửa xóa với hardlink [a]
   
  !(Softlink){https://kythuatmaytinh.files.wordpress.com/2012/02/soft.png}
  - Soft links: Tạo một file mới có giá trị inode trỏ tới địa chỉ file gốc ( # với địa chỉ inode file mới)
   - VD: [a]->[b]; [b]->[data]; Nếu xóa [b] thì [a]->[null],[null]->[data]; Do vậy dữ liệu gốc đã bị xóa, không thể truy cập được.
*Có thể tạo softlink tới bất cứ file nào, nhưng quyền hạn truy cập file thì vẫn phụ thuộc vào file chính.*
### 3.Quyền sở hữu tập tin (user owner and group owner) là gì?
 - User owner: Một người tạo tập tin thì mặc định có toàn quyền đối với tập tin đó.  
 - Group owner: Một nhóm người dùng được áp dụng một quyền chung, mọi thành viên tham gia vào nhóm đó sẽ được mặc định cấp quyền của nhóm đó.
### 4.Quyền truy cập tập tin (file permissions) là gì?
Quyền truy cập tập tin là giới hạn sử dụng mà người đó có thể thực hiện được trên tập tin đó.
### 5.Các quyền truy cập tập tin cơ bản :rwx
  - r Quyền được đọc file
  - w Quyền ghi/sửa file
  - x Quyền được thực thi file
 - Với mỗi loại người sử dụng lại được quy định bởi 3 quyền này, tương ứng với Owner, Group và Other tạo thành bộ 3x3=9 kí tự thể hiện quyền truy cập.  
VD: Sử dụng $ls -l sẽ hiện quyền truy cập của từng file, folder  
rwxrwx--- 1 root aaa 2 May 27 11:27 a  
-rwxrwx--- 1 root aaa 2 May 27 11:27 b  
--w-r----- 1 abc  aaa 6 May 27 11:47 c  
--w-rw---- 1 abc  aaa 8 May 27 13:44 d  
### 6.Command: chgrp, chown, umask
#### chgrp 
Lệnh này dùng để thay đổi group sở hữu của file.  
 - $chgrp [OPTIONS] [GROUP] [FILE] sử dụng cơ bản
  - -R	Với thay đổi group sở hữu với folder, -R sẽ thay đổi quyền sở hữu group cả các file và folder con bên trong đó nữa.
#### chown
 - $chown [OPTION] [USER][:[GROUP]] [FILE] sử dụng cơ bản
Lệnh này dùng để thay đổi owner của file.  
  - Khi câu lệnh là USER:GROUP thì nó sẽ thay đổi cả group luôn
  - -R	Chức năng tương tự với chgrp
#### umask
Lệnh này dùng để kiểm tra, thay đổi quyền mặc định khi một file/folder mới được tạo ra. Umask được hiểu là khi một file/folder mới được tạo ra, nó được cấp một số quyền mặc định, nhưng ta muốn quản lý phân quyền cho nó, nên nó cần phải qua một lớp kiểm duyệt phân quyền nữa, gọi là umask. umask chỉ có thể làm giảm quyền truy cập chứ ko thể tăng thêm quyền được.  
  - Quyền mặc định với file: 666
  - Quyền mặc định với folder: 777
Cách hoạt động của umask: Mỗi con số tượng trưng cho bộ 3 bit quyền truy cập lần lượt của Owner, Group và Other. Với Umask cũng tương tự gồm 3 con số, mỗi số tượng trưng cho 3 bit quyền truy cập. Nếu ta muốn giới hạn quyền nào với umask thì ta sẽ đặt bit đấy =1.  
VD:
    - Cấm other rwx	umask 007  
    - Cấm thêm group w	umask 027	(mặc định group ko có quyền execute)
    - Cấm thêm group r	umask 067
## 2.Quyền nâng cao 
### 1.Số inode là gì
Inode là một cấu trúc lưu trữ dữ liệu trên hệ điều hành Unix. Số inode là một số định danh riêng cho file/folder đấy. Để truy cập vào một file/folder, máy tính sẽ truy cập đến inode của nó, ở đây có 2 trường chính ghi siêu dữ liệu (metadata) của file/folder gồm ngày sửa đổi cuối, quyền truy cập, dung lượng...., trường thứ 2 chính là địa chỉ lưu trữ dữ liệu thật sự của file đó.
 - Kiểm tra inode : $ls -i
### 2.Directory trong linux là gì
Như đã đề cập ở trên, tất cả mọi thứ trong Linux đều là file. Bàn sâu hơn nữa, tất cả mọi thứ trên Linux đều là inode.  
Inode của directory có chút khác biệt với inode lưu trữ tệp thông thường, dữ liệu của nó lưu trữ danh mục của chính nó, của danh mục cha, và tất cả các danh mục con của nó.
### 3. . và .. trong thư mục
 - Thư mục . chính là thư mục hiện thời.
 - Thư mục .. chính là thư mục cha của nó. với thư mục gốc ( thư mục / ), thì . và .. bằng nhau (inode =2).
### 4.Hard link và sym link
*Đã được giải thích ở trên*
### 5. So sánh hardlink và sym link
*Đã được giải thích ở trên*
