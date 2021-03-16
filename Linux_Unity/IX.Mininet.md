
### 0 Giới thiệu
 - Mininet là một công cụ giả lập hệ thống mạng ảo, cùng với các phần mềm khác như GNS3, packet tracer,...
 - Mininet có ưu điểm là thao tác nhanh chóng, gọn nhẹ, dễ dàng tạo mới.
### 1 Cài đặt
```
git clone git://github.com/mininet/mininet
cd mininet
mininet/util/install.sh -a
```
  - Tải từ package
```
sudo apt-get install mininet
```
### 2 Câu lệnh
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
### 3 Tạo topo,script với Miniedit
 - Miniedit là 1 .py có trong example folder của mininet (sudo /usr/share/doc/mininet/examples/miniedit.py) giúp tạo topo tự động .
![image Giao diện miniedit](https://user-images.githubusercontent.com/43545058/104394284-e3ba3580-5578-11eb-8757-5609ff207a25.png)
 - Tạo topo đơn giản với host, sw, controller
  - Chuột phải vào từng thành phần để cấu hình nếu cần (để mặc định cũng được) (Nếu có controller thì có thể chuyển controller type là remote)
  - (Tùy chọn) Chọn Run xem có bị lỗi gì không, tại Tab Run->Show OVS summary để kiểm tra.
  - Chọn File->export level 2 script để xuất ra .py file  
![image](https://user-images.githubusercontent.com/43545058/104395596-a0ad9180-557b-11eb-912f-6ed39e21b450.png)
  - Chạy topo với lệnh $sudo python [file-name].py
###  4 Sửa file script 
 - Nếu chỉ tạo topo cơ bản với host,sw và controller như trên thì chưa đủ mô hình, nếu thêm router vào thì cần chỉnh sửa script để thêm ip và ip route.
  - Tạo topo như hình, với controller thì đổi thành remote, sau đó export ra .py file (file mininet_topo.py)
#### 4.1 Mở và sửa file mininet_topo.py:
 - Trước dòng net.build(): Sửa các thành phần sau
  - Sửa lại các host sao cho khác mạng nhau (ip/subnet) cho phù hợp.
  - Phải tạo trước 1 SW trước khi tạo các thành phần khác.
  - Trên các dòng net.addLink, sắp xếp lại thứ tự kết nối sao cho tuyến tính, tuần tự với nhau (dòng trước có liên quan đến dòng sau, không chỉnh nếu lỗi tự chịu)
 - Sau dòng net.build(): Thêm các thành phần sau
```
#Cho phép ip forward qua các port
r3.cmd("echo 1 > /proc/sys/net/ipv4/ip_forward") 
#Thêm ip cho port
r3.cmd("ip addr add 10.0.2.1/24 dev r3-eth1")
r3.cmd("ip addr add 10.0.1.1/24 dev r3-eth0")

#Thêm ip route cho router (Mạng nhỏ này thì chưa cần)
r3.cmd("ip route add 10.0.2.0/24 via dev r3-eth1")
r3.cmd("ip route add 10.0.1.0/24 via dev r3-eth0")
#Thêm ip route cho host
    h1.cmd("ip route add default via 10.0.1.1")
    h2.cmd("ip route add default via 10.0.1.1")
    h3.cmd("ip route add default via 10.0.2.1")
```
![image](https://user-images.githubusercontent.com/43545058/104406146-30aa0600-5591-11eb-90f6-b16c3488c0ad.png)
