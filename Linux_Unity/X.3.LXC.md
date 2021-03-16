## 1. Giới thiệu Linux Container (LXC)
### 1.1 Khaí niệm container
 - Một container là một gói phần mềm bao gồm mọi thứ phần mềm cần thiết để chạy. Điều đó bao gồm các tool hệ thống, thư viện, cấu hình. Các container được cài đặt và chạy cô lập với các container khác và cô lập với hệ điều hành.
### 1.2 Ưu điểm container
 - Với đặc tính trên, container có các ưu điểm sau:
  - Các phần mềm trong container có thể chạy đồng nhất trên nhiều môi trường khác nhau.
  - Container gồm các cơ chế cô lập nên tăng cường bảo mật.
  - Dung lượng nhẹ hơn so với VM, khởi chạy nhanh chóng hơn.
  - Tránh được lỗi phụ thuộc (depencency) các gói cài đặt.
### 1.3 LXC
 - Một Linux container (LXC) là tập của một hoặc nhiều các tiến trình được cô lập với phần còn lại của hệ thống. Tất cả các file cần thiết để chạy mỗi tiến trình trong LXC được cung cấp bởi image riêng biệt, nghĩa là LXC dễ dàng di chuyển và nhất quán từ khâu phát triển, testing, thương mại.
 - LXC do đó giúp nhanh chóng trong việc phát triển sản phẩm, an toàn bảo mật.
![image So sánh kiến trúc VM hypervisor với LXC](https://user-images.githubusercontent.com/43545058/111018768-629ee380-83ed-11eb-8e10-0af55e7ff465.png)
## 2. Công nghệ sử dụng container
### 2.1 Cgroup
( Xem XI.3.Cgroup.md )

### 2.2 SElinux
 - Là một cơ chế giúp phân quyền, cô lập các process Linux theo UID khi truy cập các file, socket hay các tiến trình khác.
### 2.3 namespace
 - Là một cơ chế giúp cô lập các thành phần chung với nhau theo một namespace - chỉ những thành phần có cùng namespace thì mới có thể nhìn thấy nhau. Namespace hiện nay áp dụng với:mount,hostname, domain, IPC, PID, network, UID/GID, cgroup (từ Linux bản 4.6 trở đi)
### LXD
 - Được xây dựng trên nền tảng LXC giúp cung cấp trải nghiệm người dùng tốt hơn. LXD gọi lXC qua liblxc và Go để tạo và quản lý container. Với việc thêm một số tính năng, thì việc sử dụng và triển khai LXD cùng với sẽ tốt hơn so với mình LXC.
## 3. Cài đặt
### 3.1 Tải từ Ubuntu
```
snap install lxd
```
### Khởi tạo
```
lxd init
```
 - Chọn các giá trị mặc định
  - Clustering:[no]
  - Storage pool:[create new]
  - MAAS server:[no]
  - Network bridge:[yes] 
  - Nerwork access:[no]
  - Automatic Image Update:[yes]
  - Print YAML "lxd init"
  



