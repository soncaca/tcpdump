# tcpdump
Capture gói tin là một trong những cách cơ bản nhất và mạnh mẽ để làm phân tích mạng. Bạn có thể tìm hiểu hầu như bất cứ điều gì về những gì đang xảy ra bên trong một mạng bằng cách chặn và kiểm tra các dữ liệu thô mà đi qua nó. Công cụ phân tích mạng lưới hiện đại có thể nắm bắt, giải thích và mô tả lưu lượng truy cập mạng này theo một cách ...

Một câu lệnh tcpdump gồm 2 phần: các option theo sau là các biểu thức số học.

Biểu thức số học xác định các gói tin để capture, và các tùy chọn xác định, trong một phần, làm thế nào những gói dữ liệu được hiển thị cũng như các khía cạnh khác của hành vi  chương trình.

Có thể xem các options bằng cách gõ : `man tcpdump`

Còn đây là một vài trong số những cái đáng chú ý:

-i interface: chọn giao diện card mạng cần lắng nghe ( eth0,wlan0..)
-v, -vv, -vvv: hiển thị nhiều thông tin.
-q: ít thông tin.
-e: hiển thị thông tin lớp liên kết (MAC Address)-
-N: display relative hostnames.
-t: không in mốc thời gian.
-n: vô hiệu hóa phân giải tên như vậy bạn không phải chờ đợi trên những phản hồi DNS để hiển thị các gói tin.
-s0 (or -s 0): thiết lập số lượng gói tin xuất hiện. 0 có nghĩa là hiển thị đầy đủ gói tin

Biểu thức lọc (Filter Expression)

Các biểu thức lọc là tiêu chuẩn logic (true hoặc false) cho "phù hợp" các gói tin. Tất cả các gói không phù hợp với các biểu thức được bỏ qua. Các cú pháp biểu thức lọc rất mạnh mẽ và linh hoạt. Chủ yếu bao gồm các từ khóa được gọi là primitives, đại diện cho các gói phù hợp với tiêu chuẩn,  chẳng hạn như giao thức, địa chỉ, cổng và điều hướng. Có thể kết hợp các biểu thức and hoặc or, hoặc nhóm lại và lồng vào nhau bằng dấu ngoặc đơn và phủ định với not.

Một vài ví dụ về biểu thức lọc:

tcp

port 25 and not host 10.0.0.3

icmp or arp or udp

vlan 3 and ether src host aa:bb:cc:dd:ee:ff

arp or udp port 53

icmp and \(dst host mrorange or dst host mrbrown\)
