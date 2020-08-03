## Tìm hiểu ảo hóa và KVM
### 1.Ảo hóa là gì? chức năng, lợi ích của ảo hóa?
 - Ảo hóa là một công nghệ giúp mô phỏng hoạt động phần cứng vật lý của máy tính.
 - Ảo hóa có thể mô phỏng ra nhiều thiết bị vật lý khác nhau, có chức năng, thành phần và hoạt động udocjw mô phỏng như ngoài đời thật.
 - Ảo hóa giúp tối ưu tài nguyên, tăng cường bảo mật, quản trị, phục hồi dữ liệu, kiểm thử...
### 2.Các loại ảo hóa, đặc điểm của từng loại
#### 2.1 Ảo hóa máy tính/hệ điều hành
 - Tạo một máy tính ảo với hệ điều hành ảo trong một ứng dụng với lượng tài nguyên cho sẵn.
 - Đây là loại ảo hóa phổ biến nhất.
 - Được sử dụng để làm lab, giảm bớt tài nguyên vật lý không cần thiết.
#### 2.2 Ảo hóa server
 - Ảo hóa hệ thống máy chủ cho phép ta có thể chạy nhiều máy ảo trên một máy chủ vật lý, đem lại nhiều lợi ích như tăng tính di động, dễ dàng thiết lập với các máy chủ ảo, giúp việc quản lý, chia sẻ tài nguyên tốt hơn, quản lý luồng làm việc phù hợp với nhu cầu, tăng hiệu suất làm việc của một máy chủ vật lý.  Xét về kiến trúc hệ thống, các mô hình ảo hóa hệ  thống máy chủ  có thể  ở  hai dạng sau:
   -  Host-based:  Kiến trúc này sử dụng một lớp hypervisor chạy trên nền tảng hệ điều hành, sử dụng các dịch vụ được hệ điều hành cung cấp để phân chia tài nguyên tới các máy ảo. Ta xem hypervisor này là một lớp phần mềm riêng biệt, do đó các hệ điều hành khách của máy ảo sẽ nằm trên lớp hypervisor rồi đến hệ điều hành của máy chủ và cuối cùng là hệ thống phần cứng…  Một số hệ thống hypervisor dạng Hosted có thể kể đến như VMware Server, VMware Workstation, Microsoft Virtual Server… 
    -  Hypervisor-based:  hay còn gọi là bare-metal hypervisor. Trong kiến trúc này, lớp phần mềm hypervisor chạy trực tiếp trên nền tảng phần cứng của máy chủ, không thông qua bất kì một hệ điều hành hay một nền tảng nào khác. Qua đó, các hypervisor này có khả năng điều khiển, kiểm soát phần cứng của máy chủ. Đồng thời, nó cũng có khả năng quản lý các hệ điều hành chạy trên nó.  Nói cách khác, các hệ điều hành sẽ nằm trên các hypervisor dạng bare-metal rồi đến hệ thống phần cứng. Một số ví dụ về các hệ thống Bare-metal hypervisor như là Oracle  VM, VMware ESX Server, IBM's POWER Hypervisor, Microsoft's Hyper-V, Citrix XenServer…
#### 2.3 Ảo hóa ứng dụng
 - Ảo hóa ứng dụng là một dạng công nghệ  ảo hóa khác cho phép chúng ta tách rời mối liên kết giữa  ứng dụng và hệ  điều hành và cho phép phân phối lại  ứng dụng phù hợp với nhu cầu user. Một ứng dụng được  ảo hóa sẽ  không được cài đặt lên máy tính một cách thông thường,  mặc dù ở góc độ người sử dụng, ứng dụng vẫn  hoạt động một cách bình  thường. Việc quản lý việc cập nhật phần mềm trở nên  dễ dàng hơn, giải quyết sự đụng độ giữa các ứng dụng và việc thử nghiệm sự  tương thích của chúng cũng trở nên dễ dàng hơn.
 -  Application Streaming: ứng dụng được chia thành nhiều đoạn mã và được truyền sang máy người sử dụng khi cần đến đoạn mã  đó. Các đoạn mã này thường được đóng gói và truyền đi dưới giao thức HTTP, CIFS hoặc RTSP   
 -  Desktop Virtualization/Virtual Desktop Infrastructure (VDI): ứng dụng sẽ được cài đặt và chạy trên một máy ảo. Một hạ tầng quản lý sẽ tự đông tạo ra  các desktop ảo và cung cấp các desktop ảo này đến các đối tượng sử dụng. Hiện tại, Viettel IDC đang cung cấp dịch vụ VDI, mang tên dịch vụ máy tính ảo Cloud PC.
#### 2.4 Ảo hóa hệ thống mạng
 - Ảo hóa các thiết bị mạng với nhau thành một mạng lưới mạng ảo với chức năng tương tự
#### 2.5 Ảo hóa phần cứng
 - TƯơng tự với ảo hóa hệ điều hành nhưng không cần OS.
#### 2.6 Ảo hóa hệ thống lưu trữ
 - Ảo hóa các máy chủ quản lý bởi hệ thống lưu trữ ảo.
#### 2.7 Ảo hóa hệ thống quản trị
 - Chủ yếu được sử dụng wor data center. Mục tiêu chính của nó kaf các admin có quyền hạn qua các group khác nhau và có vai trò khác nhau.
### 3.Phân loại các loại ảo hóa hiện có như VmWare ESXi, Xen server, KVM, Vmware WorkStation, Oracle Virtuabox,...
#### 3.1 Hyper-V
 - Phát hành bởi Microsoft, Hyper-V cung cấp nhiều tính năng, tương thích với Window Server và nhiều hệ điều hành khác.
#### 3.2 KVM
 - KVM (Kernel-based Virtual Machine), là một phần của Red Hat Virtualization Suite, là một giải pháp ảo hóa hoàn hiện. KVM chuyển nhân Linux thành hypervisor và được gộp chung với nhân Linux từ phiên bản 2.6.20 trở đi.
#### 3.3 vSphere
 - VMware có một tập hợp các sản phẩm và giải pháp ảo hóa, được gọi là vSphere, và trong số đó giải pháp hypervisor của họ là VMware ESXi.
#### 3.4 Virtualbox
 - Virtualbox là một host hypervisor mã nguồn mở phát triển bởi Oracle với mục đích sử dụng cho cá nhân là chủ yếu.
#### 3.5 Xen Server
 - Dựa theo Xen Project Hypervisor,XenServer là một nền tảng ảo hóa máy chủ mã nguồn mở (bare-metal), ban đầu được phát triển bởi đại học Cambridge, sau đó được phát triển bởi Linux Foundation với hỗ trợ từ Intel.
### 4.KVM là gì và để làm gì? KVM thuộc loại ảo hóa nào? So sánh KVM với các loại hypervisor còn lại
 - KVM ra đời phiên bản đầu tiên vào năm 2007 bởi công ty Qumranet tại Isarel, KVM được tích hợp sẵn vào nhân của hệ điều hành Linux bắt đầu từ phiên bản 2.6.20. Năm 2008, RedHat đã mua lại Qumranet và bắt đầu phát triển, phổ biến KVM Hypervisor.

 - KVM (Kernel-based virtual machine) là giải pháp ảo hóa cho hệ thống linux trên nền tảng phần cứng x86 có các module mở rộng hỗ trợ ảo hóa (Intel VT-x hoặc AMD-V).
 - KVM có ưu điểm so với các hypervisor khác là mã nguồn mở, sử dụng miễn phí, tiêu thụ tài nguyên ít và quản lý tài nguyên linh hoạt nhờ sử dụng trên các ưu điểm ủa nhân Linux.
### 5.Các thành phần trong KVM và cách hoạt động, mối quan hệ của KVM với OS là như thế nào?
#### 5.1 Sơ lược kiến trúc KVM
  - Host: PC vật lý.
  - Guest: PC ảo.
 - KVM: Một module trong nhân kernel có chức năng ảo hóa và lập lịch CPU và RAM.
 - Qemu: Một ứng dụng hypervisor loại 2.
 - KVM khi thực hiện ảo hóa: Không có đủ công cụ để thực hiện một giải pháp hoàn thiện.
 - Qemu khi thực hiện ảo hóa: Có thể hoạt động tương tự như các hypervisor loại 2 khác như Virtualbox, nhưng quá trình xử lý rất chậm.
 - KVM đi kèm với Qemu: Qemu hỗ trợ rất nhiều tương thích với KVM trên Linux, tận dụng được tốc độ và ngắn gọn trong việc lập lịch CPU và RAM của KVM, thay vào đó Qemu chỉ cần ảo hóa phần cứng. Bộ đôi này thường đi kèm với nhau và được áp dụng phổ biến hiện nay (qemu-kvm).
 - KVM-Qemu được quản lý bởi libvirt: libvirt là một API mã nguồn mở, là một deamon giúp dễ dàng quản lý, quản trị hơn.
 ![image](https://user-images.githubusercontent.com/43545058/88757203-b24ada80-d18f-11ea-80b3-ad6ee8ede9ab.png)
 <img src = "http://imgur.com/JevJgt7.jpg">  
#### CPU trong kvm
 - Qemu là một process của host, nhờ có SeLinux nên các process khác của host không thể truy cập được vào process của Qemu.
 - Trong Qemu, KVM tạo ra các vCPU cho khác Guest, mỗi một vCPU này tương ứng với một process con của Qemu, nên được CPU của host xử sự như các process bình thường khác. 
#### Network trong kvm
 - kvm có thể tạo một mạng riêng ảo, sử dụng mạng NAT(sermode) hoặc mạng Bridge để kết nối ra ngoài.
#### Memory trong kvm
![image](https://user-images.githubusercontent.com/43545058/88766451-4160ee00-d1a2-11ea-9103-1c8aa4aa7fcf.png)
 - Guest RAM được cấp phát khi Qemu khởi động, hoặc được cấp phát mới khi một tiến trình.
 - Guest có chứa một page table riêng được ánh xạ qua host page table, từ đó host page table nhận biết được và ánh xạ tới RAM vật lý, đây là phương pháp chủ yếu được sử dụng trên các nền tảng ảo hóa.
### 6.Tìm hiểu các chế độ card mạng trong kvm
 - Private Bridge:
  - Chế độ này sẽ sử dụng một bridge riêng biệt để các VM giao tiếp với nhau mà không ảnh hưởng tới địa chỉ của KVM host.
  - Ta có thể tạo ra private bridge bằng cách chỉnh sửa file /etc/network/interfaces. Tại đây, bạn sẽ không cần phải comment các cấu hình của card vật lý đồng thời không cần thêm tham số bridge_ports cho bridge.
  - Khi tạo máy ảo và kết nối tới private bridge, các máy ảo sẽ được cấp phát địa chỉ theo dải IP mà người dùng chọn. Chúng có thể giao tiếp với nhau những không đi ra được internet.
  - Một máy ảo có thể được kết nối tới nhiều các private bridge, nhờ vậy nó có thể giao tiếp với nhau.
 - Public Bridge:
  - Chế độ này sẽ cho phép các máy ảo có cùng dải mạng vật lí với card mạng thật. Để có thể làm được điều này, bạn cần thiết lập 1 bridge và cho phép nó kết nối với cổng vật lí của thiết bị thật (eth0).
  - Sau khi đã cấu hình public bridge, khi tạo máy ảo, bạn chỉ cần chọn chế độ "Bridge br0" là các máy ảo sẽ tự động được nhận các địa chỉ ip trùng với dải địa chỉ của card vật lí.
Cơ chế cấp DHCP cho các máy ảo sẽ do Router bên ngoài đảm nhận, nhờ vậy nên các VM mới có dải địa chỉ ip trùng với card vật lí bên ngoài.
 - Nat:
  - Đây là cấu hình card mạng mặc định của KVM. Cơ chế NAT sẽ cấp cho mỗi VM một địa chỉ IP theo dải mặc định, nó sẽ hoạt động giống với một chiếc Router, chuyển tiếp các gói tin giữa những lớp mạng khác nhau trên một mạng lớn. Về mặt logic, ta có thể hiểu nó là 1 bridge riêng biệt và nó sẽ giao tiếp với bridge mà card mạng thật kết nối để các máy ảo có thể kết nối ra bên ngoài mạng internet. Chúng ta có thể xem dải mạng mặc định mà cơ chế NAT sẽ cấp cho các máy ảo ở trong file:
/var/lib/libvirt/network/default.xml
### 7.Phân biệt rõ ràng 3 định dạng raw, qcow2 và iso, tìm hiểu ngữ cảnh ứng dụng và tính năng của chúng trong kvm. Phân biệt thin và thick provisioning. Test performance của raw và qcow2

|Thin provisioning|Thick provisioning|
|---|---|
|Dung lượng không được cấp phát ngay mà sẽ được mở rộng theo thời gian|Dung lượng được cấp phát ngay lúc khởi tạo, có thể được format lại toàn bộ (gán bit 0)|
|Thời gian khởi tạo nhanh|Khởi tạo ban đầu chậm hơn|
|Tiết kiệm không gian bộ nhớ|Được cấp phát cứng ngay từ đầu|
|Có rủi ro khi quản lý bộ nhớ không tốt dẫn đến tràn bộ nhớ|Ít cần sự quản lý hơn|
|Dễ dàng mở rộng hay rút gọn dung lượng ổ cứng|Khi rút gọn ổ cứng cần copy dữ liệu ra ngoài và shutdown server|
|Tốc độ sẽ chậm hơn|Tốc độ ghi nhanh hơn|

  - Xét đến các yếu tố ưu/nhược và ý kiến cộng đồng thời gian gần đây, có sự ưu tiên lớn hơn dành cho việc sử dụng thin provisioning bởi các ưu điểm và những nhược điểm được giảm thiểu dần qua thời gian.

#### Định dạng lưu image trong KVM
  - File Image (còn gọi là file ảnh) của đĩa CD/DVD chính là một dạng file có định dạng theo các chuẩn tạo file ảnh. File image là một file đóng gói hết tất cả nội dung của một đĩa CD/DVD vào trong nó.
  - Trong KVM Guest có 2 thành phần chính đó là VM definition được lưu dưới dạng file xml tại /etc/libvirt/qemu. File này chứa các thông tin của máy ảo như tên, thông tin về tài nguyên của VM (RAM, CPU)… File còn lại là storage thường được lưu dưới dạng file image tại thư mục /var/lib/libvirt/images.
  - 3 định dạng thông dụng nhất của file image sử dụng trong KVM đó là ISO, raw và qcow2
 - File iso:
  - File ISO là file ảnh của 1 đĩa CD/DVD, nó chứa toàn bộ dữ liệu của đĩa CD/DVD đó. File ISO thường được sử dụng để cài đặt hệ điều hành vủa VM, người dùng có thể import trực tiếp hoặc tải từ trên internet về.
  - Boot từ file ISO cũng là một trong số những tùy chọn mà người dùng có thể sử dụng khi tạo máy ảo.
 - File raw :
  - Là định dạng file image phi cấu trúc
  - Khi người dùng tạo mới một máy ảo có disk format là raw thì dung lượng của file disk sẽ là đúng bằng dung lượng của ổ đĩa máy ảo bạn đã tạo
  - Định dạng raw là hình ảnh theo dạng nhị phân (bit by bit) của ổ đĩa.
  - Mặc định khi tạo máy ảo với virt-management hoặc không khai báo khi tạo VM bằng virt-install thì định dạng ổ đĩa sẽ là raw. Hay nói cách khác, raw chính là định dạng mặc định của QEMU.
 - File qcow2
  - qcow là một định dạng tập tin cho đĩa hình ảnh các tập tin được sử dụng bởi QEMU , một tổ chức màn hình máy ảo . Nó viết tắt của “QEMU Copy On Write ” và sử dụng một chiến lược tối ưu hóa lưu trữ đĩa để trì hoãn phân bổ dung lượng lưu trữ cho đến khi nó thực sự cần thiết. Các tập tin trong định dạng qcow có thể chứa một loạt các hình ảnh đĩa thường được gắn liền với khách cụ thể các hệ điều hành . Hai phiên bản của các định dạng tồn tại: qcow, và qcow2, trong đó sử dụng các .qcow và .qcow2 mở rộng tập tin, tương ứng.
  - Qcow2 là một phiên bản cập nhật của định dạng qcow, nhằm để thay thế nó. Khác biệt với bản gốc là qcow2 hỗ trợ nhiều snapshots thông qua một mô hình mới, linh hoạt để lưu trữ ảnh chụp nhanh. Khi khởi tạo máy ảo mới sẽ dựa vào disk này rồi snapshot thành một máy mới
  - Qcow2 hỗ trợ copy-on-write với những tính năng đặc biệt như snapshot, mã hóa ,nén dữ liệu…
  - Copy on write (cow) Copy-on-write ( COW ), đôi khi được gọi là chia sẻ tiềm ẩn, là một kỹ thuật quản lý tài nguyên được sử dụng trong lập trình máy tính để thực hiện có hiệu quả thao tác “nhân bản” hoặc “sao chép” trên các tài nguyên có thể thay đổi. Nếu một tài nguyên được nhân đôi nhưng không bị sửa đổi, không cần thiết phải tạo một tài nguyên mới; Tài nguyên có thể được chia sẻ giữa bản sao và bản gốc. Sửa đổi vẫn phải tạo ra một bản sao, do đó kỹ thuật: các hoạt động sao chép được hoãn đến việc viết đầu tiên. Bằng cách chia sẻ tài nguyên theo cách này, có thể làm giảm đáng kể lượng tiêu thụ tài nguyên của các bản sao chưa sửa đổi.
  - Qcow2 hỗ trợ việc tăng bộ nhớ bằng cơ chế Thin Provisioning (Máy ảo dùng bao nhiêu file có dung lượng bấy nhiêu).
*https://people.gnome.org/~markmc/qcow-image-format.html*
### 8.Dựng LAB và kiểm chứng thông tin.
#### 8.1 Cài đặt yêu cầu
 - Cài trước các gói phụ trợ: yum install qemu-kvm bridge-utils virt-manager virt-viewer
  - qemu-kvm: Phần phụ trợ cho KVM.
  - libvirt: Cung cấp libvirt mà bạn cần quản lý qemu và KVM bằng libvirt.
  - bridge-utils: chứa một tiện ích cần thiết để tạo và quản lý các thiết bị bridge.
  - virt-manager: cung cấp giao diện đồ họa để quản lý máy ảo.
  - virt-viewer: hiện thông tin lúc install.
#### 8.2 Tạo máy ảo đơn giản
  - $virt-install \
  - $--name=centos7 \	Tên máy ảo
  - $--vcpu=1 \	Lượng cpu ảo cho guest
  - $--memory=1024 \	Lượng ram ảo cho guest(MB)
  - $--cdrom=/path/to/IOS_file \	Địa chỉ file ios để boot
  - $--disk=/path/to/locate/guest_vm.qcow2,size=5,format=qcow2 \	Địa chỉ để lưu lại image guest-vm;dung lượng disk(GB); kiểu định dạng image
  - $--os-variant=centos7.0 \	Loại OS đang tạo, xem chi tiết bằng lệnh $osinfo-query os
  - $--network bridge=virbr0 \	Loại mạng mà máy ảo sử dụng
  - &--extra-args='console=ttyS0' hoặc tty0	Dùng để  hiện thông tin máy ảo qua terminal gồm các bước cài đặt và terminal cảu máy ảo.
#### Đăng nhập máy ảo
  - $virsh console [vm-name]
### 9.Sử dụng các công cụ để thao tác với KVM: CLI, Virt-manager, ... Các thao tác chính để tương tác với KVM hay máy ảo trên KVM.
#### 9.1 CLI
Chúng ta sẽ sử dụng lệnh $virsh để quản lý máy ảo ($virsh --help)  
 - $virsh help domain
  - $virsh list [--all/name/inactiv...]	Hiện danh sách domain (đi kèm với các thuộc tính)
  - create < XML-file > [--console/paused..]	Tạo một VM mới từ file cấu trúc xml
  - define ...	Tạo một VM mới từ XML file
  - destroy	Hủy vm
  - detach/attach-device/disk	Hủy,gắn phần cứng vào vm
  - edit [vm-name]	Chỉnh sửa file XML của VM
  - migrate	Chuyển vm sang host khác
  - reboot/reset/pause/resume/shutdown/restart/suspend
  - restore/save [domaain] [file]	Lưu hoặc tải lại trạng thái của vm
 
 - $virsh help monitor	Dùng để hiển thị các lệnh hiện thông tin chung về domain
 - $virsh help host	Dùng để hiện các thông tin về CPU,memory
 - $virsh help interface Hiện thông tin các lệnh về interface 
 - $virsh help filter	Hiện các lệnh về network filter
 - $virsh help network	Hiện các lệnh về network
  - net-list
  - net-define/create [xml-file]	Tạo mới network
  - net-autostart/destroy/edit/start/undefine
 - $virsh help nodedev	Hiển thị các lệnh devide
 - $virsh help secret	Hiển thị các lệnh secret để tạo mã bí mật cho mã hóa hoặc sử dụng trên các thành phần vm(volume,ceph...)
 - $virsh help snapshot	Hiện các lệnh với snapshot
  - snapshot-create/create-as/edit/delete	Sử dụng chung
  - snapshot-list/info/parent/dumpxml	Lấy thông tin
  - snapshot-revert
 - $virsh help pool	Hiển thị các lệnh về storage pool - Tập hợp các volume do cả admin quản lý và cả được đăng ký bởi vm
 - $virsh help volume	Các lệnh với volume
#### 9.2 Virt-manager
##### Cài đặt trước
 - Kiểm tra hypervisor có hỗ trợ giao diện màn hình không (Centos 7): $yum group list
  - Nếu có hai phần tử là Server with GUI và GNOME Desktop thì là có hỗ trợ.
 - Cài đặt: 
  - yum groupinstall "GNOME Desktop" "Graphical Administration Tools"
  - yum groupinstall "Server with GUI"
 - Bật Desktop GUI: ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target
 - Tải về virt-manager: yum install virt-manager -y
  - Sau đó reboot hệ thống.
  
##### Quản lý VM với virt-manager
 - Tab file: 
  - Add Connection:kết nối với hypervisor khác
  - New VM: Tạo VM mới
 - Tab Edit: Xem thông số, cài đặt và chỉnh sửa VM network, storage, network interface
 - Tab View: Xem thông số guest.
 - Chuột phải vào VM: dùng để start/pause/resume/delete/migrate/clone VM.
### 10.Tìm hiểu sâu các option khi tạo VM (của CPU, Disk, NIC)
#### 10.1 CPU
 - --vcpus=VCPUS[,maxvcpus=MAX][,sockets=#][,cores=#][,threads=#]	Lựa chọn số vCPU mà guest sẽ dùng. Nếu maxvcpu được chỉ ra, guest có thể hotplug tới MAX vcpu khi guest đang chạy, nhưng ban đầu thì chỉ với VCPU.
 - --cpuset=[CPUSET,]	Chỉ định cpu nào sẽ được dùng. Nếu giá trị là auto, thì virt-install sẽ dựa vào dữ liệu NUMA dể quyết định (xem phần 10.5)
 - --cpu MODEL[,+feature][,-feature][,match=MATCH][,vendor=VENDOR]	Sử dụng,lựa chọn đặc tính nào được sử dụng đối với guest.VD:
  - --cpu host	Hiện cấu hình CPU của host cho guest
  - --cpu core2duo,+x2apic,disable=vmx	Hiện mẫu core2duo, bật tính năng x2apic, nhưng ko bật vmx.
#### 10.2 Disk
 - --disk=DISKOPTS	Lựa chọn disk cho guest.
  - path=[PATH]	Liên kết đến phương tiện lưu trữ, có thể là file hoặc thiết bị block.
  - pool	Chỉ tới một storage pool. cần có thêm giá trị size đi kèm
  - vol	Chỉ tới một storage volume. Được xác định như 'poolname/volname'
  - devide	Chỉ định kiểu thiết bị (disk,cdrom,floppy),mặc định là disk.
  - bus	Chỉ định kiểu bus (ide,cdrom,disk,virio,xen). Nên chọn là virtio, đây là kiểu hỗ trợ ảo hóa .
  - perms	Disk permission (rw,ro,sh-shared read/write). Mặc định là rw.
  - size=SIZE	số lượng GB khi tạo storage mới.
  - sparse	Có bỏ qua quá trình truy cập thử toàn bộ ổ đĩa tạo mới này không (true/false). Mặc định là true, giúp tăng thời gian khi cài đặt nhưng có thể có xảy ra lỗi truy cập I/O khi sử dụng.
  - cache	Mode cache nào được sử dụng
  - format	Định dạng của storage (raw,qcow2,vmdk...), nên để qcow2
  - error_policy	Cơ chế khi xảy ra lỗi ghi xảy ra (stop,none,enospace)
  - type	Loại thư mục (mount,template). Mặc định là type.
  - source	Thư mục host được chia sẻ
  - target	Thư mục mount để sử dụng cho guest.
#### 10.3 Network
 - --network=NETWORK,opt1=val1,opt2=val2	Cấu hình mạng
 - NETWORK có thể nhận 3 giá trị sau
  - bridge=BRIDGE	Kết nối tới thiết bị bridge tên BRIDGE. Được dùng khi host được cấu hình tĩnh, và có sử dụng live migration
  - network=NAME	Kết nối tới mạng ảo của host tên NAME, sau đó sử dụng $virsh để cấu hình và quản lý. Sử dụng cho guest cấu hình động, NAT, không dây.
  - user	Kết nối tới LAN sử dụng SLIRP. Chỉ sử dụng nếu đang chạy QEMU guest như người dùng không có đặc quyền.
  - Nếu không có thì virt-install chọn default, cấu hình tốn hơn.
 - model	Chỉ định loại thiết bị mạng (e1000,virtio,rtl8139...).
 - mac	Chỉ định địa chỉ MAC, mặc định là ngẫu nhiên, với QEMU hoặc KVM sẽ bắt đầu bới '52:54:00'.
 
#### 10.4 NIC
 - --serial type,opt1=val1...	Các thiết bị serial kết nối với guest
  - dev,path=HOSTPATH	thiết bị serial, có thể là /dev/ttyS0.
  - file,path=FILENAME	output ra FILENAME
  - pipe,path=PIPEPATH	Xem man 7 p
  - tcp,host=HOST:PORT,mode=MODE,protocol=PROTOCOL	Thiết lập kết nối console tcp
    - HOST mặc định là 127.0.0.1, PORT thì cần thiết.
    - MODE là bind (đợi kết nối) hoặc connect (gửi output qua HOST:PORT)
    - PROTOCOL là raw hoặc telnet
  - udp,host=HOST:PORT,bind_host=BIND_HOST:BIND_PORT	Thiết lập kết nối console udp
  
#### 10.5 RAM
 - --numatune=NODESET,[mode=MODE]	Chỉ định node NUMA và mode, xem thêm ở 'man 8 numactl'
*https://www.admin-magazine.com/Archive/2014/20/Best-practices-for-KVM-on-NUMA-servers để tìm hiểu thêm về NUMA+KVM*
#### 10.6 Graphic
 - --graphic TYPE,opt1=arg1...	Lựa chọn giao diện có thể kết nối với guest
 - type	Mặc định là vnc. Ngoài ra có thể là sdl (SLD window) hoặc spice.
 - port	Chỉ định port (sử dụng bởi vnc và spice), 5900 trở lên.
 - listen	Chỉ định kết nối lắng nghe cảu VNC/Spice. Mặc định là localhost.
 - password	Chỉ định password khi muốn kết nối
 - passwordvalidto	Chỉ định thời gian hết hạn password, format là YYYY-MM-DDTHH:MM:SS
#### 10.7 Cài đặt khác
 - --connect=CONNECT	Kết nối tới hipervisor. Có thể là qemu:///system (nếu chạy trên nhân với quyền root) hoajcwqemu:///session (chạy trên nhân với quyền không phải root).
 - --debug Để debug khi cài đặt vm
 - --description	Mô tả về vm.
 - --location=LOCATION	Nguồn để cài đặt image, có thể là một địa chỉ thư mục, một NFS server (nfs://host/path) hoặc http,ftp server
 - --cdrom=CDROM	File hoặc thiết bị được sử dụng như thiết bị CD-ROM ảo. Có thể được dùng cho một IOS image hoặc URL có chứa IOS image.
*--cdrom cũng có thể dùng để boot nhưng mà nên dùng --location trong phần lớn trường hợp*
 - --pxe	Sử dụng giao thức pxe để boot
 - --init=INITPATH	Đường dẫn tệp nhị phân để guest khởi tạo.
 - --extra-args=EXTRA	Thêm các thông số khác. Ví dụ như '--console=ttyS0'
 - --os-type=OS-TYPE	Đặt thuộc tính này để cấu hình HĐH cho guest tốt hơn. Xem ở '--os-variant os'
 - --boot BOOTOPTS	Lựa chọn cấu hình riêng khi boot guest sau khi cài đặt.
*https://linux.die.net/man/1/virt-install*
### 11.Các tính năng của KVM đối với máy ảo: Tạo - xóa - sửa, sao lưu, clone, snapshot... Lab kiểm chứng các tính năng, ví dụ snapshot trên máy ảo qcow2.
#### 11.1 Tạo máy ảo
*Xem lại ở 8.2*
#### 11.2 Xóa
##### CLI
 - $virsh undefine [domain-name]	Nếu host đang tắt, thì xóa hoàn toàn cấu hình, nếu host đang bật thì chuyển thành domain tạm thời (nếu reboot sẽ mất cấu hình).
  - --managed-save	Các image saved cũng sẽ được xóa
  - --snapshot-metadata/--delete-snapshots	Các snapshot cũng sẽ được xóa (Chưa phân biệt được 2 cái)
  - --remove-all-storage	Các storage cũng sẽ được xóa
  - --wipe-storage	Vừa xóa storage vừa tẩy nó đi luôn
*https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_deployment_and_administration_guide/sect-virsh-delete*
 - 
#### 11.3 Sửa
##### RAM
##### Storage
##### Driver
#### 11.4 Sao lưu
#### 11.5 Clone
#### 11.6 Snapshot
### 12.Phân tích đường đi gói tin với 3 chế độ card mạng, nó đi qua các point nào ? (vẽ layout)
### 13.Làm rõ đường đi gói tin khi sử dụng chế độ NAT. Không cần tạo dải bridge thì máy ảo có ra ngoài được không, nó sẽ đi qua đâu, đâu là thành phần cấp dhcp cho máy ảo.
### 14.Kiểm tra switch ảo với card thật có liên quan gì đến nhau
### 15.Trả lời các câu hỏi máy ảo được lưu ở đâu, mỗi thư mục của máy ảo có các file như thế nào, làm rõ thông tin trong từng file.
### 16.Tìm hiểu về file XML của máy ảo, của network, cách tạo máy ảo, network mới bằng file xml
### 17.Chỉnh sửa cấu hình máy ảo bằng file XML
### 18.Các công nghệ liên quan tới KVM, ví dụ về network (linux bridge OpenvSwitch),  về storage ....
### 19.Tìm hiểu virsh client, các option thông dụng của virsh command
### 20.Kĩ thuật migrate VM, cách restore lại một VM trong trường hợp máy ảo bị chết.
### 21.Tạo và cấp dhcp từ hypervisor cho guest
#### 21.1 
