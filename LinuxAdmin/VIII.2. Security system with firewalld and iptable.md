


1. Một số thuật ngữ mở đầu  
2. Mở đầu firewalld  
3. Tìm hiểu iptables  
4. Tìm hiểu firewalld  


## 1. Một số thuật ngữ mở đầu  
### 1.1 packet forwarding
 - Chuyển tiếp gói tin (packet forwarding) là một hành động của một thiết bị trung gian (layer 2,3) giúp trao đổi gói tin giữa bên gửi và bên nhận.
 - Packet forwarding chỉ làm nhiệm vụ giúp gói tin đi đúng địa chỉ của nó, không làm thay đổi nội dung gói tin.
### 1.2 packet filtering
 - Lọc gói tin (packet filtering) là một hành động của một thiết bị (layer 2,3) giúp giám sát và kiểm soát các gói tin.
 - Bộ lọc (filter) sẽ gồm một tập hợp các quy tắc. Khi một gói tin được gửi đến, nó sẽ được tham chiếu tới bộ quy tắc đó mà thực hiện hành động chấp nhận hoặc loại bỏ gói tin.
### 1.3 nat
 - Netword address translation (nat) là một quá trình thay đổi thông tin địa chỉ IP trong gói tin qua một thiết bị định tuyến.
 - Nat được sử dụng phổ biến, giúp tiết kiệm tài nguyên địa chỉ ip4.
### 1.4 pat 
 - Port address translation (pat) là một chức năng thường gắn liền với nat. Nó giúp chuyển đổi port này sang port khác.
 - Khi sử dụng nat với pat, nhiều máy tính có thể cùng sử dụng một địa chỉ global IP duy nhất để truy cập mạng, nhờ phân biệt với nhau bởi số của port.
### 1.5 snat
 - Source nat (snat) là quá trình thiết bị sẽ thay đổi ip và port nguồn. Snat thường được dùng để các thiết bị trong mạng nội bộ muốn được kết nối internet sẽ được nat thành global IP.
### 1.6 masquerading
 - Masquerading (MASQ) là một trường hợp khác của snat. khi globel ip dùng để kết nối mạng là một dãy các địa chỉ mạng (ví dụ như sử dụng DHCP), thì ta sẽ sử dụng MASQ ( nhiều snat ). 
### 1.7 dnat (destination nat)
 - Destination nat (dnat) là quá trình thiết bị sẽ thay đổi ip và port đich. Dnat thường được dùng để các thiết bị bên môi trường mạng có thể truy cập vào địa chỉ máy có dịch vụ ở trong mạng nội bộ.
### 1.8 Enable packet forwarding with module: /proc/sys/net/ipv4/ip_forward
 - Enable packet forwarding trên Linux, ta sẽ chuyển trên /proc/sys/net/ipv4/ip_forward từ 0 -> 1 , mặc định là 0 (disable).
 - Để bật packet forwarding lúc khởi động hệ thống, thay đổi net. ipv4.ip_forward trong /etc/sysctl.còn thành 1.
## 2. Mở đầu firewalld  
### 2.1 Khái niệm netfilter, làm thế nào để thao tác với nó
 - Netfilter là một frameword được cung cấp bởi Linux kernel cho phép các nghiệp vụ mạng khác nhau như packet filtering, nat, pat qua các module của nó.
 ![alt ](https://en.wikipedia.org/wiki/Netfilter#/media/File:Netfilter-components.svg)  
 - $sudo ufw status	Kiểm tra trạng thái
 - sudo ufw enable	Bật ufw (unconplicated Firewall) 
### 2.2 Tìm hiểu hook trong Netfilter
 - Netfilter có các module sẽ được khởi động khi gói tin đến một tiến trình nhất định.
 ![image](https://user-images.githubusercontent.com/43545058/86868865-b723ed80-c0ff-11ea-9739-d9343ba5f346.png)
 - [1] NF_IP_PER_ROUTING	Được gọi khi một gói tin chuyển tới máyROUN
 - [2] NF_IP_LOCAL_ON	Được gọi khi một gói tin được chuyển tới đich là chính máy này
 - [3] NF_IP_FORWARD	ĐƯợc gọi khi một gói tin được chuyển tới đích là máy khác
 - [4] NF_IP_POST_ROUTING	ĐƯợc gọi khi gói tin chuẩn bị đi ra khỏi máy
 - [5] NF_IP_LOCAL_OUT	ĐƯợc gọi khi một gói tin được tạo ra ở chính máy này
## 3. Tìm hiểu iptables  
### 3.1 Tổng quan về IPtable: lịch sử phát triển, mục đích, các chức năng chính
 - IPtable là một framework cho các packet filtering software nằm ở bên trong các bản Linux kernel từ bản 2.4 trở lên
 - Các software nằm bên trong framework này có thể lọc, NAT ,mangling các gói tin.
 - netfiler, ip_tables, connection tracking (ip_conntrack, nf_conntrack) và NAT subsystem là những nhân tố chính tạo nên framework này.
 - IP tables là program chạy trên userspace để quản lý các packet filtering ruleset, dành cho các sysadmin.
 - IP tables có thể dùng để NAT gói tin
 -> IP tables là một software dùng để tương tác vs netfilter
 ![image](https://user-images.githubusercontent.com/43545058/86871329-3e736000-c104-11ea-903e-a8d7c04c0db3.png)

 - iptables được tạo ra vào 1998, dùng để thay thế ipchains có nhiều thiếu sót, và kế nhiệm của nó là nftables được ra mắt vào năm 2014 hoàn thiện hơn iptables.
 - iptable có khả năng giám sát gói tin, giám sát kết nối qua các rule được định nghĩa.
 - Các chức năng chính của iptables:
  - stateless packet filtering (ipv4,6)
  - stateful packet filtering (ipv4,6)
  - NAT, PAT (ipv4,6)
  - Hạ tầng mềm dẻo và dễ mở rộng
  - Có nhiều lớp API cho các ứng dụng của bên thứ 3.
  
### 3.2 Tổng quan về IPtables command
 - $iptables --tables [TABLE] [OPTION] [CHAIN] [ [OPTION] RULE] --jump [TARGET]
  - [TABLE] tên một tập hợp các [CHAIN]
  - [CHAIN] là một tập hợp các [RULE]
  - [RULE] là một rule cụ thể, trong đó có các tùy chọn riêng
  - [TARGET] là hành động thực hiện khi thỏa mãn rule
[TABLE] có ba bangr chính
  - filter	Dùng để lọc gói tin
  - nat	DÙng để nat
  - mangle	DÙng cho các mục đích khác
*Nếu không có mặc định là filter*
[OPTION]  
  - -A	append	chèn vào cuối (để kiểm tra rule đó cuối cùng)
  - -I	insert	chèn trên đỉnh (để kiểm tra rule đó đầu tiên)
  - -R	replace
  - -C	check if exist
  - -D	delete [num]
  - -P	policy chung (đây là rule mặc định, thực hiện cuối cùng)
  - -L/--list	Hiện danh sách
  - -F/--flush	Xóa hết các rule ở chains đó
  - -p/--protocol	Chỉ định giao thức cụ thể (tcp,udp....)
  - -s/--source [IP/mask]	Chỉ định IP nguồn
  - -d/--destination [IP/mask]	Chỉ định IP đích
  - -i/--in-interface/imput	[name][+]	Chỉ định interface input
  - -o/--out-interface/output	[name][+]	Chỉ định interface output
  - -v/--verbose	Hiện nhiều thông tin hơn
  - -n	Hiện địa chỉ ip và port theo số
  - -N/-X	Tạo mới/xóa chain tự định nghĩa
  - -m	Thêm nhiều bộ lọc hơn * chi tiết tìm hiểu sau *
  - .....
[CHAIN] là các hook sẽ thực hiện các rule  
 - table filter: INPUT, OUTPUT, FORWARD
 - table nat: POSTROUTING, PREROUTING, OUTPUT
 - table mangle : INPUT, OUTPUT, FORWARD, POSTROUTING, PREROUTING
 
[TARGET] sẽ thực khi khi thỏa mãn rule 
  - ACCEPT
  - DROP
  - REJECT
  - SNAT
  - MASQUERADE
![image Sự khác biệt giữa DROP và REJECT](https://user-images.githubusercontent.com/43545058/86877661-82209680-c111-11ea-8191-f899f04f8e9b.png)
  - QUEUE	Packet được gửi tới user-space
  
  
### 3.3 Flow của packet trên từng table (3 kiểu packet)
![image](https://user-images.githubusercontent.com/43545058/86876579-3bca3800-c10f-11ea-996a-1455140c3f3c.png)
![image](https://user-images.githubusercontent.com/43545058/86876612-500e3500-c10f-11ea-8ab3-28678c0852b4.png)
![image](https://user-images.githubusercontent.com/43545058/86876626-5a303380-c10f-11ea-8436-86b454bd6685.png)


### 3.4 Cơ chế hoạt động của các rule trong IPtables
![image Sơ đồ trình tự kiểm tra của các chain](https://user-images.githubusercontent.com/43545058/86888926-295af900-c125-11ea-906c-5d762b9ecd89.png)
 - Khi gói tin có đích là máy local thì nó sé đi theo nhánh INPUT, còn có đích không là máy local thì nó sẽ đi theo nhánh FORWARD, vì vậy nên sử dụng PREROUTING và POSTROUTING phù hợp
 - Trong một chain, các rule thực hiện theo thứ tự từ trên xuống dưới, khi thỏa mãn một rule bất kì có target là [ACCEPT|DROP...] thì sẽ dừng lại luôn, còn với target như QUEUE thì sẽ tiếp tục kiểm tra tiếp.

### 3.5 Tìm hiểu các command phổ biến trên IPtables
 - $iptables -t filter -L -n -v	Kiểm tra các rule và các chain của filter. 
 - $iptables  -F OUTPUT	Xóa hêt các rule trên chain [OUTPUT] của table filter (mặc định).
 - $sudo iptables [-t filter] [-A]  [INPUT] [-p tcp -i lo -d 192.168.0.0/16 --sport 80 --tcp-flags RST ACK] [-j ACCEPT]
  - [-t filter] thực hiện với table filter
  - [-A]	Mở rộng rule vào cuối bảng
  - [INPUT]	Thêm vào chain INPUT của table filter
  - [-p tcp -i lo -d 192.168.0.0/16 --sport 80 --tcp-flags RST ACK] một số rule như protocol, input interface, destination address, source port, tcp flags (cần 2 cờ)
  - [-j ACCEPT] target/ hành động thực hiện là ACCEPT

### 3.6 Cài đặt, tìm hiểu tổng quan về IPtables Service
 - iptable-service là một service, dùng để quản lý và giám sát các gói tin
 - Cài đặt trên CentOS: yum install iptables-services.
 - Sau đó enable và start iptables : systemctl start/enable iptables/ip6table
*iptables và firewalld không được sử dụng đồng thời, vì vậy muốn sử dụng iptables ta phải tắt firewalld đi (stop/disable).*
### 3.7 Mối quan hệ giữa Iptable service và iptable command
 - Cả hai đều sử dụng cú pháp và tác dụng giống nhau.
 - iptables thì luôn luôn hoạt động, nếu muốn tắt đi chỉ có cách là thêm/sửa/xóa rule, khó khăn trong việc quản lý.
 - iptables-services thì linh hoạt hơn, khi nào không muốn thì chỉ cần disable/stop và enable/start lại mà không cần sửa rule gì cả.
## 4. Tìm hiểu firewalld
### 4.1 Mối quan hệ giữa IPtables command và FirewallD Service
 - Cả hai đều cùng sử dụng iptables command để có thể giao tiếp với Linux kernel Netfilter.
 - Là hai service tách biệt nhau, với cách thức khác nhau cho cùng mục đích sử dụng.
### 4.2 Cài đặt FirewallD
 - Firewalld được cài đặt mặc định trên CentOS 7. Có thể cài đặt theo $yum install firewalld
### 4.3 Các cấu hình mặc định trên FirewallD
 - $firewall-cmd --get-default-zone	Kiểm tra zone mặc định của máy tính.
 - --get-zones	Kiểm tra tất cả các zone trong hệ thống
 - --set-default-zone=[ZONE]
### 4.4 TÌm hiểu các khái niệm : ZONE ( các rule mặc định trên zone này ) , ZONE INTERFACE, RULE, Service, Port, Rich rule, source
 - Trong FirewallD, zone là một nhóm các quy tắc nhằm chỉ ra những luồng dữ liệu được cho phép, dựa trên mức độ tin tưởng của điểm xuất phát luồng dữ liệu đó trong hệ thống mạng. Để sử dụng, bạn có thể lựa chọn zone mặc đinh, thiết lập các quy tắc trong zone hay chỉ định giao diện mạng(Network Interface) để quy định hành vi được cho phép
 - Các zone được xác định trước theo mức độ tin cậy, theo thứ tự từ  "ít-tin-cậy-nhất" đến "đáng-tin-cậy-nhất":
    - drop: ít tin cậy nhất – toàn bộ các kết nối đến sẽ bị từ chối mà không phản hồi, chỉ cho phép duy nhất kết nối đi ra.
    - block: tương tự như drop nhưng các kết nối đến bị từ chối và phản hồi bằng tin nhắn từ icmp-host-prohibited (hoặc icmp6-adm-prohibited).
    - public: đại diện cho mạng công cộng, không đáng tin cậy. Các máy tính/services khác không được tin tưởng trong hệ thống nhưng vẫn cho phép các kết nối đến trên cơ sở chọn từng trường hợp cụ thể.
    - external: hệ thống mạng bên ngoài trong trường hợp bạn sử dụng tường lửa làm gateway, được cấu hình giả lập NAT để giữ bảo mật mạng nội bộ mà vẫn có thể truy cập.
internal: đối lập với external zone, sử dụng cho phần nội bộ của gateway. Các máy tính/services thuộc zone này thì khá đáng tin cậy.
    - dmz: sử dụng cho các máy tính/service trong khu vực DMZ(Demilitarized) – cách ly không cho phép truy cập vào phần còn lại của hệ thống mạng, chỉ cho phép một số kết nối đến nhất định.
    - work: sử dụng trong công việc, tin tưởng hầu hết các máy tính và một vài services được cho phép hoạt động.
    - home: môi trường gia đình – tin tưởng hầu hết các máy tính khác và thêm một vài services được cho phép hoạt động.
    - trusted: đáng tin cậy nhất – tin tưởng toàn bộ thiết bị trong hệ thống.
  
 - Trong FirewallD, các quy tắc được cấu hình thời gian hiệu lực Runtime hoặc Permanent.

    - Runtime(mặc định): có tác dụng ngay lập tức, mất hiệu lực khi reboot hệ thống.
    - Permanent: không áp dụng cho hệ thống đang chạy, cần reload mới có hiệu lực, tác dụng vĩnh viễn cả khi reboot hệ thống.
 - Interface: giao diện áp dụng firewalld
 - Services: Các dịch vụ cho phép
 - Ports: Các port cho phép
 - Source: Các nguồn được chấp nhận (có thể là nhiều dải IP)
 - Rich-rule: Các quy tắc được bổ sung
### 4.5 Tìm hiểu các command phổ biến trên firewalld-cmd
 - $firewall-cmd --zone=[ZONE]
  - --add-service/--remove-service=[SERVICE]	cho phép service sử dụng (http,ssh,...)
  - --get-service
  - --add-port/--remove-port=[PORT]/[PROTOCOL]
  - --add-sourse-port/--remove-source-port=[PORT]
  - --add-protocol/--remove-protocol=[PROTOCOL]
  - --reload
  - --list-all-zones
  - --set-default-zone=[ZONE]
  - --get-active-zone
  - --set-target=[default|ACCEPT|DROP|REJECT]
  - nmcli connection add type ethernet con-name *connection-name* ifname *interface-name* (tạo một connection mới)
  - nmcli c mod [DEV] connection.zone [internal/external/zone???]
  - [--zone=external] --add-masquerade
  - --permanent
  - [--zone=external] --add-forward-port=port=[NUM] :proto=[PROTO] :toport=[NUM] : toaddr=[ADDR]
  - Chọn zone cho kết nối:
   - Mỗi một source chỉ được có 1 zone, nhưng 1 zone có thể có nhiều source hoặc nhiều interface.
   - Kết nối tìm zone nào trùng với source trước, nếu không có thì tìm zone nào tùng với interface, nếu không có thì chuyển sang zone mặc định.
   - Nếu các rule trùng với gói tin thì được phép qua, nếu không thì sẽ chuyển sang zone mặc định. Zone mặc định lại tiếp tục kiểm tra rule.
### 4.6 So sánh iptable service và firewalld service
 - Cả hai đều sử dụng với mục đích tương tự nhau.
 - iptable có cấu hình cú pháp chính xác, vì vậy đôi khi cần sử dụng cho công việc cụ thể gì đó cần phải thực hiện nhiều tùy chọn hơn cho các câu lệnh (ví dụ cần chặn nhiều cổng một lúc)
 - firewalld có giao diện trực quan hơn, phân loại service nhiều hơn. Tuy vậy có nhiều tùy chọn dài và cụ thể, gây khó khăn trong việc sử dụng linh hoạt các câu lệnh. Và quản lý đôi khi gặp khó khăn.
