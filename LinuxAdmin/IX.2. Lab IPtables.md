![image](https://user-images.githubusercontent.com/43545058/86985847-61f9e180-c1bc-11ea-82aa-e63ed009733f.png)
## 1.Yêu cầu đặt ra  
 - 1. Máy PC 2 chỉ có thể liên hệ với PC 1 trên cổng 22.  
 - 2. Máy PC 1 có thể ping tới Server DC.  
 - 3. Server DC có thể liên hệ với PC 1 trên cổng 22 thông qua địa chỉ IP Public của Server nội bộ.  

## 2.Mô hình thiết kế mô phỏng đáp ứng
![image](https://user-images.githubusercontent.com/43545058/87114366-93e17580-c29a-11ea-8192-5ddd6a4f22f5.png)
 - PC1 <==> PC1
 - PC2 <==> PC2
 - Private network <==> Mạng nội bộ trong VMware (172.16.0.0/24)
 - Server <==> PC thật (có IP mạng cty truy cập mạng) sử dụng VMware (có chức năng NAT từ mạng nội bộ VMware).
 - Public network <==> Mạng nội bộ của cty (192.168.101.0/24)
 - Server DC <==> Một máy tính khác thuộc mạng nội bộ cty
## 3.Giải pháp thực hiện
### Cài đặt sẵn có
 - PC thật sử dụng Ubuntu 20.04, kết nối với mạng nội bộ cty với địa chỉ 192.168.101.168/24.
 - PC thật có cài đặt VMware. VMware sử dụng mạng NAT 172.16.0.0/24 là mạng nội bộ.
 - VMware có hai máy ảo CentOS 7 có cài đặt iptables-service.
### 1. Máy PC 2 chỉ có thể liên hệ với PC 1 trên cổng 22. 
 - Đặt default gateway của PC2 là IP của PC1 (có thể ping, ssh port 22), PC1 cho phép nhận tin từ cổng đó, không cho phép forwarding.
### 2. Máy PC 1 có thể ping tới Server DC.  
 - PC1 có thể ping thông qua mạng cty. PC 1 cho phép gửi và nhận ping.
 - PC vật lý cho phép gửi và nhận ping.
### 3. Server DC có thể liên hệ với PC 1 trên cổng 22 thông qua địa chỉ IP Public của Server nội bộ.  
 - PC vật lý cho phép nhận và chuyển tiếp gói tin ping tới PC1, tương tự có thể chuyển tiếp gói tin từ PC1 qua PC khác.
## 4. Cài đặt cấu hình
### 1.Máy PC 2 chỉ có thể liên hệ với PC 1 trên cổng 22.
 - Bật iptable service và sshd trên PC 1.
 - Đặt ip static PC 1 là 172.16.0.30, default gate-way 172.16.0.10, dns 8.8.8.8
 - Đặt rule cho iptable PC1
  - Xóa hết tất cả các rule cũ đi	$iptables -t filter -F
  - Đặt mặc định là ACCEPT	$iptables -P INPUT ACCEPT
  - Insert chặn tất cả luồng từ PC2	$iptables -t filter -I INPUT -s 172.16.0.40/24 -j DROP
  - Insert cho phép port 22 từ PC2	$iptables -t filter -I INPUT -s 172.16.0.40/24 -p tcp --dport 22 -j ACCEPT
  - Lưu lại rule trước khi khởi động lại máy	$service iptables save
  ![image Login thành công](https://user-images.githubusercontent.com/43545058/86998553-d774aa80-c1da-11ea-9bde-14ca5d4debeb.png)
  - Insert cho phép ping từ PC2	$iptables -t filter -I INPUT -s 172.16.0.40/24 -p icmp -j ACCEPT
  ![image Ping thành công](https://user-images.githubusercontent.com/43545058/86998707-24588100-c1db-11ea-8252-0912fd41250c.png)
  ![image Toàn bộ rule](https://user-images.githubusercontent.com/43545058/86998782-4a7e2100-c1db-11ea-8558-780ddf3c3894.png)y
*Có thể kiểm tra ngay sau mỗi câu lệnh*
### 2. Máy PC 2 có thể ping tới Server DC. 
 - Thêm một NIC nữa cho PC1, lấy IP và default như 1 máy tính trong mạng nội bộ cty.
 - Cho PC1 bật packet forwardind: echo 1 > /proc/sys/net/ipv4/ip_forward
 => PC1 có mạng nội bộ 172.16.0.30/24 (ens37), mạng ngoài 192.168.101.168/24 (ens33)
 - Đặt rule cho iptable PC1
 - Đặt nat	iptables -t nat -A POSTROUTING -o ens33 -j MASQUERADE
 - Cho chuyển tiếp gói tin	iptables -A FORWARD -i ens37 -j ACCEPT
 ![image](https://user-images.githubusercontent.com/43545058/87026810-78c52600-c206-11ea-96c1-32cfe88fd01c.png)
 ![image PC2 thông qua PC1 ping tới một máy trạm ngẫu nhiên trên mạng ct](https://user-images.githubusercontent.com/43545058/87026863-8c708c80-c206-11ea-9fa7-b91846cb6581.png)
*Nếu không được thì kiểm tra lại ip route đi*
### 3. Server DC có thể liên hệ với PC 1 trên cổng 22 thông qua địa chỉ IP Public của Server nội bộ.
*Thay bằng ssh bằng ping*
 - Từ 1 máy trạm nội bộ ping tới PC1(thay bằng PC2)
 - Máy tính vật lí phải chuyển tiếp gói tin ping chứ không được xử lí gói tin
 - Luồng gói tin ping PC2 <=> PC1(nat)(nat có ở lab trước) <=> PC vật lí(chuyển tiếp) <=> PC1
 - Thêm rule cho máy tính vật lý:
  - DNAT trước	$iptables -t nat -A PREROUTING -p icmp -j DNAT --to 192.168.101.248
  - Sau đấy mới FORWARD	$iptables -t filter -A FORWARD -p icmp -j ACCEPT
  - Rồi SNAT lại $iptables -t nat -A POSTROUTING -p icmp -j SNAT --to 192.168.101.162
  ![image Trông như đang ping tới PC vật lý nhưng thực ra là PC1 đang trả lời](https://user-images.githubusercontent.com/43545058/87113716-1bc68000-c299-11ea-99ca-7592074a875e.png)
