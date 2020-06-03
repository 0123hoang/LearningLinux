1. Session  
2. Remote access with SSH  
3. SeLinux  
## 1.Session
### 1.1.Phiên trong Linux
Phiên (session) trong Linux thường được đề cấp tới shell session. Khi một người dùng mở một terminal mới, thì có nghĩa là mở một shell session mới.
### 1.2 TTY và PTS session
- tty session là một phiên được sử dụng trực tiếp trên phần cứng hoặc nhân kernel.
- pty (pseudo terminal devide) là một terminal được giả lập trên một chương trình nào đó. pts là phần phục vụ của pty.  
(các cổng tty được kết nối trực tiếp với máy tính như chuột, bàn phím hay dây serial, còn pts thì kết nối vs máy tính qua các ứng dụng kết nối từ xa như ssh, telnet, hoặc từ các chương trình như xterm, screen).  
### 1.3 Interactive và non-interactive shell
#### Interactive shell 
Là shell đọc lệnh và input từ một tty của người dùng.
#### Non-interactive shell
Là shell không cần input từ người dùng.Ví dụ như một shell script.
## 2.Remote access with SSH
### 2.1 SSH là gì
 - SSH hay là Secure Shell là một giao thức điều khiển từ xa cho phép người dùng đăng nhập và sử dụng máy tính quá internet với các phương thức mã hóa bảo mật.  
 - Sử dụng SSH bằng phần mềm như Putty hoặc câu lệnh $ssh {user}@{host}, với user là tên tài khoản muốn đăng nhập, host là địa chỉ server, có thể là IP hoặc web,...
 - SSH sử dụng giao thức TCP/IP để truyền và mã hóa dữ liệu
### 2.2 Public key và private key
#### Private key
Là một giá trị bí mật chỉ riêng người dùng đó sở hữu và không đươc gửi cho ai khác.
#### Public key
Là một giá trị công khai có thể phân phát công cộng cho mọi người cùng biết. Mối liên hệ giữa public key và private key là người dùng khác nếu mã hóa bản tin bằng public key thì chỉ mình sở hữu private key mới có thể giải mã được (mã hóa bất đối xứng).
### 2.3 Sử dụng ssh login vào server sử dụng password
#### Quy trình một phiên đăng nhập ssh như sau:
  - Thiết lập 3 bước kêt nối TCP
  - Hai bên trao đổi phương thức mã hóa sẽ sủ dụng với nhau
  - Server gửi khóa công khai cho Client.
  - Client xác thực khóa công khai đó bởi một cơ quan xác thực.
  - Client gửi lại khóa công khai của mình cho Server (đã được mã hóa). Bắt đầu từ bước này thông tin hai bên được mã hóa.
#### Xác thực Client bằng password:
  - Client sẽ gửi đi password (đã được mã hóa). Server sẽ kiểm tra trên hệ thống, nếu trùng khớp thì sẽ chấp nhận bắt đầu kết nối.
#### Xác thực Client bằng key-pair
  - Server tạo một cặp khóa - private key và public key có quan hệ với nhau. Public key để trong thư mục làm việc của từng Client trên Server cho phép kết nối, private key được gửi qua Client từ trước đó.
  - Khi một phiên bắt đầu, người dùng chỉ cần gõ tên tài khoản và địa chỉ host vào. Với private key được gửi đi, server kiểm tra có trùng khớp với public key ở trong thư mục làm việc không, nếu khớp với nhau thì cho phép kết nối. Client không cần phải nhập mật khẩu nữa
### 2.4 Sử dụng ssh login vào server sử dụng key-pair
Bên server:
 - $ssh-keygen -t rsa	Tạo cặp khóa sử dụng cách thức rsa
 - Đặt file lưu cặp khóa, thông thường để mặc đinh trong ~/.ssh/id-rsa và ~/.ssh/id-rsa.pub
 - Nhập passphare: Có nghĩa là cả mật khẩu này được yêu cầu khi Client đăng nhập, thông thường sẽ để trống
 - Sử dụng $ssh-copy-id [Tên User]@[Host] dùng để copy Public key vào thư mục làm việc của user trên server.
 - Người dùng muốn đăng nhập sẽ copy private key về máy local dùng để xác nhận.
### 2.5 X-forwarding via ssh
X Window System là một gói phần mềm và giao thưc mạng giúp kết nối cục bộ, sử dụng chuột, màn hình , bàn phím với giao diện đồ họa.  
Có thể sử dụng X forwarding trên một máy tính cá nhân dùng để chạy phần mềm đồ họa ( X client) một cách an toàn
 - $ssh -X [User]@[host]	đăng nhập ssh với X forwarding
## 3 SeLinux
### 3.1 SeLinux là gì
 - SELinux (Security-Enhanced Linux) là một kiến trúc về bảo mật được tích hợp vào nhân Linux, do vậy mà SELinux có ở mọi hệ thống phát triển dựa trên Linux từ mobile (Android), Desktop, Server. 
### 3.2 Tác dụng của SeLinux
 - SELinux cung cấp một hệ thống MAC (Mandatory Access Control - Điều khiển truy cập bắt buộc) trong nhân Linux. Trong Linux có cơ chế DAC (Discretionary Access Control - Truy cập theo quyền), một ứng dụng (tiến trình) chạy trong hệ thống với một user (UID, SUID, user đó được phân các quyền tương tác với các đối tượng của hệ thống như các file, socket, các tiến trình khác. Khi hệ thống có chạy MAC, nó sẽ bảo vệ hệ thống khỏi các phần mềm độc hại hoặc các phần mềm có lỗ hổng bảo mật.
### 3.3 Các mode trong SeLinux
 - SELinux trên hệ thống có thể ở các chế độ.

  - Disabled SELinux bị tắt hoàn toàn, không lưu thông tin gì
  - Permissive SELinux cho phép mọi truy cấp, có ghi log tại /var/log/audit/audit.log
  - Enforcing Ghi log yêu cầu truy cập tại /var/log/audit/audit.log, sau đó kiểm tra chính sách, nếu không có quyền sẽ bị từ chối
### 3.4 Disable SeLinux
#### Xem trạng thái hiện thời
 - $sestatus	Kiểm tra trạng thái hiện thời
#### Thay đổi cấu hỉnh SeLinux
 - Truy cập vào /etc/selinux/config
  - Đặt SELINUX=disable dùng để tắt đi, sau đó khởi động lại để áp dụng cấu hình.
