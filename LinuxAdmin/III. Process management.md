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
- Viết tắt của Process Status, hiện trạng thái của các tiến trình
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
 - ps auxfw	Hiển thị tất, và cả cây tiến trình nữanữa
#### pstree
 - Dùng để hiển thị các tiến trình theo dạng cây
 - $pstree [ PID | USER ]	Hiện thông tin với người dùng cụ thể hoặc từ số PID đó trở lên
  - -g	Hiện PPID
  - -p Hiện PID
  - -t	Hiện tên thread
  - -N	[TYPE]	Sắp xếp theo [cgroup|ipc|mnt|net|pid|user|uts]

#### top
 - Dùng để hiển thị thông tin các tiến trình, nhưng với thời gian thực
##### Các tham số 
| Tên | Tham số |
|--- |--- |
| PID | Mã số tiến trình |
| USER | Tên user |
| PR | Độ ưu tiên khi lập lịch |
| NI | Giá trị nice, số âm thì giá trị cao, số dương thì giá trị thấp, số 0 nghĩa là không được thay đổi |
| VIRT | Virtual memory size (KiB), lượng bộ nhớ ảo sử dụng bởi tiến trình đó |
| RES | Resident set size |
| SHR | Shaređ Memory Size (KiB)
| S | Trạng thái tiến trình |
| %CPU | %cpu chiếm dụng
| %MEM | %memory chiếm dụng
| TIME+ | Giống TIME |
| COMMAND | Hiển thị lệnh hoặc tên câu lệnh đã mở tiến trính đó |
 - Các giá trị của trạng thái tiến trình:
  - D không thể ngắt
  - I đang nghỉ
  - R đang chạy
  - S dang ngủ
  - T bị dừng bởi tín hiệu
  - t bị dừng bởi debugger
  - Z zombie
  *Các thông số khác được xem ở man page top*
###### Các tùy chọn
  - h	Hiển thị tất cả các tùy chọn
  - q hoặc ESC	Thoát màn hình
  - P	Sắp xếp theo %cpu
  - N	Sắp xếp theo PID
  - T	Sắp xếp theo thời gian chạy
  - R	Đảo ngược thứ tự sắp xếp
  - V	Hiển thị theo cây tiến trình
  - c	Hiển thị đầy đủ đường dẫn câu lệnh
  - o/O	Tạo bộ lọc với tham số với điều kiện (VD: %CPU>0.0)
  - 1	Hiển thị tất cả các lõi CPU	
  - f	Ẩn/ hiển các  cột ( sử dụng Space để chọn/bỏ chọn, sử dụng phím mũi tên để di chuyển và đổi chỗ cá hàng)
  
### 1.4 Chỉ số nice (renice)
 - Chỉ số nice nói lên độ ưu tiên của tiến trình, có giá trị trong khoảng -20 -> 19, số càng bé càng có độ ưu tiên cao, số 0 là mặc định. Có thể thay đổi độ ưu tiên cho tiến trình bằng câu lệnh $nice và $renice.
 - $nice [OPTION] [COMMAND [ARG] ]	Thay đổi chỉ với tiến trình chưa chạy.
 - $renice [-n] [-p [ppid] ] | [-g [pgrp] ] | [-u [user] ] 	Ngắt tiến trình và thay đổi nice.
## 2. Gửi tín hiệu tới process bằng kill  
 - Lệnh kill dùng để gửi tín hiệu đến tiến trình.
 - $kill [OPTION] [PID]
   - -l Hiện danh sách tất cả các tín hiệu 
  - Có 64 tín hiệu tất cả, nhưng chủ yếu sử dụng các tín hiệu liên quan đến ngắt tiến trình:
   - -1 Khởi động lại tiến trình
   - -2 Ngắt kết nối từ bàn phím
   - -9 Kill tiến trình
   - -15 Tạm ngừng 1 tiến trình
   - -17, -19, -23	Dừng 1 tiến trình
### 2.1 -9 -15 trong kill
 - Các bước khi xóa một tiến trình đầy đủ như sau:
  - Ngắt kết nối socket.
  - Xóa temp files.
  - Thông báo cho các tiến tình con là nó sẽ bị xóa.
  - Thiết lập lại các đặc tính đầu cuối của nó
 - Các bước được khuyên sử dụng khi kill một tiến trình như sau:
  - kill -15 [PID|NAME], đợi 1-2 giây
  - kill -2
  - kill -1
 - Sử dụng kill -9 sẽ có thể dẫn đến việc mất mát dữ liệu không mong muốn và tiến trình không đuọcw dọn dẹp đầy đủ.
### 2.3 Command: kill, pkill và killall
- Khác với kill,, pkll ngoài gửi thông tin tín hiệu đến tiến trình mà nó còn có thêm một số lựa chọn khác
 -  $pkill [option] [PARTERN]
  - -e hiển thị tiến trình nào sẽ bị ngắt
  - -c Đến tiến trình trùng khớp
  - -P [PPID] chỉ dừng các tiến trình con của tiến tình cha đó
  - -U	Ngắt tiến trình theo user
  - -t [tty,..] Ngắt tiến trình theo terminal
  - -n	Xóa tiến trình gần đây nhất
  - -o Xóa tiến trình cuối cùng.

 - $killall cũng tương tự với pkill nhưng có một số khác biệt sau
  - -y [TIME] ngừng các tiến trình sau thời gian này
  - -o [TIME] ngừng các tiến trình trước thời gian này
  - -r Tìm kiếm tên theo biểu thức chính quy
## 3. Background processes 
### 3.1 Background processes và foreground process
 - Foreground process là câu lệnh hay task nào đó chạy trực tiếp và phải đợi nó khi nó hoàn tất. Một số tiến trình yêu cầu tham số đầu vào sẽ vào trang thái "đóng băng", đợi người dùng nhập tham số.
 - Background process là câu lệnh hay task nào đó chạy ngầm dưới terminal, không cần tới sự tương tác của người dùng. Nếu có nó sẽ đợi 
### 3.2 Chạy command dưới dạng background process
 - Khi muốn chạy command dưới background, ta thêm & vào cuối câu lệnh.
### 3.3 Command: jobs, fg, bg
  - Ctrl-C	Dùng để ngắt tiến trình ở foreground
  - Ctrl-Z	Dùng để tạm dùng tiến trình ở foreground 
#### $jobs
Hiện danh sách các tiến tình đang ở background
 - $jobs [OPTION] Hiện danh sách các tiến trình ở background
  - Gồm có thứ tự, trạng thái, tên tiến trình hoặc command
  - -r	Chỉ hiện các tiến trính đang chạy
  - -s Chỉ hiện các tiến trình đang bị dừng
  - -p	Chỉ hiện các PID
#### $fg
Dùng để gọi một tiến trình từ background lên foreground
 - $fg [JOB_NUMBER|JOB_COMMAND]	Gọi 1 tiến trình lên foreground cụ thể, ví dụ  $fg %2
#### $bg
Dùng để gọi một tiến trình từ foundground xuống background
 - $bg [JOB_NUMBER|JOB_COMMAND]	Gọi 1 tiến trình xuống background, ví dụ $bg %ping
