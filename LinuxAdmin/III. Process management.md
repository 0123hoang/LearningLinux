1. Giới thiệu về process  
2. Gửi tín hiệu tới process bằng kill  
3. Background processes  
## 1. Giới thiệu về process  
### 1.1 Tiến trình là gì
 - Tiến trình (process) được hiểu là một chương trình đang chạy trên hệ điều hành. Mỗi một câu lệnh được thực hiện, hệ điều hành khởi tạo 1 hoặc nhiều tiến trính tương ứng.
### 1.2 Process, PID, PPID, init, kill, deamon, zombie
#### PID
 - PID hay là Process ID là mã số định danh riêng cho từng tiến trình. Nó phải là duy nhất.
 - Giá trị cao nhất có thể của PID là 2^22 , hoặc có thể thay đổi ở /proc/sys/kernel/pid_max
 - PID được đánh số thứ tự lần lượt. Khi được gán max, thì PID được gán quay vòng lại
#### PPID
 - Một tiến trình có thế gọi nhiều tiến trình con, đó gọi là tiến trình cha, và các tiến trình con đó có tiến trình cha đã gọi nó là PPID.
#### init process
 - Khi khởi động boot, thì tiến trình PID 1 sẽ được khởi chạy đầu tiên bởi kernel, đó gọi là init process, từ đó nó sẽ gọi các tiến trình khác.
#### deamon process
 - Deamon là một loại chương trình hoạt động ẩn trong background mà không cần sự kiểm soát bởi user. Deamon sẽ được kích hỏạt bởi một sự kiện hoặc điều kiện nào đó xẩy ra cụ thể. Do đó deamon không có terminal, nên không có tty.
#### zombie process
 - Được hiểu là một tiến trình con được thông báo là đã được hủy bỏ nhưng chưa hoàn toàn xóa bỏ khỏi bộ nhớ, vẫn chiếm lấy tài nguyên máy, PID và nó có trạng thái Z.
#### kill process
 - Lệnh kill dùng để can thiệp yêu cầu tắt tiến trình trước khi nó hoàn thành.  
  - Có thể kill tiến trình bằng ctrl+C hoặc sử dụng câu lệnh kill (chi tiết bên dưới)
### 1.3 Command: $$, pidof, ps, pstree, top
#### PID với $
 - $!	Chỉ ra PID được thực hiện gần nhất ở trong background
 - $$	Chỉ ra chính PID của bash shell này
 - $0	Chỉ ra tên của shell hoặc shell script này (tham số đầu tiên của dòng lệnh)
 - $*	Lưu tất cả các tham số đã được nhập vào
 - $@	Giống $* nhưng các giá trị được lưu trong ngoặc kép
#### pidof
Dùng để tìm pid của chương trình đang chạy
 - $pidof [OPTION] [PROGRAM]	Tìm kiếm PID theo tên
  - -x	Tìm kiếm cả các script nữa
  - -z	Tìm cả các process không thể bị chặn (uninterruptible - D) hoặc zombie (Z)
  - -d [SEPARATE]	Nếu có nhiều PID, ngăn cách nhau bởi [SEPARATE], mặc định là dấu cách
#### ps
Viết tắt của Process Status, hiện trạng thái của các tiến trình
##### Các thông số của ps
| Tên | Thông số |
|--- |--- |
| USER | Hiện tài khoản sử dụng process đó |
| PID | PID của tiến trình đó |
| PPID| PPID của tiến trình đó |
| %CPU| |
|%MEM| |
| CPU| |
| MEM| |
|START| Thời gian bắt đầu tiến trình đó |
|SZ| Tổng bộ nhớ dử dụng |
|TIME| Tổng CPU sử dụng |
|COMMAND| Câu lệnh PID đó
|TT hoặc TTY| Terminal theo loại gì|
|S hoặc STAT| Hiển thị trạng thái hiện thời của tiến trình đó |
Các thông số khác của ps được xem ở ![alt đây](https://www.man7.org/linux/man-pages/man1/ps.1.html)
 - Trong đó các hiển thị trên dòng Status:
 |Kí hiệu| Ý nghĩa|
 |--- |--- |
 |D|Không thể ngắt (IO)|
 |I| Luồng kernel đang nghỉ |
 |R| Đang chạy hoặc có thể chạy (hàng chờ)|
 |S| Đang đợi một event để hòan thành |
 |T| Dừng bởi job control signal|
 |t| Dừng bởi debugger|
 |W| Paging|
 |X| Đã chết (ko bh được thấy)|
 |Z| Tiến trình Zombie|
 |<| Độ ưu tiên cao (không tốt với người dùng khác)|
 |N| Độ ưu tiên thấp (tốt vói người dùng khác)|
 |L| Có pages locked vào memory (cho real-time và custom-IO)|
 |s| Là một session leader|
 |l| Là một multi-threaded|
 |+| Đang ở trong foreground process group|
##### Một số câu lệnh ps hay dùng
 - ps aux	Hiển thị tất cả tiến trình theo format BSD
 - ps x	Hiển thị tất cả quá trình sở hữu bởi người dùng.
 - ps fp [NUMBER]	Hiển thị tiến trình có PID cụ thể
 - ps fg [GROUP]/[NUMBER]	Hiển thị tiến trình của group cụ thể
 - ps -U root -u root	Hiển thị tiến trình sở hữu bởi root
 - ps auxfw	Hiển thị tất, và cả cây tiến trình nữa
#### pstree
#### top
 	
### 1.4 Chỉ số nice (renice)
## 2. Gửi tín hiệu tới process bằng kill  
### 2.1 Command kill
### 2.2 -9 -15 trong kill
### 2.3 Command: kill, pkill và killall
## 3. Background processes 
### 3.1 Background processes và foreground process
### 3.2 Chạy command dưới dạng background process
### 3.3 Command: jobs, fg, bg
