## 1. Giới thiệu
 - OVS là một phần mềm switch đa lớp, giúp thực hiện là một nền tảng switch tiêu chuẩn quản lý các giao diện, điều khiển, forwarding.
 - OVS phù hợp với các chức năng như một switch ảo trong môi trường máy ảo. OVS hỗ trợ đa nền tảng công nghệ ảo hóa nhân Linux như Xen, KVM, VirtualBox
 - Một số tính năng của OVS:
  - Hỗ trợ 802.1Q VLAN, trunk/access port.
  - Gộp NIC bằng LACP hoặc giao thức khác.
  - NetFlow, sFlow(R) dùng để phân tích lưu lượng mạng.
  - Cấu hình QoS, đặt chính sách (policy).
  - Tunnel VXLAN, STT, LISP, GRE, Geneve.
  - QUản lý kết nối với 802.1ag.
  - OpenFlow để quản lý switch.
  - Forwarding với hiệu suất cao. 
### 1.1.Một số bộ phận của OVS
  - ovs-vswitchd : deamon đi kèm switch.
  - ovsdb-server : database server mà ovs-vswitchd truy vấn để lấy cấu hình.
  - ovs-dpctl : Tool để cấu hình switch kernel module.
  - ovs-vsctl : Một tiện ích để truy vấn và cập nhật cấu hình trên ovs-vswitchd.
  - ovs-appctl : Tiện tích gửi lệnh tới deamon.
 - Một số tool khác như:
  - ovs-ofctl : dùng để truy vấn và quản lý switch và controller OpenFlow.
  - ovs-pki : Dùng để tạo, quản lý cơ hở hạ tầng public-key của OpenFlow switch.
  - ovs-testcontroler : controller OpenFlow dùng để test.
  - Một patch giúp tcpdump phân tích gói tin OpenFlow.
## 2. Cài đặt
### 2.1. Cài đặt trực tiếp
```
sudo apt update
sudo apt upgrade
sudo apt install openvswitch-switch
```
 - Sau đó khởi động deamon
  - sudo ovs-vswitchd
 - Kiểm tra
  - systemctl status ovs-vswitchd.service
## 3. Cấu hình ovs-vsctl
### Tạo bridge mới
  - ovs-vsctl add-br [BRIDGE]
    - del-br
    - list-br
### Tạo port trên br đó
  - ovs-vsctl add-port [BRIDGE] [PORT]
    - del-port
    - list-ports [BRIDGE]
### Tạo port vlan access
 - ovs-vsctl add-port [BRIDGE] [PORT] tag=[vlan-number]
  - $ovs-vsctl add-port br-ex tap0 tag=100
### Tạo port trunk    
 - Có 2 sw br1,br2, 2 port trk1,trk2 tương ứng trên hai sw
```
ovs-vsctl set interface trk1 type=patch options:peer=trk2
ovs-vsctl set interface trk2 type=patch options:peer=trk1
```
### Tạo tuntap device (NIC)
```
ip tuntap add mode tap pc1
```
## 4. KVM cơ bản với OVS
### Tạo network libvirt với bridge vừa tạo
 - Tạo file ovs1.xml; Ví dụ:
```
<network>
  <name>ovs1</name>
  <forward mode='bridge'/>
  <bridge name='br2'/>
  <virtualport type='openvswitch'/>
  </portgroup>
  <portgroup name='vlan-100'>
    <vlan>
      <tag id='100'/>
    </vlan>
  </portgroup>
  <portgroup name='vlan-200'>
    <vlan>
      <tag id='200'/>
    </vlan>
  </portgroup>
  <portgroup name='vlan-all'>
    <vlan trunk='yes'>
      <tag id='100'/>
      <tag id='200'/>
    </vlan>
  </portgroup>
</network>
```
### Sau đó tích hợp vào libvirt
 - virsh net-define ovs1.xml
 - virsh net-start ovs1
 - virsh net-autostart ovs1
### Tạo máy ảo mới/ sửa phần network xml file của máy ảo đã có sẵn

```
<interface type='network'>
      <mac address='52:54:00:63:87:60'/>
      <source network='ovs1'/>
      <model type='virtio'/>
      <address type='pci' domain='0x0000' bus='0x01' slot='0x00' function='0x0'/>
    </interface>
```
### Tạo dhcp server bằng dnsmasq
 - (Xem phần V.3.dnsmasq)
### Khởi động máy ảo
#### Kiểm tra bên máy guest
 - Kiểm tra bằng $ip a xem có nhận được ip chưa
 - Kiểm tra máy ảo có nhận ip động bằng DHCP không
 - Kiểm tra device mạng có tự động bật lên khi boot không
#### Kiểm tra bên host
 - Kiểm tra log (/var/log/dnsmasq.log) xem có nhận và phân phối được không.
 - Kiểm tra lease file (/var/lib/misc/dnsmasq)
 - Kiểm tra status dnsmasq bằng $systemctl status dnsmasq hoặc $journalctl -u dnsmasq -f
 - Kiểm tra cấu hình vlan, iptable, firewalld.
 - Kiểm tra bằng wireshark, tcpdump.
## 5. Lab OVS
### 5.1 Lab 1
#### Mô tả bài toán 
 - PC1,PC2 nhận DHCP từ dnsmasq, nối với SW1.
 - PC3 có IP tĩnh, nối với SW2.
 - Cả 3 PC kết nối được với internet.
 - PC1 và PC3 cùng VLAN, ping được tới nhau; PC2 khác vlan không ping được tới PC1.
#### Các bước thực hiện
##### Tạo NIC
 - Tạo NIC
 - Up nó lên
 
```
ip tuntap add mode tap pc1
ip tuntap add mode tap pc2
ip tuntap add mode tap pc3


ifconfig pc1 up
ifconfig pc2 up
ifconfig pc3 up
```

##### Tạo SW
 - Tạo SW1 có hai port với VLAN 1, và VLAN 2.
 - Tạo SW2 có port VLAN 2.
 - Trên hai SW tạo cổng trunk để kết nối với nhau.
```
ovs-vsctl add-br br1
ovs-vsctl add-br br2

ovs-vsctl add-port br1 pc1 tag=100
ovs-vsctl add-port br1 pc2 tag=200
ovs-vsctl add-port br2 pc3 tag=100

ovs-vsctl add-port br1 trk1
ovs-vsctl add-port br2 trk2

ovs-vsctl set interface trk2 type=patch option:peer=trk1
ovs-vsctl set interface trk1 type=patch option:peer=trk2
```
 - Tạo 2 port để tí gắn vào dhcp sw
```
ovs-vsctl add-port br1 dhcp1 tag=100
ovs-vsctl add-port br1 dhcp2 tag=200
sudo ovs-vsctl set int dhcp1 type=internal
sudo ovs-vsctl set int dhcp2 type=internal
```
##### Tạo network tương ứng cho kết nối với libvirt
 - Tạo 2 file network tương ứng ovsnet1.xml vaf ovs2net.xml
```
<network>
  <name>ovs1</name>
  <forward mode='bridge'/>
  <bridge name='br2' />
  <virtualport type='openvswitch'/>
  <portgroup name='vlan-100'>
    <vlan>
      <tag id='100'/>
    </vlan>
  </portgroup>
  <portgroup name='vlan-200'>
    <vlan>
      <tag id='200'/>
    </vlan>
  </portgroup>
  <portgroup name='vlan-all'>
    <vlan trunk='yes'>
      <tag id='100'/>
      <tag id='200'/>
    </vlan>
  </portgroup>
</network>
```
  - Network thứ 2 chỉ cần thay tên là ovs2 là được
 - Kết nối với libvirt
```
virsh net-define ovsnet1.xml 
virsh net-define ovsnet2.xml 
virsh net-start ovs1
virsh net-start ovs2
virsh net-autostart ovs1
virsh net-autostart ovs2
```
##### Đặt ip cho SW
```
ip add add 192.168.111.1/24 dev dhcp1
ip add add 192.168.222.1/24 dev dhcp2
ip link set br2 up
ip link set br1 up

```
##### Tạo dnsmasq service trên host
 - Tạo 2 range khác nhau cho 2 vlan
 - Sửa file /etc/dnsmasq.conf:
```
dhcp-range=192.168.111.2,192.168.111.100
dhcp-option=option:router,192.168.111.1
interface=dhcp1,dhcp2
dhcp-range=192.168.222.2,192.168.222.200
dhcp-option=option:router,192.168.222.1
```
 - Restart dnsmasq service
```
dnsmasq --test
systemctl restart dnsmasq
```
##### Tạo guest PC
###### Với guest có sẵn
 - Thay đổi phần network của guest (đang tắt máy)
```
 <interface type='bridge'>
      <mac address='52:54:00:54:88:d8'/>
      <source bridge='br1'/>
      <vlan>
        <tag id='100'/>
      </vlan>
      <virtualport type='openvswitch'>
        <parameters interfaceid='fe531219-633f-4cf1-a56c-78d52ffee433'/>
      </virtualport>
      <target dev='pc1'/>
      <model type='virtio'/>
      <address type='pci' domain='0x0000' bus='0x01' slot='0x00' function='0x0'/>
    </interface>
```
###### Tạo guest mới
 - 1. Với cloud-init
  - Tạo 1 file network.cfg để cấu hình network,....
![image](https://user-images.githubusercontent.com/43545058/102156515-4c49c000-3eb0-11eb-9731-e42f10b83855.png)
 - 2. Với virt-install
 - 3. Với virt manager
##### Tạo iptable trên host
 - Mạng của guest pc1 sẽ đi qua NIC dhcp1, sau đó được gửi tới NIC mạng của host. (trừ đi các gói tin có port/ protocol lấy dhcp).
 - Tương tự với dhcp2.
*Check iproute trên guest*
*Nhớ $echo 1 > /proc/sys/net/ipv4/ip_forward để forward gói tin*
```

iptables -t filter -A FORWARD -i dhcp1 -j ACCEPT
iptables -t filter -A FORWARD -i dhcp2 -j ACCEPT
```
- Sau đó sẽ được NAT qua mạng bên ngoài. -> guest sẽ ping được tới mạng bên ngoài.
```
iptables -t nat -A POSTROUTING -o wlp2s0 -j MASQUERADE

```
### 5.2 Lab 2
#### Mô tả bài toán
 - PC1 có ip DHCP từ host, nối với SW1.
 - PC2 nhận DHCP từ PC1, kết nối mạng thông qua PC1
#### Các bước thực hiện
 - Tham khảo lab trên
##### Tạo NIC
##### Tạo SW bằng OVS
##### Tạo Network
##### Đặt IP cho network để cấp dhcp
##### Đặt dnsmasq service trên host
##### Tạo guest PC
##### Đặt dnsmasq service trên guest pc1
  - Nếu không yum/apt install dnsmasq được:
    - Thử ping 8.8.8.8 -> Xem lại iptables, ip route, gateway, NIC device up hay không 
    - Thử ping google.com.vn -> Không có dns server
        - Thêm cấu hình host /etc/dnsmasq.conf: dhcp-option=option:dns-server,8.8.8.8
## 6  Một số chi tiết các câu lệnh khác
### ovs-vsctl
 - init
 - show
 - emer-reset : reset lại cấu hình ( vẫn để lại các if, port thì phải )
#### Bridge
 - add-br [BRIDGE] Tạo bridge mới
  - del-br/list-brs
#### Port
 - add-port [BRIDGE] [PORT] Tạo port trên bridge
  - del-port/list-ports
  - port-to-br : Tìm port tương ứng với br nào.
#### Interface
 - list-ifaces [bridge]
 - add-bond-iface [PORT] [IFACE]
 - add-bond [BRIDGE] [PORT] [IFACEs] [column[:key]=value]... : Gắn [interface] (ít nhất 2) vào [port] (có thể tạo mới luôn) trên [bridge] (có thể tạo mới luôn), kèm theo các tùy chọn khác. 
#### Controller, Manager
 - Controller là kết nối tới controller ở xa, còn Manager là cho phép kết nối từ xa.
 - set-controller [BRIDGE] [target]
 - get/del-controller [BRIDGE]
  - BRIDGE: Tên bridge
  - target: Địa chỉ OpenFlow controller, có các mẫu sau:
    - ssl:ip[:port], đi kèm với --private-key, --certificate, --ca-cert là bắt buộc.
    - tcp:ip[:port]
    - unix:[file] : Địa chỉ socket file (Unix) hoặc file có chứa localhost TCP port (Window).
    - punix:[file] / pssl:[port][:ip] / ptcp:[port][:ip] : Nghe kết nối OpenFlow .
 - Với Manager thì câu lệnh cũng tương tự.
 
#### Database
 - Quy định các cấu hình khác.
 - list [TABLE] [RECORD]...
 - find/set/get/add/remove/clear/destroy [TABLE] [column[:key]=value] : Thay đổi các thuộc tính khác.
##### Danh sách thuộc tính [TABLE]
 Open_vSwitch
 Bridge
 Port
 Interface
 Flow_Table
 QoS
 Queue
 Mirror
 Controller
 Manager
 NetFlow
 SSL
 sFlow
 IPFIX
 Flow_Sample_Collector_Set
 AutoAttach
 - Khi sử dụng lệnh $ovs-vsctl list [TABLE] với từng thuộc tính trên, ta sẽ xem được danh sách các thuộc tính có thể có.
 - Xem ví dụ và chi tiết hơn tại man page: https://www.systutorials.com/docs/linux/man/8-ovs-vsctl/
 - https://docs.openvswitch.org/en/latest/faq/configuration/
### ovs-ofctl
 *Xem ở phần VII.3.OpenFlow.md*
