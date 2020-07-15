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
*https://bugs.launchpad.net/lightdm/+bug/871070 $who và $w có thể không được hoạt động đúng đắn, vậy nên muốn danh sách toàn bộ người dùng, ta có thể truy cập trực tiếp vào /etc/passwd.*
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
- < user list > < host list > = < operator list > < tag list > < command list >
  - User list	là một nhóm người hoặc 1 bí danhkhác
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
Lệnh này dùng để thêm một người dùng mới  
    - -g	[UID]	Gán UID group 
    - -u [UID]	Tạo UID 
    - -U	Tạo một user group mới gán cho user mới có ID trùng nhau
    - -G [GROUP_NAME]	Gán tên group
    - -m	Tạo home directory mới
    - -d [DIR]	Gán thư mục Home cho tài khoản
    - -e [DATE]	Tạo ngày hết hạn cho tài khoản đấy
    - -s [DIR]	Gán thư mục shell cho tài khoản  
*Còn đối với trường thông tin còn lại - Password - sẽ được gán sau bởi lệnh $passwd*
### 5. userdel
Lệnh này dùng để xóa file và thông tin đến người dùng (tài khoản, mật khẩu)
 - $userdel [OPTION] [USER]
    - -r	Xóa luôn cả home directory và mail spool
    - -f	Xóa luôn cả files không được sở hữu bởi người dùng này
### 6. usermod
Lệnh này dùng để sửa đổi thông tin về tài khoản người dùng
 - $usermod [OPTIONS] [USER]
    - -d [HOME_DIR]	Tạo mới thư mục home cho người dùng, nếu dùng thêm -m, sẽ di chuyển toàn bộ file từ thư mục cũ sang thư mục mới.
    - -e [EXPIRE_DATE]	Tạo ngày tài khoản hết hạn (bị khóa)
    - -G GROUP1,GROUP2,...	Gán group cho tài khoản, nếu group hiện thời không được liệt kê, nó sẽ bị xóa khỏi group đó, cần phải thêm -a để thực hiện lệnh append.
    - -l [NEW_USER]	Đổi tên user, đồng thời địa chỉ thư mục home và mail cũng được đổi tương ứng
    - -L	Khóa mật khẩu
    - -U	Mở khóa mật khẩu
    - -s [SHELL]	Chọn Shell cho tài khoản đó, nếu để trống thì sẽ là shell mặc định
*Ngoài ra còn có các câu lệnh với chức năng tương tự như adduser, deluser, các câu lệnh thực thi với group như addgroup, groupadd, groupmod với một số chức năng tương đương cơ bản*
*Nếu muốn người dùng cụ thể thoát phiên sử dụng ngay lập tức, thực hiện câu lệnh $pkill -KILL -u [USER]*
### 7. Thư mục /etc/skel
Đây là thư mục mà khi người dùng mới được tạo ra (useradd), thư mục này sẽ được copy vào thư mục gốc của tài khoản đó.  
Muốn thay đổi cấu hình mặc định cho useradd, ta sửa file /etc/default/useradd.
### 8. User Login Shell : /bin/bash và  /usr/sbin/nologin
#### /bin/bash
bash là một shell phổ biến. /bin/bash là đường dẫn đến thư mục nhị phân dùng để thực thi các câu lệnh bash.  
Các  bash script file muốn chạy bash shell, dòng đầu tiên sẽ chỉ tới file bash này: #!/bin/bash  
Muốn các script file chạy theo shell khác, ta sẽ đổi đường dẫn cho shell đó, VD: #!/bin/rbash; #!/bin/zsh.  
Một người dùng cũng có thể sử dụng shell theo tùy chọn bằng cách thay đổi đường dẫn shell file (xem $usermod -s [SHELL_DIRECTORY] [USER])
#### /usr/sbin/nologin
Đây là file dùng để thay đổi dòng tin nhắn mỗi khi có tài khoản đăng nhập sai, dùng để thay dòng thông báo mặc định
File /etc/nologin.txt sẽ được đọc, nếu có.
### 9. Thư mục home user
Đây là thư mục chứa các dữ liệu cá nhân như video, hình ảnh, file và các file cấu hình của tài khoản đó, thường được đặt trong /home/[USER_NAME].  
Đường dẫn thư mục home có thể được tìm kiếm ở trong /etc/passwd, được di chuyển, thay đổi bằng $usermod -m
## 3. Quản lý mật khẩu user
### 1. Command : passwd, chpasswd
#### passwd
Dùng để đổi mật khẩu tài khoản
  - $passwd [OPTIONS] [USER] Đổi mật khẩu
    - -d	Xóa mật khẩu (mật khẩu sẽ thành để trống)
    - -e	Cho mật khẩu hiện thời hết hạn luôn, người dùng sẽ phải đổi mật khẩu với lần đăng nhập sau
    - -l	Khóa mật khẩu, người dùng sẽ không thể thay đổi mật khẩu
    - -u	Mở khóa mật khẩu
    - -S	Hiện thông tin về mật khẩu
*Muốn cho tài khoản không đăng nhập được (vẫn giữ nguyên mật khẩu), phải dùng usermod -L [USER]
#### chpasswd
Lệnh này dùng để thay đổi mật khẩu cho nhiều tài khoản cùng lúc. $chpasswd sẽ lấy input đầu vào và sẽ thay đổi mật khẩu của nhưng tài khoản đó.  
- VD
  - $echo '[USER]:[NEW_PASSWORD]' | sudo chpasswd
  - $cat [FILE_USERNAME] > sudo chpasswd
### 2. Shadow file : /etc/shadow
Đây là file lưu trữ mật khẩu được mã hóa và một sô thông tin liên quan đến mật khẩu. Dù mật khẩu được lưu dưới dạng mã hóa nhưng file này chỉ đượcc quyền root duy nhất vào đọc, giúp tăng khả năng bảo vệ tài khoản.  
Có tất cả 9 trường thông tin trong file, được ngăn cách bởi dấu :
  - 1.Tên tài khoản
  - 2.Mật khẩu dưới dạng mã hóa
  - 3.Ngày thay đổi cuối (tính từ 1/1/1970)
  - 4.Tuổi thọ ngắn nhất của mật khẩu: cần phải số ngày ít nhất thì mới được đổi mật khẩu tiếp
  - 5.Tuổi thọ dài nhất cảu mật khẩu: cần phải đổi mật khẩu trước số ngày này trước khi bị hết hạn
  - 6.Ngày cảnh báo hết hạn: giúp cảnh báo trước số lượng ngày trước khi mật khẩu bị hết hạn
  - 7.Ngày cảnh báo quá hạn: giúp cảnh báo trước số lượng ngày sau khi mật khẩu bị hết hạn
  - 8.Ngày hết hạn của tài khoản: Tính từ 1/1/1970
  - 9.Trường này được dự trữ để dùng cho sau này
*Các giá trị mặc định được khởi tạo ở /etc/login.defs*
### 3. Mã hoá mật khẩu
- Trường mật khẩu lại chia ra làm 3 trường con, ngăn cách bởi $
  - Số từ 1-6	quy định thuật toán mã hóa
  - hash_salt	mật khẩu salt sinh ra ngẫu nhiên cho từng user được thêm vào mật khẩu chính và được mã hóa cùng với mật khẩu chính
  - hash_data	phần (mật khẩu + salt) đã được mã hóa
 - Với hash_salt, mật khẩu thực sẽ được dài thêm một đoạn kí tự ngẫu nhiên, rồi sau đó mới được mã hóa trên hệ thống. Điều này giúp bảo vệ những mật khẩu yếu và những mật khẩu bị trùng nhau khi bị tấn công vét cạn.
### 4. File cấu hình /etc/login.defs
Đây là file cấu hình mặc định khi ta thực hiện một số lệnh khởi tạo và xóa người dùng, nhóm. Hệ điều hành sẽ sử dụng các giá trị trong /etc/login.defs để tự động gán các giá trị cho chúng.  
Một số giá trị trong file  
  - PASS_MAX_DAYS
  - PASS_MIN_DAYS
  - UID_MIN
  - UID_MAX
  - CREATE_HOME
*Đầy đủ các giá trị của login.defs có thể xem ở http://man7.org/linux/man-pages/man5/login.defs.5.html*
### 5. Banner login
Đây là những dòng thông báo/cảnh báo người dùng khi đăng nhập vào hệ thống. Mỗi server cần có một banner login để tránh những người dùng ngây thơ cố tình đăng nhập trái phép vào hệ thống.
 - Thay đổi banner login : $ sudo vi /etc/issue.net
 - Thay đổi đường dẫn banner login với trên ssh: 
  - $sudo vi /etc/ssh/sshd_config
    - Chỉnh sửa đường dẫn banner tại: Banner /etc/issue.net (hoặc link khác)
 - Khởi động lại ssh service: systemctl restart ssh (restart/stop/start)
## 4. Quản lý nhóm  
### 1. Command : groupadd, groupdel, usermod, groupmod, gpasswd, newgrp, vigr
#### groupadd
Dùng để thêm nhóm mới
 - $groupadd [OPTION] [GROUP_NAME]
  - -g [GID]	Gán GID cho group đó, nếu không sẽ được gán theo số mặc định
  - -r [SID]	Gán SID cho group đó (SystemID)
#### groupdel
Dùng để xóa nhóm
 - $groupdel [OPTION] [GROUP_NAME]
*$groupdel chỉ được xóa với những group không phải là group chính, và phải xóa người dùng ra khỏi group chính đó trước. Cần phải check những file nào đang thuộc quản lý của group đó trước khi xóa*
#### groupmod
Dùng để thay đổi cấu hình nhóm
 - $group [OPTIONS] [GROUP_NAME]
  - -g [GID]	Đổi GID của group
  - -n [NEW_GROUP]	Đổi tên group
#### gpasswd
Dùng để quản lý nhóm
 - $gpasswd [OPTIONS] [GROUP_NAME]
  - $gpasswd [GROUP_NAME]	Thay đổi mật khẩu cho nhóm
  - -a [USER]	Thêm user vào nhóm
  - -d [USER]	Xóa user khỏi nhóm
  - -r	Xóa mật khẩu nhóm
  - -A	[USERS]	Đặt quyền admin cho nhiều người dùng
  - -M [MEMBERS]	Đặt quyền member cho nhiều người dùng
#### newgrp
Lệnh này dùng để thêm người dùng hiện tại gán vào group mới
 - $newgrp [-] [GROUP_NAME]	thêm người dùng hiện tại vào group mới
  - -	Nếu tham gia thành công, khởi tạo lại terminal cho khớp với policy group
 
#### vigr và vipw
Hai lệnh này dùng để truy cập và chỉnh sửa một cách nhanh chóng /etc/passwd và /etc/groups.  
### 2. Tìm hiểu nhóm sudo (wheel)
Sudo (superuser do) là một lệnh dùng để thực thi với quyền root.  
Wheel là tên một group, được gán quyền thực thi sudo cụ thể trong /etc/sudoers. (tên group tùy chọn)  
Khi user được add vào group wheel, user đó sẽ được cấp quyền thực thi sudo.
### 3. So sánh nhóm : sudo (wheel) và root
Root có quyền thực thi cao nhất, không hề bị giới hạn bởi bất kì lệnh hay file nào.  
Sudo group chỉ được cấp quyền sudo cụ thể qua /etc/sudoers (Đôi khi chỉ có quyền sudo write/read/execute), nếu không được cấu hình qua sudoers, thì group đó không được thực thi quyền sudo nữa.
### 4. Thực hiện command dưới quyền sudo
Khi user muốn thực thi với quyền sudo, thì trước câu lệnh sẽ thêm lệnh $sudo vào đầu, kèm theo đó là yêu cầu mật khẩu của chính người dùng đó (không phải mật khẩu root).
## 5. User profiles  
### 1. system profile
Hệ thống Linux có một profile dùng để thực thi mỗi khi có người dùng đăng nhập vào. Nó được chứa trong /etc/profile.  
Khi hệ thống khởi tạo, file /etc/profile bao gồm cacstham số hệ thống cần thiết cung cấp cho mỗi user. Sau đó sẽ chạy file /etc/bash.bashrc, khởi tạo hầu hết các cấu hình. Sau đó thư mục /etc/profile.d sẽ được kiểm tra các script nếu có, để cài đặt cho các ứng dụng khác.
### 2. ~/.bash_profile
Nếu có file này trên thư mục home, hệ thống sẽ tìm bash trên đó và sử dụng. Nó cũng thêm biến $HOME/bin vào biến $PATH
### 3. ~/.bash_login
Nếu không có file ~/.bash_profile, thì bash sẽ kiểm tra ~/.bash_login và lấy nguồn của nó.
### 4. ~/.profile
Nếu vẫn không có file ~/.bash_profile, thì bash sẽ tìm kiếm ~/.profile với chức năng tương tự, và có thể gọi ~/.bashrc nếu có.
### 5. ~/.bashrc
~/.bashrc thường được dẫn nguồn bởi các script khác. ~/.bashrc kiểm tra /etc/bashrc và lấy nguồn của nó.  
~/.bashrc sẽ cấu hình các biến môi trường, lưu trữ các lệnh bí danh (alisas command) và tìm cả các bí danh trên ~/.bash_aliases nếu có.
### 6. ~/.bash_logout
Dùng để xóa console khi thoát bash.
*/etc/profile và /etc/bash.bashrc sẽ được gọi bởi hệ thống với mọi người dùng; còn ~/. nằm trong thư mục cá nhân nên sẽ được gọi đè và khác nhau với mỗi người dùng.*
*~/.profile sẽ khởi động mỗi khi đăng nhập; ~/.bashrc sẽ được khởi động mỗi khi mở một console mới.



