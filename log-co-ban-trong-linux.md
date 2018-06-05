# Log trong linux

## Giới thiệu
Một trong những điều khiến GNU/Linux trở thành một OS tuyệt vời là việc mọi thứ diễn ra trong hệ thống sẽ được ghi lại theo một số cách. Các thông
tin này là vô giá khi sử dụng hệ thống và là thứ đầu tiên phải nghĩ đến khi bạn trouble-shoot hệ thống và khi gặp vấn đề về application. Các file
logs sẽ nói cho bạn biết mọi thứ bạn muốn, miễn là bạn biết file log nào cần phải tìm đến đầu tiên.

Hệ thống Ubuntu cung cấp các thông tin cần quan trọng bằng việc sử dụng hệ thống file log đa dạng. Các file log này thường có format dưới dạng
plain ASCII text, và phần lớn chúng nằm trong `/var/log`. Syslogd kiểm soát một nửa log hệ thống và một vài ứng dựng nhất định. Các ứng dụng tự tạo
file log riêng, và lưu trong thư mục `/var/log`.

## Log hệ thống
Các file log hệ thống lưu những thông tin hoạt đông của các function thiết yếu trong hệ thống. Ví dụ bao gồm cơ chế ủy quyền, các tiến trình hệ
thống, các message hệ thống, syslog...

#### Authorization Log 
Authorization log lưu các thông tin về các hệ thống ủy quyền, các cơ chế ủy quyền các user, nhắc nhở về user password, ví dụ như hệ thống PAM
(Pluggable Authentication Module), *sudo* command, các đăng nhập tới *sshd*. Các thông tin này được lưu lại trong `/var/log/auth.log`
File log cung cấp các thông tin về đăng nhập user, việc sử dụng *sudo* command.

#### Daemon Log 
Một **daemon** là một chương trình được chạy ở nền hệ thống, biểu diễn một vài hoạt động quan trọng của những hệ thống. Các tiến trình này lưu log
trong `/var/log/daemen.log` và chứa các thông tin về việc hệ thống đang chạy như thế nào và các tiến trình ứng dụng như *gdm* (Gnome Display Manager)
, *hcid* (Bluetooth HCI daemon), *mysqld* (MYSQL database)

#### Debug Log
Các file log về debug lưu tại `/var/log/debug`, cung cấp thông tin cần thiết cho việc trouble-shooting các custom-build kernel.

#### Kernal Ring Buffer
Đây không hẳn là một file log, phần nào giống một khu vực trong một kernal đang chạy, bạn có thể query tới các message về việc kernal bootup thông
qua *dmesg*. Để thấy các message, sử dụng:

```sh
dmesg | less
```
Hoặc tìm kiếm các dòng để cập đến Plug & Play systen, sử dụng *grep* như sau :

```sh
dmesg | grep pnp | less
```

#### System log
Log hệ thống thông thường chứa các thông tin mặc định của hệ thống, thường được lưu trong `/var/log/syslog` hoặc `/var/log/message`.

## Log rotation - Xoay vòng file log
Thông thường khi nhìn vào thư mục `/var/log`, bạn thường thấy các log file có dạng như "daemon.log.0", "daemon.log.1.gz", đó chính là các "rotated"
log file. Hệ thống sẽ tự động nén các file log cũ lại sau một time-frame được định sẵn, và bắt đầu một file log gốc mới. 

Mục đích của quá trình "log rotation" nhằm lưu trữ và nén các log cũ để tiết kiệm không gian ổ đĩa, nhưng vẫn sẵn sàng dùng lại được nếu cần.

Thông thường, quá trình "rotation" được cấu hình trong file `/etc/logrotate.conf`. Các file cấu hình *rotate* riêng cho một số file log như:
rsyslog, dpkg, apache, apt... sẽ được thêm vào trong `/etc/logrotate.d/`