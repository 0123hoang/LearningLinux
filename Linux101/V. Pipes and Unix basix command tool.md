1. Các luồng dữ liệu cơ bản\
2. Các command filter với pipe\
3. Unix command tool

## 1.Các luồng dữ liệu cơ bản
### 1. Stdin, stdout, stderr
- Trong lập trình máy tính, luồng dữ liệu tiêu chuẩn là các kênh giao tiếp cho dữ liệu vào và ra được kết nối trước khi một chương trình và môi trường của nó được thực thi. Các luông vào/ra dữ liệu chuẩn là stdin, stdout, stderr.
  - stdin là luồng dữ liệu được nhập trực tiếp qua các dòng lệnh thông qua cửa sổ terminal.
  - stdout là luồng dữ liệu kết quả được xuất ra trực tiếp cửa sổ terminal.
  - stderr là luồng dữ liệu xuất ra các tin nhắn thông báo lỗi. Điều này sẽ giúp phân tách hai luồng dữ liệu kết quả (mong đợi) và thông tin lỗi (không mong đợi) với nhau, và xử lí hai luồng thông tin này một cách độc lập.
  ![alt Các luông dữ liệu chuẩn](https://upload.wikimedia.org/wikipedia/commons/thumb/7/70/Stdstreams-notitle.svg/1920px-Stdstreams-notitle.svg.png)
  
### 2. Điều hướng input, output, error 
- Ngoài việc nhập/xuất dữ liệu trực tiếp qua cửa sổ dòng lệnh, ta có thể chuyển hướng luồng dữ liệu qua các nguồn khác nhau, ví dụ như file, các thiết bị như màn hình, chuột, ổ cứng, lưu lượng mạng,... đều có thể là nguồn gửi đi input và là nơi xuất ra output tương ứng.
  - < hoặc 0<	: stdin
  - > hoặc 1>	: stdout
  - 2>	:stderr
### 3. > và >> khi điều hướng output và error
Trên cửa sổ dòngdòng lệnh có một số cách để điều hướng input, output
  - [STRING_1] > [STRING_2] : dấu > thể hiện được hướng đi của luồng dữ liệu (tiến tới). [STRING_1] là một nguồn input, có thể từ chính bàn phím nhập vào sửa sổ dòng lệnh, hoặc từ một file, một thiết bị. [STRING_2] là một nguồn output, đích đến ở đây có thể là cửa sổ dòng lệnh (strout), một file nào đó, một thiết bị nào đó. 
  - [STRING_2] < [STRING_1] : khi ta đổi dấu > < cho nhau, và đảo thứ tự [STRING_1] và [STINRG_2], ta cũng có chức năng tương tự: [STRING_1] là nguồn dữ liệu, [STRING_2] là đích dữ liệu.
  - [STRING_1] >> [STRING_2]: [STRING_2] ở đây chỉ có thể là file, và input từ [STRING_1] sẽ được mở rộng vào cuối [STRING_2] ( khi sử dụng > sẽ là viết đè từ đầu file) 
### 4. Điều hướng output với pipe
- Khi muốn xuất dữ liệu với đích đến là một câu lệnh tiếp theo ta sẽ sử dụng | . 
  - [STRING_1] | [COMMAND_1] : [STRING_1] có thể là input từ 1 file, 1 câu lệnh.
  - [STRING_1] | [COMMAND_!] | [COMMAND_2] > [STRING_2] :Dữ liệu được chuyển tiếp và thực hiện tuần tự thông qua các câu lệnh, và cuối cùng được viết vào [STRING_2]. 
## 2. Các command filter với pipe
### 1. Command grep, egrep, cut, tr, wc, sort, sed, awk, com, diff
 - grep là môt câu lệnh dùng để tìm kiến văn bản trong một file. Đây là một lệnh rất hữu ích và mạnh mẽ, được dùng phổ biến.
  - $grep [OPTIONS] [PATTERNS] [FILES]	In ra cấc dòng có chứa [PATTERNS]
    -  -n	Hiển thị số dòng tương ứng
    -  -c	Chỉ hiện thị số lượng khớp
    -  -l	Chỉ hiển thị file có chứa [PATTERNS]
    -  -L	Chỉ hiển thị file không chứa [PATTERNS]
    -  -i	Tìm kiếm cả chữ cái thường và in hoa
    -	-e	Tim kiếm nhiều kí tự cùng lúc với mỗi [PATTERNS] theo sau -e
    -	-E	Sử dụng biểu thức chính quy với [PATTERNS]
*Các [PATTERNS] và [FILES] đều có thể sử dụng biểu thức chính quy để áp dụng tìm kiếm với nhiều thực thể.*
  - $cut [OPTION] [FILE]	Lấy các phần dữ liệu trong 1 file
    -c [SCOPE_1],[SCOPE_2]	Lấy lượng kí tự tương ứng trên các dòng (Bắt đầu thứ tự là 1)
    -b	Lấy lượng byte tương ứng trên các dòng
    -f	Lấy lượng đoạn tương ứng, sử dụng thêm cới -d [char] dùng để định nghĩa dấu ngăn cách
    --output-delimiter=[STRING]		Ngăn cách các phần tử bằng [STRING] được định nghĩa
  - Trong đó [SCOPE] được biểu diễn như sau:
    -	N	Chỉ riêng phần tử thứ N
    -	N-	Từ phần tử thứ N trở đi
    -	N-M	Từ phần tử N tới M
    -	-M	Từ phần tử 0 tới M
    -	A,B-C,D-F,G-	Kết hợp nhiều bộ chọn khác nhau ngăn cách bởi dấu ,
 - $tr [SET1] [SET2]	Thay thế text SET1 thành SET2, có tác dụng như REPLACE - CTRL+H
  - $tr "[CHARS_1]" "[CHAR_2]" thì nó thực hiện so sánh từng chữ cái một tương ứng với nhau rồi mới thay đổi
    - tr "c1c2c3" "C1C2C3"	Thay thế tương ứng c1->C1,c2->C2...
    - tr "c1c2c3" "C1"	Thay thế c1,c2,c3 -> C1
    - tr "c1" "C1C2C3"	Thay thế c1->C1.
    -d "[CHARS]"	Xóa các chữ cái có trong file
    -s "[CHAR_1]" "[CHAR2]" Xóa các kí tự lặp liền nhau CHAR_1 thành 1 kí tự [CHAR_2] ([CHAR_2] có thể bỏ trống)
   - Một số mẫu kí tự : 
    - [:space:] Dấu cách
    - [:upper:] Các chữ cái viết hoa
    - [:lower:] Các chữ cái viết thường
    - [:digit:] Các chữ số
    - [:alpha:] Các chữ cái
*[CHARS_1] và [CHAR_2] cũng có thể sử dụng biểu thức chính quy để tạo bộ lọc nhiều phần tử*
 - wc [OPTION]  [FILE] Câu lệnh để đếm trong file
    - -c	Đếm byte
    - -m	Đếm kí tự
    - -l	Đếm dòng
    - -L	Chỉ ra dòng dài nhất
    - -w hoặc --words=[STRING] Đếm từ
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
 - $sort [OPTION] [FILE]	Dùng để sắp xếp các dòng trên file
    - -r sắp xếp theo thứ tự ngược lại
    - -R sắp xếp ngẫu nhiên
    - -M sắp xếp theo tên tháng
 - $awk :Đây là một câu lệnh dùng để xử lí text trên file. Không chỉ có những cú pháp đơn giản như thay thế, đếm số lượng, sắp xếp. awk còn dùng để xử lí dữ liệu. Các câu lệnh của awk rất đa dạng và mạnh mẽ, có thể được đóng gói trong 1 file để dễ dàng tái sử dụng sau này. Tuy vậy, để thực hiện một số yêu cầu phức tạp hơn, ta có thể sử dụng với perl và python.
  - $awk [OPTION] [FILE]	Cấu trúc cơ bản
  - $awk '{print $0 "[STRING]" }'	[FILE]	In ra toàn bộ file. Trong đó $0 là toàn bộ file, $1 là tham số t1, $2 là tham số thứ 2, $n là tham số thứ n...
  - Một số biến của awk:
    - NR	Số thứ tự của dòng
    - NF	Số từ của dòng
    - FS	Khoảng cách giữa các từ của file được đọc, tương tự với file xuất ra ta có OFS
    - RS	Phân biệt giữa các dòng từ file được đọc, tương tự với file xuất ra ta có ORS
  - $awk có vòng lặp for, while, có khởi tạo và gán giá trị cho biến tương tự như ngôn ngữ C.
 - $diff [OPTION] [FILE_1] [FILE_2] dùng để kiểm tra sự khác nhau theo từng dòng giữa hai file
    - -y	Hiện 2 file theo cột tương ứng
 - $comm [OPTION] [FILE_!] [FILE_2] so sánh hai file với nhau, và kiểm tra luôn xem nó có sắp xếp hay không
 - Kết quả chia ra làm 3 cột
  - cột 1 hiện những dòng chỉ có trên [FILE_1]
  - Cột 2 hiện những dòng chỉ có trên [FILE_2]
  - Cột 3 (ngoài cùng) hiện những dòng có ở cả hai files.
    - i	So sánh không xét in thường in hoa.
*Nhận xét: Với những yêu cầu cơ bản, có thể dùng $comm để thay cho $diff* 
### 2. Sử dụng regex với các filter
- Regex hay regular expression (biểu thức chính quy) là một tập hợp các quy tắc giúp biểu diễn các chuỗi có chung một quy tắc. Như đã đề cập một phần ở III., các biểu thức chính quy sẽ được liệt kê đầy đủ ở đây
  - /^[STRING]$/	Một chuỗi cơ bản, trong đó toàn bộ dãy được bao đóng ở cặp //.
        - ^	Đánh dấu điểm bắt đầu của chuỗi
        - $	Đánh dấu điểm cuối cùng của chuỗi
  - Với [STRING], ta có các mẫu chung:
  
## 3. Unix command tool
### 1. Command: find, locate, hostnameclt, timedateclt, sleep, tar

