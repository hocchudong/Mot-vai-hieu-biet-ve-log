
# Các giao thức liên quan đến syslog

<a name="index"></a>
## [Mục Lục](#index)
- [1. TCP/UDP](#1)
- [2. SSL/TLS](#2)
- [3. Tổng kết](#3)


<a name="1"></a>
## 1. TCP/UDP

Như ta đã biết syslog sử dụng giao thức UDP hoặc TCP để truyền tin trên mạng vậy TCP và UDP khác nhau thế nào?

### TCP 

TCP là viết tắt của Transmission Control Protocol. Đó là giao thức phổ biến nhất được sử dụng trên Internet.

TCP hoạt động theo hướng kết nối (connection-oriented), trước khi truyền dữ liệu giữa 2 máy, nó thiết lập một kết nối giữa 2 máy theo phương thức "bắt tay 3 bước (three-way-hand-shake)" bằng cách gửi gói tin ACK từ máy đích sang máy nhận, trong suốt quá trình truyền gói tin, máy gửi yêu cầu máy đích xác nhận đã nhận đủ các gói tin đã gửi, nếu có gói tin bị mất, máy đích sẽ yêu cầu máy gửi gửi lại, thường xuyên kiểm tra gói tin có bị lỗi hay ko, ngoài ra còn cho phép qui định số lượng gói tin được gửi trong một lần gửi (window-sizing), điều này đảm bảo máy nhận nhận được đầy đủ các gói tin mà máy gửi gửi đi --> truyền dữ liệu chậm hơn UDP nhưng đáng tin cậy hơn UDP.

### UDP 

UDP là viết tắt của User Datagram Protocol - một gói tương tự như một gói của thông tin.

UDP hoạt động theo hướng không kết nối (connectionless), không yêu cầu thiết lập kết nối giữa 2 máy gửi và nhận, ko có sự đảm bảo gói tin khi truyền đi cũng như không thông báo về việc mất gói tin, ko kiểm tra lỗi của gói tin
--> truyền dữ liệu nhanh hơn UDP do cơ chế hoạt động có phần đơn giản hơn tuy nhiên lại ko đáng tin cậy bằng TCP

Do đặc điểm của mình, UDP vẫn thường được sử dụng trong các ứng dụng mà đòi hỏi về tốc độ mà ít cần phải sửa lỗi. Ví dụ, UDP thường được sử dụng cho chương trình phát sóng trực tiếp và trò chơi trực tuyến.

### Ví dụ

- TCP: Người đưa thư phải đảm bảo thư có chữ ký của người gửi và đúng địa chỉ đó với người nhận là duy nhất để gửi thư đúng.
- UDP : Người đưa thư nhận thư, vứt vào hòm thư, tuy nhiên thư này gửi cho người chồng mà người vợ lại thấy ghi địa chỉ nhà mình nên cũng có thể mở ra đọc được

---

<a name="2"></a>
## 2. SSL/TLS

Giao thức SSL (Secure Socket Layer) được phát triển bởi Netscape, ngày nay giao thức SSL đã được sử dụng rộng rãi trên World Wide Web trong việc xác thực và mã hoá thông tin giữa client và server. Tổ chức IETF (Internet Engineering Task Force ) đã chuẩn hoá SSL và đặt lại tên là TLS (Transport Layer Security). Mặc dù là có sự thay đổi về tên nhưng TSL chỉ là một phiên bản mới của SSL. Phiên bản TSL 1.0 tương đương với phiên bản SSL 3.1. Tuy nhiên SSL là thuật ngữ được sử dụng.

SSL được thiết kế như là một giao thức riêng cho vấn đề bảo mật có thể hỗ trợ cho rất nhiều ứng dụng. Giao thức SSL hoạt động bên trên TCP/IP và bên dưới các giao thức ứng dụng tầng cao hơn như là HTTP (Hyper Text Transport Protocol), IMAP ( Internet Messaging Access Protocol) và FTP (File Transport Protocol). Trong khi SSL có thể sử dụng để hỗ trợ các giao dịch an toàn cho rất nhiều ứng dụng khác nhau trên Internet, thì hiện nay SSL được sử dụng chính cho các giao dịch trên Web.

SSL không phải là một giao thức đơn lẻ, mà là một tập các thủ tục đã được chuẩn hoá để thực hiện các nhiệm vụ bảo mật sau:
- Xác thực server
- Xác thực client
- Mã hóa đường truyền

SSL hoạt động thế nào?

Công nghệ SSl bảo vệ những giao dịch trực tuyến và năng cao mức độ tin cậy của website đối với khách hàng chỉ trong 3 bước cơ bản

<img src="http://i.imgur.com/rzuiDJL.jpg">

---

<a name="3"></a>
## 3. Tổng kết

Vậy ứng dụng của các giao thức này vào syslog thế nào

- UDP đảm bảo việc truyền nhận log từ client tới server, có thể sử dụng trong mạng tin cậy đòi hỏi tốc độ cao
- TCP đảm bảo độ tin cậy gói tin log truyền nhận từ client tớ server từ xa, đảm bảo không bị mất mát dữ liệu và nhận đúng dữ liệu từ máy client.
- SSL/TLS : Vì Syslog là một giao thức gửi cleartext. Điều đó có nghĩa là nó có thể bị sniff "chặn bắt" trên đường truyền. Do đó SSL/TLS mã hóa đường truyền tin giữa client và server để giữ cho nội dung log gửi được an toàn

---

## Tham khảo:

- http://en.wikipedia.org/wiki/Transport_Layer_Security
- http://www.rsyslog.com/doc/rsyslog_tls.html
