1. Kiểm tra device có trên hệ thống  
2. Debian configuration  
3. RHEL configuration  
## 1. Kiểm tra device có trên hệ thống  
### 1.1.Command ip
Đây là lệnh kiểm tra và cấu hình ip trên hệ thống
 - $ip [OPTION] OBJECT { COMMAND | help }	Cấu trúc chính
*Mặc định thì hệ thống không lưu lại các thay đổi khi restart hoặc reboot. Muốn thay đổi vĩnh viễn thì có hai cách:thêm vào startup script hoặc file cấu hình*
#### 1.1.1. Các [OPTION]
  - -h	hiện outpout dễ đọc 
  - -b [FILENAME]	lấy lệnh từ [FILENAME] hoặc từ terminal và gọi chúng.
  - -s	Xuất ra nhiều thông tin hơn
  - -d Xuất ra nhiều thông tin chi tiết hơn
*-s là các thông số thống kê, -d là các thông số cấu hình*
  - -r	Xuất ra tên phân giải địa chỉ thay vì IP (nếu có)
  - -n	[NETNS]	Áp dụng cho net name space
    - net name space: Tập hợp các quy tắc cho một cấu hình mạng (ip, firewall, route ...
    - Sử dụng $ip netns để thay thế ($ip netns help để xem)
  - -t	Sử dụng với object monitor, thêm một thông số thời gian xuất hiện, hoặc -ts để rút ngắn thời gian hiển thị
  - -j	In ra theo định dạng JSON
  - -4 Chỉ in ra ipv4
  - -6 chỉ in ra ipv6
  - -B	Ip bridge
  - -M	mpls
  - -0	link
  - -o	Hiện thông tin trên 1 dòng, ngăn cách nhau bởi \
  
#### 1.1.2. Các [OBJECT]
| OBJECT      | Tên viết tắt | Ý nghĩa |
| --- | --- | --- |
| link | l | Thiết bị mạng |
| address | a	addr | Địa chỉ mạng |
| addresslabel | addrl | Nhãn cấu hhình cho địa chỉ giao thức |
| neighbor | n	hoặc neigh | sử dụng giao thức arp |
| route | r |	Bảng routing |
| rule | ru | Chính sách routing |
| maddress | m	hoặc maddr | Địa chỉ multicast |
| mroute | mr | multicast routing |
| tunnel | t | tunnel over IP |
| xfrm | x | Framework qua giao thức IPsec |
Các [OBJECT] khác:
  - l2tp	tunnel over IP (L2TPv3)
  - monitor	Giám sát thay đổi mạng (link, route, addrs, neigh...)
  - mrule	Chính sách routing multicast
  - netns	Quản lý network name space
  - tcpmetric	Quản lý TCPmetrics
  - token	Quản lý token
*Để có thể xem tùy chọn của từng [OBJECT] sử dụng $ ip [OBJECT] help*
### 1.2./proc/net/dev
Đây là file ghi các thông số input/output của các thiết bị mạng.  
Có các thông số như Tổng số bytes, packet, error, drop, fifo error, frame error, collisions, multicast, carrier.  
Phần lớn thông số ở file này có thể kiểm tra với lệnh $ip -s link 
## 2. Debian configuration
### 2.1.Command: man interfaces
man trên Debian cũng được sử dụng với chức năng và tùy chọn tương tự với các hệ điều hành Linux khác.  
### 2.2./etc/network/interfaces
File này dùng để 	 các giao diện mạng.  
Các cú pháp trong /etc/network/interfaces:  
  - auto [INTERFACE_NAME]	Khởi động interface đó khi khởi động hệ thống
  - allow-auto [INTERFACE_NAME]	TƯơng tự trên
  - allow-hotplug [INTERFACE_NAME]	Khởi động một interface khi nhân hệ điều hành phát hiện một sự kiện < hotplug > từ interface
  - iface [CONFIG_NAME] ở đầu dòng	Định nghĩa tên một cấu hình mạng
  - mapping [INTERFACE_NAME_GLOB] ở đầu dòng	Định nghĩa tham chiếu (mapping) giá trị của [CONFIG_NAME] được sử dụng trên [INTERFACE_NAME]
  - # ở đầu dòng	Như một comment
  - \ ở cuối dòng	Nối dòng dưới và dòng hiện tại với nhau
 - Cú pháp phổ biến cho một interface như sau:
iface < config_name > < address_family > < method_name >  
 < option1 > < value1 >  
 < option2 > < value2 >  
 ...  
*Nếu hệ điều hành sử dung NetworkManager, các file cấu hình interface được đặt ở /etc/NetworkManager/system-connections/*
*Muốn sử dụng file /etc/network/interfaces. đặt managed=true trong /etc/NetworkManager.conf*
### 2.3.Cấu hình IP động, IP tĩnh. Tự động on card khi máy khởi động
 - Cấu hình static tại /etc/network/interfaces mẫu:
iface eth0 inet static  
 address 192.168.11.100  
 netmask 255.255.255.0  
 gateway 192.168.11.1  
 dns-domain example.com  
 dns-nameservers 192.168.11.1  
  - < method_name > có thể là: ppp, dhcp, static, manual...
  - inet biểu thị đây là ipv4
 - Với cấu hình dynamic ip
 iface eth0 inet dhcp  
 - Tự động on card khi máy khởi động: Sử dụng cả hai hoặc 1 trong 2 auto và hot-plug
  - auto	Khởi động cùng với boot
  - allow-hotplug	Đợi kernel, driver, udev khởi động trước.
Ví dụ:  
auto < Interface >   
allow-hotplug < Interface >  
iface < Interface > inet dhcp  
### 2.4.nmcli, nmtui, ipconfig, dhclient, route, ip route, ping, ifup, ifdown  
#### 2.4.1 nmcli 
Đây là một công cụ dòng lệnh để quản lý NetworkManager và các trạng thái của nó.
 - nmcli [OPTIONS] {help | general | networking | radio | connection | decive | agent | monitor } [COMMAND] [ARGUMENTS]
##### [OPTIONS]
 - -a | --ask	DÙng để nhắc câu lệnh khi thiếu tham số
 - -f | --field {field1,field2... | all | common}	Dùng để in ra các cột in ra theo tùy chọn
 - -m | --mode {tabular | multiline}	Định dạng dữ liệu theo nhiều dòng hay theo cột
 - -s | --show-secrets	Khi cần cung cấp mật khẩu trên terminal, tùy chọn này sẽ hiện đối với người dùng
 - -t | --terse	Hiện output ngăn cách nhau bởi dấu :
 - -w | --wait [SECONDS]	Đợi một khoảng thời gian để câu lệnh được hoàn thành
##### nmcli help 
Dùng để hỏi chung
##### nmcli general {status | hostname | permission | logging } [ARGUMENTS..]
Dùng để hiện thông tin chung
##### nmcli networking {on | off | connectivity} [ARGUMENTS...]
Dùng để tắt bật hoặc hiển thị mức độ kết nối hiện thời
##### nmcli radio {all | wwifi | wwan } [ARGUMENTS] {on | off}
DÙng để tắt|bật hoặc kiểm tra trạng thái của chỉ kết nối wifi
##### nmcli connection {show | up | down | modify | add | edit | clone | delete | monitor | reload | load | import | export } [ARGUMENTS] 
Dùng để kiểm tra quá quản lý các kết nối với máy tính hiện thời.
##### nmcli device {status | show | set | connect | reapply | modify | disconnect | delete | monitor | wifi | lldp} [ARGUMENTS...]
DÙng để quản lý và kiểm tra các interface
### 2.4.2. nmtui - Text User Interface
Dùng để quản lý và hiện thông tin của NetworkManager với giao diện điều khiển.  
  - nmtui-edit	Quản lý, chỉnh sửa các mạng hiện thời
  - nmtui-connect	Quản lý, chỉnh sửa các kết nối
  - nmtui-hostname	Sửa host-name
### 2.4.3 route
*Đã bị lỗi thời và được thay thế bởi ip route*
### 2.4.4 dhclient
Dùng để cấu hình 1 hay nhiều giao diện mạng sử dụng DHCP, hoặc BOOTP cho client.  
 - Khi khởi động, dhclient đọc dữ liệu từ /etc/dhcp/dhclient.conf cho việc khởi tạo.  
 - Các ip được cấp phát được lưu trong /etc/dhcp/dhclient.leases.
 - Khi một ip được cấp phát, nó sẽ chạy /sbin/dhclient-script
 - $dhclent [OPTIONS]	Tìm và kết nối dhcp server.
  - -4	Sử dụng DHCPv4
  - -6	Sử dụng DHCPv6, chỉ được chọn -4 hoặc -6.
  - -d	Điều khiển dhclient thanhf tiến trình chạy ngầm.
  - -r	Xóa các IP leases.
  - -x	Dừng DHCP mà không xóa IP leases.
  - -s [SERVER_ADDR]	Đặt ip hoặc tên domain cho server.
  - -p [PORT_NUMBER]	Đặt UDP port cho DHCP (mặc định là 68)
  - -sf [SCRIPT_FILE]	Đường dẫn tới file script
  - -e	[VAR]=[VALUE]	Gán thêm giá trị cho biến môi trường khi dhclient-script được thực thi
### 2.4.5. ip route
*Đã được trình bày ở phần $ip*
### 2.4.6. ping
Dùng để ping/kiểm tra kết nối tới một địa chỉ mạng.
 - $ping [IP_ADDR]	Sử dụng chủ yếu
### 2.4.7. ifup và ifdown
Được viết tắt của interface up/down, đây là lệnh dùng để tắt/ bật interface nhanh chóng.
 - ifup [INTERFACE_NAME]
 - ifdown [INTERFACE_NAME]
 - ifquery [INTERFACE_NAME]	Hiển thị cấu hình của interface
## 3. RHEL configuration
### 3.1.Command: man interfaces
man trên RHEL cũng được sử dụng với chức năng và tùy chọn tương tự với các hệ điều hành Linux khác.  
### 3.2./etc/sysconfig/network-scripts/
Đây là thư mục lưu trữ các script cấu hình riêng cho các interface : ifcfg-[INTERFACE_NAME]
 - Các thuộc tính trong file cấu hình:
  - DEVICE=[INTERFACE_NAME]	Tên interface
  - BOOTPROTO={none,dhcp,bootp}	Giao thức sử dụng khi boot
  - DNS{1,2}=[ADDRESS]	Đặt địa chỉ DNS server được đặt trong /etc/resolv.conf
  - IPADDR= [ADDRESS]	Địa chỉ interface đó
  - NETMASK= [MASK]	Netmask
  - ONBOOT={yes,no}	Có được kích hoạt khi boot không
  - PEERDNS= {yes,no}	Sửa file /etc/resolv.conf nếu DNS được đặt. Nếu sử dụng DHCP, mặc định là yes
  - SRCADDR [ADDRESS]	Đặt cụ thể địa chỉ IP được gán địa chỉ nguồn nào.
  - USERCTL= {yes,no}	Cho phép non-root user điều khiển interface này hay là không
  ....  
*Nếu interface sử dụng kết nối PPP, sẽ có một số thông số khác thêm vào*
 - Clone một cấu hình từ interface này sang interface khác : ifcfg-[INTERFACE_NAME]-[CLONE_NAME]
### 3.3.Cấu hình IP động, IP tĩnh. Tự động oncard khi máy khởi động
Đối với RHEL/Centos, file cấu hình được đặt trong thư mục /etc/sysconfig/network-scripts với từng script cho interface riêng.
Một số cách để cấu hình IP cho interface:
#### 1.Sửa trực tiếp tại /etc/sysconfig/network-scripts/ifcfg-[INTERFACE_NAME]
  - Cấu hình IP static: Đổi giá trị IPADDR = [STATIC_ADDRESS], BOOTPROTO= none, NETMASK=[NETMASK]. GATEWAY=[GATEWAY]
  - Cấu hình IP dynamic: Đổi BOOTPROTO={dhcp,bootp}
  - Tự động oncard khi máy khởi động: ONBOOT=yes
#### 2.Sửa với câu lệnh $nmcli
  - sudo nmcli connection modify [INTERFACE_NAME] [+|-] [setting].[property] [value]
    - [+|-] được sử dụng để thêm/bớt thuộc tính]
    - [INTERFACE_NAME] để chỉ tên interface
    - [setting], [property] là các thuộc tính riêng.
    - [value] là giá trị được gán cho các thuộc tính đó
*Xem đầy đủ các [setting], [property] tại $man nm-settings*
VD:
  - Thêm 1 địa chỉ IPv4 tĩnh vào interface enp1s0:
   - $ sudo nmcli connection modify enp1s0 IPv4.address 192.168.122.66/24 IPv4.gateway 192.168.122.1 IPv4.dns 192.168.122.1 ipv4.method manual
  - Thêm 1 địa chỉ IPv4 động:
   - $nmcli device modify eth0 ipv4.method auto (dùng để bật dhcp)
  - Tự động on-card khi máy khởi động: 
   - $nmcli device set [INTERFACE_NAME] autoconnect yes
#### 3.Sửa với $nmtui
Thao tác với các phím mũi tên và bàn phím để cấu hình một cách trực quan hơn.
  - Thêm 1 địa chỉ tĩnh: Chọn tên mạng -> < Add > hoặc < Edit > . Phần IPV4 CONFIGURATION, chuyển < Automatic > thành < manual >, chọn < Show > rồi thêm/ sửa IP address, DNS, default gateway.
  - Thêm 1 địa chỉ động:  Chọn tên mạng -> < Add > hoặc < Edit > . Phần IPV4 CONFIGURATION, chuyển < manual > thành < Automatic > ,
  - Tự động on-card khi máy khởi động: Chọn tên mạng -> < Add > hoặc < Edit > . Đánh dấu x vào phần < Automatically connect > bằng ấn phím Space.
### 3.4. nmcli, nmtui, ipconfig, dhclient, route, ip route, ping, ifup, ifdown
#### nmcli
Đây là một công cụ dòng lệnh để quản lý NetworkManager và các trạng thái của nó.
 - $nmcli [ OPTIONS ] OBJECT { COMMAND | help }
  - OBJECT := { general | networking | radio | connection | device }
##### OPTION
   - -t	Hiện output ngăn cách nhau bởi dấu :
   - -f [FIELD_NAMES]| all | common	Hiện output những dòng cụ thể
   - -w [SECOND]	Đợi một khoảng thời gian
##### nmcli general {status | hostname | permission | logging } 
Dùng để hiện thông tin chung
##### nmcli networking {on | off | connectivity} 
Dùng để tắt bật hoặc hiển thị mức độ kết nối hiện thời
##### nmcli radio {all | wifi | wwan | wiwax } {on | off}
DÙng để tắt|bật hoặc kiểm tra trạng thái của chỉ kết nối wifi
##### nmcli connection {show | up | down | modify | add | edit | clone | delete | reload | load }
Dùng để kiểm tra quá quản lý các kết nối với máy tính hiện thời.
##### nmcli device {status | show | connect | wifi | wiwax | disconnect }
Dùng để kiểm tra và quản lý các thiết bị
*Nhận xét chung:nmcli trên RHEL có một số câu lệnh được lược bỏ như monitor, import, export...*
#### nmtui
Dùng để quản lý và chỉnh sửa cấu hình mạng với giao diện màn hình.  
*Có phần giao diện và chức năng cũng tương tự trên Debian/Ubuntu, nhưng có một số tùy chọn lược bỏ như phần IPv4/ IPv6 cấu hình lược bỏ "Ignore automatically obtained route" và "Ignore automatically obtained DNS parameters".*
#### dhclient
Dùng để cấu hình 1 hay nhiều giao diện mạng sử dụng DHCP, hoặc BOOTP cho client.  
*RHEL sử dụng file /etc/dhcp/dhclient-[DEVICE]-conf để lưu cấu hình dhclient cho từng thiết bị.*
*Nếu có sử dụng NetworkManager thì các file sẽ được đặt ở /var/lib/NetworkManager/dhclient-*.conf*
 - $dhclent [OPTIONS]	Tìm và kết nối dhcp server.
#### ifup và ifdown
Được viết tắt của interface up/down, đây là lệnh dùng để tắt/ bật interface nhanh chóng.
 - ifup [INTERFACE_NAME]
 - ifdown [INTERFACE_NAME]
#### 2.4.6. ping
Dùng để ping/kiểm tra kết nối tới một địa chỉ mạng.
 - $ping [IP_ADDR]	Sử dụng chủ yếu

