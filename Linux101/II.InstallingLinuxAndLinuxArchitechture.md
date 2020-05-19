1. Cài đặt một số bản phân phối của Linux
2. Kiến trúc trong Linux


## 1. Cài đặt một số bản phân phối Linux
### 1. Nguồn IOS
 - Cách thông dụng nhất là tải về bản IOS chính thức trên trang chủ của phiên bản Linux đó. Một cách khác là download IOS qua Torrent. Một cách khác nữa là ta có thể boot trực tiếp qua mạng (ví dụ như Ubuntu Live installer).

### 2. Cài đặt Debian 8
 - Sau khi có Debian IOS từ trang chủ của Debian, ta tạo một USB bootable device, sau đó khi khởi động lại máy tính, ta vào boot hệ thống để chọn boot Debian. Sau một số lựa chọn cơ bản như ngôn ngữ, domain, đặt mật khẩu, ta đến phần chọn phân vùng ổ cứng. Ta chọn Manual để cấu hình tùy chọn ổ cứng hoặc các tùy chọn còn lại để format toàn bộ dữ liệu ổ cứng để cài mới Debian.
	Sử dụng và tạo phân vùng trống để cài đặt Debian trong đó:
- 1. Primary, min khoảng 20GB, mount point \ , ghi dữ liệu theo kiểu Beginning, và đặt Bootable flag= on để boot vào ổ đĩa này.
- 2. Primary, dung lượng bạn chọn để lưu trữ dữ liệu, mount point \home, ghi dữ liệu theo kiểu Beginning,  để Booosable flag= off.
- 3. Ngoài ra ta cũng có thể tạo thêm một swap disks dùng để lưu trữ dữ liệu tạm thời khi mà RAM bị đầy.
	
 - Bước quan trọng nhất đã xong, ta chấp nhận cấu hình rồi đợi cài đặt, ta có thể tùy chọn thêm một số phần mềm đi kèm sau đó. Tiếp theo ta chọn YES vào phần install GRUB boot loader để giúp có thể chọn được boot theo hệ điều hành nào khi máy được cài cùng lúc nhiều hệ điều hành. Sau đó thì ta đã có thể đăng nhập và vào giao diện chính của Debian.
	
### 3. Cài đặt Ubuntu Server 2016
 - Sau khi ta đã tải về IOS và tạo USB bootable device, ta khởi động lại máy rồi vào phần boot của máy. Tại đây có bước cấu hình mạng và tài khoản của server. Ta đặt địa chỉ, net mask, host name, tạo user. Sau đó ta vào bước phân vùng ổ cứng tương tự như cài đặt Debian như đã nêu ở trên. Sau đó ta có thể cài đặt thêm một số service cho server Ubuntu này. Tiếp theo ta cũng cần cài đặt GRUB boot loader để có thể boot nhiều hệ điều hành. Khi hoàn thành tất cả, một màn hình dòng lệnh hiện ra để ta nhập tài khoản và mật khẩu, như vậy là ta đã cài đặt xong Ubuntu Server 2016. Để có thể sử dụng GUI, ta gõ câu lệnh sudo apt install ubuntu-gnome-desktop.
	
### 4. Cài đặt CentOS 7.6 Minimal
 - Sau khi ta đã tải về IOS và tạo USB bootable device, ta khởi động lại máy rồi vào phần boot của máy. Sau khi chọn ngôn ngữ, hiện ra một giao diện để cài đặt. Ở đây có một số cài đặt quan trọng là Installation Destination dùng để chỉ ổ cứng để cài hệ điều hành, ta có thể chọn automation hoặc tạo mới ổ cứng để chưa CentOS. Tới phần Software Selection, ta chọn Minimal Install để cài đặt bản Minimal cho CentOS, có thể thêm một số phần mầm kèm theo ở bên góc phải. Sau khi chọn Begin Installation, ta nhập mật khẩu cho quyền root, và bắt đầu cho việc cài đặt. Việc cài đặt sẽ hoàn tất sau khoảng 10-15p.
	
	
## 2. Kiến trúc trong Linux
### 1. Kiến trúc Linux
 - Kiến trúc tổng quan của Linux gồm có 2 thành phần chính, User space và Kernel space.
  - User space là phần không gian của người dùng, chứa các ứng dụng, các chương trình người dùng thực thi, và cũng chứa cả thư viện GNU C.
  - Kernel space là phần không gian dành cho hệ điều hành, có chứa nhân kernel, giao diện lời gọi hệ thống (SCI) và mã nguồn phụ thuộc vào phần cứng.
	
 - Khi người dùng hoặc ứng dụng gửi yêu cầu, yêu cầu đó sẽ được gửi xuống SCI hoặc thông qua GNU C xuống SCI.
	Tại SCI sẽ xử lí lời gọi từ tầng bên trên để điều hướng yêu cầu đến phần cụ thể xuống nhân Kernel.
	Nhân Kernel bao gồm nhiều hệ thống con khác nhau, đều xử lý yêu cầu mà không phụ thuộc vào phần cứng, rồi sau đó chuyển xuống tầng dưới. Các hệ thống con của Kernel là:
  - Quản lý tiến trình: Tạo, dừng, xóa một tiến trình, tạo bộ lập lịch, phân chia tài nguyên cho các tíến trình.
  - Quản lý bộ nhớ: Quản lý bộ nhớ đang sử dụng, cung cấp cơ chế ánh xạ giữa bộ nhớ vật lý và bộ nhớ ảo.
  - Hệ thống file ảo: Giúp cho các APi có thể sử dụng các cấu trúc file khác nhau  trên hê thống mà sử dụng cùng một các phương thức giống nhau.
  - Hệ thống network stack: Kết nối từ lớp socket của ứng dựng, xử lí chúng ở lớp TCPIP, rồi sau đó gửi đến phần cứng mạng cần thiết.
  - Device Drivers: Mã nguồn để giao tiếp với từng loại phần cứng chung.

  - Lớp dưới cùng là lớp mã nguồn phụ thuộc vào phần cứng, quản lý từng loại phần cứng khác nhau.
	
###2.Kernel, Shell là gì
 - Kernel là thành phần trung tâm của một hệ máy tính. Nó thường là tầng mức trừu tượng nhất của máy tính, cung cấp giao tiếp giữa phần vật lý của thiết bị(Ram, CPU, Disk, Network card,...) đến lớp bên trên.
	Một số loại Kernel chính:
  - Kernel nguyên khối: OS và Kernel cùng chạy trong 1 không gian bộ nhớ.
  - Kernel vi mô : Kernel thực hiện hầu hết các công việc mà không cần thêm GUI.
  - Kernel lai: Pha giữa Kernel khối và Kernel vi mô, nó di chuyển trình điều khiển nhưng giữ các dịch vụ hệ thống bên trong Kernel.
	
 - Shell là một ứng dụng giúp người dùng thao tác giữa người dùng với nhân Linux thông qua các dòng lệnh, script. Shell có rất nhiều phiên bản, trong đó có hai kiểu Shell chính là Bourne Shell (dòng nhắc lệnh  mặc định là $) và C Shell (dòng nhắc lệnh mặc định là %). Hiện nay Shell được đánh giá mạnh nhất là zsh, bên cạnh hững cái tên phổ biến khác như fish, bash, sh, ksh ,..
	
### 3.Bash Shell là gì
 - Là một chương trình họ Shell giúp tương tác với nhân máy tính hệ *Nix. Đây là một Shell có tuổi thọ lâu đời, và được sử dụng tích hợp trên nhiều hệ máy.
		
### 4.Command là gì. Các tương tác với hệ thống
 - Command line (dòng lệnh) là một giao diện giúp giao tiếp với máy tính thông qua các dòng lệnh. Command line có thể gửi yêu cầu, gửi input tới và nhận output từ máy tính. Cần phân biệt giữa Command line với Shell, Shell là một ứng dụng giúp người dùng tương tác với OS, còn Command line dùng để tương tác giữa người dùng với máy tính, người dùng nhập input và yêu cầu máy tính thực hiện và máy tính trả về out put; người dùng dùng command line để chạy shell.

	
	
