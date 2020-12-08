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
  - systemctl status ovs-switchd.service
## 3. Cấu hình ovs-vsctl
### Tạo bridge mới
  - ovs-vsctl add-br [BRIDGE]
    - del-br
    - list-br
### Tạo port trên br đó
  - ovs-vsctl add-port [BRIDGE] [PORT]
    - del-port
    - list-ports [BRIDGE]
    
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
## 6. OpenFlow
##
