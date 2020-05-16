1.Manual
2.Làm việc với thư mục
3.Làm việc với tập tin
4.Hệ thống phân cấp


## 1.Manual
### 1.man
 - man là một lệnh dùng để hiện thị tất cả các lệnh có thể sử dụng được và cách sử dụng, các tham số, ý nghĩa của các tham số,.... khi thực hiện câu lệnh đó. Có thể nói học cách sử dụng man là bước đầu để học được các câu lệnh khác trong terminal.
 - Một số tùy chọn chính:
  - $ man [NUMBER] [COMMAND NAME] 	 hiển thị mô tả toàn bộ về câu lệnh, trong đó [NUMBER] chỉ số trang có thể có (trong trường hợp dài)
  - $ man -a [COMAND NAME] 	 kiểm tra xem có những trang nào có thể có
  - $ man -f [COMMAND NAME] 	 một mô tả ngắn gọn về câu lệnh
  - $ man -k [COMMAND NAME] 	 tìm kiếm một lệnh có chứa [COMMAND NAME]
  - $ man -I [COMMAND NAME] 	 hiển thị mô tả của chính xác tên lệnh
## 2. Làm việc với thư mục 
### 1. Đường dẫn tương đối và đường dẫn tuyệt đối
 - Đường dẫn tuyệt đối là đường dẫn có chỉ mục bắt đầu từ /
 - Đường dẫn tương đối là đường dẫn hiện thời của file/folder đang được thực thi hiện thời hoặc đang được trình diễn.

### 2. Một số câu lệnh đơn giản
 - Kiểm tra thư mục hiện tại :
  - $pwd
	
 - Di chuyển giữa các thư mục:( Sử dụng tab để hiện các thư mục/File có thể có)
  - $cd [FOLDER NAME]			tiến vào thư mục con đó
  - $cd [FOLDER NAME_2]/[FOLDER NAME_2]   Tiến vào thư mục con 2
  - $cd ..  				Trở về thư mục cha
  - $cd ../..				Trở về thư mục cha tầng 2
  - $cd /.	hoặc $cd ~			Trở về thư mục gốc 
  - 
 - Tạo thư mục
  - $mkdir [FOLDER NAME] 			Tạo thư mục
  - $mkdir -m [NUMBER] [FOLDER NAME]	Tạo thư mục + cấp quyền (như chmod)
  - $mkdir -p [FOLDER NAME_1]/[FOLDER NAME_2]	Tạo đồng thời cả hai thư mục cha và con(nếu cần thiết)
	
 - Xóa thư mục
  - Thay  mkdir -> rmdir với chức năng tương ứng
	
 - Tạo file
  - $>[FILENAME] 
  - $gedit [FILENAME] 
  - $vi [FILENAME]
  - $nano [FILENAME]
  
*Có thể tạo nhiều file cùng lúc, ngắn cách bởi dấu cách; tạo file có dấu cách bằng cách thêm / trước dấu cách đó*
	
	
- Liệt kê danh sách:
  - $ls 		Câu lệnh cơ bản
  - $ls [FILENAME]		Hiện danh sách với tập hợp file theo mẫu
  - $ls -l * : Hiện danh sách với kích thước
    -S		Sắp xếp theo kích thước
    -t		Sắp xếp theo thời gian sưa đổi cuối
    -a		Hiện tất cả file, bao gôm file chứa . ở đầu
    -h		Hiện kích thước file
*1 lệnh có thể bao gồm nhiều tùy chọn đi chung với nhau, ví dụ $ls -lSa*
	    
## 3. Làm việc với tập tin trong Linux
### 1. Chữ hoa và chữ thường trong Linux
- Tên file/folder trong Linux phân biệt chữ hoa và chữ thường, vì vậy trong một số tùy chọn của các câu lệnh có tùy chọn dùng để phân biệt hoặc không hai cách nhìn nhận này.
### 2 Mọi thứ trong Linux đều là file
- Hay như một cách nói chính xác hơn của chính cha đẻ Linux, Linus TOrvards :" Mọi thứ đều là những dòng chảy của byte", tất cả mọi dữ liệu, mọi thông tin trên Linux bao gồm cả processes, files, thư mục, sockets, pipes ... đều có thể được truy xuất thông qua các file lưu trữ qua hệ thống virtual system trong nhân Kernel, vì vậy công việc của ta là chỉ cần lệnh đọc "file" đó thôi, và hệ thống sẽ trả về kết quả như lệnh đọc thông thường.
## 3. Một số câu lệnh với file
#### Kiểm tra cấu trúc file với  $file
  - $file [FILENAME] 	Kiểm tra cấu trúc của file
	
#### Chỉnh sửa thời gian với touch
File có 3 loại kiểu thời gian: 
 - Access time: thay đổi mỗi khi truy cập
 - Modified time: thay đổi mỗi khi thay đổi nội dung file
 - Changed time : thay đổi mỗi khi 1 trong 2 time ở trên thay đổi và thay đổi cùng với time thay đổi đó
					
  - $touch [FILENAME] 		Thay đổi Modified time
  - $touch -m [FILENAME]		Tương tự
  - $touch -a [FILENAME]		Thay đổi Access time
  - $touch -r [FILENAME_1] [FILENAME_2] 		Chuyển thời gian tương ứng từ FILENAME_1 sang FILENAME_2
	Các tùy chọn: 
    - -t 'STAMP' hoặc --time="STAMP"		Lựa chọn thời gian cụ thể theo định dạng 'YYYYMMDDHHmm.ss'
    - -d 'STINRG hoặc --DATE="STRING"	Lựa chọn thời gian theo ngữ cảnh(yesterday, 8 MAr, next Tuesday...)
		         	
#### Xóa file
  - $rm [FILENAME]		 Câu lệnh thông thường
  - $rm [FOLDERNAME]/[FILENAME] Xóa file tại đường dẫn
  - $rm -i [FILENAME]	 Hiện cảnh báo các file sẽ xóa
  - $rm -I [FILENAME]	 Hiện cảnh báo nếu xóa 3 file trở lên
*Tùy chọn: Có thể xóa nhiều file theo mẫu biểu thức chính quy:*
    - * : bất kì 0-n chữ cái, số nào 
    - x? : 0-1 chữ cái hoặc số x
    - x+ : 1-n chữ cái hoặc số x
    - {x,y}: lần lượt xóa từng file khi thay x,y
    - {x..y}: Xóa dải giá trị x -> y
	
#### Copy file
  - $cp [FILENAME1] [FILENAME2]	copy cùng thư mục
  - $cp [FILENAME1] [FOLDER]/[FILENAME] copy khác thư mục
Tùy chọn:
    - -u	update từ FILENAME1 sang FILENAME2 nếu khác nhau
    - -i	cảnh báo nếu ghi đè
    - -b	tạo một backup trước khi copy
*Với copy một FOLDER, -r sẽ copy toàn bộ file,folder con của folder cha.*
#### Di chuyển file, folder
- Cũng tương tự với cp. ta có thể thêm tùy chọn -v để kiểm tra phương thức, -i để đảm bảo không bị ghi đè dữ liệu.	
Đọc file với head,tail
  - $head [FILENAME] 	đọc 10 dòng đầu của file
  - $head -n [NUMBER] 	đọc n dòng đầu của file 
    - -v 		Tạo thêm dòng hiện tên file 
Với tail, ta có thêm một câu lệnh hữu ích nữa
  - $tail -F [FILENAME]     hiển thị đồng thời các thay đổi của file (chỉ áp dụng khi file được append)
	
Cắt và hiển thị file với cat
  - $cat [FILENAME]	xuất ra nội dung file qua terminal
Cat là một câu lệnh được đánh giá là vô dụng (xem chi tiết với Useless use of cat), bởi vì nó ngoài việc đọc dữ liệu của file thì nó không làm thêm được gì khác, và còn tạo thêm một process thừa khi kết hợp nhiều câu lệnh với nhau. Và cũng có rất nhiều câu lệnh khác có đầy đủ chức năng và còn hữu ích hơn nên việc câu lệnh được viết thêm cat là không cần thiết.
	
 - EOF - hay End Of File - được định nghĩa trong Here Document, là một từ ngữ ám chỉ trạng thái kết thúc của một file.
khi ta muốn ghi nội dung một file trên Terminal nhưng mà có nhiều dấu xuống dòng, thì ta cần định nghĩa khi nào là kết thúc của file đó, ở đây ví dụ là string EOF.
    - ví dụ: $cat << -e-o-f >filename
    - file contents
    - file contents
    - -e-o-f
    - $
 Như vậy khi gặp kí tự được định nghĩa -e-o-f, terminal sẽ hiểu là kết thúc việc ghi dữ liệu vào file.
	  
#### Làm việc với strings
  - strings [FILENAME]		Hiện ra output các string(mặc định tối thiểu 4 kí tự) của file
  - strings -n [NUMBER] [FILENAME]	Tùy chỉnh min character của string
  - -s [STRING]			Thay ngăn cách dấu xuống dòng bằng dãy tùy chọn
	 
	
  - -tac [FILENAME] 		Đọc file theo từng string và xuất ra theo thứ tự ngược lại
	
## 4. Hệ thống phân cấp
### 1.Filesystem là gì?
- Filesystem là các phương pháp và các các trúc dữ liệu để hệ điều hành có thể thao dõi và quản lý. Filesystem phải được khởi tạo khi lần đầu sử dụng phần vùng ổ cứng đó. Hiện nay, cách tổ chức phân vùng phổ biến là ext4, hỗ trợ kích thước 1 file tới 16TB và tương thích ngược được với các phiên bản trước đó.
- Filesystem có ba thành phần chính: Super block, Inode và Storage block
- Superblock: Được đặt tại vị trí đầu của file system, lưu trữ thông tin về kích thước và cấu trúc filesystem, thời gian cập nhật filesystem cuối cùng và thông tin trạng thái.
- Inode: Lưu trữ thông tin về tập tin và thư mục trong filesystem, gốm có các thông tin: loại tập tin và quyền hạn, người sở hữu, kích thước và số hard link đến tập tin, ngày giờ chỉnh sửa cuối, vị trí lưu nội dung tập tin trong filesystem.
- Storageblock: Nơi lưu dữ liệu thực sự của tập tin và thư mục, chia thành các DataBlock, mỗi block chưa 1024 kí tự. DataBlock của tập tin thường lưu inode của tập tin và nội dung tập tin. Data Block của thư mục thì lưu danh sách những entry Inode, tên tập tin và thư mục con.
- Một số kiểu lưu trữ là: Tập tin dữ liệu, thư mục, tập tin thiết bị (Linux hành xử với thiết bị như là các file), tập tin liên kết.
	
### 2.Hệ thống phân cấp trong Linux
- Mọi thứ trong Linux đều là file, được tổ chức theo cấu trúc dạng cây. Trong đó, thư mục root là / , là thư mục lớn nhất bao trùm toàn bộ filesystem.
- Trong / là các thư mục con với mục đích sử dụng và chức năng khác nhau riêng biệt:
  - Các thư mục chứa các thư viện:
    - /bin 	chứa tệp thực thi nhị phân (executable),  tất cả user được sử dụng lệnh ở đây, được dùng khi boot hệ thống lần đầu hoặc không có phần ổ cứng nào được boot
    - /sbin	giống /bin, nhưng những tệp ở đây được sử dụng bởi admin hệ thống, dùng để duy trì hệ thống
    - /lib	chứa các file thư viện hỗ trợ các thư mục nằm ở /bin và /sbin; đuôi fie có thể là .ld, .lib hoặc .so
    - /opt	chứa các ứng dụng add-on từ các nhà cung cấp
		
  - Các thư mục cấu hình:
    - /etc	chứa các tập tin cấu hình hệ thống, các tập tin lệnh để khởi động các dịch vụ hệ thống, ngoài ra bao gồm shell script startup và shutdown, chạy/ngừng các chương trình cá nhân.
    - /boot	chứa các tập tin cấu hình khởi động hệ thống. Các file Kernel initrd, vmlinux, grub đều nằm trong /root
		
  - Các thư mục dữ liệu:
    - /home	chứa dữ liệu cá nhân của user
    - /root	là thư mục gốc của root user
    - /srv	chứa các service chứa dữ liệu liên quan đến CVS
    - /media  chứa các mount point của các loại thiết bị có thể tháo dời
    - /mnt	chứa mount point các thiết bị được mount trong hệ thống
    - /tmp	chứa các tập tin tạm thời được tạo bởi hệ thống và user. Cấc tập tin này dược xóa khi hệ thống được khwoir động lại
		
		
  - Các thư mục chứa trong RAM:
    - /dev	chứa cá tập tin nhận biết các thiết bị trong hệ thống, bao gồm các thiết bị đầu cuối, USB,...
    - /proc   chứa các thông tin về system process, chứa các thông pin pid vaf tài nguyên hệ thống
    - /sys	lưu trữ và cho phép sửa dổi thiết bị kết nối với hệ thống.
		
  - Các thư mục tài nguyên hệ thống
    - /usr /bin	chứa hầu hết các file thực thi được người dùng sử dụng
        -/include	chứa các thư viện C
        -/lib		chứa các thư viện đối tượng, cũng bao gồm các file nhị phân được sử dụng gián tiếp qua các ứng dụng.
        -/local	chứa các tài nguyên cho người quản trị hệ thống khi cài đặt phần mềm cục bộ.
        -/share	chứa các dữ liệu static về cấu trúc độc lập. 
        -/src		chứa mã nguồn của Linux
			
  - Các thư mục biến động:
    - /var			chứa các thực thể, các biến để hệ thống thay đổi dữ liệu trong lúc sử dụng
        - /log		chứa các log files từ hệ thống và các chương trình khác. /var/log nên cần dược dọn dẹp theo thời hạn.	
        - /mail		chứa file mail của người dùng
        - /lib		chứa các biến dữ liệu động.
		
	
