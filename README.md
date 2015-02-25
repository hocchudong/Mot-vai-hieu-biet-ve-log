#Một số hiểu biết cơ bản về log
<a name="ml"></a>
[Mục lục](#ml)

- [1. Khái niệm cơ bản về log](#1)
- [2. Log tập trung](#2)
- [3. Syslog, Rsyslog](#3)
- [Tài liệu tham khảo](#4)

---
<a name="1"></a>
####1. Khái niệm về log
*Log là gì? Log để làm gì?*

Trước hết Bạn là người quản trị mạng của một doanh nghiệp, trong hệ thống mạng của bạn có một máy chủ chứa dữ liệu rất quan trọng. Một buổi tối bạn để máy chủ đó chạy suốt đêm nhưng khi về đến nhà bạn truy cập vào máy chủ thì báo lỗi từ chối dịch vụ do không thể kết nối, buổi sáng bạn vội vã đến xem xét tình hình thì thấy một số dữ liệu đã bị mất và vấn đề lúc này là xem ai đã gây ra vấn đề trên. Vậy phải làm thế nào để điều tra xử lý, hay đơn giản là tìm nguyên nhân để khắc phục hậu quả vừa xảy ra. Log sẽ giúp bạn làm việc này.

<img src="https://lh6.googleusercontent.com/-iMgGDt5jbEE/UYzNUUHrCXI/AAAAAAAAAaA/6bpFV0uCO5s/w712-h534-no/logstack.jpg">
<ul><ul><ul><ul><ul><ul><ul><ul>nguồn https://plus.google.com/+RainerGerhards/posts</ul></ul></ul></ul></ul></ul></ul></ul>

Vậy nên tác dụng của log là
- Log **ghi lại liên tục** các thông báo về hoạt động của cả hệ thống hoặc của các dịch vụ được triển khai trên hệ thống và file tương ứng. Log file thường là các file văn bản thông thường dưới dạng “clear text” tức là bạn có thể dễ dàng đọc được nó, vì thế có thể sử dụng các trình soạn thảo văn bản (vi, vim, nano...) hoặc các trình xem văn bản thông thường (cat, tailf, head...) là có thể xem được file log.
- Các file log có thể nói cho bạn bất cứ thứ gì bạn cần biết, để giải quyết các rắc rối mà bạn gặp phải miễn là bạn biết ứng dụng nào, tiến trình nào được ghi vào log nào cụ thể.
- Trong hầu hết hệ thống Linux thì **`/var/log`** là nơi lưu lại tất cả các log.
- Như đã nói ở trên, tác dụng của log là vô cùng to lớn, nó có thể giúp quản trị viên **theo dõi hệ thống của mình tôt hơn, hoặc giải quyết các vấn đề gặp phải** với hệ thống hoặc service. Điều này đặc biệt quan trọng với các hệ thống cần phải online 24/24 để phục vụ nhu cầu của mọi người dùng.
---
<a name="2"></a>
####2. Log tập trung

**Tác dụng của log là vô cùng to lớn vậy làm thế nào để quản lý log tốt hơn?**

Để quản lý log một cách tốt hơn, xu thế hiện nay sẽ sử dụng **log tập trun**g. Vậy log tập trung là gì? Tác dụng của nó thế nào?

Hiểu một cách đơn giản : Log tâp trung là quá trình tập trung, thu thập, phân tích... các log cần thiết từ nhiều nguồn khác nhau về một nơi an toàn để thuận lợi cho việc phân tích, theo dõi hệ thống.

**Tại sao lại phải sử dụng log tập trung?**

- Do có nhiều nguồn sinh log <ul>
<li> Có nhiều nguồn sinh ra log, log nằm trên nhiều máy chủ khác nhau nên khó quản lý.</li>
<li>Nội dung log không đồng nhất (Giả sử log từ nguồn 1 có có ghi thông tin về ip mà không ghi thông tin về user name đăng nhập mà log từ nguồn 2 lại có) -> khó khăn trong việc kết hợp các log với nhau để xử lý vấn đề gặp phải.</li>
<li>Định dạng log cũng không đồng nhất -> khó khăn trong việc chuẩn hóa</li>
</ul>

<img src="http://tomstockton.us/pictures/062/too_many_logs.jpg">
nguồn http://tomstockton.us


- Đảm bảo tính toàn vẹn, bí mật, sẵn sàng của log.
<ul>
<li> Do có nhiều các rootkit được thiết kế để xóa bỏ logs.
<li> Do log mới được ghi đè lên log cũ.
-> Log phải được lưu trữ ở một nơi an toàn và phải có kênh truyền đủ đảm bảo tính an toàn và sẵn sàng sử dụng  để phân tích hệ thống.
</ul>

**Do đó lợi ích của log tập trung đem lại là**
- Giúp quản trị viên có cái nhìn chi tiết về hệ thống -> có định hướng tốt hơn về hướng giải quyết
- Khi hệ thống bị tấn công, tất cả các hoạt động của hệ thống đều được lưu lại ở một nơi an toàn -> đảm bảo tính toàn vẹn log để phân tích điều tra
- Log tập trung kết hợp với các ứng dụng thu thập và phân tích log khác nữa giúp cho việc phân tích log trở nên thuận lợi hơn -> giảm thiểu nguồn nhân lực.
</ul>

---
<a name="3"></a>
####3. Syslog và Rsyslog
Syslog là một giao thức client/server là giao thức dùng để chuyển log và thông điệp đến máy nhận log. Máy nhận log thường được gọi là syslogd, syslog daemon hoặc syslog server. Syslog có thể gửi qua UDP hoặc TCP. Các dữ liệu được gửi dạng cleartext. Syslog dùng cổng 514.

Syslog được phát triển năm 1980 bởi Eric Allman, nó là một phần của dự án Sendmail, và ban đầu chỉ được sử dụng duy nhất cho Sendmail. Nó đã thể hiện giá trị của mình và các ứng dụng khác cũng bắt đầu sử dụng nó. Syslog hiện nay trở thành giải pháp khai thác log tiêu chuẩn trên Unix-Linux cũng như trên hàng loạt các hệ điều hành khác và thường được tìm thấy trong các thiết bị mạng như router

Rsyslog - "The rocket-fast system for log processing" được bắt đầu phát triển từ năm 2004 bởi Rainer Gerhards  rsyslog là một phần mềm mã nguồn mở sử dụng trên Linux dùng để chuyển tiếp các log message đến một địa chỉ trên mạng (log receiver, log server) Nó thực hiện giao thức syslog cơ bản, đặc biệt là sử dụng TCP cho việc truyền tải log từ client tới server. Hiện nay rsyslog là phần mềm được cài đặt sẵn trên hầu hết hệ thống Unix và các bản phân phối của Linux như : Fedora, openSUSE, Debian, Ubuntu, Red Hat Enterprise Linux, FreeBSD…

Twitter của tác giả Rsyslog [Twitter](https://twitter.com/rgerhards/)

---
<a name="4"></a>
Bài viết sử dụng tài liệu tham khảo sau:
```sh
http://en.wikipedia.org/wiki/Syslog
http://en.wikipedia.org/wiki/Rsyslog
https://phulc.wordpress.com/tag/su-can-thiet-cua-he-thong-quan-ly-log-tap-trung/

```


