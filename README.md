# Linux Kernel Module: Module Tạo Một Số Ngẫu Nhiên

### Lý do thực hiện
- Đồ án thực hành môn Hệ điều hành.
- Tìm hiểu về Linux kernel module và hệ thống quản lý file và device trong  linux, giao tiếp giữa tiến trình ở user space và code kernel space.
### Kết quả
- Module sinh một số ngẫu nhiên thành công.
- Tạo một character device cho phép tiến trình ở userspace có thể open và read các số ngẫu nhiên.
### Hệ điều hành và Ngôn ngữ được dùng
- Module tạo trên hệ điều hành **Ubuntu 18.04 LTS**, phiên bản **Linux headers 4.4.0-31-generic**.
- File module và test module được viết bằng C với thư viện hỗ trợ linux.
### Một số hàm/lệnh tiêu biểu
#### Cài đặt trong file module
-	**[module]_init()** và **[module]_exit()** là hàm khởi chạy và thoát của module. [module] thay thế bằng tên của module.
-	**printk()** giống với **printf()**. Nhưng có một khác biệt quan trọng là lập trình viên phải xác định log level khi gọi hàm này. Log level được định nghĩa trong **linux/kern_levels.h** như là: **KERN_EMERG**, **KERN_ALERT**, **KERN_CRIT**, **KERN_ERR**, **KERN_WARNING**, **KERN_NOTICE**, **KERN_INFO**, **KERN_DEBUG**, và **KERN_DEFAULT**. Hàm này nằm trong **linux/printk.h**, cũng được bao hàm bởi **linux/kernel.h**
-	**dev_open()**: Được gọi mỗi khi device được mở từ user space.
-	**dev_read()**: Được gọi mỗi khi dữ liệu được gửi ttừ device đến user space.
-	**dev_write()**: Được gọi mỗi khi dữ liệu được gửi từ user space tới device.
-	**dev_release()**: Được gọi khi device được đóng từ user space.
-	**get_random_byte(void *buf, int nbytes)**: Hàm trả về số ngẫu nhiên được sinh ra từ những byte ngẫu nhiên, lưu trong buffer.
#### Syntax trong Makefile
-	**obj-m**: định nghĩa một loadable module. Ta có thể dùng **obj-y** cho những mục đích khác. Syntax ngày càng phức tạp khi xây dngj module từ nhiều đối tượng khác nhau.
-	**$(shell uname -r)**: trả về phiên bản kernel hiện thời.
-	**-C**: chuyển đường dẫn về đường dẫn kernel trước khi thực hiện.
-	**M=$(PWD)**: cho lệnh “**make**” biết “địa chỉ” project file.
-	**modules**: xác định mục đích cho external kernel modules. Ở đây, có nghĩa là xây dựng module.
#### Một số lệnh thực hiện linux kernel module và character device
- **sudo insmod [Tên module].ko** : Gắn module vào kernel space.
- **lsmod**: Kiểm tra những module có trong kernel space.
- **modinfo [Tên module].ko**: Thông tin về module.
- **sudo rmmode [Tên module].ko**: Tháo module ra khỏi kernel space.
- **tail -f /var/log/kern.log**: Xem thông tin ghi nhận lại khi chạy xong module. 
- **sudo ./test**: Chạy mã mẫu device test.
### Cách thực hiện 
1. Cập nhật và cài đặt Linux header phù hợp
* Mở terminal
* Gõ "**sudo apt-get update**" để cập nhật ubuntu.
* Gõ "**apt-cache search linux-headers-$(uname -r)**" để xác định Linux header phù hợp
* Gõ "**sudo apt-get install linux-headers-…**" thay thế “…” theo phiên bản Linux header tìm được để cài đặt Linux header.
Từ khóa "**uname**" cho phép xác định chính xác phiên bản Linux header cần cài.
2. Dùng lệnh "**mkdir Module_First**" để tạo một thư mục mới tên **Module_First**. Tên có thể tùy chọn.
3. Dùng lệnh "**cd Module_First**" để vào thư mục.
4. Chép 3 file: **Module_Create_Random_Number.c**, **testModule_Create_Random_Number.c** và **Makefile** vào thư mục trên.
5. Dùng lệnh "**make**" để thực thi file Makefile. Sau đó kiểm tra xem file **Module_Create_Random_Number.ko** đã sinh ra hay chưa bằng lệnh "**ls -l *.ko**".
Kết quả:
6. Dùng lệnh "**sudo insmod Module_Create_Random_Number.ko**" để thêm module vào kernel space.
Kết quả:
7. Dùng lệnh "**lsmod**" để xem danh sách các module đang thực thi trên kernel space.
Kết quả:
8. Dùng lệnh "**sudo ./test**" để chạy file test "**testModule_Create_Random_Number.c**".
Kết quả:
9. Dùng lệnh "**sudo tail -f /var/log/kern.log**" để xem thông báo của hệ thống.
Kết quả:

### Giấy phép
Theo GPL.
### Nguồn tham khảo
Các file đã viết bên trên được thực hiện dựa vào những ví dụ trong [bài viết](http://derekmolloy.ie/writing-a-linux-kernel-module-part-2-a-character-device/) của  Dr.Derek Molly.
