![image](https://user-images.githubusercontent.com/43545058/87114955-28000c80-c29c-11ea-883b-d82391e61d91.png)
## 1. Yêu cầu đặt ra 
- Cho phép PC 1 và PC 2 truy cập internet
- Chặn tất cả liên hệ giữa PC1 và PC2
- Chặn PC1 truy cập vào website : baomoi.com
- Máy ngoài internet có thể truy cập ssh vào PC2 bằng IP cổng ngoài của node firewalld
## 2. Mô hình thiết kế mô phỏng đáp ứng
![image](https://user-images.githubusercontent.com/43545058/87116672-9ba41880-c2a0-11ea-9065-46847cf01724.png)
 - PC1 <=> PC1
 - PC2 <=> PC2
 - FirewallD <=> FirewallD
 - Host only <=> Mạng Host-only của VMware
 - NAT <=> Nat từ FirewallD
 - Truy cập internet bằng mạng bridge cảu VMware qua PC vật lý qua mạng nội bộ cty
## 3. Giải pháp thực hiện
#### 1.Cho phép PC 1 và PC 2 truy cập internet
 - PC1 và PC2 sẽ được kết nối với PC firewalld, sau đó được NAT qua IP bridge kết nối mạng
#### 2.Chặn tất cả liên hệ giữa PC1 và PC2
 - Firewalld chặn kết nối có nguồn và đích là PC1 và PC2
#### 3.Chặn PC1 truy cập vào website : baomoi.com
 - Firewalld chặn ip có nguồn PC1, có đích là web baomoi.com, có thể thêm port hoặc protocol
#### 4.Máy ngoài internet có thể truy cập ssh vào PC2 bằng IP cổng ngoài của node firewalld
 - Firewalld chuyển tiếp ssh chứ không thực hiện ssh
## 4. Cài đặt cấu hình
#### Cài đặt sẵn có
 - PC1, PC2 và firewallD sử dụng CentOS7, có cài đặt firewallD trên PCfirewalld
 - PC1 địa chỉ 172.16.0.30/24, PC2 địa chỉ 172.16.0.40/24, PCfirewalld 172.16.0.10/24 (ens37) cho mạng host-only và 192.168.101.248/24 (ens33) cho mạng nội bộ cty

#### 1.Cho phép PC 1 và PC 2 truy cập internet
 - Tạo nat cho PCfirewalld
  - Đặt internal và external interface: 
    - $nmcli connection mod ens33 connection.zone external
    - $nmcli connection mod ens37 connection.zone internal
  - Đặt interface về rule	
    - $firewall-cmd --zone=external --add-interface=ens33 --permanent
    - $firewall-cmd --zone=internal --add-interface=ens37 --permanent
  - Cấu hình nat:
    - $firewall-cmd --zone=external --add-masquerade --permanent	
#### 2.Chặn tất cả liên hệ giữa PC1 và PC2
 - Firewalld không có cấu hình áp dụng bộ lọc với đích (destination), tuy vậy firewalld vẫn làm việc dựa trên iptables command, vì vậy ta có thể thêm rich rule một iptables command vào
  - $firewall-cmd --direct --add-rule ipv4 -t filter OUTPUT -d 172.16.0.30 -p tcp -j DROP
#### 3.Chặn PC1 truy cập vào website : baomoi.com
 - Tạo một rich rule mới chặn truy cập vào web site đó
  - $firewall-cmd --permanent --zone=internal --add-rich-rule='rule family=ipv4 source address=[IP] reject'
#### 4.Máy ngoài internet có thể truy cập ssh vào PC2 bằng IP cổng ngoài của node firewalld
 - $firewall-cmd --zone=external --add-forward-port=port=22:proto=tcp:toaddr=172.16.0.40	forward tới pc2
 ![image](https://user-images.githubusercontent.com/43545058/87282420-71f82500-c51e-11ea-9f84-143a6f78ff56.png)

