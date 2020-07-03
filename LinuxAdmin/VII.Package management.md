1. Thuật ngữ mở đầu  
2. DEB package management  
3. RPM package management  

## 1. Thuật ngữ mở đầu
### 1.1 Repository
 - Repository hay được viết tắt là repo dịch theo nghĩa tiếng Việt là kho, tức là nơi lưu trữ của một dự án, một chương trình, một phần mềm nào đó, được đóng gói thành các packet.
 - Đia chỉ repo được lưu ở /etc/apt/source.list hoặc /etc/yum.repos.d/
 - Có hai định dạng packet phổ biến là .rpm và .deb.
 - Một gói phần mềm là:
  - Một phần mềm dịch ra mã thực thi (cũng có gói chứa mã nguồn)
  - meta-data
  - Được đóng gói theo định dạng và phân phối như .rpm và .deb
  - Có thể tìm kiếm, cài đặt, gỡ bỏ, cập nhật bằng hệ thống quản lý
  - Nhằm cung cấp một ứng dụng hoặc một dịch vụ
### 1.2 .rpm package
 - Red Hat Package Manager (RPM) là công cụ dùng để quản lý gói mặc định và mã nguồn mở mặc định cho các hệ thống dựa trên Red Hat (RHEL, CentOS và Fedora), hoạt động trên định dạng .rpm.
 - Thông tin của các gói đã cài đặt được lưu trong /var/lib/rpm
### 1.3 .deb package
 - DEB hay .deb là phần mở rộng của định dạng đóng gói phần mềm cho các bản phân phối dựa trên Debian, hay Ubuntu và những bản phân phối khác.
 - dpkg cung cấp các tính năng cơ bản cho cài đặt và thao tác với các file .deb.Thông thường, người dùng cuối không trực tiếp quản lý các gói tin với dpkg thay vào đó là trình quản lý gói tin APT hoặc các APT front-ends như synaptic hay KPackage.
 - Một package .deb gồm ba tập tin
  - debian-binary: số phiên bản của đin dạng deb.
  - control.tar.gz: tất cả các gói metadata. Nó thông báo với dpkg cấu hình khi các gói phần mềm đang được cài đặt.
  - data.tar, data.tar.gz, data.tar.bz2, data.tar.lzma or data.tar.xz: các file cài đặt thực tế.

*Sử dụng $alien để có thể chuyển đổi package từ .deb sang .rpm*
### 1.4 dependency
 - Dependency hiểu theo nghĩa là sự phụ thuộc. Các gói phần mềm đôi khi muốn được cài đặt và sử dụng trên máy cần phải cài đặt trước một số phần mềm trước đó nữa.
### 1.5 opensource
 - Mã nguồn mở nghĩa là mã nguồn được chia sẻ công khai với cộng đồng, được tự do sử dụng vào đóng góp vào mã nguồn mở.
 
## 2. DEB package management
### 2.1 command:dpkg
 - Debian package hay dpkg là phần mềm làm nền tảng cho các hệ thống quản lý gói tin .deb trên hệ điều hành.
 - dpkg là một công cụ cấp thấp, nó không xử lí các gói tin có quan hệ phức tạp, chẳng hạn như nhưng gói tin phụ thuộc, khiến người dùng phải thao tắc trực tiếp trước đó
 - Thông tin về các package ở /var/lib/dpkg
 - $dpkg [OPTIONS]  < command >
  - -i	insstall
  - --unpack
  - -r remove
  - -s| --status
  - --update-avail	Thay thế thông tin hiện thời của package
  - -S| --search
  
### 2.2 apt-get là gì? Tìm hiểu apt-get
 - Advanced Packageing Tool hay APT, là phần mềm miễn phí dùng để quản lý việc cài đặt phần mềm trên Linux. APT là một phiên bản cao cấp hơn của dpkg, có thể xử lí các gói tin phức tạp, như những gói tin phụ thuộc.
 - APT được sử dụng quản lý gói .deb với câu lệnh apt-get, nhưng cũng có thể làm việc với hệ thống rpm thông qua apt-rpm
 - $apt-get [OPTION] [PACKAGE]
  - install/reinstall
  - update/upgrade
  - remove
  - purge	remove file và config file
  - autoremove	remove nhưng package không sử dụng
  - clean/autoclean	xóa các file đã tải về 
  - check	Kiểm tra không có lỗi phụ thuộc phần mềm nào
*Sử dụng $apt thay vì $apt-get; $apt là phiên bản cải tiến hơn của $apt-get, có cấu trúc tương đương tương thích ngược với apt và sửa một số lỗi phụ thuộc của apt-get*  
### 2.3 Tìm hiểu apt source list
 - /etc/apt/sources.list và /etc/apt/sources.list.d/ lưu trữ các nguồn package.
 - Được biểu diễn theo một số khuôn dạng khác nhau

|Type| Phân loại format file| deb chứa package nhị phân, deb-src chứa package mã nguồn|
|---|---|---|
|URI|link repo| link http, https, ftp url, hoặc đường dẫn file database|
|operating system| Tên phiên bản hệ điều hành| focal-update, bionic...|
|section names/component| Tên group repo*|

 - Tên group repo gồm có 5 giá trị, có thể có nhiều giá trị cùng lúc
  - main	Mặc định khi cài đặt. Repo này free và mở(FOSS), được hỗ trợ đầy đủ bởi Linux developers, và được cung cấp các bản cập nhật an ninh cho đến khi hết vong đời cập nhật của nó
  - universe	Repo này free và mở, nhưng không được cung cấp các cập nhật an toàn thường xuyên. Repo này được đóng góp và quản lý bởi cộng đồng
  - multiverse	Repo này bao gôm cả những phần mềm có bản quyền
  - restricted	Repo này free và mở nhưng chỉ được mở khi cần hỗ trợ phần cứng.
  - partner	Bao gồm các gói độc quyền cho các đối tác cung cấp khác.
  - Các repo cung cấp bởi bên thứ ba	Cung cấp những packet tùy chọn do bên thứ ba quản lý.

### 2.4 Luồng làm việc của apt-get
$apt-get thực hiện chủ yếu những bước sau:  
 - Thực hiện truy vấn trên các repo trên sourse.list kiểm tra danh sách các package
 - Phân giải phụ thuộc: Xem package nào được cài đặt một cách không đúng đắn chưa
 - Tính toán số package cần được tải: Tính toán package nào đã được tải trên hệ thống, package nào đúng version.
 - Tải package files: Tải và lưu vào cache. Pha này có thể tạm ngưng/tiếp tục được.
 - Cài đặt package fils: apt-get gọi tới dpkg để tiến hành cài đặt mỗi file trong cache
 - dpgk giải nén package và copy tới vị trí phù hợp, kiểm tra xem file đã tồn tại trước chưa và có bị thay đổi không.
 - chạy các package maintainer scripts: preins, postint (prerm, postrm nếu package được upgrade). Các script này lưu ở /var/lib/info/< package-name >.
 - Thực hiện một số hành động triggers (xem dpkg trigger). 
### 2.5 So sánh dpkg với apt-get
 - dpkg là một công cụ giúp cài đặt, build xóa hoặc quản lý các .deb package. Khi thực hiện yêu cầu cài đặt một package nào đó, thì dpkg không check dependency (các package phụ thuộc khác) mà chỉ thông báo thôi.
 - apt-get là một hệ thống quản lý package quản lý các .deb package. Khi thực hiện cài đặt các package, apt-get check cả dependency, sau đó yêu cầu xuống dpkg tải và cài đặt các package dependency đó.
## 3. RPM package management
### 3.1 Command : rpm
 - rpm giúp cài đặt và quản lý các package .rpm trên các hệ thống Red Hat như RHEL, CentOS, tương tự như dpgk với .deb
 - $rpm [OPTION] [PACKAGE]
  - -i	Cài đặt
  - -v	Hiện thông tin cụ thể
  - -h	In dấu băm khi package được giải nén
  - -qpR	Check dependency
  - -ql	Check các file thuộc package đó
  - -qa	Check tất cả package đã được cài
  - -Uvh	Upgrade một package
  - -e	remove
  - -qi	info package
  - -qip	info package trước khi được cài đặt
### 3.2 Yum là gì? Làm việc với yum
 - Yum là công cụ quản lý và cài đặt phần mềm cho các hệ thống Red Hat, tương tự với apt-get với .deb
 - $yum [OPTION] [COMMAND]
  - check	Kiểm tra lỗi
  - checkupdate
  - clean	Xóa cache data
  - erase	Xóa package khỏi hệ thống
  - history
  - info
  - install/reinstall
  - list
  - update/upgrade
  - version
  - -v	Hiện nhiều thông tin hơn
### 3.3 Tìm hiểu yum repository
 - Cấu hình yum chính được lưu ở /etc/yum.conf, repo được lưu ở đây hoặc lưu riêng biệt ở /etc/yum.repos.d/
 
|name|Tên repo|
|---|---|
|baseurl| Link repo|
|gpgcheck| =1: có check gpgkey; 0: không check|
|enabled| 0-1 : Không/Có kiểm tra repo này|
|gpgkey|Địa chỉ lưu trữ gpgkey|
*Mỗi rpm packet cần phải có gpgkey để xác thực và bảo toàn dữ liệu*
### 3.4 Luồng làm việc của yum
 - Check /etc/yum.conf để tìm các cấu hình cơ bản nhất của yum.
 - Check /etc/sysconfig/rhn/up2date (RHEl only) Tìm repo của Red Hat Network hoặc RHN Satellite server)
 - Check /etc/yum.repos.d/*.repo file để tìm kiếm các repo
 - Tải RPM package và metadata từ repo. Sau đó yum tải về và check dependency package nếu cần và được lưu trữ trong cache
 - Yum chạy rpm command để cài đặt từng package một. Có thể chạy cả pre-script và post-script
 - Lưu yum repo metadata tới local rpm database (/var/lib/rpm/)
### 3.5 So sánh yum với rpm
 - rpm là công cụ quản lý package trong khi yum là một frontend sử dụng rpm
 - rpm không thể check dependency trong khi yum có thể, do vậy thân thiện với người dùng hơn.
