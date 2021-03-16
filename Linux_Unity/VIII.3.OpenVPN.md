## Giới thiệu

### Cách OpenVPN xác thực danh tính
#### Các bên tham gia
##### Certificate Authority (CA)
 - Là cơ quan giúp xác thực danh tính của Server, Client.
##### Server
 - Có nhiệm vụ kết nối, phục vụ nhu cầu Client.
##### Client
 - Bên sử dụng dịch vụ của Server.
### Tài nguyên tham gia
 - OpenVPN sẽ bao gồm một PKI (public key infrastructure): Một bộ key public-private cho CA và các cặp khóa public-private cho server và client 
### Các bước thiết lập xác thực danh tính
 - CA sẽ khởi tạo cặp khóa riêng của mình, tạo cặp khóa cho server, tạo cặp khóa cho từng client.
 - Client và server sẽ được gửi và giữ một cách bí mật private key của mình,
 - Khi Client muốn kết nối với Server, C gửi public key cho S. S gửi tiếp C-public-key đó cho CA. CA thấy đúng là key do mình tạo thì sẽ phản hồi lại với S. S nhờ đó sẽ chấp nhận yêu cầu C.
*Thời gian giữa S và C phải tương đối đồng bộ với nhau*
## Tải và cài đặt
### Cài đặt bằng script
```
curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh
chmod +x openvpn-install.sh
sudo bash openvpn-install.sh
```
#### Cấu hình ban đầu server 
 - IP của server 
 - IP public để server lắng nghe (chọn ip public thì đi ra ngoài thì router mới định tuyến được)
 - Bật IPv6 NAT (không)
 - Port (để default - 1194)
 - Protocol (UDP)
 - Chọn DNS (defaullt - hoặc tùy chọn cũng được)
 - Bật nén dữ liệu (không)
 - Tạo user mới đầu tiên 
  - User sẽ được lưu trong /root/[user].ovpn
### Tải và cài đặt bằng link zip
#### OpenSSL
```
wget https://www.openssl.org/source/openssl-1.0.2h.tar.gz
tar xzf openssl-1.0.2h.tar.gz
cd openssl-1.0.2h
./config shared no-ssl2 no-ssl3 no-comp enable-ec_nistp_64_gcc_128 -Wl,-rpath=/usr/local/ssl/lib --prefix=/usr/local
make depend
make -j 4
make test
sudo make install
sudo ln -s /usr/local/ssl/bin/openssl /usr/local/bin/openssl
```
#### OpenVPN
```
wget https://swupdate.openvpn.org/community/releases/openvpn-2.3.11.tar.gz
tar xzf openvpn-2.3.11.tar.gz
sudo apt-get install libpam0g-dev
cd openvpn-2.3.11
make -j 4
make check
make install
```
#### easy-rsa
 - Dùng để tạo,quản lý KPI
 - Tải về file https://github.com/OpenVPN/easy-rsa-old
 - Khởi tạo ban đầu (có thể sửa thông tin trong vars trước
```
cd /easy-rsa/2.0
source vars
./clean-all
```
 - Tạo CA 
```
./pkitool --initca
```
 - Tạo server tên là server1
*common name phải là duy nhất, cần phải sửa trong vars*
*Hoặc không thì sửa trong keys/index.txt.attr: unique_subject=no*
```
source v
./pkitool --server server1
./pkitool client1
```
## Cấu hình server
#### Khởi động/tắt server
```
sudo systemctl stop openvpn-server@server.service
sudo systemctl start openvpn-server@server.service
sudo systemctl restart openvpn-server@server.service
```
#### File cấu hình server 
 - /etc/openvpn/server.conf
 ![image Sơ đồ các file liên quan đến chứng thực trong server](https://user-images.githubusercontent.com/43545058/105315833-85cdc380-5bf2-11eb-96ed-bf2636270fdf.png)

##### crl.pem
 - Certificate Revocation Lists (CRL) là danh sách các chứng thực bị thu hồi (do bị lộ, hết hạn, muốn chủ động thu lại quyền...) và sẽ không sử dụng lại các chứng thực này nữa.
## Cấu hình client
### Tạo chứng thực client tại CA
#### Với script
 ./openvpn-install.sh rồi chọn thêm tên user thôi
#### Tạo bằng easy-rsa 
 ./easy-rsa gen-req client [user-name]
 ./pkitool [user-name]
(Ở trên có hướng dẫn rồi)
### Phân phát khóa 
#### Client
 - Đưa client ca.crt,[user].ovpn, và server.crt (Có thể đặt trong cùng một folder)
#### Server
 - Server không cần đưa gì cả
*Server đã có server.crt và server.key chưa?*
## Khởi động service
### CA
...?
### Server
 - systemctl start openvpn@server
 - systemctl enable openvpn@server
### Client
 - systemctl start openvpn@client
 - systemctl enable openvpn@client
 - sudo openvpn --client --config [user].opvn


