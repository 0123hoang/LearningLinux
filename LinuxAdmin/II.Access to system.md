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
### 2.4 Sử dụng ssh login vào server sử dụng key-pair
### 2.5 X-forwarding via ssh
## 3 SeLinux
### 3.1 SeLinux là gì
### 3.2 Tác dụng của SeLinux
### 3.3 Các mode trong SeLinux
### 3.4 Disable SeLinux
