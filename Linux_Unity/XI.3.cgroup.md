## 1 Giới thiệu
 - Là một cơ chế giúp cấp phát các tài nguyên hệ thống cho một hoặc một tập các process như: cấp cpu, ram, io, device, số lượng tối ra process... Ta có thể cấp phát, cấp phát động, quản lý tài nguyên, hạn chế truy cập tài nguyên. Với công cụ này, người quản trị có toàn quyền quản lý, phân phát tài nguyên phần cứng cho các ứng dụng, user, giúp tăng hiệu quả hiệu suất sử dụng.
## 2 Mô hình phân cấp
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
## 2.1 Quy tắc phân cấp 
### 2.1.1 
 - Một cây có thể có 1 hoặc nhiều subsystem gốc.
![image](https://user-images.githubusercontent.com/43545058/111249010-996b3880-863d-11eb-8f8f-f1d5e2799f42.png)
### 2.1.2
 - Một subsystem gốc chỉ có thể nối tới một cây thứ hai mới khi chưa có subsystem gốc nào sẵn.
![image](https://user-images.githubusercontent.com/43545058/111249335-23b39c80-863e-11eb-9406-0eb5e14e2c93.png)
### 2.1.3
 - Các task ban đầu được mặc định gắn với root cgroup. Các task có thể được gắn với nhiều cây, với điều kiện chỉ gắn với duy nhất một cgroup trên mỗi cây. Khi lỡ gắn thêm cgroup thứ hai vào task, cgroup đầu tiên sẽ bị loại đi.
![image](https://user-images.githubusercontent.com/43545058/111249865-f5828c80-863e-11eb-8e31-d42239068450.png)
### 2.1.4
 - Khi tạo tiến trình con, thì mặc định các tiến trình con cũng sẽ thuộc các cgroup giống như tiến trình cha. Các tiến trình con thay đổi cgroup là độc lập với tiến trình cha.
![image](https://user-images.githubusercontent.com/43545058/111250548-1ac3ca80-8640-11eb-9ecb-b96a100f1158.png)
## 3 Cài đặt
### Ubuntu 20.04
```
$ sudo apt-get install cgroup-lite cgroup-tools cgroupfs-mount libcgroup1
$ cp /usr/share/doc/cgroup-tools/examples/*.conf /etc/ -v
$ cgconfigparser -l /etc/cgconfig.conf
$ vi /etc/default/grub
# Add GRUB_CMDLINE_LINUX_DEFAULT="cgroup_enable=memory swapaccount=1"
$ sudo update-grub
$ reboot
```
#### Giải thích
  - Tải một số lib cần thiết về
  - Copy file cấu hình mặc định về để test. Xóa đi phần mount tại vì cgroup mount sẵn ở /sys/fs/cgroup/ rồi
  - cgconfigparse -l <file> để đọc cấu hình file
  -...

Nếu lỡ xóa cgroup folder (cgclear), folder chưa được mount:
```
cgclear
cgroups-mount
cgconfigparser -l /etc/cgconfig.conf
```
### Centos (Chưa test)
$ yum install libcgroup
## 4. Cấu hình chính
### 4.1 /etc/cgconfig.conf
 - Gồm có hai phần chính mount và group
#### 4.1.1 mount
 - mount tạo ra các entry, gắn các tài nguyên của hệ thống tới cây này, cú pháp như sau:
```
mount {
    subsystem = sys/fs/cgroup/<ten subsystem>;
    …
}
```
 - Cấu hình kia tương ứng với:
```
mkdir /cgroup/name
mount -t cgroup -o <subsystems> name /cgroup/name
```
 - Thường thì việc mount được thực hiện tự động nên không cần phần này. Tuy nhiên có một số trường hợp cần được mount bằng tay.
### 4.1.2 group
 - Group tạo các cgroup và đặt cấu hình tài nguyên theo quy tắc sau:
```
group <name> {
    [<permissions>]
    <controller> {
        <param name> = <param value>;
        …
    }
    …
}
```
 - Ví dụ: group daemons có cgroup con là ftp, phần permissions (tùy chọn) và cpu được đặt như bên dưới.
```
group daemons/ftp {
        perm {
                task {
                        uid = root;

                }
                admin {
                        uid = root;
                        gid = root;
                }
        }
        cpu {
                cpu.shares = 500;
        }
}
```
### 4.1.2 /etc/cgrules.conf
 - Có hai dạng
  - user subsystems control_group
  - user:command subsystems control_group
 - Quy định người dùng nào (khi sử dụng câu lệnh nào) thì sẽ được vào cgroup <control_group> với subsystem là <subsystems>. Ví dụ
  - maria:ftp			devices		/usergroup/staff/ftp
    - Lỗi bất ngờ khi sử dụng file này
```
Note, however, that the daemon moves the process to the cgroup only after the appropriate condition is fulfilled. Therefore, the ftp process might run for a short time in the wrong group. Furthermore, if the process quickly spawns children while in the wrong group, these children might not be moved.
```
*Xem thêm tại https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/sec-moving_a_process_to_a_control_group/*
## 5. Chỉnh sửa cấu hình
### 5.1 mount point
 - Kiểm tra các mount point: lssubsys -am
 - Thêm các subsystem vào cùng một mount point
```
mkdir /cgroup/name
mount -t cgroup -o <subsystems> name /cgroup/name
```
 - Gỡ subsystem ra khỏi mount point
  - Vẫn thực hiện lệnh mount nhưng mà bỏ bớt <subsystem> cần gỡ đi
 - unmount một cây
  - Dùng umount /folder/ thôi
### 5.2 group
#### 5.2.1 Tạo group mới: cgcreate
 - $cgcreate -t uid:gid -a uid:gid -g subsystems:path
  - -t(tùy chọn): Đặt user/group id quản lý task
  - -a(tùy chọn): Đặt user/group id quản lý phần còn lại
  - -g: Đặt sussystem và tên path, Ví dụ: -g cpu,net_cls,memory:/test; Ở đây test sẽ có 3 sussystem.
#### 5.2.2 Xóa group: cgdelete
 - $cgdelete -g sussystems:path
  - g: Xóa đi những sussystem, những sussystem khác giữ nguyên
 - Cách khác
```
mkdir /cgroup/hierarchy/name/child_name
```
#### 5.2.3 Thêm tham số: cgset
 - $cgset -r <nmae=value> <cgroup-path>
  - cgset -r cpuacct.usage=0 group1
  - cgset --copy-from path_to_source_cgroup path_to_target_cgroup
 - Trực tiếp
```
echo 0-1 > /cgroup/cpu_and_mem/group1/cpuset.cpus
```
#### 5.2.4 Thêm task vào cgroup theo pid: cgclassify
 - cgclassify -g subsystems:path_to_cgroup pidlist
 - Trực tiếp
```
echo 1701 > /cgroup/cpu_and_mem/group1/tasks
```
##### Check task nào đang thuộc cgroup nào
 - $cat /proc/<PID>/cgroup
 - Tìm theo tên
```
#!/bin/bash
THISPID=`ps -eo pid,comm | pgrep $1 -x`
for pid in $THISPID;do
	cat /proc/$pid/cgroup
done
```
*-x là exact match*
#### 5.2.5 Thêm task vào cgroup theo name: cgexec
 - cgexec -g subsystems:path_to_cgroup command arguments
 - Nếu khớp command và arguments thì sẽ được chuyển vào cgroup đó
  - Ví dụ: cgexec -g cpu:group1 firefox http://www.redhat.com 
 - Trực tiếp
```
sh -c "echo \$$ > /cgroup/cpu_and_mem/group1/tasks && firefox"
```
