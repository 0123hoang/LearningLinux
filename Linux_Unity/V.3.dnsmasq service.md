## 1.Giới thiệu dnsmasq
 - Là một server deamon có tác dụng làm dhcp server cho host.
 - Có thể gắn vào interface trên VSwitch để sử dụng cho mạng ảo, sử dụng kèm với libvirt,...
## 2. Tải và cài đặt
 - (Debian) sudo apt install dnsmasq
 - systemctl start dnsmasq.service
 - systemctl enable dnsmasq.service
## 3. Cấu hình
 - Mặc định, dnsmasq sẽ đọc file /etc/resolv.conf để tìm dns server, cổng dhcp là 127.0.0.1:53
### 3.1 Tại file /etc/dnsmasq.conf
 - Thêm dns server
  - server=[ip]@[interface]
  - server=[ip-src]@[ip-dst]#[port]
 - Đổi port
 - Chỉ nghe trên interface này
  - interface=[interface-name]
 - Trừ interface này
  - except-interface=[interface-name]
 - Chỉ nghe trên ip này
  - listen-address=[ip]
 - Gắn dnsmasq vào interface
  - bind-interfaces
 - Thêm dải DHCP
  - dhcp-range=[ip-start],[ip-end],[net-mask],[lease-time]
 - Chỉ cấp với tag trùng nhau
  - dhcp-range=tag:[tag-name],[ip-start],[ip-end],[net-mask],[lease-time]
 - Dải static không dùng DHCP
  - dhcp-range=[ip],static
 - Đặt ip host và host-name cụ thể theo MAC
  - dhcp-host=[MAC],[name],[ip]
 - Chạy shell file khi tạo/hủy DHCP
  - dhcp-script=[path-to-shell-file]
 - Log dns query, log dhcp
  - log-queries
  - log-dhcp
 - Ghi log tại  file cụ thể
  - log-facility=/var/log/dnsmasq.log
*Nếu dnsmasq xung đột với systemd-resolved: bind-interfaces để khác cặp ip:port là được* 
 - restart lai dnsmasq
### 3.2 Test systax conf file
 - dnsmasq --test
### 3.3 Lease file location
 - /var/lib/misc/dnsmasq
