# tcpdump
Capture gói tin là một trong những cách cơ bản nhất và mạnh mẽ để làm phân tích mạng. Bạn có thể tìm hiểu hầu như bất cứ điều gì về những gì đang xảy ra bên trong một mạng bằng cách chặn và kiểm tra các dữ liệu thô mà đi qua nó. Công cụ phân tích mạng lưới hiện đại có thể nắm bắt, giải thích và mô tả lưu lượng truy cập mạng này theo một cách ...

Một câu lệnh tcpdump gồm 2 phần: các option theo sau là các biểu thức số học.

Biểu thức số học xác định các gói tin để capture, và các tùy chọn xác định, trong một phần, làm thế nào những gói dữ liệu được hiển thị cũng như các khía cạnh khác của hành vi  chương trình.

Có thể xem các options bằng cách gõ : `man tcpdump`

Còn đây là một vài trong số những cái đáng chú ý:

-i interface: chọn giao diện card mạng cần lắng nghe ( eth0,wlan0..)

-v, -vv, -vvv: hiển thị nhiều thông tin.

-q: ít thông tin.

-e: hiển thị thông tin lớp liên kết (MAC Address)

-N: display relative hostnames.

-t: không in mốc thời gian.

-n: vô hiệu hóa phân giải tên như vậy bạn không phải chờ đợi trên những phản hồi DNS để hiển thị các gói tin.

-s0 (or -s 0): thiết lập số lượng gói tin xuất hiện. 0 có nghĩa là hiển thị đầy đủ gói tin

Biểu thức lọc (Filter Expression)

Các biểu thức lọc là tiêu chuẩn logic (true hoặc false) cho "phù hợp" các gói tin. Tất cả các gói không phù hợp với các biểu thức được bỏ qua. Các cú pháp biểu thức lọc rất mạnh mẽ và linh hoạt. Chủ yếu bao gồm các từ khóa được gọi là primitives, đại diện cho các gói phù hợp với tiêu chuẩn,  chẳng hạn như giao thức, địa chỉ, cổng và điều hướng. Có thể kết hợp các biểu thức and hoặc or, hoặc nhóm lại và lồng vào nhau bằng dấu ngoặc đơn và phủ định với not.



tcpdump -D : liệt kê danh sách card mạng

* host // tìm kiếm lưu lượng truy cập dựa trên địa chỉ IP (cũng hoạt động với host name  nếu bạn không sử dụng -n)


` tcpdump host 1.2.3.4`

* src, dst // bắt gói tin  từ chỉ một nguồn hoặc đích (loại bỏ một phía của một host conversation)

` tcpdump src 2.3.4.5`

`tcpdump dst 3.4.5.6 `

* net // bắt gói tin toàn bộ một mạng

` tcpdump net 1.2.3.0/24 `

* bắt goi tin đường truyền dựa trên các giao thức// làm việc với  tcp, udp, arp and icmp.

`tcpdump icmp`

* port //  chỉ thấy lưu lượng truy cập đến hoặc từ một cổng nhất định

`tcpdump port 3389`

* src, dst port // lọc dựa trên  source or destination port 

` tcpdump src port 1025 `

`tcpdump dst port 389`

* src/dst, port, protocol // kết hợp cả 3

` tcpdump src port 1025 and tcp`

` tcpdump udp and src port 53`

Bạn cũng có tùy chọn để lọc theo một loạt các port thay vì khai báo, riêng lẻ, và chỉ thấy các gói tin đó là trên hoặc dưới một kích thước nhất định.

* Port Ranges // see traffic to any port in a range xem lưu lượng truy nhập từ dải công xác định

`tcpdump portrange 21-23`

* Lọc kích cỡ gói tin // chỉ thấy các gói tin theo một kích thước nhất định (theo byte)

`tcpdump less 32`

`tcpdump greater 128`

hoặc

`tcpdump > 32`

`tcpdump <= 128`

# Lưu lại lưu lượng capture thành 1 file

tcpdump cho phép bạn lưu lại lưu lượng truy nhập bắt được thành 1 file với tùy chọn : ` -w ` và để đọc lại file đó thì ta sử dụng tùy chọn : ` -r`

* ví dụ : lưu lại thành 1 file

`tcpdump -s 1514 port 80 -w capture_file`

* để đọc lại capture_file

`tcpdump -r capture_file`

# Sáng tạo từ sự kết hợp

Một biểu thức đơn có thể cho ta được kết quả mong muốn , nhưng sự kỳ diệu thực sự của tcpdump xuất phát từ khả năng kết hợp chúng theo những cách sáng tạo để cô lập một cách chính xác những gì bạn đang tìm kiếm. Có ba cách để làm kết hợp , và nếu bạn là dân IT thì các biểu thức logic sau đây khá là quen thuộc với chúng ta:

1) AND
  
  `and hoặc &&`
  
2) OR

  `or hoặc ||`
  
3) phủ định

  `not hoặc ! `
  
  ví dụ:
  
* bắt lưu lượng từ 10.5.2.3 hướng đến cổng 3389

tcpdump -nnvvS src 10.5.2.3 and dst port 3389

* Lưu lượng bắt đầu từ mạng 192.168 cho đến mạng  10 hoặc 172.16 

tcpdump -nvX src net 192.168.0.0/16 and dst net 10.0.0.0/8 or 172.16.0.0/16

* Non-ICMP traffic destined for 192.168.0.2 from the 172.16 network

tcpdump -nvvXSs 1514 dst 192.168.0.2 and src net and not icmp

* Traffic originating from Mars that isn’t to the SSH port

tcpdump -vv src mars and not dst port 22

# Nhóm

có thể nhóm các biểu thức laị với nhau  bằng ` () ` và phải đặt trong dấu ` '' ` -  Dấu nháy đơn được sử dụng để báo cho tcpdump để bỏ qua một số ký tự đặc biệt - trong trường hợp này "()" dấu ngoặc.

* ví dụ:
` tcpdump src 10.0.2.4 and (dst port 3389 or 22) ` --> sẽ báo lỗi

` tcpdump ‘src 10.0.2.4 and (dst port 3389 or 22)’ ` hoặc  ` tcpdump ‘src 10.0.2.4 and \(dst port 3389 or 22\)’ `  --> đúng

  
  







