## 1. Giới thiệu OpenFlow (protocol)
 - OpenFlow là tiêu chuẩn đầu tiên, cung cấp khả năng truyền thông giữa các giao diện của lớp điều khiển và lớp chuyển tiếp trong kiến trúc SDN. OpenFlow cho phép truy cập trực tiếp và điều khiển mặt phẳng chuyển tiếp của các thiết bị mạng như switch và router, cả thiết bị vật lý và thiết bị ảo, do đó giúp di chuyển phần điều khiển mạng ra khỏi các thiết bị chuyển mạch thực tế tới phần mềm điều khiển trung tâm. Các quyết định về các luồng traffic sẽ được quyết định tập trung tại OpenFlow Controller giúp đơn giản trong việc quản trị cấu hình trong toàn hệ thống. Một thiết bị OpenFlow bao gồm ít nhất 3 thành phần:

 - Secure Channel: kênh kết nối thiết bị tới bộ điều khiển (controller), cho phép các lệnh và các gói tin được gửi giữa bộ điều khiển và thiết bị.
 - OpenFlow Protocol: giao thức cung cấp phương thức tiêu chuẩn và mở cho một bộ điều khiển truyền thông với thiết bị.
 - Flow Table: một liên kết hành động với mỗi luồng, giúp thiết bị xử lý các luồng thế nào.
 ![image OpenFlow đóng vai trò là giao thức điều khiển trong SDN](https://user-images.githubusercontent.com/43545058/103843822-17094a00-50cb-11eb-84c4-1f17cf2035c4.png)
 - *:OpenFLow được dùng để update bảng định tuyến cho thiết bị mạng. Nếu muốn thay đổi cấu hình khác của thiết bị, ta sẽ dùng NETCONF,OVSDB,SNMP... đi kèm.
## 2. Thành phần trong OpenFlow
![image Các thành phần trong OpenFlow](https://i0.wp.com/overlaid.net/wp-content/uploads/2017/02/openflow-components.png?w=700&ssl=1)
 - Một OpenFlow SW gồm ít nhất 1 bảng định tuyến, 1 bảng group dùng để định tuyến, và các channel dùng để điều khiển từ bên ngoài.
### 2.1 2 Luồng chạy cơ bản của gói tin
 - Ingress: Khi packet bắt đầu vào tại input-port, gói tin sẽ được xử lý qua một (một chuỗi) các flow table (bắt đầu từ flow table mức ưu tiên cao nhất - mức 0). Nếu gói tin khớp với điều kiện flow table này thì sẽ được chuyển tiếp đến flow table tiếp theo. Nếu không khớp với bất kì flow table nào thì nó sẽ chạy qua flow table mặc định (table-miss) - nếu có - sẽ khớp với mọi gói tin. Mỗi flow table nếu khớp mà có quy định action thì sẽ được thực hiện ngay action đó, trong đó bao gồm chuyển tiếp qua port hoặc gửi tới group table (một tập các thao tác nâng cao cho gói tin) hoặc meter table (dùng để điều khiển QoS)
 -  Exgress (Tùy chọn): Nếu không có quá trình này thì gói tin sẽ được gửi ra khỏi thiết bị. Nếu có, gói tin sẽ được gửi tới port cụ thể, bắt đầu xử lý qua một chuỗi các flow-table khác, việc xử lý tương tự như Ingress.
 ![image Quá trình xử lí của gói tin trong OpenFlow](https://i0.wp.com/overlaid.net/wp-content/uploads/2017/02/openflow-flow-det.png?w=700&ssl=1)
 
## Các gói tin OpenFlow
 - Gồm các loại tin nhắn điều khiển controller-switch, kiểm tra thống kê, thông số, trạng thái của port, thay đổi flow table, xác nhận gói tin, thông báo lỗi, các gói tin quảng bá và duy trì kết nối.
 ![image Một số gói tin của OpenFlow khi bắt đầu kết nối](https://user-images.githubusercontent.com/43545058/103869850-7d0ec500-50fd-11eb-9132-2b17ae851749.png) 
## OpenFlow trong OVS
### ovs-vsctl command
 - Đặt OpenFlow controller trên OVS
 - $ovs-vsctl set-controller [BRIDGE] [target]
 - $ovs-vsctl get/del-controller [BRIDGE]
  - BRIDGE: Tên bridge
  - target: Địa chỉ OpenFlow controller, có các mẫu sau:
    - ssl:ip[:port], đi kèm với --private-key, --certificate, --ca-cert là bắt buộc.
    - tcp:ip[:port]
    - unix:[file] : Địa chỉ socket file (Unix) hoặc file có chứa localhost TCP port (Window).
    - punix:[file] / pssl:[port][:ip] / ptcp:[port][:ip] : Nghe kết nối OpenFlow .
### ovs-ofctl
 Câu lệnh để quản lý cấu hình OpenFlow
 - show [SW]: Thông tin cơ bản SW (port, des, MAC, Speed, config, state)
 - dump-ports [SW] [PORT]: Thống kê số packet,bytes trên port
#### Flow
 - dump-flows [SW] : Xem bảng định tuyến (flow-table) trên SW
  - priority: 0-65535, mặc định là 32768, số to nhất được duyệt trước
 - dump-flows [SW] [FLOW] : Xem bảng định tuyến riêng từng flow table
 - add-flow [SW] [FLOW] : Thêm flow với các tham số
 - add-flows [SW] [FILE] : Thêm flow từ file
 - mod-flows [SW] [FLOW] : Sửa action với flow khớp với [FLOW]
 - del-flows [SW] [FLOW_S] : Xóa flow(s) khớp
 - diff-flow [SOURCE_1] [SOURCE_2]
Cấu trúc [FLOW] gồm hai phần chính: field và action
*[FLOW] để trong "" để có thể dùng dấu cách, ngoặc đơn*
##### [FLOW] field
*Xem chi tiết ở ovs-field(7)*
 - Các trường (field) trong OpenFlow được lấy từ layer2-4. Nếu một layer có nhiều hơn 1 lớp giao thức, lớp t2 trở đi trong layer đó sẽ không được xem xét.
 - Cú pháp điều kiện khớp có thể là:
  - Khớp tuyệt đối : nw_src=10.1.2.3
  - Khớp theo dải: nw_src=10.1.0.0/255.255.0.0; nw_src=10.1.0.0/16,1000 ≤ tcp_dst ≤ 1999
  - Khớp theo bộ: tcp_src ∈ {80, 443, 8080}
  - Khớp toàn bộ: any nw_src, nw_src=*
  - Khớp ngoại lệ : tcp_dst ≠ 80
###### protocol
 - Biểu thị giao thức nào sẽ được kiểm tra. Trong nhiều trường hợp, điều này là cần thiết. Trong đó có
  - ip/ipv6
  - icmp/icmp6
  - tcp/tcp6
  - udp/udp6
  - eth
  - sctp/sctp6
  - arp/rarp
  - mpls/mplsm
###### Table
 - table=[NUM] Số hiệu table
###### Layer 2(Ethernet field )
|Tên|Nội dung|
|---|---|
|eth_src/ dl_src| MAC nguồn|
|eth_dst/ dl_dst| MAC đích|
|eth_type| Không phổ biến|
###### Vlan
|Tên|Nội dung|
|---|---|
|vlan_vid|Số hiệu vlan|
|vlan_pcp|vlan priority (0-7), mặc định là 0 (ưu tiên thấp nhất)|
|vlan_tci| Các thông tin vlan được gộp với nhau (ít dùng)|
 - OpenvSwitch chỉ hỗ trợ đóng gói 1 lần vlan.
###### Mpls (thêm sau)
###### Layer 3 (IPv4/6)
|Tên|Nội dung|
|---|---|
|ip_src/ nw_src||
|ip_dst/ nw_dst||
|ipv6_src||
|ipv6_dst||
|ipv6_label| flow label - đánh dấu các flow khác nhau|
|nw_proto|Không dùng|
|nw_ttl| Time to live (0-255)|
|nw_frag /ip_frag| Xem gói tin có phân mảnh hay không ("no","yes","first" (offset=0),"later","not_later") *Không có xác định gói tin cuối nhỉ*|
|nw_tos|Type of Service, bao gồm dscp và ecn (Tìm hiểu sau)|
|ip_dscp||
|nw_ecn/ ip_ecn||
###### Layer 3 ARP
 - Giao thức phân giải địa chỉ, dùng để quảng bá địa chỉ IP cho các node lân cận.
 - Bắt/ Lọc gói tin loại này chắc không dùng nhiều.
###### Layer 3 NSH (Network service header)
 - (Giao thức này chưa dùng mấy)
###### Layer 4 TCP,UDP, SCTP
|Tên|Nội dung|
|---|---|
|tcp_src / tp_src|tcp port|
|tcp_dst / tp_src|tcp port|
|tcp_flags| nếu match=set thì "+", match=unset thì "-", VD:tcp_flag=tcp_flags=+syn-ack| 
|udp_src||
|udp_dst||
|sctp_src| Giao thức này không dùng mấy|
|sctp_dst||

![image tcp_flags](https://user-images.githubusercontent.com/43545058/103999908-1c03f180-51d0-11eb-8cce-6165a7729329.png)

###### Layer4 ICMP4/6
|Tên|Nội dung|
|---|---|
|icmp_type|
|icmp_code|Mã gói tin icmp|
|icmpv6_type|
|icmpv6_code|
|nd_target| Các trường này thuộc icmp6|
|nd_sll| # |
|nd_tll| # |

###### Tunnel
|Tên|Nội dung|
|---|---|
|tun_id / tunnel_id||
|tun_src||
|tun_dst||
|tun_ipv6_src||
|tun_ipv6_dst||
|...|...|
###### Metadata
|Tên|Nội dung|
|---|---|
|in_port| số hiệu cổng mà gói tin đi vào theo OpenFlow|
|in_port_oxm||
|pkt_mark| số hiệu mark_packet (đánh dấu luông gói tin - chỉ hoạt động trên kernel Linux |
|actset_output| số hiệu cổng gói tin đi ra theo OpenFlow|
|packet_type| là eth_type(không quan trọng)|
###### Connection tracking (Theo dõi kết nối)
 - OpenFlow từ 2.5 trở đi hỗ trợ theo dõi trạng thái kết nối đầu cuối.
|Tên|Nội dung|
|---|---|
|ct_state| Các flag biểu thị trạng thái set(+) hoặc unset(-) (Chi tiết bên dưới)|
|ct_zone|4 byte hexa, quy định zone sẽ theo dõi|
|ct_mark|8 byte hexa,..?|
|ct_label|????|
|ct_nw_src||
|ct_nw_dst||
|ct_ipv6_src||
|ct_ipv6_dst||
|ct_nw_proto| 6 cho TCP,17-UDP,1 - ICMP|
|ct_tp_src|?|
|ct_tp_dst|?|

|Giá trị ct_state|Nội dung|
|---|---|
|trk| track hoặc untrack. Khi untrack, mọi flag khác sẽ =0, chỉ được track sau khi thực hiện ct action|
|new| gói tin là kết nối mới|
|est| gói tin là một phần của kết nối trước|
|rel| ... Có liên quan đến các gói tin trước (data, thông báo,..)|
|rpl| Gói tin phản hồi |
|inv| Trạng thái không hợp lệ, không thể theo dõi kết nối được|
|snat| Gói tin đã được đổi địa chỉ/port nguồn bởi ct action (xem phần action bên dưới)|
|dnat| Gói tin đã được đổi địa chỉ/port đích bởi ct action|
###### Tên gọi khác
 - Trên mạng, các câu lệnh với flow thích thay tên dễ gợi nhớ hơn bằng  mã hexa (??)
|Trên mạng nó ghi như này|Thực ra nó là|
|---|---|
|eth_type=0x0806 / dl_type=0x0806|arp|
|eth_type=0x8035 / dl_type=0x8035|rarp|
|eth_type=0x0800 / dl_type=0x0800|ip|
|eth_type=0x86dd / dl_type=0x86dd |ipv6|
|eth_type=0x8847 hoặc eth_type=0x8848|  MPLS/MPLS multicast|
|ip_proto=6| tcp |
|ip_proto=17| udp |
|ip_proto=132| sctp|
|ip_proto=1| icmpv4 |
|ip_proto=58| icmpv6|

##### [FLOW] action
 Được chia làm hai dạng:
 - action list: Một danh sách hành động thực hiện theo thứ tự.
 - action set: có thêm sắp xếp thứ tự  lại và loại bỏ thừa/trùng lặp.

*Xem chi tiết ở ovs-actions(7)
###### conjunction - Nhóm các flow lại
 - Với action=conjunction(id, k/n) với id là id của conjunction, k là số chiều, n là tổng số chiều
 - và thêm flow: conj_id=id action=[ACTION] với [ACTION] là action thực sự được thực thi với conjunction.
 - Các bộ flow cùng chiều thì sẽ chỉ kiểm tra 1 lần, nếu khớp thì sẽ đi đến bộ thuộc chiều tiếp theo luôn
  - Có các hành động chỉ được thực hiện duy nhất 1 lần trong 1 flow, nếu có hơn 1 thì sẽ chỉ thực hiện hành động cuối cùng
  - Có các hành động được thực hiện theo thứ tự và ảnh hưởng lẫn nhau.
  - Có các hành động chỉ được thực hiện duy nhất 1 lần, nếu có hơn 1 thì chỉ thực hiện hành động đầu tiên
|Chỉ thực hiện hành động cuối cùng|Thực hiện tuần tự|Chỉ thực hiện hành động đầu tiên|
|---|---|---|
|strip_vlan|	load
|pop_mpls|	move	|	group|
|decap|	mod_dl_dst|	output|
|encap|	mod_dl_src|	resubmit|
|push_mpls|	mod_nw_dst|	ct_clear|
|push_vlan | 	mod_nw_src|	ct|
|dec_ttl|	mod_nw_tos|	|
|dec_mpls_ttl|	mod_nw_enc|	|
|dec_nsh_ttl|	mod_nw_ttl|	|
|	|	mod_tp_dst|	|
|	|	mod_tp_src|	|	
|	|	mod_vlan_pcp|	|
|	|	mod_vlan_vid|	|
|	|	set_field|	|
|	|	set_tunnel|	|
|	|	set_tunnel64|	|
|	|	set_queue|	|
 - Nếu match và action không liên quan đến nhau (không nhất quán) thì sẽ bị ngăn cấm hoặc không thực hiện được (cần phải kiểm tra)
###### number (Chỉ là số)
 - Số hiệu cổng ra (giống output)
 - VD: actions=1,2,3,4
###### output
  
 - output:[PORT]
    - :[FIELD]
    - (port=[PORT],max_len=[nbyte]
 - Trong đó
  - [PORT] có các giá trị
    - 1 - 65535, là số hiệu port
    - local
    - in_port: nhận cổng nào thì trả ra cổng đó
    - normal: Bình thường
    - flood: broadcast ra mọi cổng, trừ cổng được nhận và cổng được off flood
    - all: broadcast ra mọi cổng, trừ cổng được nhận
    - controller: gửi gói tin và metadata của nó cho OpenFlow controller
###### bundle
###### group
 - group:[GROUP] Số hiệu group [0-2^32]. Nếu group bị xóa, thì flow có chứa group này cũng bị xóa
###### strip_vlan / pop_vlan
 - Gỡ thẻ vlan ngoài cùng (nếu có).
###### push_vlan
 - push_vlan:[TYPE] Gắn thẻ vlan nào (0x8100 cho 802.1q hoặc 0x88a8 cho 802.1ad)
  - push tối đa 2 tag.
###### mpls
 - push_mpls:[TYPE]  
 - pop_mpls:[TYPE]
 - ...
###### encap
 Mã hóa dữ liệu theo giao thức
 - encap(ethernet) Mã hóa L3 trong Ethernet frame
 - encap(nsh(md_type=[md_type]),[tlv(class,type,value)]) Với md_type=1 hoặc 2, mã hóa gói tin L2 trong NSH. Với md_type=2, có thêm trường tlv
###### decap
 Giải mã hóa lớp ngoài cùng của gói tin
 - decap([packet_types=(ns=[namespace,type=[type])]) Với [namespace] nhận 0 (L2 packte) hoặc 1 (L3 packet), [type] là L3 protocol type, nhận 0x894f cho nsh, 0x0 cho Ethernet.
 - Nếu lớp ngoài là VLAN hoặc thuật toán không hỗ trợ (Ethernet hoặc NSH), sẽ xuất ra lỗi.
 - Thử chỉ để là decap() xem?

###### Thay đổi giá trị    
 - load:[VALUE]->[DST]
 - set_field:[VALUE]/[MASK]->dst
  - load chỉ sửa đổi giá trị thuần túy của nó, còn set_field sẽ thay đổi giá trị theo mẫu. VD:
    - set_field:00:11:22:33:44:55->eth_src
    - load:0x001122334455->eth_src
###### Move
 Copy giá trị
 - ...
###### mod
 Đổi MAC
 - mod_dl_src:[MAC]
 - mod_dl_dst:[MAC]
*Nếu match chỉ với L3 packet, hành động này không được thực thi.*
 Đổi ip
 - mod_nw_src:[IP]
 - mod_nw_dst:[IP]
*Tương tự như trên*
 - mod_nw_tos:[TOS] Đặt các bit DSCP thành số tos theo bội của 4.(4 bit đầu tos)
 - mod_nw_ecn:[ECN] Đặt các bit ECN có giá trị từ 0-3 (2 bit sau tos)
 
 Đổi port
 - mod_tp_src:[PORT]
 - mod_tp_dst:[PORT]
*Chỉ tác dụng với tcp,udp,sctp header*
###### dec-ttl Giảm time to live
 - dec_ttl
 - dec_ttl(id1,[id2]...)
 Nếu ttl trước khi giảm là 0 hoặc 1, thì sẽ được báo cho các controller có [id]
###### tunnel
 - set_tunnel:[id]
###### connection tracking
 - ct([args]) Gửi gói tin qua connection tracker, có
  - table=[table] số OpenFlow table đi qua
  - nat 
  - nat(type=[addrs] : [port][,flag])
    - Có thể là 1 hoặc 1 dải IP
    - Có thể là 1 hoặc 1 dải port
    - flag là random (ngẫu nhiên trong dải IP), persistent (Dùng lại IP)
 - ct(commit,[args]) Bắt đầu tracker, ct_flag bây giờ mới bật.
    - force
    - exec([action...]) : Sửa ct_mark và ct_label
    - alg 
###### learn - thay đổi flow
 - learn([args])
  - Trong đó, [arg] là các tham số cho flow mới, VD:udp, actions=learn(dl_type=0x800, nw_proto=17, udp_dst=udp_src)
###### time out 
 - fin_timeout([key=value])
  - idle_timeout=[seconds] Kết nối hết hạn sau số giây không hoạt động
  - hard_timeout=[seconds] Kết nối hết hạn sau số giây, bất kể có hoạt động hay không
###### resubmit
 Tìm một flow table khớp với gói tin và thực hiện action flow đó, xong mới thực hiện nốt action flow này
 - resubmit:[port] Với port=in_port để tìm cụ thể hơn
 - resubmit([port],[table][,ct]) Với port với table cụ thể, còn ct....
  - Hay dùng lệnh này với việc tìm trên table khác : resubmit(,[table]).
###### Khác
 - note:[text] Thêm ghi chú vào cho dễ nhìn thôi
 - meter:[meter_id] Áp dụng meter table id.
 - goto_table:[table] Nhảy tới table tiếp theo
#### Group
 - add-group [SW] [GROUP] Thêm group
 - add-group [SW] [FILE] 
 - del-groups [SW] [GROUP] Xóa group khớp
 - mod-group [SW] [GROUP]
 - dump-groups [SW] [GROUP]
 - dump-group-features [SW] 
 - dump-group-stats [SW] [GROUP]
 - insert-buckets [SW] [GROUP]
 - remove_buckets [SW] [GROUP]
 - Group table cho phép OpenFlow thực hiện chuyển tiếp cho nhiều liên kết, bao gồm cả load-balancing,multicasst, active/standby. Có thể nói group là tập hợp các flow cho phép cấu hình nâng cao hơn.
![image Ví dụ group table trên OpenFlow](https://user-images.githubusercontent.com/43545058/104260486-354dbc00-54b6-11eb-8255-487beda6f32e.png)
 - Group table gồm nhiều bucket, mỗi bucket sẽ có tham số và action riêng. Có 4 dạng(type) của group:
  - All Nhận mọi gói tin, copy gói tin ra mọi bucket để thực hiện .
```
ovs-ofctl add-group br0 group_id=20,type=all,bucket=mod_dl_src=00:00:00:11:11:11,mod_dl_dst=22:22:22:00:00:00,output:13,bucket=mod_dl_src=00:00:00:11:11:11,mod_dl_dst=22:22:22:00:00:00,output:14
#(đổi mac rồi truyền qua 2 port)
```
  - Indirect Chỉ bao gồm 1 bucket, dùng để gọn lại flow khi flow nó dài, hoặc dùng nhiều lần action.
  - Fast-failover 1 bucket sẽ kiểm tra trạng thái 1 port, nếu port down thì bucket đó không được sử dụng, nếu port up thì chỉ 1 bucket duy nhất được sử dụng, và chỉ thay đổi khi chính port đó down, và chuyển sang bucket khác. Group này dùng để ngăn ngừa và phục hồi lỗi kết nối.

```
ovs-ofctl add-group br0 group_id=1,type=fast_failover,bucket=watch_port:1,output:1                                                          
ovs-ofctl add-group br0 group_id=2,type=fast_failover,bucket=watch_port:2,output:2 
ovs-ofctl add-group br0 group_id=3,type=fast_failover,bucket=watch_port:3,watch_group:1,output:5,bucket=watch_port:4,watch_group:2,output:6     
 
ovs-ofctl add-flow br0 in_port=11,dl_src=22:11:11:11:11:11,dl_dst=22:00:00:00:00:00,dl_type=0x0800,nw_proto=6,nw_src=1.1.2.100,nw_dst=2.2.2.100,actions=group:3
# Có 3 port, port nào down thì redirect ra port khác
```
  - Select Mục đích chính dùng để load-balancing. Sẽ sử dụng một thuật toán hash, sau đó dựa vào round-robin để xác định đầu ra duy nhất cho gói tin.
  
  
  
 - Các trường thuộc tính cơ bản
  - group_id=[id] Số hiệu group, hoặc "all"
  - type=[type] Kiểu group: all,ff/fast_failover,select,indirect
  - command_bucket_id=[id] Khi dùng lệnh insert-buckets hoặc remove-buckets. Có [all,first,last]
  - selection_method= [method] Phương pháp chọn bucket khi type=select. Có [dp_hash,hash], mặc định là dp_hash
  - selection_method_param=[number] Trọng số cho hash 
  - fields=field Trương thông tin sẽ được hash(?)
  - bucket=[arg], trong đó có
   - bucket_id=[id]
   - action=[action_s]
   - weight=[value] Giá trị cho bucket dùng để chọn sw cho group với type là select
   - watch_port=[port] Dùng để xem port đó up hay down (type group là ff)
   - watch_gorup=[group] Dùng thay watch_port cũng được
 ![image Cấu trúc group table](https://user-images.githubusercontent.com/43545058/104260738-a7be9c00-54b6-11eb-99af-a8bff954f756.png)
#### Meter
 - add-meter [SW] [METER] Thêm meter
 - mod-meter [SW] [METER] Sửa
 - del-meters [SW] [METER_S] Xóa meter(s) khớp
 - dump-meters [SW] [METER]
 - meter-stats [SW] [METER]
 - meter-feature] [SW]
 - Có các trường thuộc tính sau:
  - meter=[id] Là số hoặc [all,controller,slowpath]
  - kbps/ pktps Giới hạn băng thông
  - burst : bật tính năng cho phép sử dụng tham số burst_size
  - stats : bật tính năng cho phép thu thập phân tích meter,band
  - band=[args], Các hành động nếu lưu lượng cao hơn cho phép; Trong đó:
   - type=drop,dscp_remark
   - rate=[value] Giới hạn tỉ lệ band
   - burst_size=[size]
   - prec_level=[level] Level của DSCP nếu có đặt type thì sẽ dùng(lên gg mà tra, sai level nó sẽ báo)
```
ovs-ofctl add-meter s2 meter=1,kbps,burst,stats,bands=type=drop,rate=30000,burst_size=1000
```
*Xem thêm nhiều ví dụ ở đây https://docs.pica8.com/pages/viewpage.action?pageId=5112993*
#### Khác
  - --log-file[=FILE] Đặt log file
  - --verbose=[SPEC] logging level
  - --names Hiện tên port thay vì địa chỉ port
  - --no-names
  - -O --protocol Đặt protocol (OpenFlow10, OpenFlow11, OpenFlow12, OpenFlow13, OpenFlow14, OpenFlow15)
  - --sort/--rsort[=field] Sắp xếp theo thứ tự tăng/giảm dần
 - --color=always|never|auto : Thêm màu cho dễ nhìn
