1. Mở đầu về user  
2. Quản lý user  
3. Quản lý mật khẩu user  
4. Quản lý nhóm  
5. User profiles  

## 1. Mở đầu về user  
### 1. Người dùng là gì?
 - Người dùng (User) là một thuật ngữ chuyên dụng trong ngành công nghệ thông tin. Một người dùng là một người sử dụng máy tính hoặc dịch vụ mạng.
 - Đặc điểm của người dùng là phần lớn không có chuyên môn cao trong việc am hiểu phần mềm hay máy tính mình đang sử dụng, và có khả năng sử dụng chúng một cách không thích hợp có thể gây ảnh hưởng đến bản thân và tổ chức.
 - Một người dùng thường được cấp một tài khoản dành riêng cho người đó, tài khoản đó được quản lý và giám sát bởi hệ thống thông qua các thuộc tính như username, login name, nickname, domain.
### 2. Các loại người dùng trên hệ thống
Trên hệ thống Linux có 3 loại tài khoản người dùng chính: administrative(root), regular(thông thường) và service(dịch vụ)
 - Người dùng admin được tạo ra khi cài đặt Linux, là người có quyền cao nhất, hay được gọi là super user.
 - Người dùng thông thường có quyền thực hiện một số tác vụ tiêu chuẩn trên máy như soạn thảo, trình duyệt web,... Họ không có đặc quyền như admin như các quyền chỉnh sửa quyền tài khoản hay xóa/chỉnh sửa những files cài đặt quan trọng
 - Người dùng dịch vụ được cấp những dịch vụ tối thiểu cho phép thực hiện những nhiệm vụ đó. 
  - Mỗi người dùng trên Linux được đăng kí số định danh riêng (UID), thông thường lớn hơn 500. UID nhỏ hơn 500 được dành cho người dùng hệ thống.
## 2. Quản lý user  
### 1. Command : whoami, who, w, id, su , su, visudo
 - $whoami [OPTION]	Dùng để hiển thị người dùng đang sử dụng hiện thời.
 - $who	Hiện thông tin về tất cả người dùng đang đăng nhập hiện thời.
  - -a	Hiện tất cả user
  - -H	Hiện user với các dòng thông tin tương ứng.
 - $w [OPTIONS]	Hiện thông tin người dùng đang đăng nhập vào và đang làm gì.
  - -i	Hiện ip thay vì tên của người dùng (nếu có thể)
 - $id [OPTION] [USER]	Hiện thông tin của người dùng hoặc nhóm người dùng.
  - -u	Chỉ in ra userID
  - -g	Chỉ in ra các groupID của người dùng hiện thời
  - -G	In ra tất cả groupID
 - $su	[OPTION] [-] [USER]	Chuyển đổi tài khoản người dùng
  - $su - [USER]	Đăng nhập với tên [USER]
    - $su -c [COMMAND] [USER] Thực hiện câu lệnh với tài khoản khác
    - -s [PATH_TO_SHELL]	Chuyển shell khác để dùng
    - -p	Chuyển sang tài khoản khác nhưng vẫn giữ thư mục hiện tại
 - $visudo	Dùng để chỉnh sửa file sudoers, phải được thực hiện với quyền sudo
#### sudoers file chứa trong /etc/. Đây là file lưu trữ quyền và phân quyền người dùng trong kệ thống. Cấu trúc file sudoers gồm có các phần sau
##### Bí danh
 - Bí danh (aliases) là tên gọi thay thế. Có 4 kiểu bí danh
  - Bí danh cho người dùng (UserAlias)	Dùng để thay thế cho một người, một nhóm người hay nhiều nhóm người.
    - User_Alias USERS = tom, jerry
    - User_Alias ADMINS = %admin (dấu % biểu thị đây là một nhóm)
    - User_Alias NETWORK = USER, !ADMINS (dấu ! biểu thị loại trừ quyền)
  - Bí danh cho UID (Runas Aliases)	Dùng dể thay thế riêng cho UID
   - Runas_Alias ROOT = #0 (dấu # biểu thị là UID )
   - Runas_Alias ADMINS = %admin, ROOT
  - Bí danh cho Host (Host_Aliases)	Dùng để thay thế cho một địa chỉ mạng, hoặc một network
   - Host_Alias SERVERS = 192.168.0.1, 192.168.0.2, server1
   - Host_Alias NETWORK = 192.168.0.0/255.255.255.0
   - Host_Alias WORKSTATIONsS = NETWORK, !SERVERS
  - Bí danh cho câu lệnh (Command_Aliases)	Dùng đẻ thay thế cho lệnh, hay một nhóm lệnh
   - Command_Alias SHUTDOWN_CMDS = /sbin/poweroff, /sbin/reboot, /sbin/halt
#### Đặc tả riêng cho người dùng
Dây là nơi mà lệnh $who sẽ sử dụng. Cấu trúc của nó như sau
- <user list> <host list> = <operator list> <tag list> <command list>
  - User list	là một nhóm người hoặc 1 bí danh
  - Host list	là một nhóm host hoặc bí danh host
  - Operation list	là một nhóm người, bí danh được cấp quyền như Runas_Alias
  - Command list	là một nhóm hay bí danh các câu lệnh
  - Tag list	là các nhãn cho các quyền đặc biệt, có thể có hoặc để trống (NOEXEC, EXEC, NOPASSWD, NOSETENV...), có thể gán riêng cho từng nhóm hoặc từng bí danh
Một số ví dụ
    - ray	rushmore = NOPASSWD: /bin/kill, PASSWD: /bin/ls, /usr/bin/lprm	
    - dgb	boulder = (operator : operator) /bin/ls, (root) /bin/kill, /usr/bin/lprm
### 2. File /etc/passwd
#### Giới thiệt /etc/passwd
Là file chứa tên các thông tin về user như UserID, Password, UID, GID, Home directory, bash directory,...
#### Nguyên lý hoạt động 
Password được đánh dấu bằng dẫu x và được lưu trữ thực sự ở file /etc/shadow. Việc này nhằm tránh người dùng khác có thể thấy được tất cả các mật khẩu của người dùng khác - dù chỉ là mật khẩu đã được mã hóa. /etc/shadow thì chỉ được duy nhất admin root có quyền truy cập.
#### Cấu trúc file 
Câu trúc file gồm có 7 trường chính
 - Username	Tên người dùng khi đăng nhập
 - Password	Đánh dấu bằng 'x'
 - UserID	Định danh duy nhất cho mỗi user
 - GroupID	Chứa groupID đầu tiên user này tham gia
 - UserIDInfo	Chứa thêm thông tin (GECOS field)
 - HomeDirectory	Chứa thư mục làm việc chính
 - Shell	Đường dẫn tới login shell
### 3. Tài khoản : root là gì. Đăng nhập vào tài khoản root với command : su root
- Tài khoản root là tên người dùng hoặc tài khoản mà theo mặc định có quyền truy cập vào tất cả các lệnh và file trên Linux hoặc hệ điều hành giống Unix khác. Root cũng được gọi là tài khoản root, người dùng root và siêu người dùng. Tài khoản root là đặc quyền lớn nhất trên hệ thống và có quyền lực tuyệt đối đối với nó (tức là truy cập đầy đủ vào tất cả các file và lệnh). Một trong số các quyền hạn của root là khả năng sửa đổi hệ thống theo bất kỳ cách nào bạn muốn, cũng như cấp và thu hồi quyền truy cập (nghĩa là khả năng đọc, sửa đổi và thực thi các file và thư mục cụ thể) cho những user khác, kể cả mặc định dành riêng cho root.
 - Đăng nhập vào tài khoản root bằng lệnh $sudo su -
### 4. useradd và file cấu hình /etc/default/useradd
 - $useradd dùng để thêm một người dùng mới
   - g	[UID]	Gán UID group 
   - u [UID]	Tạo UID 
   - U	Tạo một user group mới gán cho user mới có ID trùng nhau
   - G [GROUP_NAME]	Gán tên group
   - m	Tạo home directory mới
   - d [DIR]	Gán thư mục Home cho tài khoản
   - e [DATE]	Tạo ngày hết hạn cho tài khoản đấy
   - s [DIR]	Gán thư mục shell cho tài khoản  
*Còn đối với trường thông tin còn lại - Password - sẽ được gán sau bởi lệnh $passwd*
### 5. userdel
Lệnh này dùng để xóa file và thông tin đến người dùng
 - $userdel [OPTION] [USER]
 	-r	Xóa luôn cả home directory và mail spool
 	-f	Xóa luôn cả files không được sở hữu bởi người dùng này
### 6. usermod
### 7. Thư mục /ec/skel
### 8. User Login Shell : /bin/bash và  /usr/sbin/nologin
### 9. Thư mục home user
## 3. Quản lý mật khẩu user
### 1. Command : passwd, chpasswd
### 2. Shadow file : /etc/shadow
### 3. Mã hoá mật khẩu
### 4. File cấu hình /etc/login.def
### 5. Banner login
## 4. Quản lý nhóm  
### 1. Command : groupadd, groupdel, usermod, groupmod, gpasswd, newgrp, vigr
### 2. Tìm hiểu nhóm sudo (wheel)
### 3. So sánh nhóm : sudo (wheel) và root
### 4. Thực hiện command dưới quyền sudo
## 5. User profiles  
### 1. system profile
### 2. ~/.bash_profile
### 3. ~/.bash_login
### 4. ~/.profile
### 5. ~/.bashrc
### 6. ~/.bash_logout



