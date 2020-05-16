1. Command trong Linux
2. Toán tủ điều khiển trong Linux
3. Biến Shell
4. Nhúng Shell
5. Shell history


## 1. Command trong Linux shell
### 1. Commands and arguments
 - Cấu trúc file 
  - Đuôi là .sh
  - Luôn bắt đầu nội dung file với #!bin/bash 
  - Sử dụng shell file
    - Cần phải đảm bảo cấp quyền thực thi cho file:
    - $ chmod 777 [FILENAME]
    - Lệnh thực thi file shell:	
    - $bash [FILENAME]
    - $sh  [FILENAME]
    - $[FILENAME]
    
### 2. `` và ""
  - "" : Các kí tự trong "" được hiểu như là một string
  - `` : Các kí tự trong `` được hiểu như là một lệnh
  - -> $() : TƯơng tự như ``, nhưng dễ đọc hơn, nên sử dụng thay thế ``
	
### 3. External or builtin commands, alias
- Builtin commands: Các lệnh được thực thi ngay trong shell mà không cần phải tạo thêm tiến trình hoặc mã nguồn bên ngoài. Builtin commands sẽ thực thi nhanh hơn so với external commands.
- External commands: Các lệnh được thực thi sẽ gọi thêm tiến trình khác và thực thi các mã nguồn ở địa chỉ ngoài. Điều này gây thêm lượng thời gian cần thực thi và một số chương trình mở rộng không được đồng bộ giữa các hệ thống khiến cho file shell không được thực thi.
- Alias: Các lệnh được định nghĩa rút gọn lại do chính người dùng đặt ra.
  - Sử dụng: Để có thể sử dụng lâu dài, ta sửa file ~/.bashrc hoặc ~/.zshrc hoặc ~/.config/fish/config.fish tương ứng với loại shell sử dụng
    - Gán bí danh cho câu lệnh sử dụng :  alias [STRING]="[COMMAND_STRING]"
    - Sau đó lưu lại để có thể sử dụng ở terminal mới, hoặc thêm lệnh $source ~/.bashrc để sử dụng ngay ở terminal hiện tại
				  
### 4. type, which, whatis, whereis, whoami
  - $type -a [COMMAND_NAME]: hiển thị kiểu và nơi lưu trữ nguồn của lệnh đó
Kiểu có thể là builtin, alias, nếu là external commands sẽ hiện ra toàn bộ đường dẫn.
  - $which [COMMAND_NAME]: hiển thị đường dẫn của lệnh đó, trống nếu đó là builtin commands.
  - $whatis [COMMAND_NAME]: hiển thị ngắn gọn miêu tả của lệnh đó
  - $whereis [COMMAND_NAME]: hiển thị đường dẫn mã nguồn và file hướng dẫn của lệnh đó
  - $whoami :	hiển thị tên người sử dụng hiện thời
		
## 2. Toán tủ điều khiển khi thực hiện command
### 1. ; & $? && ||  |  # \
  - ;	Khi hai lệnh được ngăn cách bởi ; thì hai lệnh này thực hiện tách biệt và không tác động đến nhau, trả về return status là của lệnh cuối cùng.
  - &	Khi hai lệnh được ngăn cách bởi & thì hai lệnh này thực hiện đồng thời và không tác động đến nhau, ta có thể tiếp tục sử dụng temrminal mà không cần đợi shell chạy xong. Truy cập vào background bằng $bg và &fg, kiểm tra tác vụ với $jobs, kill và tạm ngưng tiến trình với ctrl+C hoặc ctrl+z
  - $?      Kiểm tra trạng thái trả về của 1 câu lệnh cú pháp (0 là thành công)
  - &&	Câu lệnh 1 trả về true thì mới thực hiện câu lệnh 2
  - || 	Câu lệnh 1 trả về false thì mới thực hiện câu lệnh 2 (ngược lại với &&)
  - |	Gửi output từ câu lệnh 1 làm input cho câu lệnh 2
  - |&	Gửi cả output và error từ câu lệnh 1 làm input cho câu lệnh 2
  - #	Khi ở đầu dòng, # biểu thị một comment
  - \	Khi ở trong "", dùng để hiển thị một kí tự đặc biệt hoặc một số lệnh con trỏ
### 2. End of line backslash
  - \ ở cuối dòng: chương trình sẽ hiểu dòng dưới là mở rộng của dòng trên (hai dòng lệnh như 1 dòng)
	
## 3. Biến Shell
### 1. Gắn biến và gọi biến
		
 - Có hai loại biến:	
  - 	Biến hệ thống: Được tạo ra và quản lý bởi Linux, viết bằng chữ hoa.
  -   	Biến người dùng: Được tạo ra và quản lý bởi người dùng, viết bằng chữ thường hoặc hoa.
 - Gắn biến:	Khi gắn giá trị cho biến, không có dấu cách giữ dấu = (Ví dụ $a=5)	
 - Một số quy tắc đặt tên biến: 
  - Sử dụng _ để ngăn cách từ trong tên 1 biến	($a_b_c)
  - Tên biến môi trường và hằng số được viết in hoa($LOG_HOME,$PI)
  - Tên biến cục bộ được viết in thường ($a,$b), nếu biến cục bộ có liên kết với biến môi trường thì phần nào liên kết với biến môi trường thì in hoa($a_SYS)
  - Sử dụng _ đầu tiên để biểu thị biến private
												
### 2. set, unset, export, env
  - $set [VAR[=value]]		Đặt giá trị cho biến môi trường
  - $unset
  - $env  		Hiển thị các biến môi trường	
  - $export [VAR]	Chuyển giá trị của biến sang tiến trình khác
  - $export -p		Hiện tất cả các biến hiện có trong tiến trình hiện có
  - $export -n [VAR]	Loại bỏ biến đó khỏi tiến trình hiện thời
### 3. $PS1, $PATH
- PS1 là một dãy lời nhắc chính, tiếp sau đó là PS2, PS3, PS4. Có thể sửa đổi tại file ~/.bashrc để thêm các thuộc tính cần thiết.
  - Ví dụ: PS1="\d \h $" 	trong đó \d là ngày hiện tại, \h là hostname
  - Thêm vào file ~/.bashrc dòng câu lệnh trên để có thể sửa PS1 về sau.	
 - $PATH là một biến môi trường, dùng để chỉ địa chỉ logic trong hệ thống, thường được sử dụng trong các shell script khi muốn gọi một thư viện tại một đường dẫn cố định
  - Ví dụ:
    - $PATH="usr/bin"
    - export $PATH

## 4. Nhúng shell
### 1. Nhúng shell trên command: ${} và ``
- Như đã đề cập bên trên, `` dễ gây hiểu nhầm và gây khó khăn trong việc đọc hiểu lỗi, nên thay vào đó ta thay thế `` bằng $()
- ${VAR} là tham số thay thế, có thể sử dụng để truy cập vào từng phần tử của array
Ví dụ: ${array[1]}
	 
	
## 5. Shell history
### 1. Sử dụng lại command gần nhất
  - $!!  	Sử dụng lại command gần nhất
  - $![STRING]	Sử dụng lại command có kí tự đầu khớp STRING nhất
	
### 2. Command history
  - $history	Xem lại tất cả các command
  
### 3. Chỉ định sử dụng lại command với !n
  - $n		Sử dụng lại command với số thứ tự n (xem $history)
  
### 4. $HISTSIZE, $HISTFILE, $HISTFILESIZE
  - $HISTFILESIZE [NUMBER]	Giới hạn tổng số câu lệnh được lưu thực sự trong file ~/.bash_history
  - $HISTFILE [NUMBER]	Giới hạn tổng số câu lệnh được nạp vào trong history lúc bắt đầu phiên mới
  - $HISTSIZE [NUMBER]	Giới hạn tổng số câu lệnh xem được trong history

### 5. Ctrl + R
  - Khi ở terminal, Ctrl + R để tìm kiếm lịch sử các câu lệnh đã sử dụng trước đó. Khi đã tìm được rồi thì ấn ESC để thoát
