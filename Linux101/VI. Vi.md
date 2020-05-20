## 1.Tìm hiểu cú pháp, sử dụng cơ bản với vi
### 1. Giới thiệu vi
Vi là một trình soạn thảo xuất hiện từ sớm trên các hệ điều hành Unix. Giống như các trình soạn thảo khác, vi có rất nhiều chức năng giúp hỗ trợ cho việc soạn thảo .
### 2. Bắt đầu với vi
#### 1. Sử dụng file với vi
- $vi [FILE]	Sử dụng file với vi
  - -r	Dùng để recover lại file khi đang sửa mà hệ thống bị lỗi
- Trong vi, khi muốn sử dụng các câu lệnh, thì ta gõ Esc để thoát mode chỉnh sửa file.
#### 2. Thoát
  - :x	Thoát và lưu
  - :w	Lưu
  - :q	Thoát
  - :q! Thoát và không lưu
  - :w [FILE] Lưu trên file khác
  - :w! [FILE] Lưu trên file đã tồn tại sẵn
### 3. Di chuyển trong file với vi
#### 1. Di chuyển với con trỏ
  - h	Sang trái
  - j	Xuống dưới
  - k	Lên trên
  - l	Sang phải
*Dùng phím mũi tên cũng được*
#### 2. Di chuyển trên dòng
  - b	Sang trái một từ
  - w	Sang phải một từ
*Dùng ctrl+ phím mũi tên cũng được*
#### 3. Di chuyển trên file
  - nG	Đến dòng thứ G
  - G	Đến dòng cuối cùng
  - (	Đến đầu đoạn văn
  - ) 	Đến cuối đoạn văn
  - ``	Trở lại chỗ con trỏ trước đó
### 4. Chỉnh sửa trong file với vi
#### 1. Chèn 
  - i	Chèn bắt đầu tại điểm con trỏ
  - o	Tạo thêm dòng mới và bắt đầu chèn vào đó
#### 2. Sửa lại
  - ~	Đổi qua lại giữa in hoa in thường
  - cw	Sửa từ hiện tại
  - ncw	Sửa n từ tiếp theo
  - S	Sửa dòng hiện tại
#### 3. Xóa
  - x	Xóa kí tự ở bên phải con trỏ
  - X	Xóa kí tự ở bên trái con trỏ
  - 3dw	Xóa 3 từ ở bên phải con trỏ
  - d0	Xóa đến đầu dòng
  - d$	Xóa đến cuối dòng
  - dd xóa dòng hiện tại
  - 3dd xóa 3 dòng tiếp theo
  - d{ xóa đến đầu đoạn
  - d} xóa đến cuối đoạn 
  - :1,$ d	Xóa toàn bộ file
  - a3dd	Xóa 3 dòng tiếp
#### 4. Đánh dấu
 - Một chức năng khá thú vị của vi, giúp đánh dấu những điểm quan trọng cần phải xem xét nhiều lần trong file (cắc điểm đánh dấu này không được lưu lại)
  - ma	Đánh dấu điểm đó bằng nhãn a, có thể thay bằng a-z
  - `a	Đi đến điểm đánh dấu a
  - `[	và `]	Di chuyển giữa các điểm đánh dấu
#### 5. Copy và paste
  - Y hoặc yy	Copy dòng hiện tại vào buffer
  - 3yy	Copy 3 dòng tiếp theo vào buffer
  - y$	Copy mọi thứ từ con trỏ đến cuối file
  - y^	Copy mọi thứ từ đầu đến con trỏ
  - yW	Copy đến từ tiếp theo (không bao gồm từ tiếp theo)
  - p	Paste buffer vào trên dòng hiện tại
  - P	Paste buffer vào dưới dòng hiện tại
#### 6. Undo
  - u	Để undo, ấn lần nữa để redo
  - U	Để undo mọi thay đổi trên dòng đó
*Vi chỉ có thể undo được 1 mức, nên đây cũng có thể gọi là một điểm trừ của vi*
#### 7. Tìm kiếm
  - :/[STRING]	Để tìm kiếm một string ở phía phải con trỏ
  - :?[STRING]	Để tìm kiếm một string ở phía trái con trỏ
  - n	Đi đến phần tử tìm kiếm tiếp theo
  - N	Đi đến phần tử tìm kiếm theo hướng ngược lại
#### 8. Các câu lệnh khác
  - .	Lặp lại lệnh trước đó
  - J	Nối hai đoạn lại với nhau
  - :! [COMMAND]	Thực hiện lệnh trên terminal rồi trở lại vi
  

