 ## 1.Giới thiệu Cloud-init
 ## 2.Tích hợp
 ## 3.Các giai đoạn boot (boot stage)
 ## 4.Các command
 ## 5.User data
 ## 6.Instance data

## 1.Giới thiệu Cloud-init
 - Cloud-init là một phương pháp tiêu chuẩn công nghiệp giúp khởi tạo các phiên bản trên đám mây trên nhiều nền tảng.
 - Các phiên bản đám mây được khởi tạo dữ liệu từ image và data từ người dùng, từ nhà phân phối, từ metadata của cloud.
 - Với những dữ liệu trên, Cloud-init sẽ giúp tự động hóa cài đặt khi boot lần đầu, bao gồm cả cấu hình thiết bị, network, ssh, cấu hình người dùng, cập nhật phiên bản, chạy lệnh terminal...
## 2.Tích hợp
 - Cloud-init hỗ trợ nhiều phiên bản Linux khác nhau như ArchLinux, Ubuntu,Debian, RHEL/CenOS, FreeBSD...
 - Cloud-init hỗ trợ trên nhiều nền tảng đám mây công cộng như AWS, Azure, Google Cloud, IBM Cloud, CloudStack
 - Cloud-init cũng hỗ trợ trên các nền tảng đám mây riêng như OpenStack, LXD, KVM, MAAS...
### 2.1 Tích hợp với virt-install
 - Với tùy chọn --cloud-init, meta-data sẽ được truyền tới VM. Một file NoCloud ISO sẽ được tạo ra và gắn tới VM như một CDROM tại lần boot đầu tiên. Các đề mục phụ như sau:
  - root-password-generate=on: Mặc định là on, tạo ngẫu nhiên mật khẩu root cho VM, xuất ra console.
  - disable=on: Mặc định là on, tắt cloud-init cho những lần boot sau.
  - root-password-file=....
  - meta-data= :Chỉ định file meta-data trực tiếp. Nếu có trường này, các trường meta-data khác của cloud-init sẽ được bỏ qua.
  - user-data= :Chỉ định file meta-data trực tiếp. Nếu có trường này, các trường user-data khác của cloud-init sẽ được bỏ qua.
  - ssh-key= : Gửi public key cho guest (ví dụ: ssh-key=/home/user/.ssh/id_rsa.pub)
## 3.Các giai đoạn boot (boot stage)
 - Cloud-init cần được tích hợp vào trong giai đoạn boot một cách đúng đắn. Có 5 giai đoạn sau:
### 3.1 Generator
 - Mặc định, generator sẽ bật cloud-init. Nó sẽ không bật cloud-init nếu
  - File /etc/cloud/cloud-init.disable tồn tại.
  - File /proc/cmdline chứa dòng cloud-init=disable.
### 3.2 Local
![image](https://user-images.githubusercontent.com/43545058/98646916-a95cce00-2366-11eb-9387-d409b5011fc6.png)
 - Phần này sẽ định vị dữ liệu cục bộ, và xác nhận cấu hình network sẽ được sử dụng, được cung cấp qua metadata, fallback, hoặc được disable trong /etc/cloud/cloud.cfg.
 - Giai đoạn này phải chặn bật network lên (network bring-up) hay các cấu hình được áp dụng từ trước (có bao gồm các hooks DHCP,..).
### 3.3 Network
![image](https://user-images.githubusercontent.com/43545058/98648591-11141880-2369-11eb-8c2e-f63af68dbdde.png)
 - Sau khi network được bật lên, hệ thống tiến hành mount.
### 3.4 Config
 - Chạy module config.
### 3.5 Final
 - Được chạy ở giai đoạn cuối cùng của boot. Giai đoạn này chạy các shell-scripts, install package, cấu hình các pluggin
## 4.Các command
### Sử dụng $cloud-init --help
 - analyze : Lấy thông tin phân tích tài nguyên, sự kiện của cloud-init khi boot.
 - collect-log : Lấy thông tin log, thông tin hệ thống và file data. Các log này được lấy từ 
```
/var/log/cloud-init.log
/var/log/cloud-init-output.log
/run/cloud-init
/var/lib/cloud/instance/user-data.txt
cloud-init package version
dmesg output
journalctl output
```
 - features : In ra các đặc tính được hỗ trợ.
 - init : Thông thường được chạy bởi hệ thống init OS để thực thi init và init-local
 - modules : Thường được chạy bởi hệ thống init OS để thực thi module trong boot gồm  module:config và module:final. Các module được chạy trên các giai đoạn boot khác nhau được ghi trong /etc/cloud/cloud.cfg.
 - query : Trả về metadata của guest, được lưu ở /run/cloud-init/instance-data.json
 - single : Chạy 1 module cloud-config riêng lẻ.
 - status : Trả về trạng thái của cloud-init.
## 5.Thông tin chung
### 5.1 File log
 - /var/log/cloud-init.log
 - /var/log/cloud-init-output.log
 - /run/cloud-init
### 5.2 File cấu hình
 - /etc/cloud/cloud.cfg
 - /etc/cloud/cloud.cfg.d/*.cfg
### 5.3 File data
#### instance
 - /var/lib/cloud/instance/ chỉ tới thư muc instance-id gần đây nhất, dùng để kiểm tra data (user/vendor) đã được truyền đúng chưa.
#### data
 - /var/lib/cloud/data/ chứa thông tin về lần boot trước
*file user-data từ trên internet kéo về *
## Cài đặt cloud-init
```
sudo apt install cloud-init -y
systemctl enable cloud-init-local.service
systemctl enable cloud-init.service
systemctl enable cloud-config.service
systemctl enable cloud-final.service
```
## 7.User data, meta data và vendor data
### 7.1 Các định dạng User Data
 - Gzip: Các thông tin được nén (tối đa 16384 bytes) sẽ được giải nén.
 - Mime Multi Part archive: Khi sử dụng file mime-mutli part, người dùng có thể chỉ rõ nhiều dạng dữ liệu.
 - User-data script: Bắt đầu với "#!" or "Content-Type: text/x-shellscript" và được thực thi tại "rc.local-like" trong lần boot đầu tiên. "rc.local-like" nghĩa là "gần cuối lúc boot"
 - Include file: Bắt đầu với "#include" hoặc "Content-Type: text/x-include-url". File này bao gồm danh sách các dòng url, và thông tin của các url sẽ được đọc (có thể là gzip,mime-mutli-part,plain text).
 - Cloud Config data: Bắt đầu với "#cloud-config" hoặc "Content-Type: text/cloud-config" theo định dạng yaml. Trong đây bao gồm những cài đặt như: apt upgrade, thêm apt source, thêm ssh key...
 - Upstart job: Nội dung được đặt trong file /etc/init, bắt đầu với "#upstart-job" hoặc "Content-Type: text/upstart-job"
 - Cloud Boothook: Được đặt trong /var/lib/cloud . bắt đầu với  "#cloud-boothook" hoặc "Content-Type: text/cloud-boothook". Không có cơ chế nào được cung cấp để hook này chạy 1 lần. Nó cần phải tự quản lý...
 - Part Handle: Đặt trong /var/lib/cloud/data, bắt đầu với "#part-handler" hoặc "Content-Type: text/part-handler", được viết bằng python, gồm một hàm list_types và một hàm handle_part. List_types trả về danh sách các mime-type mà hàm handle_part sử dụng.
### 7.2 Thứ tự thực hiện
 - cloud-boothook hoặc bootcmd (cloud-config)
  - Chạy mỗi khi boot
 - x-shellscript hoặc runcmd (cloud-config)
  - Chạy chỉ duy nhất 1 lần (lần boot đầu tiên)
 - upstart-job
  - Chạy mỗi khi boot.
  
Cloud-init được khởi động theo cách sau:
Upstart --[runs]--> rc ---[runs]--> cloud-init
### 7.3 Meta data 
 - Lưu trữ thông tin về máy.
### 7.4 Cách sử dụng
 - Với các file user-data, meta-data và có thể thêm vendor-data là những file lưu trữ cấu hình cài đặt về máy và user khi boot.
### 7.5 Tạo userdata đơn giản
```
#cloud-config
hostname: ctest1
fqdn: ctest1.example.com
manage_etc_hosts: true
users:
  - name: centos
    sudo: ALL=(ALL) NOPASSWD:ALL
    groups: adm,sys
    home: /home/centos
    shell: /bin/bash
    lock_passwd: false
    ssh-authorized-keys:
      - <sshPUBKEY>
# only cert auth via ssh (console access can still login)
ssh_pwauth: false
disable_root: false
chpasswd:
  list: |
     root:linux
     centos:newpass123
  expire: False
packages:
  - qemu-guest-agent
  - bind-utils
  - vim-enhanced

# manually set BOOTPROTO for static IP
# older cloud-config binary has bug?
runcmd:
    - [ sh, -c, 'sed -i s/BOOTPROTO=dhcp/BOOTPROTO=static/ /etc/sysconfig/network-scripts/ifcfg-eth0' ]
    - [ sh, -c, 'ifdown eth0 && sleep 1 && ifup eth0 && sleep 1 && ip a' ]
# written to /var/log/cloud-init.log, /var/log/messages
final_message: "The system is finally up, after $UPTIME seconds"
```
 - Một số ví dụ / module khác xem ở https://cloudinit.readthedocs.io/en/latest/topics/examples.html
*Cú pháp phải theo đúng định dạng yaml (dấu :, dấu cách..), có thể kiểm tra ở http://www.yamllint.com/*
## 8 Tạo image với cloud-init 
 - Phần này sẽ trình bày một số cách sử dụng cloud init trong việc boot image mà không cần có cloud.
### 8.1 Sử dụng cloud-localds
 - Dùng để tạo disk-image gắn kèm với userdata/metadata.
 - Dùng để tạo một ví dụ đơn giản tại local 
 - Cú pháp chung:
  - $cloud-localds [options] output [user-data-file] [meta-data-file] 
 - Ví dụ
 - Tạo một image dùng backing file, với image Centos tải từ trên trang chủ về
```
qemu-img create -b ~/Downloads/CentOS-7-x86_64-GenericCloud.qcow2 -f qcow2 snapshot-centos7-cloudimg.qcow2 10G
```
 - [Tùy chọn]Rebase lại image về đúng định dạng qcow2
```
sudo qemu-img rebase -F qcow2 -b CentOS-7-x86_64-GenericCloud.qcow2 snapshot-centos7-cloudimg.qcow2 
```
 - [Tùy chọn]Kiểm tra lại
```
qemu-img info snapshot-centos7-cloudimg.qcow2
```
 - Tạo 'user-data' file (lấy từ ví dụ ở trên)
 - [Tùy chọn] Tạo file cấu hình network 'network_config_static.cfg'
```
version: 2
ethernets:
  eth0:
     dhcp4: false
     # default libvirt network
     addresses: [ 192.168.122.152/24 ]
     gateway4: 192.168.122.1
     nameservers:
       addresses: [ 192.168.122.1,8.8.8.8 ]
     search: [ example.com ]
```
 - Tạo disk đính kèm với userdata file
```
cloud-localds ctest1-seed.qcow2 user-data
```
 - [Tùy chọn] Nếu muốn gắn thêm file cấu hình network thì sửa câu lệnh trên như sau
```
cloud-localds -v --network-config=network_config_static.cfg ctest1-seed.qcow2 cloud_init.cfg
```
 - Tạo VM mới với virt-install
```
virt-install --name ctest1 \
  --virt-type kvm --memory 2048 --vcpus 2 \
  --boot hd,menu=on \
  --disk path=ctest1-seed.qcow2,device=cdrom \
  --disk path=snapshot-centos7-cloudimg.qcow2,device=disk \
  --graphics vnc \
  --os-type Linux --os-variant centos7.0 \
  --network network:default
```
*Mặc định thì user login sẽ bị block nếu không có cấu hình 'lock_passwd: false'; root user không có mật khẩu và bị khóa nếu không có cấu hình 'disable_root: false'; ssh phải được sử dụng mật khẩu nếu có 'ssh_pwauth: true'*

### 8.2 Sử dụng geniosimg/mkisofs để tạo seed
 - Với genisoimage
```
$ genisoimage  -output [ISO file] -volid cidata -joliet -rock [user-data-file] [meta-data-file] ...
```
 - Ví dụ 
```
$ genisoimage  -output seed.iso -volid cidata -joliet -rock user-data meta-data
```
*Khi thay đổi user-data file thì phải tạo lại lại file seed.iso và phải xóa đi image cũ, tạo image mới*
 - Lấy seed kia để cài vào image qcow2
```
virt-install --name ctest2   --virt-type kvm --memory 2048 --vcpus 2   --boot hd,menu=on   --disk path=seed4.iso,device=cdrom   --disk path=test5.qcow2,device=disk   --graphics vnc   --os-type Linux --os-variant ubuntu18.04   --network network:default
```
### 8.3 Sử dụng cloud-init với instance có sẵn (no-cloud)
 - Có một guest instance iso, qcow2 đang khởi động sẵn, muốn cài đặt chung cloud-init trên instance này làm mẫu.
 - Tại guest:
 - Tải cloud-init
 - Tạo folder /var/lib/cloud/seed/nocloud-net
  - sudo mkdir -p /var/lib/cloud/seed/nocloud-net
 - Tạo file user-data và meta-data chứa config cloud-init (Có thể tham khảo ở trên)
 - (Tùy chọn) bật log: sửa file vi /etc/cloud/cloud.cfg.d/05_logging.cfg 
  - output: {all: '| tee -a /var/log/cloud-init-output.log'}
 - (Debian/Ubuntu) Kiểm tra loại datasource có tìm kiếm không
  -  sudo dpkg-reconfigure cloud-init 
  - Tích vào NoCloud và None ( Sẽ làm thay đổi file /etc/cloud/cloud.cfg.d/90_dpkg.cfg ) 
 - (Centos) dpkg-reconfigure không có câu lệnh nào tương tự trên Centos. Vì vậy ta sẽ phải cấu hình bằng tay vào file /etc/cloud/cloud.cfg:
 ![image](https://user-images.githubusercontent.com/43545058/101134402-e7cf6b00-363c-11eb-88ae-d6bca73b61d7.png)
  - Thêm các thuộc tính vào trường datasource_list.
  - VỚi dsmode: 
    - pass : Bỏ qua datasource này
    - local hoặc net: tìm đến datasource này là cuối cùng (local khác với net là local không cần mạng để up trước khi thực hiện user-data)
 - cloud-init -d init
 - (Tùy chọn) kiểm tra log tại /var/log/cloud-init-output.log
 - (Tùy chọn) Có thể cần reboot lại
 - (Tùy chọn) Có thể cần systemctl restart cloud-init
 - (Tùy chọn) có thể kiểm tra 
*http://www.whiteboardcoder.com/2016/04/install-cloud-init-on-ubuntu-and-use.html*
 - Cài xong rồi thì chỉ việc copy image qcow2 sang nơi khác để dùng
```
virt-install --name ctest2   --virt-type kvm --memory 2048 --vcpus 2   --boot hd,menu=on  --disk path=test5.qcow2,device=disk   --graphics vnc   --os-type Linux --os-variant ubuntu18.04   --network network:default
```
(không cần seed nữa)
#### 8.3.1 Gỡ cấu hình cloud-init
  - Xóa file user-data/meta-data ở /var/lib/cloud/seed/nocloud-net
  - sudo cloud-init clean
  - sudo cloud-init -d init
 - Tuy nhiên dữ liệu cũ của file cấu hình cloud-init (user-data) vẫn còn (user, thư mục home,...), nên cần phải xóa đi (xóa user,...)
### 8.4 Sử dụng cloud-init với cloud và lấy meta-data bằng URL (phần sau)
http://www.whiteboardcoder.com/2016/04/cloud-init-nocloud-with-url-for-meta.html

 

