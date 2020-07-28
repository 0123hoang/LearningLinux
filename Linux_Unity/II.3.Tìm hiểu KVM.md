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
 - Về bản chất, KVM không thực sự là một hypervisor có chức năng giải lập phần cứng để chạy các máy ảo. Chính xác KVM chỉ là một module của kernel linux hỗ trợ cơ chế mapping các chỉ dẫn trên CPU ảo (của guest VM) sang chỉ dẫn trên CPU vật lý (của máy chủ chứa VM). Hoặc có thể hình dung KVM giống như một driver cho hypervisor để sử dụng được tính năng ảo hóa của các vi xử lý như Intel VT-x hay AMD-V, mục tiêu là tăng hiệu suất cho guest VM.

 - KVM hiện nay được thiết kế để giao tiếp với các hạt nhân thông qua một kernel module có thể nạp được. Hỗ trợ một loạt các hệ thống điều hành máy guest như: Linux, BSD, Solaris, Windows, Haiku, ReactOS và hệ điều hành nghiên cứu AROS. Sử dụng kết hợp với QEMU, KVM có thể chạy Mac OS X.

 - Trong kiến trúc của KVM, Virtual machine được thực hiện như là quy trình xử lý thông thường của Linux, được lập lịch hoạt động như các scheduler tiểu chuẩn của Linux. Trên thực tế, mỗi CPU ảo hoạt động như một tiến trình xử lý của Linux. Điều này cho phép KVM được hưởng lợi từ tất cả các tính năng của nhân Linux.
 <img src = "http://imgur.com/JevJgt7.jpg">  
 Kiến trúc KVM
### 6.Tìm hiểu các chế độ card mạng trong kvm
### 7.Phân biệt rõ ràng 3 định dạng raw, qcow2 và iso, tìm hiểu ngữ cảnh ứng dụng và tính năng của chúng trong kvm. Phân biệt thin và thick provisioning. Test performance của raw và qcow2
### 8.Dựng LAB và kiểm chứng thông tin.
### 9.Sử dụng các công cụ để thao tác với KVM: CLI, Virt-manager, ... Các thao tác chính để tương tác với KVM hay máy ảo trên KVM.
### 10.Tìm hiểu sâu các option khi tạo VM (của CPU, Disk, NIC)
### 11.Các tính năng của KVM đối với máy ảo: Tạo - xóa - sửa, sao lưu, clone, snapshot... Lab kiểm chứng các tính năng, ví dụ snapshot trên máy ảo qcow2.
### 12.Phân tích đường đi gói tin với 3 chế độ card mạng, nó đi qua các point nào ? (vẽ layout)
### 13.Làm rõ đường đi gói tin khi sử dụng chế độ NAT. Không cần tạo dải bridge thì máy ảo có ra ngoài được không, nó sẽ đi qua đâu, đâu là thành phần cấp shcp cho máy ảo.
### 14.Kiểm tra switch ảo với card thật có liên quan gì đến nhau
### 15.Trả lời các câu hỏi máy ảo được lưu ở đâu, mỗi thư mục của máy ảo có các file như thế nào, làm rõ thông tin trong từng file.
### 16.Tìm hiểu về file XML của máy ảo, của network, cách tạo máy ảo, network mới bằng file xml
### 17.Chỉnh sửa cấu hình máy ảo bằng file XML
### 18.Các công nghệ liên quan tới KVM, ví dụ về network (linux bridge OpenvSwitch),  về storage ....
### 19.Tìm hiểu virsh client, các option thông dụng của virsh command
### 20.Kĩ thuật migrate VM, cách restore lại một VM trong trường hợp máy ảo bị chết.
