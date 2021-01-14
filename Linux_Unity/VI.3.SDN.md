## 1 SDN
 - SDN hay mạng điều khiển bằng phần mềm (Software Defined Networking) được dựa trên cơ chế tách riêng việc kiểm soát một luồng mạng với luồng dữ liệu (control plane và data plane). SDN dựa trên giao thức luồng mở (Open Flow) và là kết quả nghiên cứu của Đại học Stanford và California Berkeley. SDN tách định tuyến và chuyển các luồng dữ liệu riêng rẽ và chuyển kiểm soát luồng sang thành phần mạng riêng có tên gọi là thiết bị kiểm soát luồng (Flow Controller). Điều này cho phép luồng các gói dữ liệu đi qua mạng được kiểm soát theo lập trình. Trong SDN, control plane được tách ra từ các thiết bị vật lý và chuyển đến các bộ điều khiển. Bộ điều khiển này có thể nhìn thấy toàn bộ mạng lưới, và do đó cho phép các kỹ sư mạng làm cho chính sách chuyển tiếp tối ưu dựa trên toàn bộ mạng. Các bộ điều khiển tương tác với các thiết bị mạng vật lý thông qua một giao thức chuẩn OpenFlow. Kiến trúc của SDN gồm 3 lớp riêng biệt: lớp ứng dụng, lớp điều khiển, và lớp cơ sở hạ tầng (lớp chuyển tiếp).
 ![image](https://user-images.githubusercontent.com/43545058/102194474-eda43680-3eef-11eb-9b05-01a180a8a352.png)
 - Lớp ứng dụng: Là các ứng dụng kinh doanh được triển khai trên mạng, được kết nối tới lớp điều khiển thông qua các API, cung cấp khả năng cho phép lớp ứng dụng lập trình lại (cấu hình lại) mạng (điều chỉnh các tham số trễ, băng thông, định tuyến, …) thông qua lớp điều khiển.
 - Lớp điều khiển: Là nơi tập trung các bộ điều khiển thực hiện việc điều khiển cấu hình mạng theo các yêu cầu từ lớp ứng dụng và khả năng của mạng. Các bộ điều khiển này có thể là các phần mềm được lập trình.
 - Lớp cơ sở hạ tầng: Là các thiết bị mạng thực tế (vật lý hay ảo hóa) thực hiện việc chuyển tiếp gói tin theo sự điều khiển của lớp điểu khiển. Một thiết bị mạng có thể hoạt động theo sự điều khiển của nhiều bộ điều khiển khác nhau, điều này giúp tăng cường khả năng ảo hóa của mạng.
## 2 NFV 
 - Công nghệ Ảo hóa Chức năng mạng (Network Function Virtualization – NFV) áp dụng công nghệ ảo hóa (Virtualization) và điện toán đám mây (Cloud Computing) vào các máy chủ, thiết bị chuyển mạch và thiết bị lưu trữ phổ thông (Commercial off the Shelf) nhằm tạo ra một môi trường để triển khai các hàm chức năng mạng ảo hóa (Virtualised Network Function – VNF) như: switching, firewall, routing, load balancing,… có chức năng tương tự như trên các thiết bị mạng chuyên trách truyền thống.
 - Với cách tiếp cận truyền thống của các nhà cung cấp dịch vụ mạng, ứng với mỗi dịch vụ, mỗi chức năng mạng sẽ phải có những thiết bị chuyên trách riêng đảm nhận. Do mỗi thiết bị chỉ đảm trách những nhiệm vụ riêng nên hiệu năng sẽ rất cao nhưng lại khiến việc triển khai, vận hành, bảo dưỡng hay mở rộng trở nên phức tạp.
## 3 NFV và SDN
 - Nếu như khái niệm NFV xuất phát từ nhu cầu của các nhà cung cấp dịch vụ muốn giảm bớt chi phí đầu tư các thiết bị phần cứng bằng cách ảo hóa các thiết bị mạng để có thể triển khai các dịch vụ mạng trên phần cứng phổ thông, thì SDN lại xuất phát từ các trường đại học, viện nghiên cứu, data center muốn tách bạch việc điều khiểu mạng khỏi các thiết bị vật lý, để dễ dàng cấu hình, quản lý tập trung một lượng lớn các thiết bị này.

 - Về bản chất, hai công nghệ này là độc lập với nhau. Bên này có thể áp dụng được vào thực tiễn mà không cần phụ thuộc vào bên kia. Thật khó để nói được SDN hay NFV, công nghệ nào tốt hơn. Vì như đã so sánh rõ ở trên, hai công nghệ này phục vụ cho 2 mục đích hoàn toàn khác nhau.
|Tiêu chí so sánh|	SDN |	NFV |
|---|---|---|
|Mục đích|	Phân tách giữa control plane và data plane, quản lý tập trung, cấu hình mạng bằng cách lập trình |Chuyển dời các chức năng mạng từ phần cứng chuyên dụng sang các thiết bị phổ thông.|
|Đối tượng phục vụ|	Các viện nghiên cứu, trung tâm dữ liệu	|Các nhà cung cấp dịch vụ mạng.|
|Thiết bị	|Máy chủ, thiết bị chuyển mạch phổ thông	|Máy chủ, thiết bị chuyển mạch và lưu trữ phổ thông|
|Ứng dụng	|Điều phối mạng. Quản lý luồng traffic đi qua các thiết bị.|	Ảo hóa các thiết bị mạng như: router, firewall, CDN,… Khởi tạo và triển khai hàng loạt các thiết bị ảo.|
|Tổ chức chuẩn hóa	|Open Networking Forum	|ETSI NFV Working Group|
 - Mục tiêu chung của cả SDN và NFV là điều khiển hạ tầng mạng dễ dàng hơn, tiết kiệm chi phí và hạn chế việc tương tác trực tiếp với các thiết bị phần cứng. Như vậy, ta có thể thấy rằng hai công nghệ này không hề đối chọi với nhau mà còn lại bổ sung, hoàn thiện lẫn nhau, tạo nên 1 giải pháp hoàn chỉnh cho ngành viễn thông.

 - Việc quản lý tập trung của SDN kết hợp với khả năng ảo hóa các thiết bị mạng của NFV sẽ đem lại những lợi ích vô cùng lớn với hạ tầng viễn thông. Đặc biệt là việc chuẩn bị cho công nghệ 5G sắp tới thì việc ứng dụng 2 công nghệ này tạo nên nền tảng cho 5G (theo như AT&T).
## 4 OpenFlow (protocol)
 - Ngắn gọn: OpenFlow là một giao thức trong SDN giúp quản lý bảng định tuyến trên SW.
*Xem chi tiết ở phần VII.3.OpenFlow.md*
## 5 NETCONF, RESTCONF (protocol)
 - REST API: Một API dựa trên kết nối HTTP, giúp giao tiếp với web server bằng các câu lệnh như GET,POST,PUT,DELETE.
 - NETCONF: Để giao tiếp với các thiết bị mạng (không thể giao tiếp trên kết nối HTTP được), NETCONF được sinh ra để làm nhiệm vụ này. NETCONF thực hiện dựa trên kết nối SSH qua TCP cổng 830 (mặc định), qua các câu lệnh như get,get-config,edit-config,copy-config,delete-config. NETCONF sử dụng XML để mã hóa dữ liệu, sử dụng YANG là một ngôn ngữ cấu trúc dữ liệu làm khung định nghĩa dữ liệu.
 - RESTCONF: Ý tưởng và sử dụng như NETCONF nhưng thực hiện trên HTTP. RESTCONF hỗ trợ XML và JSON để mã hóa.
 - Một số so sánh NETCONF và RESTCONF
|Tính chất| NETCONF| RESTCONF|
|---|---|---|
|Dễ sử dụng|có|có|
|Tự động làm đầy các tham số  không cần thiết mặc định|...|Có|
|Có thể lấy dữ liệu cấu hình, trạng thái, thống kê từ mỗi thiết bị và so sánh với nhau|Sử dụng <get> và <get-config>| HTTP GET sẽ được sử dụng cho cả hai trường hợp, với phần thông tin mở rộng là "config","nonconfig" hoặc "all"|
|Có thể cấu hình đồng thời cho toàn mạng thay vì chỉ từng thiết bị đơn lẻ|Khi NETCONF thực thi hành động, nó lock -> thực hiện thay đổi -> xác nhận thay đổi -> unlock. Quy trình two-phase này có thể thực hiện được trên toàn mạng| RESTCONF không hỗ trợ quy trình two-phase (mỗi lời gọi HTTP là một giao dịch riêng biệt) nhưng vẫn có thể thay đổi cấu hình từng thiết bị một, thực hiện trên toàn mạng.| 
|Thay đổi cấu hình từ thiết bị này cho giống thiết bị khác|Có thể với các trạng thái thiết bị như running (cấu hình hiện thời),startup (cấu hình lúc khởi động),candidate (cấu hình chỉ cho client xem)| Không thể sửa cấu hình trên RESTCONF server|
|Dump và restore cấu hình nguyên bản|Có|Có|
|Kiểm tra tính đúng đắn của cấu hình|Có|Có|
|Cấu hình được tập trung tại master, tới mỗi thiết bị sẽ được phiên dịch tùy theo thiết bị|Có|Có|
*Chi tiết hơn tại https://www.claise.be/netconf-versus-restconf-capabilitity-comparisons-for-data-model-driven-management-2/*
## 5 Opendaylight (ODL)
### 5.1 Giới thiệu
 - ODL là một dự án mã nguồn mở, được sử dụng như một nền tảng cho SDN với đặc tính như mã nguồn mở, tập trung hóa, dùng để quản lý các thiết bị mạng.
  - Sử dụng ngôn ngữ Java.
  - Maven làm build system.
  - OSGi dùng để quản lý, tải các gói, các feature linh hoạt.
  - Karaf: Trình chạy để load các modules/bundles.


#### Một chút về CLI và SNMP
 - Khi sử dụng CLI để quản lý các thiết bị, có nhiều bất lợi khó khăn có thể kể đến như:
  - Không quản lý được thực hiện thay đổi cấu hình đã đúng hay chưa.
  - Khi cấu hình xảy ra lỗi thì phải cài lại từ đầu.
  - Các bản cập nhật khác nhau của thiết bị có thể thay đổi/xóa mất câu lênh-> khó quản lý được script
  - Khó cho con người sử dụng.
 - SNMP là một tập hợp các giao thức để kiểm tra và quản lý các thiết bị mạng, với tuổi đời khá lâu (1980), được tích hợp trên nhiều thiết bị, nhỏ gọn tuy nhiên nó cũng có một số nhược điểm sau:
  - Quản lý thiết bị trên SNMP phức tạp và khó khăn.
  - Sử dụng giao thức UDP nên gói tin có thể bị thất lạc.
  - SNMP chủ yếu sử dụng với mục đích quản lý hiệu năng và lỗi.
#### OF-CONFIG (Protocol)
 - Đây là một giao thức để quản lý và cấu hình các OpenvSwitch dựa trên cấu trúc, giao thức của NETCONF.
![image Kiến trúc OF-CONFIG tích hợp vào OVSDB server](https://user-images.githubusercontent.com/43545058/103853216-21820e80-50e0-11eb-8918-c850486d92f7.png)
 - Theo mô hình, OF-CONFIG sẽ giúp chuyển các cấu hình từ NETCONF về câu lệnh ovsdb, từ đó tác dụng lên ovsdb server.
 - Với ý tưởng thống nhất sự quản lý về NETCONF, OF-CONFIG có thể được xem là một hướng phát triển tốt. Tuy nhiên, nó chưa thực sự hoàn chỉnh (https://sci-hub.se/https://ieeexplore.ieee.org/document/7502920), và phiên  bản gần nhất trên website github của họ cũng không hoạt động,chỉ dừng ở phiên bản 1.12. (https://github.com/openvswitch/of-config)
 - => Sẽ sử dụng giao thức ovsdb khi giao tiếp với OVS.
### 5.2 Cài đặt cơ bản
 - Tải file zip Opendaylight từ trang chủ https://docs.opendaylight.org/en/stable-aluminium/getting-started-guide/installing_opendaylight.html#downloading-and-installing-opendaylight
(Nên tải bản cũ Oxygen để có sẵn nhiều feature hơn)
 - unzip ...
 - cd 
 - ./bin/karaf
 - Sau khi đăng nhập, thêm các feature :
  - >feature:install odl-restconf odl-l2switch-switch odl-mdsal-apidocs odl-dlux-core odl-dluxapps-nodes odl-dluxapps-topology odl-dluxapps-yangui odl-dluxapps-yangvisualizer odl-dluxapps-yangman
### 5.3 Thêm gói feature phụ (repo)
 - Tại folder của ODL, tạo file "etc/org.ops4j.pax.url.mvn.repositories". Đây là file chứa các URL, ngăn cách với nhau bởi dấu phẩy, là địa chỉ các repo có chứa các feature đang cần thêm.
  - VD về 1 repo có feature odl-mdsal-apidocs: https://repo1.maven.org/maven2/org/opendaylight/netconf/odl-mdsal-apidocs/1.7.4/
 - Sau đó khởi động lại và kiểm tra với lệnh feature:list | grep "tên feature"
#### Cách khác
 >feature:repo-add [link] Với link là link xml file
 Ví dụ:feature:repo-add  https://repo1.maven.org/maven2/org/opendaylight/l2switch/odl-l2switch-all/0.7.4/odl-l2switch-all-0.7.4-features.xml
### 5.4 Không đăng nhập được ODL trên web
 - Thoát ODL
 - $ ./bin/karaf clear

 - $ ./bin/karaf
 - Tải thêm các feature sau:
 - >feature:install odl-restconf odl-l2switch-switch odl-mdsal-apidocs odl-dlux-core
 - Truy cập http://localhost:8181/index.html#/login với tk/mk là admin

 -  - (Tùy chọn)Sửa/thêm vào file etc/org.opendaylight.aaa.authn.cfg
```
authEnabled=false
```

## 6 Mininet
 - Mininet là một công cụ giả lập hệ thống mạng ảo, cùng với các phần mềm khác như GNS3, packet tracer,...
 - Mininet có ưu điểm là thao tác nhanh chóng, gọn nhẹ, dễ dàng tạo mới.
### 6.1 Cài đặt
```
git clone git://github.com/mininet/mininet
cd mininet
mininet/util/install.sh -a
```
  - Tải từ package
```
sudo apt-get install mininet
```
### 6.2 Câu lệnh
 - mn [options]
  - --help 
  - --nat Thêm cấu hình NAT cho topo
  - --mac Tự động thêm MAC cho host
  - -x/--xterm Thêm xterm (1 kiểu terminal) cho mỗi host)
  - -i/--ipbase [IPBASE] Đặt dải IP cho host
  - --topo=[TOPO] Đặt cấu hình topo cho mạng:
    - minimal: 1 SW và 2 host
    - linear,[num]: các SW nối tuyến tính với nhau, mỗi SW 1 host
    - single,[num]: 1 SW nối với các host
    - tree,[level][,leaf] : số lượng các lớp của cây, mỗi cây có số [leaf] host
  - --controller=...: Đặt controller
    - remote,ip=[IP]:[PORT]
    - ovsc.... OVScontroller
    - ryu..... Ryucontroller
 - mininet> ấn Tab để hiện thông tin các câu lệnh 
  - net : Hiển thị các kết nối+ port
  - links : Trạng thái kết nối
  - link [node1] [node2] up/down: 
  - switch [SW] up/down:
  - dump : Thông tin chung cấu hình mn
  - sh [command]: Dùng shell để chạy lệnh
    - sh [OVS-command]: Sử dụng các lệnh của OVS trên mininet
 - Các tùy chọn nâng cao khác:
  - Sử dụng script để tạo topo, để thực thi khi chạy topo.
  - Sử dụng câu lệnh để phân tích mạng, gói tin.
### 6.3 Tạo topo,script với Miniedit
 - Miniedit là 1 .py có trong example folder của mininet (sudo /usr/share/doc/mininet/examples/miniedit.py) giúp tạo topo tự động .
![image Giao diện miniedit](https://user-images.githubusercontent.com/43545058/104394284-e3ba3580-5578-11eb-8757-5609ff207a25.png)
 - Tạo topo đơn giản với host, sw, controller
  - Chuột phải vào từng thành phần để cấu hình nếu cần (để mặc định cũng được) (Nếu có controller thì có thể chuyển controller type là remote)
  - (Tùy chọn) Chọn Run xem có bị lỗi gì không, tại Tab Run->Show OVS summary để kiểm tra.
  - Chọn File->export level 2 script để xuất ra .py file  
![image](https://user-images.githubusercontent.com/43545058/104395596-a0ad9180-557b-11eb-912f-6ed39e21b450.png)
  - Chạy topo với lệnh $sudo python [file-name].py
###  6.4 Sửa file script 
 - Nếu chỉ tạo topo cơ bản với host,sw và controller như trên thì chưa đủ mô hình, nếu thêm router vào thì cần chỉnh sửa script để thêm ip và ip route.
  - Tạo topo như hình, với controller thì đổi thành remote, sau đó export ra .py file (file mininet_topo.py)
#### Mở và sửa file mininet_topo.py:
 - Trước dòng net.build(): Sửa các thành phần sau
  - Sửa lại các host sao cho khác mạng nhau (ip/subnet) cho phù hợp.
  - Trên các dòng net.addLink, sắp xếp lại thứ tự kết nối sao cho tuyến tính, tuần tự với nhau (dòng trước có liên quan đến dòng sau, không chỉnh nếu lỗi tự chịu)
 - Sau dòng net.build(): Thêm các thành phần sau
```
#Cho phép ip forward qua các port
r3.cmd("echo 1 > /proc/sys/net/ipv4/ip_forward") 
#Thêm ip cho port
r3.cmd("ip addr add 10.0.2.1/24 dev r3-eth1")
r3.cmd("ip addr add 10.0.1.1/24 dev r3-eth0")
#Thêm route cho router
r3.cmd("ip route add 10.0.2.0/24 via dev r3-eth1")
r3.cmd("ip route add 10.0.1.0/24 via dev r3-eth0")
#Thêm ip route cho router (Mạng nhỏ này thì chưa cần)
#Thêm ip route cho host
    h1.cmd("ip route add default via 10.0.1.1")
    h2.cmd("ip route add default via 10.0.1.1")
    h3.cmd("ip route add default via 10.0.2.1")
```
![image](https://user-images.githubusercontent.com/43545058/104406146-30aa0600-5591-11eb-90f6-b16c3488c0ad.png)
## 7 Opendaylight và Mininet
 - ODL sẽ kết nối qua cổng 6633,6653 hoặc 6640
 - Để kết nối với Mininet với SDN Opendaylight, khi chạy lệnh mn, ta thêm tùy chọn --controller=remote,ip=[IP]:[6633]. Sau đó pingall để ODL nhận diện được hết các node.
## 8 Opendaylight và OVS
 - Trên OVS, tại mỗi switch đặt cấu hình điều khiển sau
```
 ovs-vsctl set-controller [BRIDGE] tcp:[IP]:6633
```
  - Với port 6633 là mặc định để kêt nối với ODL, IP là địa chỉ ODL, giao thức có thể là tcp,udp hoặc ssl...
## Tạo lab SDN over VPN on VM
### Mục tiêu
 - Test xem kết hợp SDN trên đường truyền VPN trên các máy ảo riêng biệt từng đôi một đặt tại các vị trí khác nhau.
### Ứng dụng tham gia
 - Mininet: Ứng dụng để tạo mạng ảo cho nhanh
 - Opendaylight: SDN để giám sát thiết bị mạng
 - kvm+qemu: Ứng dụng tạo máy ảo
 - OVS: Ứng dụng tạo SW ảo
 - OpenVPN: Ứng dụng tạo VPN server.
### Mô hình mô phỏng 
![image](https://user-images.githubusercontent.com/43545058/104439400-a4650680-55c3-11eb-8699-577c03bb359e.png)
 - PC1 và PC3 cùng tunnel, có thể kết nối được với nhau.
 - PC2 không kết nối được với PC1 và 3




## Calico
 

