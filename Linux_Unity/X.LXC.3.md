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
#### 2.1.1 Giới thiệu
 - Là một cơ chế giúp cấp phát các tài nguyên hệ thống cho một hoặc một tập các process như: cấp cpu, ram, io, device, số lượng tối ra process... Ta có thể cấp phát, cấp phát động, quản lý tài nguyên, hạn chế truy cập tài nguyên. Với công cụ này, người quản trị có toàn quyền quản lý, phân phát tài nguyên phần cứng cho các ứng dụng, user, giúp tăng hiệu quả hiệu suất sử dụng.
#### 2.1.2 Mô hình phân cấp
 - Với mô hình cây tiến trình Linux, tiến trình init sẽ được khởi tạo đầu tiên, các tiến trình sau sẽ được khởi tạo và là tiến trình con của tiến trình này. Vì vậy, các tiến trình trong Linux đều có chung 1 gốc (init process) và kế thừa các biến môi trường.
 - Với mô hình cây trên cgroup, cũng với mô hình cây, thừa hưởng các đặc tính từ nhánh cha. Linux có tiến trình gốc là init, còn cgroup có các subsystem gốc, tương ứng với các nguồn tài nguyên của hệ thống. Khác với Linux, các nhánh của cgroup có thể có nhiều subsystem kết nối tới. Danh sách một số subsystem:
  - cpu: Giới hạn tối thiểu/tối đa CPU khi hệ thống bận.
  - cpuacct: Tạo báo cáo tự động về sử dụng CPU trong cgroup.
  - cpuset: Gán process tới CPU hoặc NUMA cố định.
  - memory: Giới hạn và tạo báo cáo sử dụng RAM tiến trình,RAM hệ thống, swap.
  - devices: Cho phép truy cập hay hạn chế process tới thiết bị.
  - freezer: Suspend hoặc resume process.
  - net_cls: Đánh dấu gói tin giúp Linux trafic controller quản lý gói tin từ các process khác nhau.
  - blkio: Giới hạn I/O với thiết bị phần cứng (USB,NIC, storage drive)
  - perf_event: Đánh dấu task để phân tích hiệu năng.
  - net_prio: Đặt priority cho mạng.
  - hugetlb: Giới hạn sử dụng huge page.
  - pids: Giới hạn số lượng tiến trình có thể khởi tạo bởi cgroup.
  - rdma: Giới hạn sử dụng remote direct memory access (RDMA)/IB mỗi cgroup. 
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
  



